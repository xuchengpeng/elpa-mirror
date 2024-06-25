           ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
             EMACS MAJOR-MODE TO WORK WITH BEANCOUNT LEDGER
                                 FILES
           ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━


This package provides `beancount-mode' an Emacs [major-mode]
implementing syntax highlighting, indentation, completion , and other
facilities to edit and work with Beancount ledger files.

To instruct Emacs to activate `beancount-mode' when opening files with a
`.beancount' extension, you can add this code to your Emacs
configuration, typically in the `~/.emacs.d/init.el' file:

┌────
│ (add-to-list 'load-path "/path/to/beancount-mode/")
│ (require 'beancount)
│ (add-to-list 'auto-mode-alist '("\\.beancount\\'" . beancount-mode))
└────

Most facilities commonly provided by Emacs major modes are implemented
by `beancount-mode'. Documentation on the provided functionality and on
the default keybindings can be obtained with the `describe-mode' command
in a buffer with `beancount-mode' active.

In a nutshell, when `beancount-mode' is active:

• The "TAB" key either indents, completes, or folds the heading at
  point, depending on the context.

• Amounts in postings are indented so that the decimal point is at the
  `beancount-number-alignment-column' column. Setting this variable to 0
  will cause the alignment column to be determined from file content.

• Postings in transactions, as well as metadata, links, and tags
  following directives, and are indented with
  `beancount-transaction-indent' spaces.

• Pressing the "RET" key causes the current line to be automatically
  indented. If the current line is a posting, the amount will be
  indented as described above.

The automatic indentation behavior is defined by Emacs auto indent
mechanism, however, it can be surprising or undesired. It can be
disabled setting `electric-indent-chars' to `nil' after loading
`beancount-mode', for example like this:

┌────
│ (add-hook 'beancount-mode-hook
│   (lambda () (setq-local electric-indent-chars nil)))
└────

Beancount ledger files can grow very large. It is thus often practical
to structure them in sections and subsections. To support this,
`beancount-mode' leverages [Outline minor-mode] to enable navigation of
the document structure to fold and unfold the document sections,
similarly to what is possible in [Org mode]. Lines starting with one
asterisks "`*'" or three or more semicolons "`;;;'" are interpreted as
section headings. In Beancount, the semicolon starts a comment and all
lines starting with an asterisks are ignored. The number of semicolons
or asterisks determines the heading level.

To enable this functionality, `outline-minor-mode' should be explicitly
activated. It is possible to do so automatically when `beancout-mode' is
activated:

┌────
│ (add-hook 'beancount-mode-hook #'outline-minor-mode)
└────

Outline minor mode uses a rather peculiar choice of keybindings. It is
possible to map the most used functionality to keys more familiar to
`org-mode' users adding a few lines to the Emacs configuration:

┌────
│ (define-key beancount-mode-map (kbd "C-c C-n") #'outline-next-visible-heading)
│ (define-key beancount-mode-map (kbd "C-c C-p") #'outline-previous-visible-heading)
└────

Alternatively the keybindings for `outline-minor-mode' can be globally
remapped. Please refer to the `outline-minor-mode' documentation in the
Emacs manual for more details.

You can enable on-the-fly checks on your ledger file using `bean-check'
via flymake:

┌────
│ (add-hook 'beancount-mode-hook #'flymake-bean-check-enable)
└────

The `etc/emacsrc' file contains some example configuration for
`beancount-mode' and some experiments that may find their way into the
main codebase.


[major-mode]
<https://www.gnu.org/software/emacs/manual/html_node/emacs/Major-Modes.html>

[Outline minor-mode]
<https://www.gnu.org/software/emacs/manual/html_node/emacs/Outline-Mode.html>

[Org mode] <https://orgmode.org/>


1 Keybinding compatibility
══════════════════════════

  In mid-2023 the default keybindings for many commands in
  `beancount-mode' were changed to become compliant with the [Emacs
  keybinding conventions].  However, if you are accustomed to the old
  keybindings and would prefer to continue using them, just put this
  into your Emacs configuration before `beancount.el' is loaded:

  ┌────
  │ (setq beancount-mode-old-style-keybindings t)
  └────


[Emacs keybinding conventions]
<https://www.gnu.org/software/emacs/manual/html_node/elisp/Key-Binding-Conventions.html>
