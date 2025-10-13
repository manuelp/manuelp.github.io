---
layout: post
title: Clojure and Java data structures interoperability
categories: blog
---

Clojure is a language [designed to be hosted](https://clojure.org/jvm_hosted), this means that it utilizes all the power of the hosting platform *without trying to hide or abstract it*. This design choice has several consequences:

* You have full access to the hosting platform.
* Usually Clojure code can interoperate nicely with other code that runs in the same platform, even if it's written in another language (for example Clojure on the JVM can easily interoperate with code written in Java, Ruby, Python, Groovy, etc).
* It uses the basic types available on the hosting platform and builds on them.

However, Clojure emphasizes *functional programming* and *immutability*, and this has to be taken into account when interoperating with code written in another language that works on lossy *mutable state*.

## Using Clojure data from Java ##

Usually there is no problem in using Clojure data structures in Java code since all data types in Clojure implements the relevant Java interfaces like `java.util.List`, `java.util.Map`, etc.

Let's see the classes and interfaces [hierarchies](https://github.com/manuelp/obj-hierarchy) for the main data structures of Clojure: 

* Lists
  * Class: `clojure.lang.PersistentList`, interfaces: `clojure.lang.ISeq`, `clojure.lang.Sequential`, **`java.util.List`**, `java.io.Serializable`, `clojure.lang.IHashEq`
  * SuperClass: `clojure.lang.ASeq`, interfaces: `clojure.lang.IObj`, `java.io.Serializable`
  * SuperClass: `clojure.lang.Obj`
  * SuperClass: `java.lang.Object`
* Vectors
  * Class: `clojure.lang.PersistentVector`, interfaces: `clojure.lang.IPersistentVector`, `java.lang.Iterable`, **`java.util.List`**, `java.util.RandomAccess`, `java.lang.Comparable`, `java.io.Serializable`, `clojure.lang.IHashEq`
  * SuperClass: `clojure.lang.APersistentVector`, interfaces: `clojure.lang.IFn`
  * SuperClass: `clojure.lang.AFn`
  * SuperClass: `java.lang.Object`
* Maps
  * Class: `clojure.lang.PersistentArrayMap`, interfaces: `clojure.lang.IPersistentMap`, **`java.util.Map`**, `java.lang.Iterable`, `java.io.Serializable`, `clojure.lang.MapEquivalence`, `clojure.lang.IHashEq`
  * SuperClass: `clojure.lang.APersistentMap`, interfaces: `clojure.lang.IFn`
  * SuperClass: `clojure.lang.AFn`
  * SuperClass: `java.lang.Object`
* Sets
  * Class: `clojure.lang.PersistentHashSet`, interfaces: `clojure.lang.IPersistentSet`, `java.util.Collection`, **`java.util.Set`**, `java.io.Serializable`, `clojure.lang.IHashEq`
  * SuperClass: `clojure.lang.APersistentSet`, interfaces: `clojure.lang.IFn`
  * SuperClass: `clojure.lang.AFn`
  * SuperClass: `java.lang.Object`

So you can easily write libraries in Clojure that can be used from Java without problems. Clojure data structures outside the Clojure contex usually behave like regular Java collections, maps and sets.

## Using Java data from Clojure ##

Usually, the other way around is [straightforward](https://clojure.org/java_interop#Java%20Interop-Support%20for%20Java%20in%20Clojure%20Library%20Functions) too. But there are some corner cases that you have to be aware of *when writing libraries that can be used from Java*.

Idiomatic Clojure code often works on data structures through the [seq](https://clojure.org/sequences)(uence) abstraction. In fact a lot of higher-order functions in clojure.core just call `seq` on their collections arguments. But there are corner cases, and we'll look at one of them that I discovered recently when writing the [assert-json](https://github.com/manuelp/assert-json) library.

### A corner case ###

Let's build a regular map:

```clojure
(def m {:a 1 :b 2})

(map class m) ;= (clojure.lang.MapEntry clojure.lang.MapEntry)

(first (first m)) ;= :a
(second (first m)) ;= 1
```

Ok, so `map` when iterating on a... map, works on a *sequence of `clojure.lang.MapEntry`*. And `first` and `second` functions can be used on these `MapEntry` to extract respectively the key and value of the map entry. Easy.

Now let's try it with a `java.util.HashMap`:

```clojure
(def hm (doto (java.util.HashMap.)
              (.put "a" 1)
              (.put "b" 2))

(map class hm) ;= (java.util.HashMap$Entry java.util.HashMap$Entry)

(first (first hm)) ; java.lang.IllegalArgumentException: Don't know how to create ISeq from: java.util.HashMap$Entry
                   ;   RT.java:494 clojure.lang.RT.seqFrom
                   ;   RT.java:475 clojure.lang.RT.seq
                   ;   RT.java:567 clojure.lang.RT.first
                   ;   core.clj:55 clojure.core/first
```

What's the problem here? In the stacktrace we see that `first` calls `seq` on its argument, and `seq` on a `java.util.HashMap$Entry` fails miserably. Why? Let's read the documentation of this function:

```clojure
(doc seq) ;-------------------------
          ;clojure.core/seq
          ;([coll])
          ;Returns a seq on the collection. If the collection is
          ;    empty, returns nil.  (seq nil) returns nil. seq also works on
          ;    Strings, native Java arrays (of reference types) and any objects
          ;    that implement Iterable.
```

Well, since `seq`uence is a sequential list abstraction, every `seq`able data structure must be `Iterable` (or either a String or an array of reference types). If you look again in the *Using Clojure data from Java* section, you'll see that Clojure maps implements the `java.lang.Iterable` interface that in this case iterates on `clojure.lang.MapEntry` instances. These `MapEntry` objects live in this hierarchy:

* Class: `clojure.lang.MapEntry`, interfaces: `clojure.lang.IMapEntry`
* SuperClass: `clojure.lang.AMapEntry`, interfaces: `clojure.lang.IPersistentVector`, **`java.lang.Iterable`**, `java.util.List`, `java.util.RandomAccess`, `java.lang.Comparable`, `java.io.Serializable`, `clojure.lang.IHashEq`
* SuperClass: `clojure.lang.APersistentVector`, interfaces: `clojure.lang.IFn`
* SuperClass: `clojure.lang.AFn`
* SuperClass: `java.lang.Object`

And so `MapEntry` objects are `Iterable` too. That's why `first` on map entries works.

On the Java side? We already know that `HashMap`s are `Iterable` on `java.util.HashMap$Entry` instances. But these `HashMap$Entry` are `Iterable` too? Probably not, let's see the class and interfaces hirarchy:

* Class: `java.util.HashMap$Entry`
* SuperClass: `java.lang.Object`

Uhm, no they are not.

### A better solution ###

The problem here is that *to extract key and value from a map entry* I used the wrong functions. `first` and `second` are more appropriate on generic sequences, instead for this specific task there are more suitable (and interoperable) functions: `key` and `val`.

```clojure
(def m {:a 1 :b 2})

(key (first m)) ;= :a
(val (first m)) ;= 1

(def hm (doto (java.util.HashMap.)
              (.put "a" 1)
              (.put "b" 2))
              
(key (first hm)) ;= "a"
(val (first hm)) ;= 1
```

## Wrap up ##

Clojure and Java are usually nicely interoperable, but pay attention to use the appropriate functions in Clojure to manipulate data structures when it's possible that your code will be used from Java.
