---
layout: post
title: Discussione - Adozione dell'Agile
categories: agile
---

Di seguito riporto qualche appunto sull'[articolo](https://www.infoq.com/articles/failed-agile-adoption-reasons) *Why Agile Adoption Fails in Some Organizations* apparso su InfoQ il 4 Novembre 2009.

L'agile presuppone che l'organizzazione finanzi il *team* e che lo sviluppo sia un'attività continuativa e considerata come un *investimento*. In questo contesto si riconosce l'inevitabile inaccuratezza delle stime iniziali e si procede in modo iterativo e incrementale, accumulando esperienza e imparando a prevedere le tempistiche mano a mano che si avanza con lo sviluppo. In un team agile le stime sulle tempistiche hanno scarsa importanza e il team procede a produrre valore in modo continuativo, ma non si sa con precisione a priori quanto tempo ci vorrà per completarlo.

In uno scenario diverso in cui l'organizzazione finanzia separatamente i singoli *progetti*, che sono visti come un *costo*, il management richiede delle stime sui tempi e non è disposto a finanziarlo oltre i tempi preventivati. In questo contesto, a differenza del caso precedente, si hanno dei problemi se la stima iniziale non si rivela corretta e il management è costretto a una scelta: 

 - Modificare i piani e riducendo lo *scope* del progetto.
 - Oppure ottenere un ROI negativo (il progetto va in perdita).
  
Le principali cause del fallimento nell'adozione di uno stile agile di sviluppo possono essere riassunte nei seguenti punti:

 - L'Agile assume che l'organizzazione voglia uno sforzo di sviluppo continuativo, non un progetto a breve termine. **L'agile presuppone un finanziamento a team, non a progetto.** In altre parole questo approccio presuppone che non ci siano vincoli di budget sul singolo progetto.
 - L'Agile è una metodologia che influenza sia il management che il processo di sviluppo: ci sono ripercussioni sia nelle pratiche adottate dal team, sia nella gestione (pianificazione e costi) del progetto stesso. **Il management e il processo di sviluppo sono interrelati e qualsiasi metodologia influenza entrambi.**
 
Date queste considerazioni, i progetti a costo fisso sono chiaramente un contesto inadatto nel quale collocare l'Agile. Spesso le aziende che ci provano finiscono per sovrapporre un façade agile su un progetto che in realtà è condotto in stile *Waterfall*.

In ultima analisi, **se l'organizzazione è in grado di finanziare un team di sviluppo in modo continuativo, allora l'agile è un approccio vincente. Se invece i fondi sono allocati per progetto, un approccio più tradizionale è la scelta obbligata.**
  
## Discussione
Di seguito riassumo i contributi espressi nei commenti all'articolo da alcuni lettori:

 - Si assume che un approccio waterfall abbia successo quasi automaticamente in un progetto a costo fisso, quando in realtà è vero l'opposto. Con un cliente che sappia prioritizzare i propri requisiti efficacemente l'agile è un approccio utilizzabile anche in quel caso.
 - Occorre scegliere la metodologia di sviluppo in base ai requisiti e al contesto: *se i requisiti non sono chiari fin dall'inizio e c'è rischio importante che cambino, allora un approccio agile è quasi obbligatorio*. Una volta scelto l'approccio più adatto si sceglie la forma contrattuale consona. Le decisioni sulla conduzione del progetto vanno prese in modo pragmatico che rifletta le reali esigenze.
 
> Perhaps we need to start asking, "how much can we get for $X" rather than stating, "I want all this for $Y. Make that happen." -- Robert Dempsey

 - I metodi agili hanno l'obiettivo di massimizzare l'efficienza, la produttività e il business value realizzato indipendentemente dal budget quindi sono ugualmente applicabili in un progetto a costo fisso. Inoltre, con l'avanzare del progetto e specialmente con l'acquisizione di competenze da parte del team diventa possibile stimare con sempre maggior precisione i tempi richiesti. Ovviamente, in un progetto a costo fisso è ancora più importante avere un team con esperienza e un cliente in grado di prioritizzare le features.

## Commenti personali
Innanzitutto l'approccio agile (iterativo e incrementale) comporta importanti vantaggi per tutte le parti coinvolte (clienti, management, sviluppatori), ed è utile educarle *tutte* sui suoi vantaggi.

Inoltre, data la natura dell'agile (focus su *efficienza* e *qualità*) è applicabile anche in un contesto a costo fisso secondo me. Però qualsiasi progetto a costo fisso comporta un rischio sostenibile solo in questi casi:

 - I requisiti sono chiari fin dall'inizio, sono immutabili e il team è composto da esperti che hanno già affrontato progetti nello stesso dominio applicativo. Solo in questo caso è possibile fare una stima ragionevolmente precisa e adottare un ciclo di vita di tipo sequenziale (*Waterfall*) con un certo successo.
 - Lo scope è variabile. In questo caso l'approccio agile continua a essere il più conveniente.

### Scelta pragmatica
La scelta della metodologia di sviluppo va fatta in modo pragmatico a seconda del contesto (progetto, dominio applicativo, cliente, team, ecc), non c'è un approccio universalmente migliore (*Silver Bullet*). 

La scelta va fatta anche in base ai parametri che si vuole ottimizzare: *costi, tempi, scope e qualità*. E' evidente che il fissare tutti questi parametri a priori è praticamente impossibile, allora occorre trovare la combinazione più conveniente per le parti in causa. Fissando la qualità come un parametro imprescindibile e considerando che costi e tempi sono strettamente connessi, individuo i seguenti casi interessanti:

<table>
    <tr>

        <th>Tempo</th>
        <th>Costo</th>
        <th>Scope</th>
        <th>Commento</th>
    </tr>
    <tr>
        <td>fisso</td>

        <td>fisso</td>
        <td>fisso</td>
        <td>Praticamente impossibile.</td>
    </tr>
    <tr>
        <td>fisso</td>
        <td>fisso</td>

        <td>variabile</td>
        <td>Sicurezza sui costi ma scope variabile.</td>
    </tr>
    <tr>
        <td>variabile</td>
        <td>fisso</td>
        <td>fisso</td>

        <td>Alto rischio di ROI negativo.</td>
    </tr>
    <tr>
        <td>variabile</td>
        <td>variabile</td>
        <td>fisso</td>
        <td>Scenario ideale per l'applicazione dell'Agile.</td>
    </tr>

    <tr>
        <td>variabile</td>
        <td>variabile</td>
        <td>variabile</td>
        <td>Efficiente con l'Agile.</td>
    </tr>
</table>

> Forse dobbiamo cominciare a domandare "quanto posso avere per X$" invece che affermare "voglio tutto questo per X$. Fatelo." --Robert Dempsey

Alla luce di queste considerazioni, a meno che non si tratti di un progetto a requisiti completamente specificati e immutabili, e condotto da un team esperto nel dominio applicativo, **conviene adottare un approccio agile negoziando la forma contrattuale più adatta.**
