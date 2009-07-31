---
layout: post
title: Integrazione Continua
categories: development
---

Terminato oggi un progetto di dimensioni importanti (con ottimi risultati), è tempo di fare una retrospettiva.

Nel nostro team abbiamo implementato uno stile di sviluppo [test-driven][], ma non abbiamo predisposto alcun server di build automatizzato. In principio, anche se interessante, sembrava ridondante... Abbiamo ugualmente portato a termine il progetto, ma se avessimo utilizzato una soluzione di questo tipo, avremmo avuto una serie di vantaggi:

 - **Enforcing dell'approccio test-driven**: nonostante fosse stato stabilito di scrivere i test prima e per ogni funzionalità, in qualche caso non si è rispettata questa decisione. Probabilmente un sistema che compilasse i sorgenti a ogni *commit*, che facesse girare tutti i test **automaticamente** e inviasse un'email a tutti in caso di errori sarebbe stato un aiuto in questo senso.
 - **Pacchettizzazione**: come ambiente di sviluppo abbiamo utilizzato [Eclipse][], che assolve egregiamente i suoi compiti compilando automaticamente i sorgenti ad ogni modifica (segnalando eventuali errori di compilazione) e fornendo un'interfaccia a [JUnit][] per i test. Ma non abbiamo tenuto conto della pacchettizzazione del prodotto finito ("lo faremo alla fine quando sarà ora"). Chiaramente alla fine è stato fatto, ma se avessimo avuto un ciclo di vita *evolutivo* con un buon numero di piccole iterazioni e un server di build che si sarebbe occupato di creare una versione pacchettizzata pronta per essere distribuita *ad ogni commit*, la vita sarebbe stata molto più semplice. *Se il deployment è continuo, è facile attuarlo. Se non lo è, allora probabilmente ci sono brutte sorprese all'orizzonte...*
 
La lezione è stata imparata.

[test-driven]: http://en.wikipedia.org/wiki/Test-driven_development "Test-Driven Development"
[Eclipse]: http://www.eclipse.org/ "Eclipse SDK"
[JUnit]: http://www.junit.org/ "JUnit"
