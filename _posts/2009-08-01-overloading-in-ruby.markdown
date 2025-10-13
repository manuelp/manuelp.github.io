---
layout: post
title: Overloading in Ruby
categories: ruby
---

A differenza di linguaggi come Java e C++, Ruby *non supporta direttamente l'overloading di metodi e costruttori*. Il che è abbastanza grave quando si dimostra necessario disporre di più costruttori con un numero diverso di argomenti.

Una soluzione in certi casi potrebbe essere quella di usare un design-pattern come il *factory method*. In tutti gli altri, fortunatamente, è abbastanza semplice emulare l'overloading grazie al supporto di ruby alle liste di argomenti di lunghezza variabile.

Un esempio, tratto da [rubylearning](https://rubylearning.com/satishtalim/ruby_overloading_methods.html) per chiarire il concetto:

```ruby
# The Rectangle constructor accepts arguments in either  
# of the following forms:  
#   Rectangle.new([x_top, y_left], length, width)  
#   Rectangle.new([x_top, y_left], [x_bottom, y_right])  
class Rectangle
  def initialize(*args)
    if args.size < 2  || args.size > 3  
      # modify this to raise exception, later  
      puts 'This method takes either 2 or 3 arguments'  
    else  
      if args.size == 2  
        puts 'Two arguments'  
      else  
        puts 'Three arguments'  
      end  
    end  
  end
end

Rectangle.new([10, 23], 4, 10)
Rectangle.new([10, 23], [14, 13])
```
