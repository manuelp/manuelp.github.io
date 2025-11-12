---
layout: post
title: Finding pairs duplicates
tags: [Java, FP, Case-Study, Refactoring]
withtoc: yes
mathjax: true
---

In this post I'm going to illustrate how we can write a solution to a simple problem using FP in Java 8, and then generalize it through a series of small incremental refactorings to make it more flexible and reusable.

# Tuples

A [tuple](https://en.wikipedia.org/wiki/Tuple) is an *finite ordered list of $$n$$ elements, where $$n\geq0$$*. Values contained in a tuple can have heterogeneous types. Here are some examples:

```haskell
(1)
(2, "Hello")
(LocalDateTime.now(), "Martha", 123)
```

These data structures are useful when you have values that belong together but it's not worth it to create an ad-hoc type for them.

[Functional Java](https://www.functionaljava.org/) models tuples as [products](https://www.functionaljava.org/javadoc/4.4/functionaljava/fj/P.html) $$P$$ (terminology borrowed from [category theory](https://en.wikipedia.org/wiki/Category_theory)) with various implementations: [P1](https://www.functionaljava.org/javadoc/4.4/functionaljava/fj/P1.html), [P2](https://www.functionaljava.org/javadoc/4.4/functionaljava/fj/P2.html), ..., [P8](https://www.functionaljava.org/javadoc/4.4/functionaljava/fj/P8.html). Here are some examples:

```java
P2<Integer, String> x = P.p(1, "Hello");
P3<Integer, Integer, Integer> y = P.p(1, 2, 3);
```

# Setting the stage: the problem

Our problem statement is this: we have a list of tuples of type `P2<String, String>`, and we want to identify the duplicates.

To specify this requirement, we write a unit test[^1]:

```java
public class FindPairsDuplicatesTest {
  private void assertTuple(P2<String, String> expected,
                           P2<String, String> actual) {
    assertTrue(Equal.p2Equal(Equal.stringEqual, Equal.stringEqual)
                    .eq(actual, expected));
  }

  @Test
  public void can_find_duplicated_pairs() {
    P2<String, String>       p1    = p("a", "b");
    P2<String, String>       p2    = p("c", "d");
    P2<String, String>       p3    = p("a", "b");
    List<P2<String, String>> pairs = list(p1, p2, p3);

    List<P2<String, String>> duplicates = findDuplicates(pairs);

    assertEquals(1, duplicates.length());
    assertTuple(p("a", "b"), duplicates.head());
  }

  private List<P2<String, String>> findDuplicates(
      List<P2<String, String>> pairs) {
    return List.nil();
  }
}
```

> *Warning*: the `List` we use here is [provided](https://www.functionaljava.org/javadoc/4.4/functionaljava/fj/data/List.html) by Functional Java.

We have just implemented our first failing test, with an empty implementation for our behaviour directly in the test class. Now we can implement the actual behaviour to make it pass. I started right away with a simple but representative test case to keep this post resonably short and to the point. If this was a requirement for an actual application, I'd start with a degenerate test case (for example and empty list of pairs as the input) and used *triangulation*[^2] to implement the actual functionality incrementally. 

# First implementation

Here is our first implementation, using the FP paradigm:

```java
private List<P2<String, String>> findDuplicates(
    List<P2<String, String>> pairs) {
  /*
   * This function calculates the hash code of a tuple P2<String, String>
   * using the types supplied by FJ.
   */
  F<P2<String, String>, Integer> hashCode = p -> Hash.p2Hash(Hash.stringHash,
                                                             Hash.stringHash)
                                                     .hash(p);

  /*
   * This is a predicate to find the groups of P2<String, String> with more
   * than one occurence.
   */
  F<P2<Integer, List<P2<String, String>>>, Boolean> moreThanOneOccurence =
      p -> p._2().length() > 1;

  /*
   * Finally, this function takes the first P2 in the list (assuming the
   * list is non empty).
   */
  F<P2<Integer, List<P2<String, String>>>, P2<String, String>> getPair =
      p -> p._2().head();

  /*
   * Highlighting intermediate results' types:
   *
   * TreeMap<Integer, List<P2<String, String>>> grouped =
   *     pairs.groupBy(hashCode);
   * List<P2<Integer, List<P2<String, String>>>> l =
   *    List.iterableList(grouped);
   * return l.filter(moreThanOneOccurence).map(getPair);
   *
   */
  return List.iterableList(pairs.groupBy(hashCode))
             .filter(moreThanOneOccurence)
             .map(getPair);
}
```

The algorithm is implemented here as a *data transformation pipeline of functions*:

1. First we group the pairs using their hash code, which is calculated on their content (using the higher-order function `groupBy` applied to the list of pairs and the `hashCode` function). This way, only pairs with the same hash code (thus equal) will be placed in the same group.
2. Then we filter the groups of pairs with more than one occurence (effectively finding the duplicated pairs).
3. Finally, for every group (that at this point we are sure contains *at least* two equal pairs) we extract the first one.

The output of this pipeline of functions is a list (potentially empty) of tuples `P2<String, String>` that appear at least two times in the input list.

We can convert the intermediate functions to lambda expressions and inline them to obtain a much more concise implementation:

```java
private List<P2<String, String>> findDuplicates(List<P2<String, String>> pairs) {
  Hash<P2<String, String>> p2Hash = Hash.p2Hash(Hash.stringHash,
                                                Hash.stringHash);
  return iterableList(pairs.groupBy(p2 -> p2Hash.hash(p2)))
         .filter(p -> p._2().length() > 1)
         .map(p1 -> p1._2().head());
}
```

# Generalization: tuples with arbitrary types

One thing we can do right away is generalize out method to work on `P2` products of arbitrary types:

```java
private <T, V> List<P2<T, V>> findDuplicates(List<P2<T, V>> pairs) {
  Hash<P2<T, V>> calculateHash = Hash.p2Hash(Hash.<T>anyHash(),
                                             Hash.<V>anyHash());
  return iterableList(pairs.groupBy(p -> calculateHash.hash(p)))
         .filter(p1 -> p1._2().length() > 1)
         .map(p2 -> p2._2().head());
}
```

The test case remains the same, but we have generalized or method to work on pairs or arbitrary (primitive) values, making it more flexible and reusable.

# Generalization: arbitrary value types

We can further generalize our method to make it work on *any* type (not just `P2` tuples). To make this work though, we need to promote the function that calculates the hash code of our values from an internal implementation detail to a parameter:

```java
@Test
public void can_find_duplicated_pairs() {
  P2<String, String>       p1    = p("a", "b");
  P2<String, String>       p2    = p("c", "d");
  P2<String, String>       p3    = p("a", "b");
  List<P2<String, String>> pairs = list(p1, p2, p3);
  Hash<P2<String, String>> p2Hash = Hash.p2Hash(Hash.stringHash,
                                                Hash.stringHash);

  List<P2<String, String>> duplicates = findDuplicates(p2Hash, pairs);

  assertEquals(1, duplicates.length());
  assertTuple(p("a", "b"), duplicates.head());
}

private <T> List<T> findDuplicates(Hash<T> calculateHash, List<T> pairs) {
  return iterableList(pairs.groupBy(p -> calculateHash.hash(p)))
         .filter(p1 -> p1._2().length() > 1)
         .map(p2 -> p2._2().head());
}
```

This way we can reuse `#findDuplicates()` with any value by providing a way to calculate its hash code.

# Partial application

[Partial application](https://en.wikipedia.org/wiki/Partial_application) of a function:

> refers to the process of fixing a number of arguments to a function, producing another function of smaller arity.

Here is a simple example in Javascript to illustrate the concept:

```javascript
function add(a, b) {
	return a + b;
}

function add1(n) {
	return add(n, 1);
}
```

In this example, `add1` is a partial application of the `add` function.

# Final refactoring: partial application!

We can refactor our `#findDuplicates` method into a factory method that takes the hash function as an input and returns a function that actually finds the duplicates according to it. This way we can simulate a partial application of a function with arity 2:

```haskell
findDuplicates :: Hash<T> -> List<T> -> List<T>
```

```java
@Test
public void can_find_duplicated_pairs() {
  P2<String, String>       p1    = p("a", "b");
  P2<String, String>       p2    = p("c", "d");
  P2<String, String>       p3    = p("a", "b");
  List<P2<String, String>> pairs = list(p1, p2, p3);
  Hash<P2<String, String>> p2Hash = Hash.p2Hash(Hash.stringHash,
                                                Hash.stringHash);
  F<List<P2<String, String>>,
    List<P2<String, String>>> dups = findDuplicates(p2Hash);

  List<P2<String, String>> duplicates = dups.f(pairs);

  assertEquals(1, duplicates.length());
  assertTuple(p("a", "b"), duplicates.head());
}

private <T> F<List<T>, List<T>> findDuplicates(Hash<T> calculateHash) {
  return list -> iterableList(list.groupBy(p -> calculateHash.hash(p)))
                 .filter(p1 -> p1._2().length() > 1)
                 .map(p2 -> p2._2().head());
}
```

The advantage is that we have separated the *configuration* of a `[T] -> [T]` duplicates detection function from its *application*. This way we can pre-configure a set of duplicates detection functions and reuse them wherever we need them.

```java
public static F<List<Integer>, List<Integer>> findDuplicatedInts() {
  return findDuplicates(Hash.intHash);
}

public static F<List<String>, List<String>> findDuplicatedStrings() {
  return findDuplicates(Hash.stringHash);
}

public static F<List<Book>, List<Book>> findDuplicatedBooks() {
  return findDuplicates(Hash.p2Hash(Hash.stringHash, Hash.stringHash)
                            .contramap(b -> p(b.getTitle(), b.getAuthor())));
}
```

# Conclusions

We started out with a working implementation that satisfied our initial requirements, and through a series of small incremental refactorings we generalized our function thus making it much more general, flexible and reusable. From a mechanism specialized to detecting duplicates of tuples `(String, String)`, we arrived to a generic algorithm to find duplicates of *arbitrary* types *without changing it, just by generalizing the types involved*. We could even generalize on the `List` type, making the algorith work on any `Iterable` data structure. This way the method would become even more general and applicable to a broader set of use-cases.

This pattern of change is rather common: starting from a specific solution to a particular problem, by *generalizing* it you obtain by definition a more general, flexible and reusable solution. The easiest way to do this is, in my opinion, **"to follow the types"**: by looking at and changing the types it's much easier to see the essence of the problem, to discover hidden abstractions, and in general to transform the code incrementally into a much more general and reusable structure.

---

[^1]: We use the [TDD as if you meant it](https://coderetreat.org/facilitating/activities/tdd-as-if-you-meant-it) technique.
[^2]: See the book [Test-Driven Development: By Example](https://www.amazon.com/Test-Driven-Development-By-Example/dp/0321146530) by Kent Beck.
