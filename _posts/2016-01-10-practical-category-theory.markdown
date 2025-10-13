---
layout: post
title: Practical category theory
tags: category-theory, math, type-system
---

Sometimes you find a paper, post, presentation or podcast that hits the nail on the head: it explains some concepts in a clear and simple way, that you can understand right away. You read it at the right moment, and helps you to connect the dots and come up with new knowledge and new perspectives on what you do, how and why. You know, when you have the feeling that several pieces of the puzzle fall into place. Those are the "Aha!" moments that feels so exciting, satisfying and useful, and that you should cultivate by learning and practicing new stuff, exposing yourself to disparate perspectives[^1].

I'm studying, practicing and thinking about functional programming, type systems and design for some time now, and one of the most interesting posts I've read so far is: **[How we used Category Theory to solve a problem in Java](https://techblog.realestate.com.au/how-we-used-category-theory-to-solve-a-problem-in-java/)** by Ken Scambler. It's especially interesting to me since I'm [studying Haskell](https://haskellbook.com/) and applying the concepts I'm learning in Java (when it's useful and practical).

Here I'd just like to give you some general highlights, but it's worth to read the entire post: it's clear, concise and practical.

> category theory gives us a framework to shed light on what makes many good design concepts useful, and why.

You don't need a PhD to learn about Category Theory, as much as you don't really need to know it to be able to use Haskell or any functional language to do "real world programming". But it helps, and gives you a vocabulary and extremely useful "patterns" that can help you to write less code and create more testable, flexible and reusable architectures. 

On a related note: this is especially true with *statically and strongly typed* languages. I won't go into details on why is that right now, if you want to know more take a look at the wonderful blog post [The abject failure of weak typing](https://techblog.realestate.com.au/the-abject-failure-of-weak-typing/), also by Ken Scambler. This was another turning point for me.

> when we say that modules or processes are composable, it means we are able to combine them in self-similar ways that do not add to the cognitive burden of the system; the composite forms are indistinguishable from their component parts. This is a powerful concept, because it implies that the software has an obvious avenue of growth that does not introduce greater complexity.

This is key: *composability*. But not only that: composition that doesn't create additional complexity and concepts. *Simplicity* is one of the most important properties that our solutions should have in my opinion. The Clojure community is strong on this point, and they are right!

> Category theory may have a fearsome reputation, but not really for complexity; in fact it is the very simplicity of this scalable composition mechanism that allows the most terrifying and alien concepts and meta-concepts to be toppled to the ground, and discussed with the very same vocabulary as the most mundane.
>
> This is precisely what it brings to software development. We don’t want ever-growing towers of concepts and meta-concepts any more than mathematicians do!

Finding the underlying basic building blocks, the simplest and orthogonal concepts that can be composed and combined to build *everything else*, is key to create sound, solid and flexible systems. One of the best way to [design a system](https://robotlolita.me/2013/04/27/the-hikikomoris-guide-to-javascript.html) is studying the *domain*, find the basic types and operations, and build everything on them.

> There are many ways to arrive at a good design, but navigating the endless void of the solution space is often the hardest part; simple solutions are frequently far from obvious. Category theory offers a ready-made repertoire of concepts and vocabulary that can help light a path toward a good design; we can think of it as “the study of composable abstractions”. Exposing this very essence of composition provides insights into software design that are not as clearly articulated elsewhere.

And here static and explicit types are maybe not mandatory but makes everything much easier.

> The fact that we’ve successfully brought category theory principles to bear with a such a thoroughly blue-collar language as Java is worth thinking about. Far from being the preserve of researchers and Haskell programmers, these principles apply universally, and can produce simple, clean modules that are easily understood by programmers with no knowledge of the theory. It is well worth the investment of familiarising yourself with the fundamental ideas.

Absolutely: studying FP, Haskell (and OCaml, Clojure, Rust, etc) and category theory will give you a profound understanding on computation, simplicity, flexibility and compositionality that you can apply *everywhere*. Even in Java. 

---

[^1]: Knowledge and the reality to me are like diamonds: they have lots of faces, and to really understand the whole you need to look at lots of them.