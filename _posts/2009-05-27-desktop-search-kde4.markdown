---
layout: post
title: Desktop-search in KDE 4.2
categories: amministrazione
---

Da parte mia, una volta provata la funzionalità di *desktop search* è
diventata irrinunciabile. E' un utilissimo strumento per utilizzare il
proprio computer con *efficienza* e significa avere tutti i propri
dati e applicazioni letteralmente a portata di dita.

Niente più gerarchie di cartelle annidate e menu incastrati uno
dentro l'altro come una matrioska. I dati memorizzati nel computer
vengono indicizzati da un motore di ricerca e ogni file è
raggiungibile non solo ricercando il suo nome, ma anche il suo
contenuto. Possiamo aprire documenti, filmati, musica, applicazioni e
chi più ne ha più ne metta in pochi secondi e dovunque si trovino su
disco.

I vantaggi sono innegabili: io per esempio mi sono sempre trovato a
dover gestire molti file (la maggior parte dei quali inutili) e a
concepire sempre nuovi sistemi di archiviazione. Con il desktop search
posso mettere tutto dentro una sola cartella (ad esempio "Archivio") e
raggiungere qualsiasi file al suo interno non appena ne ho bisogno,
senza dover navigare alcuna gerarchia.

Piattaforme
===========

Per MacOS abbiamo il migliore (mi dicono):
[Quicksilver](https://quicksilver.en.softonic.com/mac). 

Per Windows ci sono [Google Desktop](https://desktop.google.com/it/) e
[Launchy](https://www.launchy.net/) (meno sofisticato
di Google Deskop).

Su piattaforma Linux, per Gnome c'è [GNOME Do](https://do.davebsd.com/) e, per
KDE,
[Nepomuk](https://nepomuk.semanticdesktop.org/) con il suo Strigi.

Usare Nepomuk con Ubuntu 9.04
=============================

Sfortunatamente Nepomuk non è ancora pronto per un'utilizzo di
produzione e attualmente è abilitabile con facilità solo il *rating* e
le annotazioni per i file.

Nonostante questo, con qualche passaggio manuale possiamo abilitare
anche il motore di indicizzazione (Strigi), che poi potremo utilizzare
con *Dolphin* o *krunner*.

Ecco i passaggi necessari:

 1. Installare il backend di Strigi *sesame*:

	    sudo aptitude install soprano-backend-sesame

    **Attenzione**: sarà necessario installare anche la la JRE!
 1. Aprire il pannello:
    
	    System Settings > Advanced > Desktop Search

	attivare *"Enable Nepomuk Semantic Desktop"* e **disabilitare**
	*"Enable Strigi Desktop File Indexer"*.
 1. Eliminare i dati correnti di Nepomuk:
	    
	    rm -Rf ~/.kde/share/apps/nepomuk
	    
 1. Configurare Strigi per usare il backend sesame nel file:
 
	    ~/.kde/share/config/nepomukserverrc
	 
	 e modificando la linea del backend nella sezione *\[main Settings\]*
	come segue:
	 
		Used Soprano Backend=sesame2
		
 1. Sfortunatamente, in Ubuntu Jaunty Jackalope sesame è linkato al
 file *libjvm.so*, che però viene cercato nella posizione standard
 */usr/lib* quando invece in Ubuntu si trova in un'altra posizione:

 	    locate libjvm.so

    nel mio caso:

	    /usr/lib/gcj-4.2-81/libjvm.so
	    /usr/lib/gcj-4.3-90/libjvm.so
	    /usr/lib/jvm/java-1.5.0-gcj-4.3-1.5.0.0/jre/lib/i386/client/libjvm.so
	    /usr/lib/jvm/java-1.5.0-gcj-4.3-1.5.0.0/jre/lib/i386/server/libjvm.so
	    /usr/lib/jvm/java-6-openjdk/jre/lib/i386/cacao/libjvm.so
	    /usr/lib/jvm/java-6-openjdk/jre/lib/i386/client/libjvm.so
	    /usr/lib/jvm/java-6-openjdk/jre/lib/i386/server/libjvm.so
	    /usr/lib/jvm/java-6-sun-1.6.0.13/jre/lib/i386/client/libjvm.so
	    /usr/lib/jvm/java-6-sun-1.6.0.13/jre/lib/i386/server/libjvm.so

    basta fare un link alla libreria *server* in /usr/lib, ad esempio:

	    ln -s  /usr/lib/jvm/java-6-openjdk/jre/lib/i386/server/libjvm.so \
		/usr/lib/libjvm.so

 1. Tornare nel pannello "System Settings" nella sezione "Desktop Search" (vedi
punto 2), e nella tab "Advanced Settings"  selezionare quali directory far
indicizzare da Strigi.
 1. Abilitare Nepomuk e Strigi. 

A questo punto dovrebbe comparire l'icona di Nepomuk nella task-list: ciò vuol
dire che è partita l'indicizzazione. 

Uso di Nepomuk
==============

Il modo più semplice e diretto per usare Nepomuk è con *krunner*: **Alt+F2**.

Nell'area di inserimento che compare al centro dello schermo si possono:

 - lanciare applicazioni
 - cercare in contatti, bookmarks, file, documenti recenti, cronologia di
navigazione 
 - eseguire calcoli
 - esplorare certe cartelle con dolphin

E altro ancora, il tutto scrivendone il nome o anche una parte (krunner effettua
il pattern matching). 

krunner funziona anche senza Nepomuk e Strigi, ma con questi ultimi possiamo
anche cercare *all'interno* dei propri file, con tutti i vantaggi che questo
comporta.