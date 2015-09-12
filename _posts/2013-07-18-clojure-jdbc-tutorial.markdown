---
layout: post
title: A short tutorial on clojure.java.jdbc
categories: blog
---

[clojure.java.jdbc](https://github.com/clojure/java.jdbc) is a useful library to have in your toolbelt when you need to work very close to JDBC and for various reasons you can't or don't want to use higher level tools like [Korma](http://sqlkorma.com/). In fact it is a thin wrapper over JDBC that makes working with it from Clojure easier and safer.

This library is undergoing a major API overhaul (starting with version 0.3.0-alpha1) to make it more idiomatic. I've used previous versions but the new API is significantly different and after toying around with it and reading bits of informations here and there, I wanted to jot down a short guide on how to use it.

## Dependencies

Using Leiningen is easy to import this library and the hsqldb driver to fiddle with it:

```clojure
  :dependencies [[org.clojure/clojure "1.5.1"]
                 [org.clojure/java.jdbc "0.3.0-alpha4"]
                 [hsqldb/hsqldb "1.8.0.10"]]
```

Than a `lein deps` and you are good to go.

## Idea

The main idea behind clojure.java.jdbc (as I understand it) is to provide functions to basically:

* *Query* a DB and *transform* resulting data sets
* *Mutate* a DB

Both managing connections and transactions in a transparent way, using plain old SQL strings.

There is a basic DSL for composing SQL strings in the `clojure.java.jdbc.sql` namespace, but as the [documentation](http://clojure.github.io/java.jdbc/#clojure.java.jdbc.sql) says it is deliberately not very sophisticated. To make this guide short we'll only explore the JDBC managing stuff, using raw SQL strings. If you want you can use it in conjunction with [HoneySQL](https://github.com/jkk/honeysql) to generate SQL strings from Clojure data structures (a-la Hiccup).

## A short example

```clojure
(require [clojure.java.jdbc :as j])

(def db-spec {:subprotocol "hsqldb"
              :subname "mem:testdb"})
```

A JDBC connection can be configured using both a map or a string. Here the HSQLDB configuration is very simple since we want a simple in-memory database, but you can specify a full connection to PostgreSQL, MySQL, Oracle, etc:

```clojure
{:classname "com.mysql.jdbc.Driver"
   :subprotocol "mysql"
   :subname "//127.0.0.1:3306/mydb"
   :user "myaccount"
   :password "secret"}
```

Now to *mutate* a database you can use `db-do-commands`:

```clojure
(j/db-do-commands db-spec true
                  "CREATE TABLE movies (id INTEGER IDENTITY, name VARCHAR(256), year INTEGER)")

(j/db-do-commands db-spec true
                  "INSERT INTO movies(name,year) VALUES('Iron Man', 2008)"
                  "INSERT INTO movies(name,year) VALUES('Gattaca', 1997)")
```

The second parameter is for running all the following SQL statements inside that expression in a transaction.

However, I should mention that for inserting new rows there is a more idiomatic way to do it:

```clojure
(j/insert! db-spec :movies
  {:name "Iron Man" :year 2008}
  {:name "Gattaca" :year 1997})
```

*Querying* database is even simpler:

```clojure
(j/query db-spec
         ["SELECT * FROM movies WHERE year>?" 2000])
```

The cool thing about `query` is that you can add keyword parameters to pass transformer functions that can operate both on single rows or the entire result set.

```clojure
(j/query db-spec ["SELECT * FROM movies"]
         :row-fn #(str (:name %) " was released in " (:year %)))
```

## Conclusion

*clojure.java.jdbc* is a valuable tool to work with databases via JDBC when other approaches such as Korma are not applicable or desired. It is even more powerful when used in conjuction with a good DSL to *compose* SQL strings.

For a more in depth look at this library, go to the corresponding section at [clojure-doc.org](http://clojure-doc.org/articles/ecosystem/java_jdbc/home.html).

## Update: 2013-11-04

clojure.java.jdbc 0.3.0 has just reached beta status. Go read the [official announcement](http://corfield.org/blog/post.cfm/clojure-java-jdbc-0-3-0-beta-1) for all the details. In short, now it has been declared feature-complete.


## Update: 2013-11-25 ## 

clojure.java.jdbc 0.0.3-beta2 has been [relesed](https://groups.google.com/forum/#!msg/clojure/pSnXWxpwvT8/9yca3iAohgcJ) with *breaking changes*. Based on the feedback from the community, now the *sql* and *ddl* namespaces have been removed (they have been extracted in a separate lib: *java-jdbc/dsl*) and the official recomendation is to use more sophisticated DSLs like [HoneySQL](https://github.com/jkk/honeysql), [SQLingvo](https://github.com/r0man/sqlingvo), [clojure-sql](https://bitbucket.org/czan/clojure-sql) or [Korma](http://www.sqlkorma.com/). The second breaking change is that the deprecated API has been moved in the *clojure.java.jdbc.deprecated* namespace to eliminated confusion on what the new API is and to highlight this distinction even in the [generated API documentation](http://clojure.github.io/java.jdbc/).

## Update: 2013-12-18

The stable 0.3.0 version has just been released. You can read the related [post](http://corfield.org/blog/post.cfm/clojure-java-jdbc-0-3-0-released) by Sean Cornfield on his blog.

