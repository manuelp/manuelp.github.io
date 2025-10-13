---
layout: post
title: Interesting links
categories: blog
---

## [Deliver Working Software Frequently](https://xprogramming.com/articles/deliver-working-software-frequently/)

Before you can run, you need to learn how to walk. This is a good primer on [agility](https://pragdave.me/blog/2014/03/04/time-to-kill-agile/). Focus first on *delivery*.

> Until you can deliver, work on delivery. Work on nothing else until then. The rest will come in due time.

## [I reckon your message broker might be a bad idea.](https://programmingisterrible.com/post/81015328859/i-reckon-your-message-broker-might-be-a-bad-idea)

Message brokers can be a bad idea if treated like "infallible gods", because they aren't. Think about three good design principles for realiable systems:

* Fail fast
* Process supervision
* End-to-End principle

> In the end, it isn’t so much about message brokers, but treating them as infallible gods. By using acknowledgements, back-pressure and other techniques you can move responsibility out of the brokers and into the edges. What was once a central point of failure becomes an effectively stateless server, no-longer hiding failures from your components. Your system works because it knows that it can fail.

## [PostgreSQL 9.4 - Looking Up (With JSONB and Logical Decoding)](https://www.craigkerstiens.com/2014/03/24/Postgres-9.4-Looking-up)

PostgreSQL 9.4 is going to be exciting with JSONB support. As [David Lesches](https://twitter.com/davidlesches/status/449675286947573760) says:

> JSONB has been committed to Postgres 9.4, making Postgres the first RDBMS with rock-solid support for schemaless data
