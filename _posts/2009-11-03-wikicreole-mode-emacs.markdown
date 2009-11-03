---
layout: post
title: Wiki Creole mode for Emacs
categories: emacs
---

I needed a syntax highlighting support for the wiki [Creole](http://www.wikicreole.org/wiki/Creole1.0) syntax in Emacs, but I couldn't find it anywhere... until I found the [dot.emacs](http://www.emacswiki.org/cgi-bin/wiki/menu-bar%2B.el/Search/AlexSchroeder/AlexSchroederConfigPyrobombus) configuration file of Alex Schroeder of [emacswiki](http://www.emacswiki.org) fame with a little wonderful elisp snippet that I show you (a little bit modified) here:

{% highlight scheme %}
;; Thanks to Alex Schroeder
(define-generic-mode 'wiki-mode 
  nil ; comments 
  nil; keywords 
  '(("^=[^=\n]+" . 'info-title-1)
    ("^==[^=\n]+" . 'info-title-2)
    ("^===[^=\n]+" . 'info-title-3)
    ("^====+[^=\n]+" . 'info-title-4)
    ("\\[\\[.*?\\]\\]" . 'link)
    ("\\[.*\\]" . 'link)
    ("\\[b\\].*?\\[/b\\]" . 'bold)
    ("\\[i\\].*?\\[/i\\]" . 'italic)
    ("\\*\\*.*?\\*\\*" . 'bold)
    ("\\*.*?\\*" . 'bold)
    ("\\_<//.*?//" . 'italic)
    ("\\_</.*?/" . 'italic)
    ("__.*?__" . 'italic)
    ("_.*?_" . 'underline)
    ("|+=?" . font-lock-string-face)
    ("\\\\\\\\[ \t]+" . font-lock-warning-face)); font-lock list
  '(".wiki\\'"); auto-mode-alist
  '((lambda () (require 'info) (require 'goto-addr))); function-list
  "Wiki stuff including Creole Markup and BBCode.")
{% endhighlight %}

Just copy this snippet in your .emacs and any file "*.wiki" will be highlighted correctly. You can also enable this mode from within emacs by: *M-x wiki-mode*.

Enjoy :)
