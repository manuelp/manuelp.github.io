---
layout: post
title: Static typing or fast delivery?
tags: [Types, FP]
---

Matteo Vaccari, in his blog post [No Frameworks, Part 1](https://matteo.vaccari.name/blog/archives/1019) said:

> You also start looking at the long compile times of C++ or Scala, and you see that those languages were not designed with the need of fast software delivery in mind.

I disagree: a strong static type system is what gives you feedback. Often you don't even need to start the application to see if things lines up correctly. When you actually have a "conversation" with a good compiler and work with it, then more often than not when things compile they just work. The main reason for that is that types encode [propositions](https://homepages.inf.ed.ac.uk/wadler/papers/propositions-as-types/propositions-as-types.pdf), and the more you can encode in types, the more the compiler can help you (like tests, but automatically and with more generality!).

I was an enthusiast user of Clojure for years, but after some transformative experiences (maybe I'll write about them some day) I found myself more and more comfortable in the static typing mindset. Nowadays, *thinking in types*, *(pure) functional programming* and using *the compiler as an ally* are for me invaluable tools for fast delivery of *maintainable* software.
