               ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
                TEX-PARENS.EL: LIKE LISP.EL BUT FOR TEX
               ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━





1 Overview
══════════

  Emacs comes with the following useful commands for working with lists,
  sexps and defuns (see the info nodes [Expressions] and [Defuns]):

  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
   bind             command             docstring                                                          
  ─────────────────────────────────────────────────────────────────────────────────────────────────────────
   C-M-@            mark-sexp           Set mark ARG sexps from point or move mark one sexp.               
   C-M-a            beginning-of-defun  Move backward to the beginning of a defun.                         
   C-M-b            backward-sexp       Move backward across one balanced expression (sexp).               
   C-M-d            down-list           Move forward down one level of parentheses.                        
   C-M-e            end-of-defun        Move forward to next end of defun.                                 
   C-M-f            forward-sexp        Move forward across one balanced expression (sexp).                
   C-M-h            mark-defun          Put mark at end of this defun, point at beginning.                 
   C-M-k            kill-sexp           Kill the sexp (balanced expression) following point.               
   C-M-n            forward-list        Move forward across one balanced group of parentheses.             
   C-M-p            backward-list       Move backward across one balanced group of parentheses.            
   C-M-t            transpose-sexps     Like C-t (‘transpose-chars’), but applies to sexps.                
   C-M-u            backward-up-list    Move backward out of one level of parentheses.                     
   C-M-<backspace>  backward-kill-sexp  Kill the sexp (balanced expression) preceding point.               
   -                up-list             Move forward out of one level of parentheses.                      
   -                delete-pair         Delete a pair of characters enclosing ARG sexps that follow point. 
   -                raise-sexp          Raise N sexps one level higher up the tree.                        
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  This package provides analogous commands adapted for tex buffers, with
  the class of parenthesis-like delimiters (namely, parentheses `()',
  brackets `[]' and braces `{}') expanded to include the following:
  • `\begin{...}' - `\end{...}' pairs
  • math environment delimiters `$...$', `\(...\)', `\[...\]', `$$...$$'
  • mathematical delimiters: parentheses, brackets, braces, `\langle' -
    `\rangle', `\lvert' - `\rvert', etc., and compositions of these with
    modifiers such as `\left' - `\right', `\Bigg', etc.  See `M-x
    customize-group tex-parens' for details.

  There is some support in this direction in the built-in [tex-mode], in
  [AUCTeX] and in [latex-extra].  There are many packages, such as
  [paredit] and [lispy], that add further useful commands and bindings
  to lisp modes, and many other packages, such as [smartparens,] [puni]
  and [paredit-everywhere,] that aim to give consistent
  parenthesis-based commands across all modes.  There's also the
  [evil-tex] package for those that prefer a modal setup.  I was unable
  to get these packages to behave in the desired manner.  Smartparens
  comes close, but doesn't seem to support arbitrary begin/end pairs,
  and has some issues when one delimiter is a prefix of another
  (<https://github.com/Fuco1/smartparens/issues/1193>).  This package
  should work out-of-the-box and behave in tex buffers just like stock
  Emacs does in lisp buffers.


[Expressions]
<https://www.gnu.org/software/emacs/manual/html_node/emacs/Expressions.html>

[Defuns]
<https://www.gnu.org/software/emacs/manual/html_node/emacs/Defuns.html>

[tex-mode]
<https://www.gnu.org/software/emacs/manual/html_node/emacs/TeX-Mode.html>

[AUCTeX] <https://www.gnu.org/software/auctex/>

[latex-extra] <https://github.com/Malabarba/latex-extra>

[paredit] <https://paredit.org/>

[lispy] <https://github.com/abo-abo/lispy>

[smartparens,] <https://github.com/Fuco1/smartparens>

[puni] <https://github.com/AmaiKinono/puni>

[paredit-everywhere,] <https://github.com/purcell/paredit-everywhere>

[evil-tex] <https://github.com/iyefrat/evil-tex>


2 Configuration
═══════════════

  This package is available via GNU ELPA, so you can install it using
  `package-install' or `list-packages'.  You can activate it via `M-x
  tex-parens-mode', which remaps binds for the commands listed above to
  their `tex-parens-*' counterparts.

  To activate automatically, add the following lines to your config (the
  first hook applies if use the built-in [tex-mode], the second if you
  use [AUCTeX]):
  ┌────
  │ (use-package tex-parens
  │   :hook
  │   (tex-mode . tex-parens-mode)
  │   (TeX-mode . tex-parens-mode))
  └────

  For finer control over how keys are bound, you can avoid
  `tex-parens-mode' and instead use something like the following:
  ┌────
  │ (use-package tex-parens
  │   :bind
  │   (:map LaTeX-mode-map
  │ 	("C-M-f" . tex-parens-forward-sexp)
  │ 	("C-M-b" . tex-parens-backward-sexp)
  │ 	("C-M-n" . tex-parens-forward-list)
  │ 	("C-M-p" . tex-parens-backward-list)
  │ 	("C-M-u" . tex-parens-backward-up-list)
  │ 	("M-u" . tex-parens-up-list)
  │ 	("C-M-g" . tex-parens-down-list)
  │ 	("M-_" . tex-parens-delete-pair)
  │ 	("C-M-SPC" . tex-parens-mark-sexp)
  │ 	("C-M-k" . tex-parens-kill-sexp)
  │ 	("C-M-t" . tex-parens-transpose-sexps)
  │ 	("C-M-<backspace>" . tex-parens-backward-kill-sexp)
  │ 	("M-+" . tex-parens-raise-sexp))
  │   :hook
  │   (tex-mode . tex-parens-setup)
  │   (TeX-mode . tex-parens-setup))
  └────

  The precise use-package declaration that I use may be found in [my
  config].

  Use `M-x customize-group tex-parens' to configure further.  If you
  tweak the customization variables concerning delimiters and modifiers,
  then you'll need to reload your tex file, reset the minor mode, or do
  `M-: (tex-parens-setup)' for the changes to take effect.


[tex-mode]
<https://www.gnu.org/software/emacs/manual/html_node/emacs/TeX-Mode.html>

[AUCTeX] <https://www.gnu.org/software/auctex/>

[my config] <https://github.com/ultronozm/emacsd/blob/main/init.el>


3 Variants
══════════

  This package contains the following additional functions defined in
  terms of the sexp/list primitives:

  • `tex-parens-burp-left', `tex-parens-burp-right': Slurp or barf sexps
    in and out of the current list.
  • `tex-parens-mark-inner': Mark the innermost balanced group around
    point.
  • `tex-parens-beginning-of-list', `tex-parens-end-of-list': Move to
    the beginning or end of the current balanced group.
  • `tex-parens-kill-to-end-of-list',
    `tex-parens-kill-to-beginning-of-list': Kill text between point and
    the end or beginning of the current list.

  See the `C-h f' documentation for each function for more details.
  They may be bound in the same manner as described above.


3.1 Avy Integration
───────────────────

  There are additional commands that use Avy to jump to and copy math
  expressions:

  • `tex-parens-avy-jump-to-math': Jump to and enter a math expression
    using Avy.
  • `tex-parens-avy-copy-math': Copy a math expression selected using
    Avy.

  To use these commands, make sure you have Avy installed and loaded,
  then bind them in your config as described above.


3.2 Adjusting Delimiter Size
────────────────────────────

  We provide the commands `tex-parens-increase-delimiter-size' and
  `tex-parens-decrease-delimiter-size' for adjusting delimiter sizes:
  `(...)' -> `\left(...\right)' -> `\bigl(...\bigr)' -> `...'.  These
  operate on the left delimiter at point.  It may be convenient to put
  them in a [repeat map], which can be achieved by adding something like
  the following to your `use-package' declaration:

  ┌────
  │ (use-package tex-parens
  │   ;; ...
  │   :bind
  │   (:map
  │    LaTeX-mode-map
  │    ;; ...
  │    ("C-c p =" . tex-parens-increase-delimiter-size)
  │    ("C-c p -" . tex-parens-decrease-delimiter-size))
  │   (:repeat-map
  │    tex-parens-delimiter-size-repeat-map
  │    ("=" . tex-parens-increase-delimiter-size)
  │    ("-" . tex-parens-decrease-delimiter-size))
  │   ;; ...
  │   :config
  │   ;; ...
  │   (repeat-mode 1))
  └────


[repeat map]
<https://www.gnu.org/software/emacs/manual/html_node/use-package/Binding-to-repeat_002dmaps.html>
