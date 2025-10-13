---
layout: post
title: L'importanza della pianificazione
category: management
---

> Proceed from a plan, whether that plan is in your head, on the back of a
> cocktail napkin, or on a wall-sized printout from a CASE tool.<br/>
> --"The Pragmatic Programmer", Andrew Hunt, David Thomas

Iniziare un progetto senza uno straccio di piano è una ricetta per il
fallimento, o quantomeno per un'efficientissima fonte di mal di testa.

Non si tratta di pianificare ogni minimo dettaglio fin dal principio. Questo è
controproducente (al di là di casi particolari come i programmi che equipaggiano
lo space shuttle). Ma non si tratta nemmeno dell'estremo opposto: procedere,
praticamente, *a caso*.

Alcuni proponenti dell'approccio [agile][1] erroneamente portano all'estremo il
concetto di [self-organization][2], sostenendo in maniera più o meno implicita
una generale antipatia per la pianificazione. 

[1]: https://it.wikipedia.org/wiki/Metodologia_agile
[2]: https://en.wikipedia.org/wiki/Self-organization

Tecnologia
==========

Nell'ambito tecnologico possiamo vedere questa "lotta" ad esempio a livello di
progettazione. 

Da una parte abbiamo i proponenti del [BDUF][], che sostengono una specifica
completa e dettagliata *prima di cominciare a codificare*. Il che nella
stragrande maggioranza dei casi significa perdere tempo in dettagli minuziosi
basati su assunzioni non verificate, per poi scoprire che la soluzione non
funziona o che ce ne sono di migliori e più eleganti, a cui non si arriva se
non avendo sott'occhio il codice.

[BDUF]: https://en.wikipedia.org/wiki/Big_Design_Up_Front "Big Design Up-Front" 

> Life is what happens to you while you are busy making other plans.<br/>
> --John Lennon

Dall'altra abbiamo certi "agilisti" (partigiani) che aborriscono qualsiasi
forma di pianificazione e preferiscono lanciarsi nella codifica "lasciando che
il design si riveli ed evolva da sè grazie ai test". Uhm...

Come fa notare [Uncle Bob][unclebob], in questa situazione come in tantissime
altre, **la verità sta nel mezzo**. Un pò di pianificazione all'inizio (senza
arrivare agli estremi del BDUF), permette di capire meglio il problema e
impostare una soluzione decente, senza rischiare di ficcarsi in un vicolo cieco
da cui è molto difficile uscire. 

[unclebob]: https://blog.objectmentor.com/articles/2009/04/25/the-scatology-of-agile-architecture "The Scatology of Agile Architecture"

Inoltre ci sono determinate cose che **devono** essere fissate up-front. Ad
esempio, io mi sono ritrovato a lavorare ad un progetto di un sistema
*distribuito* di simulazione, composto da diversi moduli che comunicavano in
modo concorrente e tramite rete, ognuno progettato e realizzato da una parte
del nostro team. E' stato necessario definire: 
 
 - architettura generale
 - interfacce di comunicazione
 - tipi di dato condivisi

Senza definire queste cose a priori non saremmo riusciti a lavorare in
parallelo e ci saremmo pestati i piedi a vicenda rallentando l'intero progetto.

Management
==========

Lo stesso concetto vale a livello di management, come ho verificato sulla mia
pelle. 

Anche qui si presenta il concetto di *self-organizing team*, ma è comunque
necessario stabilire delle linee guida e delle regole (meglio se condivise e
adottate su base consensuale), che andranno sì modificate e integrate "in
corsa", ma che sono comunque necessarie per una [comunicazione
efficace](/metaprogramming/2009/05/30/importanza-di-comunicare.html).

Avere un piano a livello di management significa poter **identificare le
priorità** (*infrastruttura*, *interfacce*, user stories, ecc.), poter
allocare risorse, pianificare costi e tempi di massima (ad esempio:
*project velocity* di [XP][]) e **coordinare il team**.

[XP]: https://www.extremeprogramming.org/

C'è un modo per affrontare con successo anche il progetto più gigantesco e
complesso: **fare un passo alla volta**. 

 - **A**: "voglio un di sistema di elaborazione di dati scientifici
che si appoggia su CUDA pronto tra quattro mesi"
 - **B**: "comincia a studiare CUDA per una settimana e fai qualche prototipo,
poi faremo un meeting per buttare giù qualche bozza di architettura e stimare i
costi"

Quale delle due alternative ti motiva di più ed è più utile?

Conclusione
===========

Sia in ambito tecnologico che in ambito gestionale occorre trovare un
equilibrio tra *pianificazione* e *auto-organizzazione*, perchè ognuno dei due
elementi è necessario alla buona riuscita di un progetto e di un team di
sviluppo.