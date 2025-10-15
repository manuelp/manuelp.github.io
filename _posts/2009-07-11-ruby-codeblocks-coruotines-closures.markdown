---
layout: post
title: Ruby - code blocks, coroutines e closures
categories: development
tags: [Ruby, ita]
---

Ruby si ispira abbastanza al Perl (purtroppo, l'ho usato un po' ma non mi è mai piaciuto come linguaggio), ma nonostante questo mi sono sforzato di andare avanti a studiarlo. Uno è in grado di giudicare le potenzialità di un linguaggio solo conoscendolo e avendolo utilizzato, questo vale specialmente per quei linguaggi che forniscono *features* che non si conoscono e che richiedono un modo di pensare diverso. 

Bisogna respingere la sensazione di "scomodità" che si prova all'inizio ed entrare nell'ottica giusta prima di poterne apprezzare le potenzialità ed esprimere un giudizio informato e non basato unicamente su simpatie/antipatie.

# Un linguaggio molto dolce
Ruby, a differenza di Python e similmente al Perl, fornisce una gran quantità di *zucchero sintattico* che porta ad avere più modi diversi per fare una stessa cosa. Infatti molti "entusiasti" del linguaggio parlano spesso di come imparino sempre nuovi modi più eleganti, concisi ed espressivi per esprimere certi concetti (e questo, almeno nel caso del Perl, spesso a discapito della leggibilità).

Non so ancora se sia una cosa positiva o meno, su questo sospendo il giudizio in attesa di acquisire una conoscenza più approfondita.

# Coroutines e code-blocks
Un paio di features interessanti che fornisce Ruby sono le *coroutines* e i *code-blocks*, che permettono (tra le altre cose) di implementare in maniera molto concisa ed espressiva degli iteratori e dei "blocchi di codice" da eseguire sotto un controllo transazionale.

In un linguaggio "tradizionale" come Java, per iterare sugli elementi di una classe collezione si può procede ad esempio così:

{% highlight java %}
ArrayList<int> lista = new ArrayList<int>();
// ...
for (int i = 0; i < lista.size(); i++) {
	System.out.println(lista.get(i) + " ");
}
{% endhighlight %}

Questo modo di iterare su una collezione è familiare a chi è abituato ad usare linguaggi come C++, Java o anche Python.

Ruby, attraverso l'uso dei *code-blocks*, permette di iterare in modo diverso:

{% highlight ruby %}
@lista = [1, 2, 3, 4]
@lista.each {|elemento| puts elemento}
{% endhighlight %}

Il che è anche abbastanza leggibile una volta fatta l'abitudine:

		lista: per ogni elemento, scrivilo sulla console

.
## Cosa sono i *code block* e le *coroutines*?
Un *code-block* non è altro che un blocco di codice (incredibilmente), delimitato da parentesi graffe o da *do-end* che non viene eseguito immediatamente, ma viene messo da parte per essere eseguito in un secondo momento. 

Questo code-block deve essere adiacente ad una chiamata a un metodo, la quale può cedere il controllo al code-block ogni volta che lo desidera tramite il costrutto `yield` tutte le volte che lo desidera, eventualmente passandogli parametri e ricevendone.

A tutti gli effetti il metodo e il code-block sono *coroutines*, ovvero funzioni che lavorano "insieme" passandosi la palla l'una con l'altra. La cosa interessante è che a un metodo che utilizza un code-block esterno, può essere passato un blocco qualunque di istruzioni. E' un concetto simile ma più potente e versatile dei puntatori a funzione del C. 

### Esempio: iteratori
{% highlight ruby %}
3.times {puts "Hello, world!"}
{% endhighlight %}

Questo codice si legge:

		Per 3 volte: stampa la stringa "Hello, world!"

`times` è la chiamata all'omonimo metodo fornito dal numero 3 (essendo Ruby un linguaggio completamente ad oggetti, è un oggetto di tipo intero) a cui viene associato un code-block che effettua la stampa. Il metodo `times` non fa altro che richiamare tante volte `yield` quanto è il valore che rappresenta l'oggetto su cui viene richiamato il metodo.

Si può ottenere lo stesso effetto di prima con una funzione *ad-hoc*:

{% highlight ruby %}
def call_block
	for i in 1..3
		yield
	end
end

call_block {puts "Hello, world!"}
{% endhighlight %}

Un code-block può ricevere parametri dalla routine da cui viene invocata tramite `yield`, e similmente riceverne al ritorno, trasformando `yield` in una vera e propria chiamata al code-block.

Inoltre, passando un parametro al code-block, possiamo implementare un iteratore per una classe collezione (analogo al metodo `each` del primo esempio) nel modo seguente:

{% highlight ruby %}
class Collezione
	def initialize(lista)
		@collezione = lista
	end

	def each 
		for i in 0..@collezione.size
			yield(@collezione[i])
		end
	end
end

lista = Collezione.new([1, 2, 3, 4])
lista.each {|elemento| puts elemento}
{% endhighlight %} 

Questo è più o meno il modo in cui sono implementati gli iteratori nella libreria standard di Ruby.

Diversamente da come illustrato nel design pattern *Iterator* della GOF, nel quale l'iteratore è una classe esterna all'oggetto su cui itera e di cui mantiene un riferimento, in Ruby un iteratore viene implementato come un semplice metodo all'interno della classe collezione. Questo a quanto pare rende il mantenere questo tipo di codice più semplice.

### Esempio: controllo transazionale su file
Quando ci si trova a lavorare con dei file (o comunque con delle risorse esterne), è importante anche *rilasciare* la risorsa una volta che si è acquisita e si sono effettuate tutte le operazioni necessarie.

Ad esempio, in Ruby è molto facile leggere il contenuto di un file e stamparlo a video:

{% highlight ruby %}
f = File.open("test_file.txt")
f.each do |line|
	puts line
end
f.close
{% endhighlight %}

Anche qui abbiamo usato un code-block (questa volta delimitato da do-end) per iterare sulle linee (delimitate dal carattere *newline*) contenute nel file.

Il difetto di questo approccio è che se ci si dimendica di invocare il metodo `close` sul file, questo rimane aperto. Un modo più corretto sarebbe quello di implementare un metodo che apra e chiuda il file, passando il controllo al code-block esterno per processarlo. In questo modo, il metodo che si occupa di aprire il file si occupa anche di chiuderlo:

{% highlight ruby %}
class File
	def File.open_and_process(*args)
		file = File.open(*args)
		while line = file.gets
			yield line
		end
		file.close
	end
end

File.open_and_process("test_file.txt", "r") {|line| puts line}
{% endhighlight %}

Naturalmente non ho considerato le eccezioni in questo esempio.

Il risultato è lo stesso, ma ora abbiamo un controllo sulle transazioni su file. Infatti, questo è il comportamento implementato nel metodo `File.open`. Se viene fornito un code-block alla chiamata, `open` restituisce l'*handler* del file al code-block per farlo processare, e al ritorno chiude il file aperto. 

# Closures
Un code-block associato ad un metodo può essere memorizzato insieme al *contesto* dal quale viene passato per essere invocato in un secondo momento. Per fare questo deve essere convertito in un oggetto `Proc` (abbreviazione di *procedure*) al cui interno contiene tutte le informazioni sul contesto in cui viene *definito* (variabili, metodi, costanti, *self*).

In questo modo, il code-block può essere utilizzato anche quando il contesto in cui è stato definito non è più presente. Un linguaggio che supporta questa funzionalità si dice che fornisce le **closures**.

Un esempio per illustrare il tutto (tratto dal [PickAxe](https://rubycentral.com/pickaxe/tut_containers.html)):

{% highlight ruby %}
def n_times(thing)
  return lambda {|n| thing * n }
end

p1 = n_times(23)
puts p1.call(3)
puts p1.call(4)
p2 = n_times("Hello ")
puts p2.call(3)
{% endhighlight %}

La stampa:

		69
		92
		Hello Hello Hello 

La funzione `lambda` converte il code-block in un oggetto di tipo `Proc` che ritorna al chiamante, con la variabile `thing` pari al parametro passato alla funzione contenente il code-block stesso. L'oggetto `Proc` poi, dispone del metodo `call` per eseguire il code-block memorizzato. 

Vediamo qui che, nonostante il parametro `thing` passato alla lambda sia ormai fuori dallo *scope* della `call`, questo è presente perchè memorizzato nell'oggetto `Proc`.
