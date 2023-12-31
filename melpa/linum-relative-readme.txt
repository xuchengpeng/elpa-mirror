![Screenshot](https://github.com/coldnew/linum-relative/raw/master/screenshot/screenshot1.jpg)

linum-relative lets you display relative line numbers for current buffer.


; Installation:

If you have `melpa` and `emacs24` installed, simply type:

    M-x package-install linum-relative

And add the following to your .emacs

    (require 'linum-relative)

; Setup & Tips:

The non-interactive function *linum-on* (which should already be built into recent GNU Emacs distributions), turns on side-bar line numbering:

    (linum-on)

and alternatively, by using command:

    M-x linum-relative-mode

Relative line numbering should already be enabled by default (by installing this package), following *linum-on* or enabling *linum-mode*. One can also use the *linum-relative-toggle* interactive function to switch between relative and non-relative line numbering:

    M-x linum-relative-toggle


; Backends

By default, linum-relative use *linum-mode* as backend, since linum-mode is based on emacs-lisp, you may have performance issue on large file.

Since linum-relative 0.6, if you also use emacs version 26.1 or above, you can setup `linum-relative-backend' to make linum-relative-mode use `display-line-number-mode' as backend, which is implement in C so the performance is really nice.

However some linum-relative's customize function may not work propely.

Here's how to use `display-line-number-mode' as backend:

```elisp
     ;; Use `display-line-number-mode' as linum-mode's backend for smooth performance
(setq linum-relative-backend 'display-line-numbers-mode)
```
