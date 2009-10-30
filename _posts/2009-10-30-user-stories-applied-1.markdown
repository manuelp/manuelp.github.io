---
layout: post
title: User Stories Applied, cap. 1
categories: agile
---

Inizio con questo post una tradizione alla [Mark Needham](http://www.markhneedham.com/blog/tag/book-club/) nella quale i annoterò i punti salienti di libri, paper e presentazioni che mi ritroverò a visionare. Questo principalmente per il fatto che il modo migliore per imparare qualcosa (e sapere se lo si è capito realmente) è [insegnarlo](http://www.markhneedham.com/blog/2009/04/21/learning-through-teaching/) a qualcun altro.

Per esigenze di lavoro ho cominciato a documentarmi sul project management e alla raccolta di requisiti, principalmente in ambito Agile, e mi sono trovato in contatto con l'idea delle *user stories*. Il libro che mi sono procurato al riguardo e che sto leggendo attualmente è: *"User Stories Applied"* di Mike Cohn.

Cominciamo con il primo capitolo, intitolato "Groundwork".

 - Ciò che è espresso a parole può essere ambiguo e impreciso. Qualsiasi espressione scritta di requisiti lascerà *qualcosa* di ambiguo o imprecisato. Basti pensare a frasi come: *"Deve essere possibile inserire password di 127 caratteri."* Che cosa vuol dire *possono*? Significa che in certi casi non è necessario? E devono essere di 127 caratteri? Di più? E di meno? Scendendo sempre più nei particolari non migliora di molto la situazione, ci saranno sempre dei "punti ciechi" e dei particolari mancanti di cui ci si accorge solo durante lo svilupo. *L'unica specifica veramente completa in ogni sua parte è il codice sorgente*. Ma **tutta questa confusione semplicemente scompare se si sposta il focus dallo scrivere i requisiti al parlarne**.
 - Spesso i clienti che richiedono un certo software non hanno le idee chiare e *non possono* fornire requisiti chiari, stabili e completi fin dall'inizio. Di conseguenza anche se l'analista funzionale è estremamente preciso e diligente nello stilare use case estremamente completi, tutto questo lavoro potrebbe rivelarsi comunque incompleto o semplicemente inesatto rispetto le *reali* esigenze del committente. Una volta che il cliente ha qualcosa di funzionante sotto gli occhi sarà in posizione migliore di dire ciò di cui ha bisogno. **Occorre evitare di fare il classico errore di realizzare ciò che il cliente chiede piuttosto che quello di cui ha bisogno.**
 - Una **user story** (storia utente) non è altro che una piccola funzionalità che ha valore per l'utente finale. Tradizionalmente scritta su un cartoncino è una semplice frase che indica una funzionalità richiesta *dal punto di vista dell'utente* (quindi storie come "Deve essere realizzato in Java" *non è* una storia), dovrebbero essere abbastanza piccole da poter essere implementate nel giro di mezza giornata o pochi giorni da uno o due sviluppatori.
 - Quando una storia si rivela troppo grande (ad esempio *"L'utente deve poter effettuare una prenotazione"*), si definisce *Epic* e viene suddivisa in due o più storie più piccole.
 - **Una storia, più che definire una funzionalità la rappresenta**. Il modo corretto di pensare alle user stories: è come dei "promemoria" di una conversazione da fare con l'utente.
 - E' importante capire le *aspettative* dell'utente per ogni storia: annotare queste aspettative sotto forma di **test di accettazione**. L'obiettivo è stabilire dei criteri secondo i quali si potrà dichiarare una storia "completa". Anche qui non si tratta di essere subito precisi e completi, la precisione verrà raggiunta quando sarà il momento.
 - Secondo la definizione di Ron Jeffries, una storia è costituita da tre aspetti: **Card, Conversation e Confirmation**. Ovvero, la forma fisica della storia (index card), la conversazione che rappresenta e i test di accettazione per sapere quando è completa.
