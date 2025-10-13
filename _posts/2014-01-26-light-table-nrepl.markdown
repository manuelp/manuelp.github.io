---
layout: post
title: How to programmatically manage a Light Table-aware nREPL server
categories: blog
---

Starting an [nREPL](https://github.com/clojure/tools.nrepl) server for a Clojure application is trivial if you use [Leiningen](https://leiningen.org/):

```bash
lein repl

# or:

lein repl :headless
```

But what if you have an application deployed as a JAR (or WAR in an application server) and you want to have remote access to an nREPL server? Fortunately, it's easy to start one programmatically:

```clojure
(require '[clojure.tools.nrepl.server :refer (start-server stop-server)])

; Starting an nREPL server is trivial:
(defonce nrepl-server (start-server :port 12345))

; So is stopping it:
(stop-server nrepl-server)
```

It works out of the box if you use Emacs with [Cider](https://github.com/clojure-emacs/cider) to connect to it, but if you use [Light Table](https://www.lighttable.com/), you need to add an additional nREPL middleware to support some functionality that LT needs to work properly. Again, this is [easy](https://docs.lighttable.com/#how) if you use Leiningen: you just need to declare the dependency to the middleware and add some `:repl-options`:

```clojure
(defproject some-awesome-thing "0.1.0-SNAPSHOT"
  :description "FIXME: write description"
  :dependencies [[org.clojure/clojure "1.5.1"]
                 [lein-light-nrepl "0.0.13"]] 
  :repl-options {:nrepl-middleware [lighttable.nrepl.handler/lighttable-ops]})

```

Using the `lighttable-ops` nREPL middleware programmatically is also quite easy, but you have to [know](https://github.com/clojure/tools.nrepl#middleware) how to compose nREPL middlewares. First, you need to add the dependency to the LT middleware in your *project.clj* as before:

```clojure
(defproject some-awesome-thing "0.1.0-SNAPSHOT"
  :description "FIXME: write description"
  :dependencies [[org.clojure/clojure "1.5.1"]
                 [lein-light-nrepl "0.0.13"]])
```

Then you can add it to the middlewares chain in your code:

```clojure
(require '[clojure.tools.nrepl.server :refer (start-server stop-server)])
(require '[lighttable.nrepl.handler :refer [lighttable-ops]])

; Starting a server with custom middlewares is trivial:
(defonce nrepl-server 
         (start-server :port 12345
                       :handler (default-handler #'lighttable-ops)))

; So is stopping it:
(stop-server nrepl-server)
```

Notice that we passed `lighttable-ops` to the `default-handler` as a *var* (in fact, `#'lighttable-ops` is a [reader macro](https://clojure.org/reader) that expands to `(var lighttable-ops)`).

Now you can connect to your project remotely using Light Table and inspect, modify and control it *live*. Enjoy :)
