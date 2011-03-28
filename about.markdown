---
layout: default
title: About
---

# Chi sono #
Chi lo sa? :)

> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
> consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
> Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.
> 
> > Blackquote into another blackquote
>
> Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
> id sem consectetuer libero luctus adipiscing.

Un pò di testo a caso...

*   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
    Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
    viverra nec, fringilla in, laoreet vitae, risus.
*   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
    Suspendisse id sem consectetuer libero luctus adipiscing.

E ancora altro *testo*.

1.  This is a list item with two paragraphs. Lorem ipsum dolor
    sit amet, consectetuer adipiscing elit. Aliquam hendrerit
    mi posuere lectus.

    Vestibulum enim wisi, viverra nec, fringilla in, laoreet
    vitae, risus. Donec sit amet nisl. Aliquam semper ipsum
    sit amet velit.

2.  Suspendisse id sem consectetuer libero luctus adipiscing.

Codice
------
Proviamo con un pò di codice:

	<span>ciao span!</span>

Ora, proviamolo `inline`...

Un pò di codice python, tanto per gradire :P

{% highlight python %}
from sys import exit

print "Ciao mondo! :D"
exit()
{% endhighlight %}

E lisp:

{% highlight common-lisp %}
(defun greet-all (persone)
  (mapc (lambda (persona)
	  (princ `("Ciao" ,persona))
	  (fresh-line))
	persone))

(fresh-line)
(greet-all '("Manuel" "Lisa"))
{% endhighlighting %}

Links
-----
Proviamo anche qualche [link](http://www.google.com "tooltip")
completo.

Ma molto spesso occorre usare più di un [riferimento][1], e allora si
può ad esempio usare uno stile diverso, reso popolare (a me) dalla
mailing-list [debian][2]. Tuttavia questo stile è utile solo per
eventualmente rendere più leggibile il testo "sorgente".

Ehi, a quando pare ci sono anche i link automatici:
<http://www.google.com>, bello!

 [1]: http://www.wordreference.com/ "Descrizione"
 [2]: http://www.debian.org/ "Descrizione 2"
