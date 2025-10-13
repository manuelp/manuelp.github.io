---
layout: post
title: Abstraction
categories: blog
---

I've noticed that when two or more people discuss about some technical matter, some words are sometimes used inappropriately and mean different things for different people. You can easily imagine that in this context having meaningful and fruitful discussions is almost impossible.

If you try to discuss about object-oriented programming for example, you'll quickly find out that there are a lot of different ideas on what OO is all about, most of them somewhat "fuzzy". Can you pinpoint exactly what OOP is? I mean, what Alan Kay had in mind when he coined that (unfortunate) term? I would have several things to say here but I digress, lets return on topic.

That's why here I'll try to define exactly what the meaning of the word *abstraction* really is, and what I mean when I use it.

## Definition ##

First, let's see what [etymonline](https://www.etymonline.com/index.php?term=abstract) has to say about it (emphasis is mine):

<blockquote> 
*abstract (adj.)*, late 14c., originally in grammar (of nouns), from Latin abstractus "drawn away," pp. of abstrahere "to drag away; detach divert," from ab(s)- "away" (see ab-) + trahere "draw" (see tract (n.1)).   

Meaning *"withdrawn or separated from material objects or practical matters"* is from mid-15c [...]
</blockquote>

So, here we have something important that we can work with. Something is abstract when it's *separated from material practical matters*. Excellent! Now let's see what the [dictionary](https://dictionary.reference.com/browse/abstract) says. As an adjective:

1. thought of apart from concrete realities, specific objects, or actual instances: an abstract idea.
2. expressing a quality or characteristic apart from any specific object or instance, as justice, poverty,  and speed.
3. theoretical; not applied or practical: abstract science.
4. difficult to understand; abstruse: abstract speculations. 

So from these definitions we can say several things. First of all that *something can be abstract*, in which case that thing is not concerned with practical considerations. 

Another interesting consideration is that when something is abstract it *can* be difficult to understand. It can happen when the abstract thing has no or very little connections with practical matters, or when it's so complex and with so many interconnected parts that mapping all of them to corresponding more *concrete* things is very difficult.

As a verb:

10. to draw or take away; remove.
11. to divert or draw away the attention of.
12. to steal.
13. to consider as a general quality or characteristic apart from specific objects or instances: to abstract the notions of time, space, and matter.
14. to make an abstract of; summarize. 

Furthermore, it's also an *action*. We can *abstract away practical details* and have a conceptual model to work with more easily.

## Abstraction in the software development domain 

Now I think we can try to give a sort-of definition of abstraction in the software development context:

> abstraction (noun) is a conceptual model of the structure or function of a software construct that is not concerned with mechanics and other practical considerations.

Probably it isn't a good definition, but it's a starting point (I'm open to discuss and refine or rewrite it). From this definition we can make some deductions:

**To abstract in the programming domain means separating a concept from it's implementation details**. This way, when we have an abstraction we have a *concept* that:

* It's generally easier to understand, reason about, specify and test.
* We can implement it in completely different ways without changing its *essence*.

If components *depends* on an abstraction, we can change its implementation and refactor how we want without affecting its users. Nice!

Notice that this concept is independent from scale: we can abstract functions, classes, components, systems or even entire networks of services. And this is exactly the premise and original meaning of OO: **a way of thinking and designing systems to manage their complexity, by creating loosely coupled abstractions that communicates by messages at all scales**. So "real" object-oriented design is a *fractal design thinking* that produces a network of abstractions (on both state and behavior) that can be easily rearranged and modified (even at runtime, see [Smalltalk](https://en.wikipedia.org/wiki/Smalltalk) for example). Actual mechanics (encapsulation, inheritance and polymorphism) are only consequences, not the definition itself.

We can map the same concept in a different way to FP, but this is matter for another post.

## Abstraction or indirection?

Sometime I see the word "abstraction" used when the real meaning is "indirection". Abstraction implies removing implementation details, whether indirection only adds another step to reference the same thing with all the specific low level details in place that you have to take into account. They are both useful but different things.

Suppose that you have to cook a pie, and you can ask a friend for help. But he doesn't have the slightest idea on how to do it, so you sit on your couch and describe to your friend with excruciating details how to cook a pie. Congratulation, you just added a level of *indirection*.

Now suppose that your friend is an excellent chef. In this case you only need to ask him "just cook an apple pie". And this, my friend, is *abstraction*.

They are quite different, isn't it? Notice how in the first case you didn't touch the pie with a finger, but you still need to know how to mix, how much to cook it, etc. etc. In the second case however, you didn't have to think about all of that low level details. You just wanted an apple pie!

If you want, let's discuss on [Reddit](https://www.reddit.com/r/programming/comments/19a8qq/what_is_abstraction/), or read Zed Shaw [take](https://zedshaw.com/essays/indirection_is_not_abstraction.html) on this topic.
