---
layout: post
title: Wiki Creole mode for Emacs
categories: emacs
---

I needed a syntax highlighting support for the wiki [Creole](http://www.wikicreole.org/wiki/Creole1.0) syntax in Emacs, but I couldn't find it anywhere... until I found the [dot.emacs](http://www.emacswiki.org/cgi-bin/wiki/menu-bar%2B.el/Search/AlexSchroeder/AlexSchroederConfigPyrobombus) configuration file of Alex Schroeder of [emacswiki](http://www.emacswiki.org) fame, which contains a little wonderful elisp snippet that I show you (a little bit modified) here:

{% highlight scheme %}
;; WikiCreole mode (wiki-mode)
;; Thanks to Alex Schroeder of www.emacswiki.org 
;; And Jason Blevins for his inspiring Markdown Mode
;; http://jblevins.org/projects/markdown-mode/
(define-generic-mode 'wiki-mode 
  nil ; comments 
  nil; keywords 
  '(("^\\(= \\)\\(.*?\\)\\($\\| =$\\)" . 'info-title-1)
    ("^\\(== \\)\\(.*?\\)\\($\\| ==$\\)" . 'info-title-2)
    ("^\\(=== \\)\\(.*?\\)\\($\\| ===$\\)" . 'info-title-3)
    ("^\\(====+ \\)\\(.*?\\)\\($\\| ====+$\\)" . 'info-title-4) 
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
    ("\\\\\\\\[ \t]+" . font-lock-warning-face)) ; font-lock list
  '(".wiki\\'"); auto-mode-alist
  '((lambda () (require 'info) (require 'goto-addr))); function-list
  "Wiki stuff including Creole Markup and BBCode.")
{% endhighlight %}

Just copy this snippet in your .emacs and any file "\*.wiki" will be highlighted correctly. You can also enable this mode from within emacs by: *M-x wiki-mode*.

I've adapted it a little bit and stealed some regexps from Jason's [markdown-mode](http://jblevins.org/projects/markdown-mode/) for highligthing titles. But there is still something missing:

 - Highlighting of lists
 - Support for images
 - Better highlithing of external links
 - Some functions for preview maybe?

Further versions of this mode will be updated here.

Enjoy :)
