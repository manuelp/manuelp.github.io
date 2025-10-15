---
layout: post
title: Imparare la programmazione funzionale
categories: blog
tags: [Haskell, FP, ita, Clojure, Learning]
---

(Un amico mi ha chiesto un consiglio su dove iniziare per imparare la programmazione funzionale, ri-pubblico qua la mia risposta leggermente adattata.)

## FP in breve

Prima di tutto una breve parentesi riguardo la programmazione funzionale (FP) in generale. Non c'è una definizione universalmente accettata di FP, ma i tratti generali sono abbastanza comuni:

* Le *funzioni* sono cittadini di prima classe, manipolabili come valori. E' possibile ovvero passarle come parametri e ritornarle tramite altre funzioni (per esempio, vedi Javascript).
* Tipicamente le funzioni sono *pure*, ovvero non ci sono effetti collaterali. In altre parole, il valore ritornato da una funzione dipende *solo ed esclusivamente dal valore dei suoi parametri*, in modo tale che per uno stesso insieme di valori in ingresso alla funzione, questa ritornerà sempre lo stesso risultato.
* In molti casi, si lavora principalmente con strutture dati *immutabili*. Questo significa che, per esempio, modificare una lista significa crearne una nuova (in modo effiente in termini di spazio e tempo ovviamente, utilizzando delle [persistent data structures](https://en.wikipedia.org/wiki/Persistent_data_structure)).

Queste caratteristiche hanno tutta una serie di grandi vantaggi in termini di (per esempio):

* Modularizzazione
* Composizione
* Riuso
* Flessibilità
* Potenza espressiva
* Capacità di astrazione
* Manutenibilità
* Possibilità di sfruttare facilmente concorrenza e parallelismo

Il tutto ha una solida base teorica nel [lambda calculus](https://en.wikipedia.org/wiki/Lambda_calculus), un modello computazionale equivalente in termini di possibilità ma *diverso da quello di [Von Neumann](https://en.wikipedia.org/wiki/Von_Neumann_architecture)*.

Il modello di Von Neumann prevede una memoria condivisa e delle procedure che la alterano. Questo crea essenzialmente un accoppiamento temporale e spaziale tra diverse unità funzionali che, anche se gestito tramite oggetti, rimane una caratteristica intrinseca di questa architettura. Il che rende praticamente impossibile sfruttare facilmente il parallelismo offerto dalle odierne CPU multi-core.

In contrasto, il modello computazionale basato sul lambda calcolo si basa su *trasformazioni di strutture dati immutabili da parte di funzioni pure*. Il che come dicevo ha tutta una serie di vantaggi, non ultima la possibilità da parte del compilatore/interprete di sfruttare in molti casi automaticamente ed in modo trasparente il parallelismo.

## Approccio didattico

Alcuni sostengono che il FP sia qualcosa di "alieno", "difficile", "impratico", "inutile". Cavolate! In realtà è piuttosto semplice ed *estremamente* utile e potente.

Il problema, piuttosto, è che semplicemente è qualcosa di un po' diverso da ciò che fin'ora è stato l'approccio mainstream. Ma anche il mainstream si sta rendendo conto che il FP è un requisito essenziale per poter lavorare in modo sano con concorrenza, parallelismo e sistemi distribuiti. Basti vedere la sempre maggiore popolarità di Clojure e Scala per esempio, ma soprattutto che anche Java si sta evolvendo in questa direzione. Già nella versione 8 sono state aggiunte [le lambda expressions e gli stream](https://www.infoq.com/presentations/java-8-lambda-streams). In ambito .NET poi, F# esiste già da tempo come cittadino di prima classe in Visual Studio. Senza contare Facebook che si sta orientando su soluzioni di stampo FP come [Flux](https://facebook.github.io/react/docs/flux-overview.html), basato su [React.js](https://facebook.github.io/react/) e [immutable-js](https://github.com/facebook/immutable-js).

L'ostacolo principale che ti troverai davanti all'inizio è che si tratta il tutto con una angolazione un pò diversa da quella a cui normalmente si è abituati proveniendo da un percorso "standard" di formazione. In realtà è semplice: *c'è molta carne al fuoco certo, ma si costruisce tutto su solide e semplici basi*. Perciò uno dei primi consigli che posso dare al riguardo è: **sospendi il giudizio, e affronta l'argomento con mente aperta** (con [Shoshin](https://en.wikipedia.org/wiki/Shoshin), per evitare l'[effetto Einstellung](https://en.wikipedia.org/wiki/Einstellung_effect)). Se ti applichi, ti posso assicurare che avrai una lunga serie di momenti "a-ha!!", che ti arricchiranno e miglioreranno professionalmente per il resto della tua vita (oltre a essere tremendamente eccitanti e soddisfacenti ;)

Ok, ma all'atto pratico da dove cominciare? Qualcuno direbbe: [Scala](https://scala-lang.org/)! Ma essendo un misto-mare pieno di [casi particolari](https://scalapuzzlers.com/) e che tenta di "fondere" il paradigma OO con quello FP, rischia veramente di confonderti le idee. Oltre a [non funzionare](https://queue.acm.org/detail.cfm?id=2611829). Ha un type system più avanzato di quello di Java, ma la sua type inference è abbastanza limitata rispetto quella di [Haskell](https://new-www.haskell.org/) per esempio. Ergo, secondo me *più è diverso da ciò che già conosci, più facile è evitare che gli schemi di pensiero abituali ti rendano più difficili le cose*.

## Percorso

### Haskell for all!

Per queste ragioni, tenderei a consigliarti di iniziare con Haskell. E' un linguaggio funzionale *puro* e staticamente tipato, che compila in codice nativo (quindi niente JVM nè .NET). Tieni conto che imparare questo stile migliorerà il tuo codice in qualsiasi linguaggio, anche se non utilizzerai Haskell o simili sul lavoro di tutti i giorni.

Il percorso che consiglio è quello delineato da [Chris Allen](https://github.com/bitemyapp/learnhaskell). Per cominciare, ti consiglio caldamente il corso [CIS 194](https://www.seas.upenn.edu/~cis194/spring13/index.html) di Brent Yorgey dell'università della Pennsylvania. E' un corso molto completo e ben fatto. Puoi trovare qualche appunto (per ora sulle prime due settimane) sul [blog](https://manuelp.herokuapp.com/category/cis-194) del sottoscritto, ma a cui consiglio di dare uno sguardo solo dopo che hai affrontato il materiale di Brent Yorgey. Non è eccessivamente impegnativo, ma molto utile.

Altro consiglio: **fai gli esercizi**. Leggere e capire non significa assimilare, per imparare veramente occorre applicare le cose, solo allora ti rendi conto di ciò che sai e cosa no. L'unico apprendimento è quello *attivo*. Inoltre tieni presente che si parte dalle basi quindi *non avere fretta* di scrivere web application in quattro e quattr'otto. Tuttavia dalle basi si progredisce abbastanza velocemente, quindi persevera e [vedrai](https://manuelp.herokuapp.com/posts/16) ;)

Come ambienti di sviluppo io uso Emacs (vedi istruzioni di [Chris Allen](https://github.com/bitemyapp/learnhaskell) anche per questi aspetti), ma potresti anche usare [FPComplete](https://www.fpcomplete.com/business/haskell-industry/): un IDE web-based gratuito (nel cui sito tra l'altro ci sono molte risorse didattiche).

Padroneggiato un pò Haskell, le possibilità sono diverse:

* Esiste una versione di Haskell sulla JVM: [Frege](https://github.com/Frege/frege).
* Web frameworks per Haskell (in ordine di "indagine"):
  * [Scotty](https://github.com/scotty-web/scotty)
  * [Snap](https://snapframework.com/)
  * [Happstack](https://happstack.com/page/view-page-slug/1/happstack)
  * [Yesod](https://www.yesodweb.com/)
* [Elm](https://elm-lang.org/), un linguaggio ispirato ad Haskell per realizzare applicazioni web client-side basate sul paradigma [FRP](https://elm-lang.org/learn/What-is-FRP.elm). Vedi la [learning roadmap](https://elm-lang.org/learn/Roadmap.elm) per maggiori dettagli.
* [PureScript](https://www.purescript.org/) è un linguaggio Haskell-like che "transpila" in Javascript. Vedi anche il libro [PureScript By Example](https://leanpub.com/purescript/) che mi dicono sia anche una buona introduzione alla FP.

### Alternative track: Clojure

Anche Clojure è una buona scelta per imparare l'approccio FP. Ha bene o male tutti gli attributi necessari e fino a poco tempo fa era il mio linguaggio preferito. Di diverso rispetto ad Haskell:

* Ha il *dynamic typing*, ovvero ti aiuta meno nella comprensione/manutenzione di una base di codice ma è più flessibile. Puoi darti la zappa sui piedi con più flessibilità insomma :P
* E' [omoiconico](https://en.wikipedia.org/wiki/Homoiconicity), ovvero la sintassi con sui si scrive il *codice* è identica a quella delle *strutture dati*. Il famoso detto:

> Code is data, data is code.

Pur essendo un linguaggio marcatamente funzionale, un po' per scelta un po' per limitazioni della piattaforma sottostante, è da una parte più semplice di Haskell dall'altra ti assiste meno a tempo di compilazione tramite il type system. C'è [core.typed](https://typedclojure.org/) certo, ma non è la stessa cosa. Questa differenza si nota molto a livello di impostazione e workflow: si usa molto la [REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop) e l'enfasi è sul modificare ed evoltere i sistemi manipolandoli direttamente a runtime (cfr. [Smalltalk](https://en.wikipedia.org/wiki/Smalltalk)).

Di interesse la sua [concezione](https://www.infoq.com/presentations/An-Introduction-to-Clojure-Time-Model) del tempo, valori e identità (dello [stato](https://clojure.org/state) insomma) che facilita di molto la concorrenza. Da notare che la STM è [presente anche in Haskell](https://www.haskell.org/haskellwiki/Software_transactional_memory).

Clojure gira sulla JVM, ma anche [su .NET](https://github.com/clojure/clojure-clr) e transpila [su JS](https://github.com/clojure/clojurescript), interoperando con naturalezza con la piattaforma ospite.

Come risorse, se vuoi approfondirlo, ti consiglio:

* (libro) [Programming Clojure, seconda edizione](https://pragprog.com/book/shcloj2/programming-clojure). E' un libro introduttivo e conciso che ho trovato molto chiaro e utile. Probabilmente è la risorsa migliore per cominciare con Clojure.
* (libro) [Clojure Programming](https://www.clojurebook.com/) (si lo so...) E' considerato più o meno universalmente come il miglior libro per "beginner" (è uscito dopo quello del punto precedente). Copre tutto il liguaggio e molti aspetti pratici come programmazione web, uso di DB di vario tipo, testing, ecc. Ma è un mattone di quasi 600 pagine, quindi l'ho messo per secondo.
* (video) [Clojure: Inside Out](https://shop.oreilly.com/product/0636920030409.do): corso tenuto da giganti, interessante e utile.
* (video) [LispCast's Introduction To Clojure](https://www.purelyfunctional.tv/intro-to-clojure) altra serie di video.
* (online) [4clojure](https://www.4clojure.com/): valangate di *esercizi* (anche con Clojure è importate farli!).

Che ho esaminato meno da vicino, ma che ti segnalo comunque:

* (libro/online) [Clojure For The Brave And True](www.braveclojure.com): una idiosincratica introduzione gratuita :)
* (online/libro?) [Clojure From The Ground Up](https://aphyr.com/tags/Clojure-from-the-ground-up). Aphyr in questi mesi si sta dedicando a completare questa introduzione.
* (online) [Hitchhicker's Guide To Clojure](https://hitchhikersclojure.com/)

Dal punto di vista dello sviluppo, io uso [Emacs](https://github.com/clojure-emacs/cider) ma ti segnalo anche:

* [LightTable](https://lighttable.com/) un avvenieristico IDE (scritto in ClojureScript) facile da usare, che ha spopolato su Kickstarter un paio di anni fa. 
* [Cursive Clojure](https://cursiveclojure.com/): IDE basato su IntelliJ IDEA che sta rapidamente guadagnando adepti e che mi pare abbastanza completo (sta evolvendo velocemente).

Ci sarebbe molto altro da dire, ma per il momento basti questo. Ulteriori aree di indagine se sei interessato:

* Puoi dare un'occhiata a del [mio codice](https://github.com/manuelp). Probabilmente di relativamente presentabile/semplice ci sono:
  * [assert-json](https://github.com/manuelp/assert-json)
  * [obj-hierarchy](https://github.com/manuelp/obj-hierarchy) (e [post](https://manuelp.herokuapp.com/posts/6) di accompagnamento)
  * [elekti](https://github.com/manuelp/elekti)
  * [actions](https://github.com/manuelp/actions)
  * [dicepass](https://github.com/manuelp/dicepass)
  * [virtual-adventure](https://github.com/manuelp/virtual-adventure)
  * [confunion](https://github.com/manuelp/confunion)
* Programmazione lato frontend con [Om](https://github.com/swannodette/om) o, forse meglio, [Reagent](https://holmsand.github.io/reagent/) (basati su [react.js](https://facebook.github.io/react/)).
* Programmazione web con [hoplon](https://hoplon.io/), [Luminus](https://luminusweb.net/) o [Pedestal](https://pedestal.io).
* Molto interessante, come database, [Datomic](https://www.datomic.com/). Un vero e proprio database funzionale.

### Altro?

Personalmente ti posso dire che parecchie delle cose imparate le sto applicando anche sul lavoro. In ambito Java per esempio sto usando molto [TotallyLazy](https://totallylazy.com/): sfortunatamente di documentazione c'è poco, ma ha una tonnellata di roba funzionale molto interessante. Comunque già solo col concetto di `Callable` e `Sequence` puoi fare molto e ti abilita un bel po' di idiomi funzionali (anche se non sempre convengono data la verbosità del linguaggio). 

Interessante anche da esplorare [lazyrecords](https://code.google.com/p/lazyrecords/) (degli stessi autori), che fornisce un accesso ai dati funzionale a-la [LINQ](https://queue.acm.org/detail.cfm?id=2024658) in ambito .NET.

Lato Javascript ci sono di interessanti da esplorare:

* Flux con React e immutable-js (precedentemente linkati)
* [fn.js](https://eliperelman.com/fn.js/)
* [lodash](https://lodash.com/)
* [bilby.js](https://github.com/puffnfresh/bilby.js)
* [wu.js](https://dailyjs.com/2010/06/08/wujs/)

Ci sarebbe tantissimo altro da dire/esplorare/discutere, ma credo di aver già dato abbastanza materiale per ora.
