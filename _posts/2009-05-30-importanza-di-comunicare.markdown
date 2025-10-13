---
layout: post
title: L'importanza di comunicare
category: metaprogramming
---

Non basta avere una buona idea, bisogna anche saperla "vendere". C'è una
concezione comune secondo cui "una buona idea si vende da sola" ma purtroppo
non è sempre vero: basta guardarsi attorno per vedere delle *ottime* idee
che non hanno riscosso un grande successo a causa di una "cattiva campagna di
marketing".

> Una buona idea è un'orfana senza una comunicazione efficace.<br/>
> -- "The Pragmatic Programmer", Andrew Hunt, David Thomas

Inoltre spendiamo buona parte delle nostre giornate comunicando (memo, email,
discussioni con colleghi e management, interviste col committente, proposte
di miglioramento, ecc.) per cui vale la pena farlo bene.

Personalmente mi sono trovato in molte situazioni in cui ho dovuto fare
attenzione al modo in cui comunicavo per sostenere la validità del mio punto di
vista. Ad esempio quando ho proposto il VCS Mercurial al team con il quale ho
sviluppato un progetto all'università per ingegneria del software, o tutte le
volte che abbiamo fatto delle presentazioni ai docenti del medesimo corso.

Come comunicare efficacemente
=============================

In "The Pragmatic Programmer" gli autori dedicano un capitolo a come comunicare
efficacemente, in cui danno alcuni consigli che applicherò ai post di questo
stesso blog e che riassumo qui.

Sapere cosa dire
----------------

Punto fondamentale e ovvio, ma troppo spesso ci si ritrova a digitare
"Introduzione" e a cominciare a scrivere senza avere un'idea precisa di quello
che si vuole comunicare.

E' utile quindi **pianificare** quello che si vuole dire, ad esempio scrivendo
un'*outline* e modificandola finché la risposta alla domanda "questo comunica
quello che sto cercando di dire?" è sì.

Adattare il messaggio all'audience
----------------------------------

Ogni idea può essere presentata in modi diversi a seconda dell'audience: ve ne
sono di diverse con bisogni, interessi e capacità differenti. E' quindi
importante personalizzare il messaggio affinché sia recepito correttamente da
chi sta ascoltando.

> We've all sat in meetings where a development geek glazes over the eyes of the
> vice president of marketing with a long monologue on the merits of some arcane
> technology. This isn't communicating: it's just talking, and it's
> annoying.

Occorre avere una chiara idea di chi ascolterà il nostro messaggio e delle sue
caratteristiche. Le seguenti domande possono aiutare:

 - Che cosa vuoi che imparino?
 - Qual'è il loro interesse in quello che hai da dire?
 - Quanto sono sofisticati?
 - Quanti dettagli vogliono?
 - Chi vuoi che faccia propria l'informazione?
 - Come puoi motivarli ad ascoltarti?

Inoltre occorre adottare uno **stile di comunicazione** adatto. L'interlocutore
preferisce andare dritto ai fatti oppure conversare? Preferisce uno stile
formale o uno più sciolto? 

Capire queste cose è importante per far recepire al meglio il messaggio.

Scegliere il momento giusto
---------------------------

Non ogni momento è il momento giusto. L'interlocutore può avere fretta, altro
da fare o semplicemente può non essere dell'umore giusto. Una parte del capire
le esigente dell'audience consiste nel capire le loro priorità, nel dubbio è
sempre meglio chiedere: *"è un buon momento per parlarti di ...?"*

Questo, almeno in un blog, non è un problema visto la natura intrinsecamente
on-demand del mezzo di comunicazione :)

Cura l'apparenza
----------------

E' uno sbaglio fare affidamento solo sul contenuto di un documento senza badare
alla presentazione. L'aspetto è una caratteristica importante che concorre a
veicolare efficacemente un'idea. **Contenuto e aspetto devono essere curati
in sinergia.**

> **TIP 10:** "It's Both What You Say and the Way You Say It"

Alcuni suggerimenti per documenti:

 - usa un sistema di impaginazione che produca risultati gradevoli: ad esempio
[LaTeX][latex], XHTML+CSS con eventualmente qualche exporter in PDF come
[FOP][fop], [DocBook][docbook], o anche qualche word-processor che supporti gli
stylesheets.
 - usa lo *spell-checking*, prima automatico e poi manualmente
(verifica *walkthrough*) 

 [latex]: https://www.latex-project.org/ "Homepage del LaTeX Project"
 [docbook]: https://www.docbook.org/ "DocBook: The Definitive Guide"
 [fop]: https://xmlgraphics.apache.org/fop/ "Apache FOP"

<br/> 

Coinvolgi l'audience
--------------------

Un altro suggerimento (che non ho mai impiegato, mea culpa) è quello di
coinvolgere gli interlocutori nel processo di produzione del documento per
raccogliere feedback e instaurare una proficua relazione di lavoro.

Cosa che sembra abbia funzionato tra l'altro per la produzione di libri interi
come [Real World Haskell][RWH] e [Programming Scala][ProgrammingScala].

 [RWH]: https://book.realworldhaskell.org/ "Real World Haskell"
 [ProgrammingScala]: https://programming-scala.labs.oreilly.com/

Ragione per cui ho adottato questo approccio per il presente blog: nonostante
il suo aspetto/layout sia ancora un work-in-progress è online e il
[repository][] dei suoi sorgenti è liberamente accessibile (in lettura) e
pubblicato con licenza [CreativeCommons][].

 [repository]: https://github.com/manuelp/manuelp.github.com/tree/master 
 [CreativeCommons]: https://creativecommons.org/licenses/by-sa/2.5/it/ "CC 2.5 Attribution-Share Alike"

Inoltre, nel caso di comunicazioni verbali è utile trasformare l'esposizione in
una sessione interattiva, ad esempio facendo domande e chiedendo di riassumere
quello che si è appena detto.