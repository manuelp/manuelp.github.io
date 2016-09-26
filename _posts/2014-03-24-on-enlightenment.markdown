---
layout: post
title: On "Enlightenment"
categories: blog
---

The more I think about it, the more I see it. They say that you should expose yourself to different kind of programming languages, and more specifically to different kind of *paradigms*. The key of that advice is to get exposure (and hopefully proficiency) with different *ways of thinking*. The more high-level thinking tools you have at your disposal, the more *effective* and *efficient* you are at identifying and solving problems. 

You become able to really *see* the problem and build a general solution. You start to deconstruct what you know, the constraints and the problem at hand, and you become able to build general solutions by *composing simple things*. Systematically. When you start to think in terms of data transformations via `map`, `filter`, `reduce`, `juxt`,  etc. you realize that you are operating on a higher level. You start to really see what the [single resposibility principle](http://en.wikipedia.org/wiki/Single_responsibility_principle) is all about. And then, you start to see all the [accidental complexity](http://en.wikipedia.org/wiki/Accidental_complexity) that used to slow you down and hide the *essence* of the solution behind reams of low level details (syntactical or otherwise). When your mind is not distracted by accidental complexity, you can step back and think clearly about what you have and what is really needed.

Unfortunately, this kind of "enlightenment" has some downsides:

* You become painfully aware that you could do much better and much faster than when you develop with less advanced languages (for example: Java). Here starts the frustration.
* Your "yet-to-reach-enlightenment" colleagues don't *see* the way you do. They don't understand (yet). This is the [blub paradox](http://paulgraham.com/avg.html) at work.

Combine both factors, and you have a recipe for [misery](http://joelmccracken.github.io/entries/the-misery-of-lisp/). It's depressing seeing friends (and yourself) wasting time with unnecessary complexity.

However, even if it requires work, practice and dedication to reach a master level, I think that *it's worth it*. I'm certainly not a master, and yet I'm seeing benefits of this learning journey even when I'm not writing [LISP](http://clojure.org/) code. [Eric Raymond](http://www.catb.org/esr/faqs/hacker-howto.html) was right:

> LISP is worth learning for a different reason — the profound enlightenment experience you will have when you finally get it. That experience will make you a better programmer for the rest of your days, even if you never actually use LISP itself a lot.

So go ahead and learn some [Clojure](http://aphyr.com/posts/301-clojure-from-the-ground-up-welcome), [Haskell](http://learnyouahaskell.com/) and [Factor](http://factorcode.org/) (for example). You don't need to spend [a lot of time](http://pragprog.com/book/btlang/seven-languages-in-seven-weeks) to get a feel of what's out there. You will be a better programmer anyway, and I think this is a worthwhile goal.
