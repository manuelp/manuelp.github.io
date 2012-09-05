---
layout: post
title: Software Construction
categories: oop,fp,tdd,bugs
---

I've just read an interesting post from Daniel Sobral: "[Bugs, TDD and Functional programming](http://dcsobral.blogspot.com.br/2012/09/bugs-tdd-and-functional-programming.html)" in which he gives some ideas on how to bridge the gap between software and physical constructs.

The point in that post is that while physical things obey to the "*law of proximity of cause and effect*" (ie. when something goes wrong you often find the cause where the problem manifested itself) because of the physics constraints and forces, in software you don't naturally have this kind of direct cause-effect relationships. 

I'm not convinced that this is the case for physical things, but I think the point is valid: *for introducing direct cause-effect relations, you need to introduce constraints* in order to **manage complexity**. And [OOP](oop,fp,alan-kay/2012/08/16/true-oop.html) is one response to this need, *functional programming* is another.

On a related note, TDD is a very useful *practice* that helps with realizing this proximity (useful both in OOP and FP contexts).
