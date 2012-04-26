---
layout: post
title: Parallelism in Clojure
categories: clojure
---

Until now, I hadn't a clear understanding of the difference between *concurrency* and *parallelism*. 

## Definitions ##

First we need a definition of both, next we'll explore what tools we have at our disposal to create parallel programs in Clojure. Here it is:

- **Concurrency**: it's the coordination of multiple threads of executions that usually access and modify some *shared state*. Here the main ingredients are *threads* and *shared state*.
- **Parallelism**: it's an *optimization technique* that also involves state but the main goal is to efficiently use all the available resources (tipically computational, but also bandwidth for example), by distributing operations that are executed *simultaneously* (rather than interleaved as in the concurrency case). Note that with parallelism the multiple simultaneous evaluations can take place on different cores or even in different machines, in the latter case we also have *distribution*.

## Concurrency ##

Clojure has excellent support for [concurrency](http://clojure.org/state) because it offers an high level mechanism (the *Software Transactional Memory*, in short STM) for persistent immutable data structures. This mechanism makes possible to write concurrent programs eliminating at its root problems like deadlocks and race conditions when you need shared mutable state between threads.

- TODO: But how do you use multiple threads in your code?
- TODO: Why parallelism is important
- TODO: Parallelism in Clojure

But if you want *parallelism*? 

## Parallelism

The catch with parallelism is that computations has to have a certain kind of structure to be parallelizable. For example, the application of a function to a seq using `map` is trivially parallelizable because every application is independed to the others. Indeed, there is a version of map that uses parallelism to distribute computations to all available cores: `pmap`. 

The docstring of the `pmap` function recites:

> clojure.core/pmap
> ([f coll] [f coll & colls])
>  Like map, except f is applied in parallel. Semi-lazy in that the
>  parallel computation stays ahead of the consumption, but doesn't
>  realize the entire result unless required. **Only useful for
>  computationally intensive functions where the time of f dominates
>  the coordination overhead.**

(Bold is mine). So there is a coordination overhead to pay attention to. For instance, the first example I tried was: multiplying every number of a large seq by itself. In this case the data set was large, but the computations are trivial, so the coordination overhead dominated the total computation time. Result: the `pmap` version was *slower*.

However, there is a technique that you can use to parallelize even trivial operations and it's called *chunking*. 

- TODO ...

The takeaway here is: *you have to understand the tools you are using*. Especially in this case, since the subject is not exactly trivial to grasp in all its implications.

So we need a computation that's not trivial, for example some regexp matching. First, let's create a function to generate a string of random characters:

```clojure
(def alphabet (map char (range (int \a) (inc (int \z)))))

(defn generate-str [coll length]
  (apply str (take length (repeatedly #(rand-nth coll)))))

```

Next, we create a function to match a specific sequence of characters in a string:

```clojure
(defn find-goodness [text]
  (re-seq #"clojure" text))

```

And now, let's compare the performance of `map` vs `pmap` in this case. Note that I'm running this on my dual-core laptop.

```clojure
(def texts (repeat 1000 (generate-str alphabet 1000000)))

(time (dorun (map find-goodness texts)))
; "Elapsed time: 2893.825668 msecs"

(time (dorun (pmap find-goodness texts)))
; "Elapsed time: 1692.288507 msecs"
```

Here you can see that by `pmap` has finished in nearly half the time by parallelizing the computations on both cores, whereas `map` used only one core.

Conversely, an example of a problem that cannot be parallelized is the realization of a lazy seq. Since every value depends on the preceding one, you cannot distribute anything. All computations has to be done sequentially.
