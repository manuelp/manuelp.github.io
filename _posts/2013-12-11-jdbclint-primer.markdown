---
layout: post
title: JDBC Lint
categories: blog
---

Recently I'm working quite heavily with straight JDBC and I'm learning some things from best practices and some others the hard way. One tool that I've found useful is [JDBC Lint](https://github.com/maginatics/jdbclint): it's a tool that acts as a dynamic proxy for `Connection` objects (so that it can be used transparently) and reports errors at runtime on the standard error channel.

For a primer on some JDBC best practices, see [this](http://javarevisited.blogspot.it/2012/08/top-10-jdbc-best-practices-for-java.html) post.
