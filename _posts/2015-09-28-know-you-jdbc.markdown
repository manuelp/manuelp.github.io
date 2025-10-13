---
layout: post
title: Know you JDBC!
tags: java, JDBC
---

Please, for your own good and for the poor soul who will have to debug and maintain your code: *if you use bare bones JDBC, **learn how it works***!

I've recently spent an entire day debugging a whole application searching for connection leaks that regularly took down an application in production more or less every hour... an experience that I don't recomment to anyone. Investing some time on [learning how JDBC works](http://www.marcobehler.com/make-it-so-java-db-connections-and-transactions-html/) will save you and your team a lot of time and stress.

The basic workflow is something like:

1. Acquire a `Connection`.
2. Create a `PreparedStatement`.
3. If there are any, bind the query parameters.
4. Execute the `PreparedStatement`.
5. If the query is a `SELECT`, process the `ResultSet`.
6. **Release all resources** calling `#close()` on them in reverse order.

The last step is especially important, since JDBC won't automatically release the acquired resources (`Statement` and `Connection`). Even if you force the GC.

The bad news is that in every step along the way JDBC can throw a `SQLException`, which is *checked*. It doesn't matter how far you got in the execution/release process, *you should always release every statement and connection claimed*. This gets especially ugly in Java 6-, where you need a lot of error-prone `try-catch`-fu. Here is a simple example:

```java
Connection        conn  = null;
PreparedStatement s     = null;
List<String>      names = new ArrayList<String>();
try {
  conn = DriverManager.getConnection("jdbc:h2:mem:exercise_db;DB_CLOSE_DELAY=-1");
  s = conn.prepareStatement("SELECT name FROM people");
  ResultSet rs = s.executeQuery();
  while (rs.next()) {
    names.add(rs.getString(1));
  }
} catch (SQLException ex1) {
  ex1.printStackTrace();
} finally {
  if (s != null) {
    try {
      s.close();
    } catch (SQLException e) {
      e.printStackTrace();
    }
  }
  if (conn != null) {
    try {
      conn.close();
    } catch (SQLException e) {
      e.printStackTrace();
    }
  }
}
```

You need to close not only the `Connection`, but [also the `PreparedStatement`](http://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html). `ResultSet` is [automatically closed](http://docs.oracle.com/javase/6/docs/api/java/sql/ResultSet.html#close%28%29) when a `PreparedStatement` is closed or used to execute another query.

This is much better in Java 7+, where you can use the [try-with-resource statement](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html):

```java
List<String> names = new ArrayList<>();
try (Connection conn = DriverManager.getConnection("jdbc:h2:mem:exercise_db;DB_CLOSE_DELAY=-1");
     PreparedStatement s = conn.prepareStatement("SELECT name FROM people")) {
  ResultSet rs = s.executeQuery();
  while (rs.next()) {
    names.add(rs.getString(1));
  }
} catch (SQLException ex1) {
  ex1.printStackTrace();
}
```

Since both `Connection` and `PreparedStatement` are [`AutoCloseable`](https://docs.oracle.com/javase/8/docs/api/java/lang/AutoCloseable.html), they are automatically managed by the VM and you don't need to explicitly close them.

Even if you use a connection pool, **you should *always* close `Connection`s and `PreparedStatement`s**: since `Connection` is an interface, a connection pool will usually return an implementation than on `#close()` will do the right thing (either actually close it, or return it to the pool).

# Resources

Take a look at this resources to correctly use SQL and JDBC and you won't suffer needlessly:

* [Make it so: Java DB connections & transactions](http://www.marcobehler.com/make-it-so-java-db-connections-and-transactions-html/)
* [Top 10 JDBC Best Practices for Java Programmer](http://javarevisited.blogspot.it/2012/08/top-10-jdbc-best-practices-for-java.html)
* [10 Common Mistakes Java Developers Make when Writing SQL](http://blog.jooq.org/2013/07/30/10-common-mistakes-java-developers-make-when-writing-sql/)
* [10 More Common Mistakes Java Developers Make when Writing SQL](http://blog.jooq.org/2013/08/12/10-more-common-mistakes-java-developers-make-when-writing-sql/)
* [Yet Another 10 Common Mistakes Java Developers Make When Writing SQL (You Won’t BELIEVE the Last One)](http://blog.jooq.org/2014/05/26/yet-another-10-common-mistakes-java-developer-make-when-writing-sql-you-wont-believe-the-last-one/)
