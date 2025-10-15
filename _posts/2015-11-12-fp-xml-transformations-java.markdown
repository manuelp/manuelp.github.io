---
layout: post
title: Functional XML transformations in Java
tags: [Case-Study, FP, Java, DSL, Essay]
withtoc: yes
---

I'd like to share something I've been working on in the last few days (between other things), which I think is an interesting case study on the intersection between FP and Java.

# Problem

I started from some existing code that sorely needed a refactoring. The feature covered by that code is easily said: [XML](https://en.wikipedia.org/wiki/XML) transformations. Basically the problem statement is this: *we want to define some generic transformations over [BPMN process definitions](https://en.wikipedia.org/wiki/Business_Process_Model_and_Notation), that are represented as XML [documents](https://docs.oracle.com/javase/7/docs/api/org/w3c/dom/Document.html)*.

The transformations needed are:

* Add/remove elements
* Add/remove attributes
* Add/overwrite textual content to existing elements
* Copy existing elements around
* Copy existing attributes around

To select elements and attributes [XPath](https://en.wikipedia.org/wiki/XPath) expressions has been used. For example we need them to define where to add elements/attributes or what elements/attributes to remove. In general, the existing solution was implemented using the [standard Java DOM model](https://docs.oracle.com/javase/7/docs/api/org/w3c/dom/package-summary.html).

The problems with the original solution were:

* No automated tests.
* No comments.
* Bad naming and general disregard for existing naming conventions.
* Bad design: difficult to understand and modify, mixed responsibilities, etc.

There was a clear need for a redesign.

# Solution

I'm going to sketch a solution to this problem using Java 8 code and type signatures written in a Haskell-Java Frankenstein notation. Since this solution is oriented towards FP, I'm going to use [Functional Java](https://functionaljava.org/) to support me in this task (which BTW is a good library for this kind of stuff).

## Transformations as functions

The core idea is that, conceptually, you can think of an XML transformation as a *pure function*:

```haskell
XmlTransformation :: Document -> Document
```

This function can be expressed using the Functional Java (FJ) [F](https://www.functionaljava.org/javadoc/4.4/functionaljava/fj/F.html) interface:

```java
public interface XmlTransformation extends F<Document, Document> {}
```

Since we don't want to suffer from [primitive obsession](https://gist.github.com/manuelp/5ebabe18145a4239a2a3), we define some useful types like:

```haskell
newtype XPathExpression = XPathExpression String
```

With just this basic abstraction in place, we can implement our basic transformations. For example:

```java
public class AddNode implements XmlTransformation {
  private AddNode(XPathExpression parentExpr, Element e)
  {
    // [...]
  }

  public static XmlTransformation addNode(XPathExpression parentExpr,
	                                      Element e)
  {
	return new AddNode(parentExpr, e);
  }

  public Document f(Document d) {
    // [...]
  }
}
```

> For simplicity, I won't bother with exceptions and I assume that if a transformation fails it simply return the unchanged input document. Everything remains more or less the same by redefining `XmlTransformation` as:
>
> ```haskell
> public interface XmlTransformation
>                  extends Try1<Document, Document, Exception> {}
> ```
>
> But at the end of this post we'll see an alternative way to manage transformation errors.

## Composition!

At this point we have the *terms* (basic transformations) but we lack a way to *combine* them. Well, we can already combine them since in the end they are just functions, but the syntax is not the most convenient:

```java
// Document document;
// Element e1, e2;
XmlTransformation a = addNode(xpathExpression("/a/b"), e1);
XmlTransformation b = addNode(xpathExpression("//c"), e2);
Document transformed = b.f(a.f(document));
```

> I've statically imported `AddNode.addNode` and `XPathExpression.xpathExpression` static factory methods [sic] for brevity.

Well, `XmlTransformation`s are just functions, so we could use the [composition operator](https://www.functionaljava.org/javadoc/4.4/functionaljava/fj/Function.html#compose-fj.F-fj.F-) to combine them. Unfortunately, we need some upcasts to make this work:

```java
// Document document;
// Element e1, e2;
XmlTransformation a = addNode(xpathExpression("/a/b"), e1);
XmlTransformation b = addNode(xpathExpression("//c"), e2);
Document transformed = Function.compose((F<Document, Document>) b,
                                        (F<Document, Document>) a)
	                           .f(document);
```

Anyway, what we want here is an easy and elegant way to combine two transformation and obtain another one. Basically:

```haskell
composeT :: (XmlTransformation, XmlTransformation) -> XmlTransformation
```

This way, starting with basic transformations as building blocks, *we can define arbitrary complex transformations just by composing simpler ones*. It's like *growing a language* for XML transformations.

> If this sounds interesting, you may be interested in the paper "[Why Functional Programming Matters](https://www.cs.kent.ac.uk/people/staff/dat/miranda/whyfp90.pdf)" by John Hughes. 

But what if we want to compose an arbitrary number of transformations? What we need now is a way to build a *data transformation pipeline* by composing our transformations:

```haskell
transformWith :: [XmlTransformation] -> XmlTransformation
```

Yes, we could just write a loop with a call to `composeT`, but by recognizing a much more fundamental abstraction lurking underneath this problem we'll be able to write a very concise, flexible and IMO elegant *combinator* for our `XmlTransformation` function.

It all becomes much easier when you recognize that you are staring in the eyes a *monoid*. The gist of it is that *unary functions forms a monoid over the [function composition](https://en.wikipedia.org/wiki/Function_composition) operation*. In other words, `XmlTransformation` forms a monoid over our `composeT` function. Indeed:

*  The *identity* value is simply the identity function `a -> a`.
* `composeT` is a binary operation, which is *associative*.

Since all the [monoid laws](https://wiki.haskell.org/Typeclassopedia#Laws_5) are satisfied, we can define a proper [`Monoid`](https://www.functionaljava.org/javadoc/4.4/functionaljava/fj/Monoid.html) instance for our `XmlTransformation` type:

```java
Monoid<XmlTransformation> m = Monoid.monoid((t1, t2) -> composeT(t2, t1),
                                            Function.<XmlTransformation>identity());
```

Then we can use this monoid instance to write `transformWith` in a much easier way:

```java
public static XmlTransformation transformWith(List<XmlTransformation> xs)
{
  return m.sumLeft(xs);
}
```

We could even define another method/function to make things nicer:

```java
public static XmlTransformation transform(Document d, XmlTransformation... xs)
{
  List<XmlTransformation> ts = Array.array(xs).toList();
  return m.sumLeft(ts).f(d);
}
```


## DSL

What we've done here is actually defining a very small [DSL](https://en.wikipedia.org/wiki/Domain-specific_language). We have some basic *terms* (our transformations):

* `addElement`
* `removeElement`
* `addAttribute`
* `removeAttribute`
* etc.

And ways to *combine* them:

* `composeT`
* `transformWith`

This is all we need (with a lot of static imports and some tactical use of varargs) to define XML transformations in a declarative way:

```java
Document transformed =
  transform(document,
            addElement(xpathExpression("//b"), node("<foo/>")),
            addAttribute(xpathExpression("/a/d"), attribute("bar", "baz")),
	   		removeElement(xpathExpression("//g"))
			//[...]
			);
```

# Conclusion

This solution IMHO is clean, concise, expressive, flexible and very powerful. It's very easy to define new basic transformations (remember: *each of them can be tested independently!*) and to compose them to build more complex ones (the power of abstraction). Their composition is already been taken care of, once and for all.

It's also easy to extend it to have a more sophisticated error-handling strategy. For example by accumulating errors in [`Either`](https://www.functionaljava.org/javadoc/4.4/functionaljava/fj/data/Either.html) values:

```java
XmlTransformation :: Either<NonEmptyList<String>>, Document>
                     -> Either<NonEmptyList<String>>, Document>
```

The implementation of `composeT` is just a little bit longer (but easy), and the Monoid definition doesn't change at all!

# Bonus: Haskell

Perhaps a comparison like this is a bit unfair, but with Haskell you get all this already for free!

```
Prelude> :i (->)
data (->) a b   -- Defined in `GHC.Prim'
instance Monad ((->) r) -- Defined in `GHC.Base'
instance Functor ((->) r) -- Defined in `GHC.Base'
instance Applicative ((->) a) -- Defined in `GHC.Base'
instance Monoid b => Monoid (a -> b) -- Defined in `GHC.Base'
```

;)
