                            ━━━━━━━━━━━━━━━━
                             COMINT-MIME.EL
                            ━━━━━━━━━━━━━━━━


This Emacs package provides a mechanism for REPLs (or comint buffers, in
Emacs parlance) to display graphics and other types of special content.

<https://raw.githubusercontent.com/astoff/comint-mime/images/python-shell.png>

The main motivation behind this package is to display plots in the
Python shell.  However, it does more than that.

First, it is not constrained to graphics, and can display other "MIME
attachments" such as HTML and LaTeX content.  In fact, the Python
backend of the package implements IPython's [rich display interface],
which includes, among other things, special treatment of dataframes and
symbolic math expressions.  You can even define rich representations for
your own classes.

Second, the package defines a flexible communication protocol between
Emacs and the inferior process, and, consequently, can be extended to
other Comint types.  Currently, besides Python, there is support for the
regular (Unix) shell.  In this case, a special command, `mimecat', is
provided to display content.  Again, this works for images, HTML, LaTeX
snippets, etc.

<https://raw.githubusercontent.com/astoff/comint-mime/images/shell.png>


[rich display interface]
<https://ipython.readthedocs.io/en/stable/config/integrating.html#rich-display>


1 Usage
═══════

  To start enjoying comint-mime, simply call `M-x comint-mime-setup'
  from a supported buffer (which, at the moment, are the `M-x shell' and
  `M-x run-python' buffers). To apply this permanently, add that same
  function to the appropriate mode hook:

  ┌────
  │ (add-hook 'shell-mode-hook 'comint-mime-setup)
  │ (add-hook 'inferior-python-mode-hook 'comint-mime-setup)
  └────

  For Python it is recommended to use the IPython interpreter.  It can
  be configured to have the same look-and-feel as the classic `python'
  program as follows.

  ┌────
  │ (when (executable-find "ipython3")
  │   (setq python-shell-interpreter "ipython3"
  │ 	python-shell-interpreter-args "--simple-prompt --classic"))
  └────

  If using the regular `python' interpreter, only Matplotlib figures are
  supported, and you need to call `plt.show()' and `plt.close()'
  manually to display and create new figures.

  *Advertisement:* For an even more featureful Python shell with
  improved completion, graphic capabilities and more, have a look at the
  [dREPL] package.


[dREPL] <http://elpa.gnu.org/packages/drepl.html>


2 Customization
═══════════════

  The following options are available:

  • `comint-mime-use-mathjax': Whether to use the [mathjax] package to
    render LaTeX fragments.  It is true by default, as long as you have
    the necessary external prerequisites installed (namely, the Node
    JavaScript runtime.)

  • `comint-mime-use-vtable': Whether to use the [vtable] package when
    displaying HTML tables.  This improves alignment and makes the
    tables navigable and sortable.  This option is enabled by default if
    vtable is available (Emacs 29 or newer).


[mathjax] <https://elpa.gnu.org/packages/mathjax.html>

[vtable]
<https://www.gnu.org/software/emacs/manual/html_mono/vtable.html>


3 Extending
═══════════

  To add support for new MIME types, see `comint-mime-renderer-alist'.

  To add support for new Comints, an entry should be added to
  `comint-mime-setup-function-alist'. This function should arrange for
  the inferior process to emit an escape sequence whenever some MIME
  content is to be displayed.

  The escape sequence has the following shape:

  ┌────
  │ ESC ] 5 1 5 1 ; header LF payload ESC \
  └────

  Here, `header' is a JSON object containing, at least, the entry
  `type', which should be the name of a MIME type. Other header entries
  can be passed; the interpretation is up to the rendering function.

  The `payload' can be either the content of the attachment, encoded in
  base64 (which is decoded before being passed to the selected
  renderer), or a `file://' URL (whose content is read and passed to the
  renderer), or yet a `tmpfile://' URL, which indicates that the file
  should be deleted after it is read.

  Note that it can take considerable time to insert large amounts of
  data in a Comint buffer, specially if it contains long lines. Consider
  using a temporary file for large data transfers.


4 Contributing
══════════════

  Discussions, suggestions and code contributions are welcome! Since
  this package is part of GNU ELPA, contributions require a copyright
  assignment to the FSF.
