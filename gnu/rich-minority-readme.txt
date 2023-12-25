#+OPTIONS: tags:nil 
#+OPTIONS: toc:nil num:nil

* rich-minority-mode

Emacs package for hiding and/or highlighting the list of minor-modes
in the mode-line.

** Usage

To activate the enrichment of your minor-modes list, call =M-x rich-minority-mode=, or add this to your init file:

#+begin_src emacs-lisp
(rich-minority-mode 1)
#+end_src

By default, this has a couple of small effects (provided as examples)
it is up to you to customize it to your liking with the following
three variables:

- ~rm-blacklist~ :: List of minor mode names that will be hidden
     from the minor-modes list. Use this to hide *only* a few modes that
     are always active and don’t really contribute information. eg:
#+begin_src emacs-lisp
(setq rm-blacklist
      (format "^ \\(%s\\)$"
              (mapconcat #'identity
                         '("Fly.*" "Projectile.*" "PgLn")
                         "\\|")))
#+end_src
- ~rm-whitelist~ :: List of minor mode names that are allowed on
     the minor-modes list. Use this to hide *all but* a few modes.
- ~rm-text-properties~ :: List text properties to apply to each
     minor-mode lighter. For instance, by default we highlight =Ovwrt=
     with a red face, so you always know if you’re in =overwrite-mode=.

** Comparison to Diminish
Diminish is an established player in the mode-line world, who also
handles the minor-modes list. What can rich-minority /offer in contrast/?

- rich-minority is more versatile:
  1. It accepts *regexps*, instead of having to specify each minor-mode individually;
     That is, where in diminish.el you would do this:
     #+begin_src emacs-lisp
     (diminish 'auto-fill-function)
     #+end_src
     rich-minority does this:
     #+BEGIN_SRC emacs-lisp
     (add-to-list " Fill" 'rm-blacklist)
     #+END_SRC
     where ~" Fill"~ could be any regular expression.
  2. It also offers a *whitelist* behaviour, in addition to the blacklist;
  3. It supports *highlighting* specific minor-modes with completely arbitrary text properties.
- rich-minority takes a cleaner, functional approach. It doesn’t hack
  into the =minor-mode-alist= variable.

What is rich-minority /missing/?

It just doesn’t have a quick and simple replacement functionality yet.
However, you can set the =display= property of a minor-mode to
whatever string you want and that will function as a replacement.

** Installation

This package is available from GNU Elpa and Melpa, you may install it
by calling =M-x list-packages=.

** License

rich-minority is released under the GPLv3 or newer.
