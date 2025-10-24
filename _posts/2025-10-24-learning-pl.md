---
layout: post
title: Learning a new programming language
tags: [Rust, Learning]
#withtoc: yes
---

I'm learning [Rust](https://rust-lang.org/). After years of programming professionaly in Scala (mainly distributed systems), I felt the need to try something new. I liked the idea of going back to a system programming language, closer to the metal, without a GC. But still stronly typed and with a modern approach to memory management. 

Not only that: I didn't want just an intellectual exercise. To have a potential return of investment I also wanted a programming language actually used in the wild. And Rust fits the bill quite well IMHO: it has good documentation, good DX and a fair amount of support as far a tooling/libraries/frameworks are concerned.

So I started last year with the [official Rust book](https://doc.rust-lang.org/book/). I got maybe one third of the way into it and shelved it (priorities shifted). Then, this year I returned into it with a hands-on [course](https://zerotomastery.io/courses/learn-rust/), that I completed successfully.

However, it's not enough to study a language to get *proficient* with it. There is more to it than syntax and concepts: it's about purposefully chasing that [desiderable difficulty](https://en.wikipedia.org/wiki/Desirable_difficulty) and learning the *tooling*, the major *libraries*, and building your mental catalog of [patterns](https://rust-unofficial.github.io/patterns/). This way your mind becomes free to think about the problem/solution spaces without getting (too much) distracted by the particular programming language intricacies.

To do that, a fun and easy way is to *practice* with some hand-on *projects*. They should be small enough to not take too much time, but have enough "meat" to give you experience with actually using the language and tooling to a meaningful degree. My recommendation is to **attempt multiple small/medium projects with a variety of characteristics**. There are lots of lists out there to take as inspiration:

- [Challenging projects every programmer should try - Austin Z. Henley](https://austinhenley.com/blog/challengingprojects.html)
- [More challenging projects every programmer should try - Austin Z. Henley](https://austinhenley.com/blog/morechallengingprojects.html)
- [Coding Challenges](https://codingchallenges.fyi/)
- [CodeCrafters](https://codecrafters.io/), [repository](https://github.com/codecrafters-io/build-your-own-x)
- [Curated list of project-based tutorials](https://github.com/practical-tutorials/project-based-learning)
- [Top 15 Rust Projects To Elevate Your Skills](https://zerotomastery.io/blog/rust-practice-projects/)

Take your time. Accept that the first few ones will be challenging, but keep going. Review your code, refactor it and purposefully find the cleanest, most idiomatic way to write them as you can. Ask for advice if you want/can.

As for me, I've written a couple of projects so far:

- [spomo](https://github.com/manuelp/spomo): a simple CLI-based Pomodoro timer, with a [Ratatui](https://ratatui.rs/) TUI
- [rendezvous-coach](https://github.com/manuelp/rendezvous-coach): a CLI-based application for chronic latecomers 😉 with Ratatui-based TUI and integration with a TTS system

I'd also take a step further: **choose one/two of those projects and keep working on them**. Add new features, redesign them, change the underlying technologies (for example the UI layer). Practice with the tooling, publish something more polished for the general public to use. The goal here is to purposefully getting experience with the long game: managing the entire lifecycle and get a better picture of what it means to work on a real project. You don't need to maintain it for life if you don't want to, it can remain a practice project (just make sure to explicitly say so in the README!).

Plus, I'd also keep learning by getting more in depth with the language itself. For example, I'm planning to read the [Programming Rust, 2nd edition](https://www.oreilly.com/library/view/programming-rust-2nd/9781492052586/) book.