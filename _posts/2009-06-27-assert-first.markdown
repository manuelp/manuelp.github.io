---
layout: post
title: Assert first!
categories: development
tags: [ita, TDD, testing]
---

Attualmente sto terminando la lettura del libro *"Test-Driven Development By Example"* di Kent Beck, ed è in mia modesta opinione una lettura molto istruttiva e utile per chi cerca di imparare questo stile di sviluppo. E' di grande beneficio vedere la tecnica all'opera e seguirne l'utilizzo passo-passo (forse l'unico modo per sviluppare un *feel* e assimilarne la mentalità), per poi rifinire la propria comprensione con dei *pattern*.

Ho già utilizzato questa tecnica per un progetto complesso sviluppato in un team (oltre che altri side-project personali) e quindi ho una seppur minima esperienza in proposito, ma ci sono ancora diverse finezze, tecniche e pattern che posso applicare per migliorare la mia capacità di sviluppare sistemi guidato dai test.

Un'idea nuova che non ho mai utilizzato è quella di **scrivere *prima* gli assert in un test**. La motivazione di questo accorgimento deriva dal vedere i test come un modo per guidare lo sviluppo delle funzionalità che un sistema deve avere. 

In pratica, un *sistema* è composto da diverse *funzionalità*, ognuna delle quali deve essere soggetta a diversi *test*, ognuno dei quali a sua volta contiene delle *asserzioni*. La funzione delle asserzioni è molteplice, ma quelle che ci interessano in questa sede sono essenzialmente:

 - Qual'è la risposta corretta?
 - Come posso controllarla?

Cominciare a scrivere un test partendo dalle asserzioni (che normalmente in un test completo si collocano alla fine del test-case: test pattern *arrange-do-check*) fissiamo subito quali sono le funzionalità sotto test, qual'è la loro forma e sintassi e come devono rispondere. A partire da questa conoscenza possiamo costruire il test-case scrivendo tutte le operazioni necessarie per ottenere quel risultato.

# Un esempio #

(Tratto da: *"Test-Driven Development By Example"*, Kent Beck)

Supponiamo di voler comunicare con un altro sistema attraverso un socket. Quando abbiamo terminato la comunicazione il socket deve essere chiuso e deve essere stata trasmessa e ricevuta con successo la stringa "abc".

Cominciamo con le asserzioni:

{% highlight java %}
public void testCompleteTransaction() {
   // ...
   assertTrue(reader.isClosed());
   assertEquals("abc", reply.contents());
}
{% endhighlight %}

Il *reply* viene da un socket:

{% highlight java %}
public void testCompleteTransaction() {
   // ...
   Buffer reply= reader.contents();
   assertTrue(reader.isClosed());
   assertEquals("abc", reply.contents());
}
{% endhighlight %}

Il socket lo creiamo connettendoci al server:

{% highlight java %}
public void testCompleteTransaction() {
   // ...
   Socket reader= Socket("localhost", defaultPort());
   Buffer reply= reader.contents();
   assertTrue(reader.isClosed());
   assertEquals("abc", reply.contents());
}
{% endhighlight %}

E infine (o in principio) dobbiamo connetterci al server:

{% highlight java %}
public void testCompleteTransaction() {
   Server writer= Server(defaultPort(), "abc");
   Socket reader= Socket("localhost", defaultPort());
   Buffer reply= reader.contents();
   assertTrue(reader.isClosed());
   assertEquals("abc", reply.contents());
}
{% endhighlight %}

