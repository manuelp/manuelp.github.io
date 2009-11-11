---
layout: post
title: Behavioural-Driven Development
categories: blog
---

Il *Behavioural Driven Development* ([BDD][]) è essenzialmente il [TDD][] fatto correttamente. Il problema è che troppo spesso il TDD è portato avanti in maniera scorretta, principalmente a causa dell'eccessivo *focus* sull'aspetto di testing.

## Perchè Behaviour-Driven?
Come [illustrato][BDDDanNorth] da Dan North, il TDD è un'ottimo stile di sviluppo ma è molto facile fraintenderlo focalizzandosi troppo sui test di unità creando un falso senso di sicurezza:

> That’s not to say that testing isn’t intrinsic to TDD – the resulting set of methods is an effective way of ensuring your code works. However, if the methods do not comprehensively describe the behaviour of your system, then they are lulling you into a false sense of security.<br/>--Dan North

Molte delle domande che possono sorgere nella mente di un principiante, ad esempio:

 - Che cosa devo testare?
 - Come chiamo questo test?
 - In che modo posso testare questa caratteristica?
 
Tutte queste domande, trovano "magicamente" una risposta quando si smette di pensare al *testing* e ci si focalizza sul *comportamento* che un sistema deve avere.

Ecco quindi che il BDD si configura prima di tutto come un "re-branding" del TDD (portato avanti tra gli altri da [Dave Astels][introBDD]) e un mutamento di prospettiva per far "afferrare" più facilmente il modo corretto di operare con questa metodologia.

> I found the shift from thinking in tests to thinking in behaviour so profound that I started to refer to TDD as BDD, or behaviour- driven development.<br/>--Dan North

Una conseguenza di questo approccio, è che non è più necessario nè raccomandato avere una classe di test per ogni classe di codice di produzione (ad esempio: *Queue* e *QueueTest*). Piuttosto di organizzare i "test" per classe interessata, si raggruppano le "specifiche di comportamento" in base alle *fixture* (o contesti) nei quali si trovano ad operare.

## I Vantaggi
Enfatizzare il comportamento sul testing provoca un mutamento nel modo di pensare e produce alcuni vantaggi:

 - Si parte dai *requisiti*, dalle features da implementare invece che dalle singole classi. Si adotta quindi un approccio top-down che guida la progettazione in base alle caratteristiche richieste. Come conseguenza si aderisce naturalmente al principio [YAGNI][].
 - Si sottolinea l'aspetto di *design*
 - Si ottengono delle **specifiche** che hanno il vantaggio di essere *eseguibili*, e che quindi possono essere fatte girare contro la base di codice per verificarne l'aderenza ai requisiti fissati (ecco il testing)
 - Permettono lo sviluppo di un [ubiquitous language][ubiqLanguage] (ovvero di un linguaggio comune tra gli sviluppatori, la parte business e il cliente), integrandosi con il [DDD][]

Inoltre, seguendo la metodologia BDD si ottengono sia delle specifiche eseguibili (test funzionali e di sistema) sia dei test di unità (applicando la verifica del *comportamento* alle singole classi).

## Strumenti
Trattandosi di TDD "fatto come si deve", è possibile utilizzare qualsiasi libreria che supporti il testing come l'onnipresente xUnit. 

Tuttavia, sono stati sviluppati dei framework che incorporano in concetti del BDD sostituendo ad esempio le parole "test" con "behaviour" o "specification" ad esempio, e fornendo molte altre caratteristiche interessanti come [DSL](http://en.wikipedia.org/wiki/Domain-specific_language) appositamente sviluppati (ad esempio usando groovy in [easyb](http://www.easyb.org/)) o [interfacce fluide](http://martinfowler.com/bliki/FluentInterface.html) (popolarizzate dal pioniere [RSpec](http://rspec.info/)), e integrazione di librerie per il [mocking](http://en.wikipedia.org/wiki/Mock_object) come [JMock](http://www.jmock.org/).

Per Java, ho trovato questi framework più o meno utilizzabili:

 - [JBehave2](http://jbehave.org/): dal "fondatore" stesso del BDD
 - [Instinct](http://code.google.com/p/instinct/)
 - [JDave](http://www.jdave.org/)

Sono tutti costruiti al di sopra di JUnit 4, possono quindi di essere fatti girare in un qualunque IDE (oltre che da CLI o Ant ovviamente).

## Esperienza personale
Nel progetto di ingegneria del software a cui ho lavorato abbiamo utilizzato estensivamente l'approccio TDD, rispettando molto il rapporto 1-1 tra classi di test e di codice di produzione.

Non c'è stato molto focus sul comportamento del sistema nel suo insieme, e nemmeno nei test di integrazione e di sistema. Fortunatamente abbiamo fatto uno studio approfondito e un'attenta progettazione durante tutto il processo di sviluppo, facendo attenzione a modularizzare il più possibile (il TDD ha aiutato in questo senso) quindi l'integrazione non è stato un problema molto grave.

Probabilmente però, se fossimo partiti dal principio creando delle *specifiche*, e usandole per guidare lo sviluppo distribuendo la progettazione lungo il processo e realizzando il prodotto *feature-per-feature* creando una versione/demo rilasciabile dopo ogni iterazione, sarebbe stato molto meglio.

## Update (26/10/2009)
Un paio di articolo di Naresh Jain chiariscono molto bene gli effetti e il significato del BDD:

 - [Goodbye Simplicity: I'm Object Obsessed](http://blogs.agilefaqs.com/2009/10/26/goodbye-simplicity-im-object-obsessed/)
 - [There is No Spoon](http://blogs.agilefaqs.com/2009/06/15/there-is-no-spoon-objects/)

[Bdd]: http://en.wikipedia.org/wiki/Behavior_Driven_Development "Behaviour Driven Development"
[TDD]: http://en.wikipedia.org/wiki/Test_Driven_Development "Test Driven Development"
[BDDDanNorth]: http://dannorth.net/introducing-bdd "Introducing BDD"
[introBDD]: http://blog.daveastels.com/files/BDD_Intro.pdf "BDD Intro"
[YAGNI]: http://en.wikipedia.org/wiki/You_Ain%27t_Gonna_Need_It "You Aren't Gonna Need It" 
[ubiqLanguage]: http://www.c2.com/cgi/wiki?UbiquitousLanguage "Ubiquitous Language"
[DDD]: http://en.wikipedia.org/wiki/Domain-driven_design "Domain Driven Design"
