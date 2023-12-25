Emacs in Rime, support multiple schemas.

* Installation

Note: ~make~ and ~gcc~ is required.

** Linux

Install librime with your package manager, if you are using fcitx-rime or ibus-rime,
the librime should be already installed.

Emacs configuration:

#+BEGIN_SRC emacs-lisp
  (use-package rime
    :custom
    (default-input-method "rime"))
#+END_SRC

** MacOS

Download librime release.

#+BEGIN_SRC bash
  wget https://github.com/rime/librime/releases/download/1.7.1/rime-1.7.1-osx.zip
  unzip rime-1.7.1-osx.zip -d ~/.emacs.d/librime
  rm -rf rime-1.7.1-osx.zip
#+END_SRC

Emacs configuration:

#+BEGIN_SRC emacs-lisp
  (use-package rime
    :init
    :custom
    (rime-librime-root "~/.emacs.d/librime/dist")
    (default-input-method "rime"))
#+END_SRC

* Keybindings in Rime.

With following configuration, you can send a serials of keybindings to Rime.
Since you may want them to help you with cursor navigation, candidate pagination and selection.

Currently the keybinding with Control(C-), Meta(M-) and Shift(S-) is supported.

#+BEGIN_SRC emacs-lisp
  ;; defaults
  (setq rime-translate-keybindings
    '("C-f" "C-b" "C-n" "C-p" "C-g"))
#+END_SRC

* Candidate menu style

Set via ~rime-show-candidate~.

| Value        | description                                                                   |
|--------------+-------------------------------------------------------------------------------|
| ~nil~        | don't show candidate at all.                                                  |
| ~minibuffer~ | Display in minibuffer.                                                        |
| ~message~    | Display with ~message~ function, useful when you use minibuffer as mode-line. |
| ~popup~      | Use popup.                                                                    |
| ~posframe~   | Use posfarme, will fallback to popup in TUI                                   |
| ~sidewindow~ | Use sidewindow.                                                               |

* The lighter

You can get a lighter via ~(rime-lighter)~, which returns you a colored ~ㄓ~.
Put it in modeline or anywhere you want.

You can customize with ~rime-title~, ~rime-indicator-face~ and ~rime-indicator-dim-face~.

* Temporarily ascii mode

If you want specific a list of rules to automatically enable ascii mode, you can customize ~rime-disable-predicates~.

Following is a example to use ascii mode in ~evil-normal-state~ or when cursor is after alphabet character or when cursor is in code.

#+BEGIN_SRC emacs-lisp
  (setq rime-disable-predicates
        '(evil-normal-state-p
          rime--after-alphabet-char-p
          rime--prog-in-code-p))
#+END_SRC

** Force enable

If one of ~rime-disable-predicates~ returns t, you can still force enable the input method with ~rime-force-enable~.
The effect will only last for one input behavior.

You probably want to give this command a keybinding.

* The soft cursor

Default to ~|~ , you can customize it with

#+BEGIN_SRC emacs-lisp
  (setq rime-cursor "˰")
#+END_SRC

* Shortcut to open Rime configuration file

Use ~rime-open-configuration~.
