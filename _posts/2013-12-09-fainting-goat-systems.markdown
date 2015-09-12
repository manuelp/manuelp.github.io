---
layout: post
title: Fainting Goat Systems
categories: blog
---

Paraphrasing [Robert Heinlein](https://en.wikipedia.org/wiki/The_Moon_Is_a_Harsh_Mistress), reality is a harsh mistress. Have you ever designed or worked on a system which was finally declared "done" (the mythical 1.0 release): it passed all [QC](http://en.wikipedia.org/wiki/Quality_control) tests, all was green, all the checks in place... but that failed miserably when deployed in production? Unfortunately, a real system *lives* in a harsh reality made of unreliable networks, crashing subsystems, crazy users and [Elbonian](https://en.wikipedia.org/wiki/Elbonia#Elbonia) hackers.

Architecture is extremely important: flexibility, rapid development, modularity. All technically cool and necessary (I'm all for it!), but you need to take into account and **design for the real world**. You don't want to design a cool system on paper only to be constantly in emergency mode, struggling and working frantically just to keep the system live (still, with 80% availability, or worse). When you are in [survival-mode](http://5whys.com/blog/definition-survival-mode.html) you can't spend time learning, practicing and developing new systems (that generates revenue). I call those "[Fainting Goat](https://en.wikipedia.org/wiki/Fainting_goat) Systems": they are beautiful, functional, but when they are under stress, they just [crash](http://www.youtube.com/watch?v=AnVv0RkiG4U) and fall to the ground. Here is an informative slide of a Fainting Goat System just deployed to production for the first time:

![A fainting goat](http://upload.wikimedia.org/wikipedia/commons/d/d6/Fainted.jpg)

The first thing is *stability*. The system should be stable, *built for cheap and easy operations* and ready to make contact with the real world and real users. It's important to release early and often, in production, and *adapt to reality*. Certain lessons can only be learned in production, with real users and real deployments.

A good book on the subject is [Release It!](http://pragprog.com/book/mnee/release-it) by the [Cognitect](http://cognitect.com/) Michael T. Nygard.
