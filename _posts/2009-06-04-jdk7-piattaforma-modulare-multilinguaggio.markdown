---
layout: post
title: JDK7 - piattaforma modulare multi-linguaggio
categories: java
tags: [java, ita]
---

Secondo un articolo di [InfoWorld](https://infoworld.com/print/77919), la nuova
nuova versione del JDK Sun adotterà un design più modulare che supporterà la
versione 7 del linguaggio Java. E questo porterà, secondo Mark Reinhold
(ingegnere principale dietro Java SE e [OpenJDK][]) a una serie di vantaggi.

[OpenJDK]: https://openjdk.java.net/ "OpenJDK"

Classpath will be dead
----------------------

La modularità verrà raggiunta attraverso il progetto [Jigsaw][], che permetterà
agli sviluppatori di sviluppare i propri moduli e di eliminare la necessità del
*classpath*. 

[Jigsaw]: https://www.w3.org/Jigsaw/ "Progetto Jigsaw"

> "Class path is dead," Reinhold said.

Un JDK per domarli tutti 
------------------------

La modularizzazione potrebbe portare anche ad
una singola versione di Java, dato la capacità di un sistema del genere di
adattarsi all'ambiente in cui si trova ad essere eseguito.

Piattaforma multi-linguaggio
----------------------------

> "We're working to define a modular form of the Java platform and its
> implementation; we're working to evolve the Java Virtual  Machine into a true,
> multilingual universal runtime for high-level languages; and finally, we're
> doing things to make developers more productive," said Mark Reinhold,
> principal engineer for Java SE and OpenJDK.

Aspetto per me decisamente più importante. Per come la vedo io, l'uso di un
ambiente di esecuzione per *codice managed* che permetta a programmi scritti in
linguaggi diversi di comunicare e cooperare tra loro, in un'ambiente comune e
con un set standard di librerie, è una carta vincente.

Già ora esistono soluzioni simili, le più famose delle quali sono il [CLR][]
della piattaforma .Net e appunto il JRE che già ora supporta un gran numero di
linguaggi anche molto diversi tra loro come [Scala][1], [Groovy][2],
[Clojure][3], [Jython][4] e [JRuby][5] per citarne alcuni.

[CLR]: https://en.wikipedia.org/wiki/Common_Language_Runtime
[1]: https://www.scala-lang.org/ "Scala"
[2]: https://groovy.codehaus.org/ "Groovy"
[3]: https://clojure.org/ "Clojure"
[4]: https://www.jython.org/ "Jython"
[5]: https://jruby.codehaus.org/

Crescente uso delle annotazioni
-------------------------------

Per facilitare il compito dei *checker statici*.

Conclusioni
-----------

Con queste nuove modifiche la piattaforma Java potrebbe diventare a tutti gli
effetti un ambiente di esecuzione in grado di supportare una gran varietà di
necessità, linguaggi e stili di sviluppo, il tutto garantendone
l'interoperabilità e l'esecuzione controllata.

Tutte caratteristiche che secondo me potrebbero diventare irrinunciabili in
molti settori.

