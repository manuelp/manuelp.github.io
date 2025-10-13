---
layout: post
title: User Stories Applied, capitolo 2
categories: agile
---

Continua l'esplorazione della tecnica delle [user stories](https://en.wikipedia.org/wiki/User_story). 

Per capire meglio che cosa sono le user stories bisogna capire cosa *non* sono mettendole in contrasto con le altre tecniche di raccolta dei requisiti (use cases, specifiche dei requisiti software IEEE 830 e interactive design scenarios).

## IEEE 830
Lo standard IEEE 830 documenta un modo di esprimere le specifiche di un software, principalmente attraverso una gerarchia di requisiti nella forma "*Il sistema dove...*". Un esempio:

> 4-6) The system shall allow a room to be reserved with a credit card.
> 4-6-1) The system shall accept Visa, MasterCard and American Express cards.
> 4-6-2) The system shall charge the credit card the indicated rate for all nights of the stay before the reservation is confirmed.
> 4-6-3) The system shall give the user a unique confirmation number.

 - Specifiche a questo livello di dettaglio sono tediose, prone a errori e *molto costose in termini di tempo*. Per sistemi di media grandezza non sono rari documenti di specifica di 300 pagine... estremamente noiose. la conseguenza è che in realtà gli sviluppatori li guardano appena, ma senza leggerli approfonditamente.
 - Documenti di questo tipo spingono a pensare ai dettagli implementativi ma **rendono molto difficile avere una visione d'insieme**. 
 - Non importa quanto impegno si mette nel documentare fin nei minimi particolari le caratteristiche del sistema. **Non appena l'utente ha di fronte il software, quasi invariabilmente avrà idee diverse (e migliori)**.
 - Le specifiche in questa forma si concentrano sugli *attributi del sistema* invece che sulle *esigenze dell'utente*. Si viene spinti a considerare il documento di specifica come una "checklist" delle cose da realizzare, e quindi a dichiarare terminato un progetto quando tutti questi requisiti sono soddisfatti. E quando l'utente modifica le sue richieste, la cosa viene classificata come un cambiamento di *scope*. In realtà **un softwarè veramente terminato quando l'utente è pienamente soddisfatto**.
 
## Use cases
In generale uno use case è una descrizione generalizzata dell'interazione tra il sistema e degli attori (che possono essere utenti o altri sistemi).

 - Uno use case in forma estesa contiene dettagli su un'interazione con in più dei "percorsi alternativi" che costituiscono degli *scenari* alternativi, in genere per gestire errori. In realtà **uno use case esteso è praticamente equivalente ad una storia con i relativi test di accettazione (che prevedono anche i casi alternativi)**. Ma...
 - Le principali differenze sono la *longevità*, la *completezza* e lo *scopo*: mentre uno use case rappresenta un accordo tra l'utente e il team di sviluppo fatto per essere valido e normativo (a volte gli use case vengono considerati obblighi contrattuali) per tutta la durata del progetto, una **storia rappresenta una conversazione da intrattenere con il cliente e usualmente è utilizzato principalmente ai fini di planning (release, iterazioni)**.
 - Gli use case, nella loro forma estesa, sono più soggetti ad essere utilizzati per contenere dettagli sull'interfaccia utente. Il che è chiaramente un male vista la volatilità del design di quest'ultima. Questo è un problema per cui Constantine e Lockwood hanno proposto una soluzione: *essential use cases*, ovvero una forma di use case ridotta contenente solo: **intenzioni dell'utente** e **responsabilità del sistema**, avvicinandoli molto alle storie.
 - Vista la volatilità dei requisiti software, organizzare un contratto attorno a requisiti a questo livello di dettaglio non fa altro che creare difficoltà: **spinge a realizzare il software che l'utente ha chiesto, non quello di cui ha bisogno**, infatti l'utente raramente ha le idee chiare fin dall'inizio su quello che vuole.

> No matter how much thinking, thinking and thinking we do we can never fully specify a non-trivial system upfront.

## Scenari di interazione
Uno scenario è in pratica una "storia" (non nel senso di user story) contenente contesto, attori, obiettivi, azioni ed eventi. Si tratta di una descrizione in forma discorsiva di un'interazione di attori umani in un particolare contesto che hanno determinati obiettivi.

 - Essendo una descrizione discorsiva e in forma di racconto, molti dettagli sono lasciati fuori percui è comunque necessaria una conversazione con il cliente per chiarirli.
 - Un singolo scenario ha uno *scope* decisamente maggiore di una storia, infatti da uno scenario tipico si possono ricavare diverse storie di solito.
