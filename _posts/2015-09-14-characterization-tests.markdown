---
layout: post
title: Legacy code and Characterization Tests
tags: testing, legacy-code
---

When you have to modify some legacy code and you don't have any tests[^1] to begin with, what do you do? 

> "Legacy code is simply code without tests."
>
> -- Michael Feathers, from "[Working Effectively With Legacy Code](https://www.amazon.com/Working-Effectively-Legacy-Michael-Feathers/dp/0131177052)"

Regardless of why there aren't any automated tests, in my opinion the right way to proceed is:

1. Write some [characterization tests](https://en.wikipedia.org/wiki/Characterization_test) to pin down the current behaviour. *This is fundamental* because tests guides you to really understand the code you are going to modify. And more importantly, if you *listen* to them, they'll reveal [code smells](https://en.wikipedia.org/wiki/Code_smell) that lie more or less unnoticed in the code base.

	> "If it stinks, change it."
	>
	> -- Grandma Beck[^2], discussing child-rearing philosophy

2. Refactor existing code to clarify it and make it more changeable.
3. Modify/add tests (one by one!) to *specify* the new/changed behaviour. This is the full TDD cycle.

This is quite simple, but not always easy:

* Maybe the setup is very long, complex and hard to read. This is a sign that the code under test breaks the [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single_responsibility_principle). The solution is to try to isolate the different responsibilities in different methods, then extract one or more new classes (that will be easier to test in isolation). *Divide et impera*!
* Perhaps it's just impossible to test: there may be lots of dependencies required. Maybe some are not specified by an interface and it's not possible to use mocks. Perhaps some dependencies are immersed in the code itself and are calls to static methods (that maybe talk directly with the DB[^3]). Those things are code smells and makes writing *unit* tests for that class very hard or downright impossible. The only solution is to break dependencies, introduce some abstractions and move code around to isolate different responsibilities.

In some cases, this could be a long and difficult process. The temptation to skip it and "Just Make It Work" may be very strong. My suggestion is: **don't**[^4]. Remember that technical debt erodes a system not by chance and suddenly, but little by little, workaround after workaround. The only way to maintain a sustainable pace is *to stop adding technical debt, and repay it refactoring after refactoring*.

> As a system evolves, its complexity increases until work is done to maintain or reduce it.
>
> --[Lehman's laws of software evolution](https://en.wikipedia.org/wiki/Lehman's_laws_of_software_evolution)

Ignoring a problem [won't make it go away](https://www.bonkersworld.net/all-engineers-are-the-same/). Maybe you spare some time now, but you are preparing for a much greater damage down the line.

> "The act of leaving a mess in the code should be as socially unacceptable as littering."
>
> --–Robert C. "Uncle Bob" Martin

---

[^1]: Tests are a good thing, and static types are also a good thing when it comes to maintainance.
[^2]: Kent Beck's Grandma
[^3]: BTW, this actually happened to me...
[^4]: Well, as all guidelines there are exceptions. Maybe there is a legitimate and pressing need to deliver, in those cases cutting corners may be OK. But *only if done in moderation, with awareness and by committing to fix it immediatly after the deadline*.