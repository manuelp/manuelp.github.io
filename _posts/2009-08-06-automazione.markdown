---
layout: post
title: Automazione!
categories: metaprogramming
---

Una cosa risaputa è che un buon programmatore deve essere *pigro*. Ma di una pigrizia particolare: *perchè svolgere a mano ogni compito ripetitivo che può essere svolto altrettanto bene e molto più velocemente da una macchina?*

Nella mia situazione, complice anche il fatto che sto imparando [Ruby](http://ruby-lang.org/it/), ho automatizzato diverse cose. In particolare, scrivo su un altro [blog](http://viasenzanome.wordpress.com/) di sviluppo personale e per un certo periodo ho avuto un dominio apposito con un blog servito dal software [Serendipity][1] che dopo un anno di attività ho chiuso per ritornare a wordpress.

Purtroppo (mea culpa) non avevo effettuato il backup del vecchio blog se non tramite uno "snapshot" del sito, quindi mi sono ritrovato con circa un'ottantina di post sotto forma di file html che volevo importare su wordpress. Inutile dire che farlo a mano sarebbe stata un'impresa non da poco...

Così in un paio di giorni (e un paio di tentativi) ho implementato un programmino di tipo *scraper* che per ogni post salvato sotto forma di file html, ne estrae: post, titolo, data di pubblicazione e commenti con relative date, nomi ed eventuali link. Il tutto ripulendo il risultato da blocchi non necessari.

Con il primo tentativo ho usato estensivamente le espressioni regolari per identificare ed estrarre le aree desiderate, ma si è rivelato un compito abbastanza lungo e anche se alla fine funzionava per alcuni post c'erano dei problemi. Mi serviva una libreria che fosse a conoscenza dell'html e mi consentisse di farne il parsing.

Con il secondo tentativo, ho utilizzato una stupenda libreria che mi ha permesso di selezionare esattamente i contenuti desiderati e anche di effettuare modifiche: [hpricot][2].

Risultato: una quindicina di minuti circa per postare sul blog originale circa 80 post con la data corretta e tutti i commenti! 

[1]: http://www.s9y.org/ "Serendipity Weblog System" 
[2]: http://wiki.github.com/why/hpricot "hpricot"
