---
layout: post
title: Riflessioni sul cloud computing
categories: web
tags: [web, ita]
---

Ultimamente si fa un gran parlare di [Cloud Computing][1], e di come questo
"nuovo" modo di intendere il software come un servizio ([SaaS][2]) possa essere
conveniente o meno.

[1]: https://it.wikipedia.org/wiki/Cloud_computing "Cloud Computing secondo wp"
[2]: https://en.wikipedia.org/wiki/Software_as_a_Service "Software As A Service"

I pro
=====

I vantaggi di un modello del genere sono presto detti: 

- non c'è alcun bisogno di predisporre datacenter
- non occorre occuparsi del *deployment* e della messa in sicurezza
- non occorre pagare personale interno che li amministri
- tutto il software è direttamente accessibile mediante il browser
- i dati sono nella *cloud* e accessibili (previa autenticazione) da qualunque
parte del mondo e da qualunque pc
- basta un pc depotenziato come un netbook per accedere a questi servizi (anche
computazionalmente intensi)

Quindi i software sono forniti come un servizio, unitamente alla loro gestione
per cui un utente non occorre che si preoccupi di altre problematiche aldilà
del loro utilizzo. Si tratta di un ulteriore passo verso l'*outsourcing*.

Esempi di questi servizi sono [GMail][3] e [Facebook][4], che sono diventati
molto popolari e che hanno aumentato di molto l'efficienza e la produttività
(almeno per quanto riguarda GMail).

[3]: https://www.gmail.com "GMail"
[4]: https://www.facebook.com "Facebook"

I contro
========

Ma se da una parte il cloud computing è molto comodo perchè solleva dalla
necessità di installazioni e configurazioni varie, dall'altra fa sorgere un
problema di fiducia e di sicurezza in generale, come fa notare
[Bruce Schneier][5].

[5]: https://www.schneier.com/blog/archives/2009/06/cloud_computing.html "Articolo di Bruce Schneier sul Cloud Computing"

Sicurezza
---------

Mentre posso gestire autonomamente il software installato in locale
predisponendo misure di sicurezza come firewall e ambienti di esecuzione
controllata, nel caso di SaaS sono completamente in balia del provider di quel
software.

Senza contare che se uso un software a codice chiuso e che viene eseguito per
di più su macchine remote, non posso essere sicuro che il servizio in questione
non faccia di più di quello che dichiara. Di conseguenza sarei portato a
fidarmi di più di un servizio basato su software *open-source* e di *standard condivisi*.

Privacy
-------

Se utilizzo il cloud computing, utilizzo un software in cui immetto i miei dati
che vengono memorizzati in maniera distribuita su server del gestore del
servizio. In pratica sto a tutti gli effetti fornendo miei dati a una compagnia
esterna che può essere o meno affidabile. Non ho nessun controllo sulle misure
di sicurezza di quella compagnia che potrebbe anche avere accesso ai miei
dati, o che potrebbe essere compromessa con relativo furto di informazioni.

E se quella compagnia ad esempio va in [bancarotta][6] o subisce dei guasti, posso [perdere
completamente i miei dati][7]. 

[6]: https://www.eweekeurope.co.uk/news/cloud-computing-forerunner-facing-bankruptcy-772 "Articolo di eweekeurope"
[7]: https://www.techcrunch.com/2009/01/03/journalspace-drama-all-data-lost-without-backup-company-deadpooled/ "Articolo su TechCrunch"

Inoltre, se i servizi sono accentrati in un'unica piattaforma, **un'eventuale
compromissione e furto d'identità potrebbero causare danni molto ingenti**. Prova
a immaginare a un ipotetico servizio che fornisse al tempo stesso email,
piattaforma per lo sviluppo di documenti, blog, gruppi di discussione e
conversazioni in tempo reale (suona familiare?). Immagina che venga
compromesso e che qualche persona senza scrupoli si impadronisca del tuo account. A
quel punto avrebbe accesso alle tue email, documenti e contatti e le sue azioni
avrebbero una portata enorme sia sulla tua vita lavorativa che personale...

Certo, possono essere adottate misure di sicurezza come crittografia (sia a
livello di connessione con https che di storage con AES ad esempio),
autenticazione e backup. Ma il punto rimane che il controllo ultimo sulla
configurazione e controllo del sistema rimane al fornitore. **E i tuoi dati sono
in quel sistema**.

Lock-in
-------

Quando i dati risiedono su server di terze parti, oltre alle problematiche
della sicurezza e della privacy occorre preoccuparsi della **portabilità**.

Non basta che il fornitore abbia una buona infrastruttura di sicurezza, usi la
crittografia, e abbia dei sistemi di backup. Occorre che i dati che io immetto
nel sistema li possa recuperare in ogni momento e integralmente, e devo essere
sicuro che quando li cancello vengano eliminati realmente (non come nel caso di
[Facebook][8]).

[8]: https://punto-informatico.it/2602790/PI/Commenti/niente-privacy-benvenuti-facebook.aspx "La finta cancellazione di Facebook"

Si tratta in altre parole di avere il controllo completo sui propri dati, in
caso contrario subiamo l'odioso *vendor lock-in* che stiamo solo ora
cominciando a superare nel campo delle suite office e delle pubbliche
amministrazioni.

Pay-per-use
===========

Non potendolo classificare come pro o contro, c'è comunque da considerare questo
aspetto.

Il cloud computing permette nuovi modelli di business, tra i quali c'è il
richiedere un corrispettivo per la fornitura di risorse e software in una
cloud. Questo può essere un aspetto positivo per il fornitore e per l'utente
(migliore servizio), ma può anche non esserlo, dipende dai punti di vista.

Riflessione
===========

Il cloud computing per come è nato ha proprio gli obiettivi di tagliare i costi
di gestione, incrementare la facilità di accesso e la produttività. Però ci sono
anche delle problematiche di sicurezza e di *fiducia* che emergono da questa
architettura. 

E' anche vero che si possono avere i vantaggi del Cloud Computing in una
*intranet*, mantenendo il controllo sui propri dati e conservando la comodità
della centralizzazione di accesso e utilizzando storage e risorse di calcolo distribuite. 
Ma al costo di sacrificare l'accesso globale (a meno di non predisporre VPN) e di
re-introdurre i costi di gestione di un datacenter. 

Bisogna stare attenti di chi fidarsi e per cosa. I meccanismi del mercato
dovrebbero spingere i fornitori a erogare un servizio di qualità, ma **quello che
si guadagna in comodità e risparmio, lo si può perdere in privacy,
sicurezza e flessibilità**.

> Trust is a concept as old as humanity, and the solutions are the same as they
> have always been. Be careful who you trust, be careful what you trust them
> with, and be careful how much you trust them. Outsourcing is the future of
> computing. Eventually we'll get this right, but you don't want to be a
> casualty along the way.<br/>
> --Bruce Schneier