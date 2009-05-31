---
layout: post
title: Git - Object Model
category: git
---

Un DVCS che ultimamente cavalca la cresta dell'onda tra
gli smanettoni è [Git][git], creato da nientemeno che Linus Torvalds e
utilizzato da progetti del calibro di [GNOME][gnome],...

 [git]: http://git-scm.com "Sito ufficiale di Git" 
 [gnome]: http://www.gnome.org "Progetto GNOME"

Si tratta di un sistema di versionamento *distribuito* e *molto veloce*, che
ha tra i suoi punti di forza anche una grande flessibilità e potenza. Tutte
caratteristiche che solleticano il mio interesse, e chi mi hanno portato a
voler sperimentare (tra l'altro, questo stesso blog è versionato con git).

In parte questa potenza è ottenuta a partire dal modello concettuale che sta
alla base di questo sistema di versionamento, che è abbastanza diverso da
quello di altri strumenti analoghi. Si può pensare a Git più come a un
*filesystem* che a un sistema di versionamento. 

Tuttavia, un grande potere comporta una grande responsabilità e nel nostro caso
questa reponsabilità consiste nell'imparare il modello concettuale, che per
fortuna non è affatto complicato. 

Ho scritto questo post a partire dal [Git Community Book][gcb], un libro
scritto collaborativamente dagli utenti e sviluppatori di Git.

 [gcb]: http://book.git-scm.com "Git Community Book"

Object model
============
Ogni cosa per Git è un *oggetto* identificato da un nome di 40 caratteri, cioè
da un *hash* calcolato a partire dal contenuto dell'oggetto stesso tramite
l'algoritmo SHA-1. Ad esempio:

    6ff87c4664981e4397625791c8ea3bbb5f2279a3

Questo modo di identificare un oggetto ha una serie di vantaggi:

 - essendo ogni oggetto identificato dal proprio hash. è molto facile e veloce
confrontare due oggetti
 - dato che l'algoritmo con cui si assegna un nome ad un oggetto è univoco, lo
stesso oggetto in repository diversi avrà lo stesso identico nome
 - come effetto collaterale si ottiene *gratis* un controllo degli errori per
ogni oggetto

Ogni oggetto è identificato da un proprio hash SHA-1 e composto da tre attributi
fondamentali: *tipo*, *dimensione* e *contenuto*. 

Esistono in tutto quattro tipi di oggetti: **blob**, **tree**, **commit** e
**tag**. Tutto il sistema di versionamento si basa su questi quattro elementi,
che possono essere manipolati pressochè in ogni maniera attraverso vari comandi
(che possono essere assemblati, scriptati e automatizzati a volontà), ed è su
questo che si basa la sua potenza.

Ne consegue anche che Git è fondamentalmente diverso dagli altri sistemi di
versionamento perchè invece di essere basato su un sistema di *delta storage*,
memorizza uno snapshot dei file del repository ad ogni *commit*. Come dicevo
prima, è più simile a un piccolo filesystem al di sopra del normale filesystem
del sistema operativo.

Blob
----

> blob, n 1: an indistinct shapeless form<br/>
> --WordNet

Come suggerisce il nome un blob non è altro che **un blocco di dati
binari che contiene un file, identificato univocamente e unicamente dal proprio
hash**. Non ha altri metadati associati (apparte quelli condivisi da tutti gli
oggetti: *tipo* e *dimensione*), nemmeno il nome del file a cui si riferisce.

Una proprietà di questo fatto è che se due file in uno stesso repository o in
differenti revisioni dello stesso hanno il medesimo contenuto, per git sono
esattamente lo stesso blob che verrà condiviso dalle due directory o
revisioni. Quindi un oggetto è *totalmente indipendente dalla sua locazione
nella gerarchia di directory* e il rinomiare il file di cui il blob è lo
snapshot non avrà alcun effetto sul blob stesso.

Tree
----

Questo oggetto è fondamentalmente **una collezione di puntatori ad altri blob e
tree** e rappresenta generalmente il contenuto di una directory.

Ogni puntatore contenuto in un oggetto tree ha alcuni attributi: *permessi*,
*tipo*, *hash* e *nome*. E' in questo oggetto che c'è l'associazione tra un
blob e il nome del file di cui è lo snapshot. Inoltre, notiamo che vengono
memorizzati anche i permessi di accesso ai file (git è nato in ambiente Unix).

Come tutti gli altri oggetti, anche un tree è identificato da un hash. Questo
conferisce gli stessi vantaggi dei blob al confronto e gestione delle
directory, uno su tutti: la possibilità di confrontare due directory (con tutti
i file e sottodirectory ivi contenute) con una semplice operazione in tempo
*O(1)*.

Commit
------

Tag
---

L'area di stage: l'Index
========================