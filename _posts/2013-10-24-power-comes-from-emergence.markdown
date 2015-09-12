---
layout: post
title: Power comes from Emergence
categories: blog
---

I've been thinking a lot about the intersection of OOP and FP, and I've come to realise one thing: *there is real power on the **composition** of [simple](http://www.infoq.com/presentations/Simple-Made-Easy) things*.

It's more general than the objects VS functions debacle, and I won't talk about immutability right now. The point I want to make is that when you have a small set of *simple* concepts/abstractions that you can compose at will, you get a great deal of expressivity and power that you can build on. Just look at [LISP](http://www.michaelnielsen.org/ddi/lisp-as-the-maxwells-equations-of-software/) or [Smalltalk](http://worrydream.com/EarlyHistoryOfSmalltalk/).

## git ##

Maybe you are among the ones that consider [git](http://git-scm.com/) difficult. Well, at its core git is quite [simple](http://www.infoq.com/presentations/git-details): it's a set of simple concepts at your disposal that give you a *lot* of power and flexibility. 

Understanding them in isolation is easy, because they are simple. However, there are a lot of ways to compose simple things and it isn't equally easy to grok all the ways you can combine them. The real power and expressiveness *emerge from the interactions between the foundational concepts*. In other words: *it's more than the sum of their parts*. 

Git it's a perfect example of the concept of [emergence](http://en.wikipedia.org/wiki/Emergence) at work. Quoting Wikipedia:

> In philosophy, systems theory, science, and art, emergence is the way complex systems and patterns arise out of a multiplicity of relatively simple interactions.

## Pedestal ##

Another example is [Pedestal](http://pedestal.io/). It's difficult to get it: in part because it's different from the "standard" Rails-esque web framework, and in part because it's designed as a set of simple concepts. Taken one by one they are quite simple, but the real power and perceived complexity is on the number of ways you can compose them.

Contrast this with the average framework: usually there is THE way to do things and you only have to plug your code here and there and BAM! It works. A blog engine in [15 minutes](http://vimeo.com/5362441), cool uh? It's certainly [easy](http://www.infoq.com/presentations/Simple-Made-Easy), but not simple and by a large degree not nearly as powerful.

BTW, yes I know that Pedestal has been designed with [real-time collaboration](http://pedestal.io/documentation/) in mind :)
