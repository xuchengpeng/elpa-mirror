
This is a generalization of an idea by Mickey Petersen of
masteringemacs fame:

Use one keystroke to jump from a code buffer to the corresponding repl
buffer and back again.

This works even if you do other stuff in between, as the last buffer
used to jump to a repl is stored in a buffer local variable in the
repl buffer.

`repl-toggle-mode` will automatically be activated for `prog-mode`
major modes and configured and the started repl-buffers.

There are no repl/mode combinations preconfigured, put something like
the following in your emacs setup for php and elisp repl:

    (setq rtog/fullscreen t)
    (require 'repl-toggle)
    (setq rtog/mode-repl-alist '((php-mode . php-boris) (emacs-lisp-mode . ielm)))

This defines a global minor mode, indicated with 'rt' in the modeline, that
grabs "C-c C-z" as repl toggling key-binding.
I don't know with which repl modes this actually works. If you use
this mode, please tell me your ~rtog/mode-repl-alist~, so that I can
update the documentation.

** ~pop~ or ~switch~: ~rtog/goto-buffer-fun~

Emacs -- of course -- has more than one function to switch between
buffers. You can customize ~rtog/goto-buffer-fun~ to accommodate your
needs. The default is ~switch-to-buffer~; to move focus to another
frame that already shows the other buffer, instead of switching the
current frame to it, use ~pop-to-buffer~.

    (setq rtog/goto-buffer-fun 'pop-to-buffer)

** Configurations known to work

- ~(php-mode . psysh)~
- ~(emacs-lisp-mode . ielm)~
- ~(elixir-mode . elixir-mode-iex)~
- ~(ruby-mode . inf-ruby)~
- ~(js2-mode . nodejs-repl)~ and ~(js3-mode . nodejs-repl)~

*** Switch to shell buffer

If the mode you want to use doesn't jump to an existing repl-buffer,
but always starts a new one, you can use `rtog/switch-to-shell-buffer'
in your configuration to get that behaviour, e.g. for `octave-mode':

    (rtog/add-repl 'octave-mode (rtog/switch-to-shell-buffer 'inferior-octave-buffer 'inferior-octave))

** Pass code at point to the REPL

When you supply the universal prefix argument to the switching function,

- C-u passes the current line or active region
- C-u C-u passes the current defun
- C-u C-u C-u passes the whole current buffer

to the repl buffer you switch to.

** fullscreen REPL
If you set =rtog/fullscreen= to true, the repl-commands will be
executed fullscreen, i.e. as single frame, restoring the window-layout
on switching back to the originating buffer.

    (setq rtog/fullscreen t)~

** Fallback REPL function

The custom variable =rtog/fallback-repl= can be customized with a
function; this function will be called if no REPL is associated with
the current buffers major mode.
