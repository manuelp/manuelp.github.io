---
layout: post
title: Roy Fielding on Versioning, Hypermedia, and REST
categories: blog
---

Some highlights from [this](https://www.infoq.com/articles/roy-fielding-on-versioning) interview to [Roy Fielding](https://en.wikipedia.org/wiki/Roy_Fielding) on InfoQ:

> "hypermedia as the engine of application state" is a REST constraint. Not an option. Not an ideal. Hypermedia is a constraint. As in, you either do it or you aren’t doing REST. You can’t have evolvability if clients have their controls baked into their design at deployment. Controls have to be learned on the fly. That’s what hypermedia enables.

> The techniques that developers learn from managing in-house software, where they might reasonably believe they have control over deployment of both clients and servers, simply don’t apply to network-based software intended to cross organizational boundaries. This is precisely the problem that REST is trying to solve: how to evolve a system gracefully without the need to break or replace already deployed components.

> don’t build an API to be RESTful — build it to have the properties you want. REST is useful because it induces certain properties that are known to benefit multi-org systems, like evolvability. Evolvability means that the system doesn’t have to be restarted or redeployed in order to adapt to change.

> there is no need to anticipate such world-breaking changes with a version ID. We have the hostname for that. What you are creating is not a new version of the API, but a new system with a new brand. 

> Websites don’t come with version numbers attached because they never need to. Neither should a RESTful API. A RESTful API (done right) is just a website for clients with a limited vocabulary.

> I had to define a system that could withstand decades of change produced by people spread all over the world. How many software systems built in 1994 still work today? I meant it literally: decades of use while the system continued to evolve, in independent and orthogonal directions, without ever needing to be shut down or redeployed. Two decades, so far.

> the initial reaction to using REST for machine-to-machine interaction is almost always of the form "we don’t see a reason to bother with hypermedia — it just slows down the interactions, as opposed to the client knowing directly what to send." The rationale behind decoupling for evolvability is simply not apparent to developers who think they are working towards a modest goal, like "works next week" or "we’ll fix it in the next release".

> What we learned from HTTP and HTML was the need to define how the protocol/language can be expected to change over time, and what recipients ought to do when they receive a change they do not yet understand. HTTP was able to improve over time because we required new syntax to be ignorable and semantics to be changed only when accompanied by a version that indicates such understanding.

> Software developers have always struggled with temporal thinking.
