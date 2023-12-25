Enables you to customise the mode names displayed in the mode line.

For major modes, the buffer-local `mode-name' variable is modified.
For minor modes, the associated value in `minor-mode-alist' is set.

Example usage:

  ;; Delighting a single mode at a time:
  (require 'delight)
  (delight 'abbrev-mode " Abv" "abbrev")
  (delight 'rainbow-mode)

  ;; Delighting multiple modes together:
  (require 'delight)
  (delight '((abbrev-mode " Abv" "abbrev")
             (smart-tab-mode " \\t" "smart-tab")
             (eldoc-mode nil "eldoc")
             (rainbow-mode)
             (overwrite-mode " Ov" t)
             (emacs-lisp-mode "Elisp" :major)))

The first argument is the mode symbol.

The second argument is the replacement name to use in the mode line
(or nil to hide it).

The third argument is either the keyword :major for major modes or,
for minor modes, the library which defines the mode.  This is passed
to `eval-after-load' and so should be either the name (as a string)
of the library file which defines the mode, or the feature (symbol)
provided by that library.  If this argument is nil, the mode symbol
will be passed as the feature.  If this argument is either t or 'emacs
then it is assumed that the mode is already loaded (you can use this
with standard minor modes that are pre-loaded by default when Emacs
starts).

In the above example, `rainbow-mode' is the symbol for both the minor
mode and the feature which provides it, and its lighter text will be
hidden from the mode line.

To determine which library defines a mode, use e.g.: C-h f eldoc-mode.
The name of the library is displayed in the first paragraph, with an
".el" suffix (in this example it displays "eldoc.el", and therefore we
could use the value "eldoc" for the library).

If you simply cannot figure out which library to specify, an
alternative approach is to evaluate (delight 'something-mode nil t)
once you know for sure that the mode has already been loaded, perhaps
by using the mode hook for that mode.

If all else fails, it's worth looking at C-h v minor-mode-alist
(after enabling the minor mode in question).  There are rare cases
where the entry in `minor-mode-alist' has a different symbol to the
minor mode with which it is associated, and in these situations you
will need to specify the name in the alist, rather than the name of
the mode itself.  Known examples (and how to delight them) are:

`auto-fill-mode':  (delight 'auto-fill-function " AF" t)
`server-mode':  (delight 'server-buffer-clients " SV" 'server)

* Important notes:

Although strings are common, any mode line construct is permitted as
the value (for both minor and major modes); so before you override a
value you should check the existing one, as you may want to replicate
any structural elements in your replacement if it turns out not to be
a simple string.

For major modes, M-: mode-name
For minor modes, M-: (cadr (assq 'MODE minor-mode-alist))
for the minor MODE in question.

Conversely, you may incorporate additional mode line constructs in
your replacement values, if you so wish. e.g.:

  (delight 'emacs-lisp-mode
           '("Elisp" (lexical-binding ":Lex" ":Dyn"))
           :major)

See `mode-line-format' for information about mode line constructs, and
M-: (info "(elisp) Mode Line Format") for further details.

Settings for minor modes are held in a global variable and tend to take
immediate effect upon calling ‘delight’.  Major mode names are held in
buffer-local variables, however, so changes to these will not take
effect in a given buffer unless the major mode is called again, or the
buffer is reverted.  Calling M-x normal-mode is sufficient in most
cases.

Also bear in mind that some modes may dynamically update these values
themselves (for instance dired-mode updates mode-name if you change the
sorting criteria) in which cases this library may prove inadequate.

Some modes also implement direct support for customizing these values;
so if delight is not sufficient for a particular mode, be sure to check
whether the library in question provides its own way of doing this.

* Conflict with `c-mode' and related major modes:

Major modes based on cc-mode.el (including ‘c-mode’, ‘c++-mode’, and
derivatives such as ‘php-mode’) cannot be delighted, due to Emacs bug
#2034: https://debbugs.gnu.org/cgi/bugreport.cgi?bug=2034

cc-mode.el assumes that ‘mode-name’ is always a string (which was true
in Emacs 22 and earlier), while delight.el makes use of the fact that
‘mode-name’ can (since Emacs 23) contain any mode line construct.  The
two are therefore incompatible.

The symptom of this conflict is the following error (where the "..."
varies):

  (wrong-type-argument stringp (delight-mode-name-inhibit ...))

The conflicting function is ‘c-update-modeline’ which adds the various
suffix characters documented at M-: (info "(ccmode) Minor Modes").
(E.g. In the mode line of a ‘c-mode’ buffer, the name C might be
changed to "C/*l" or similar, depending on the minor modes.)

If you are willing (or indeed wishing) to eliminate those suffixes
entirely for all relevant major modes, then you can work around this
conflict between the two libraries by disabling ‘c-update-modeline’
entirely, like so:

  (advice-add 'c-update-modeline :override #'ignore)

* Integration with mode line replacement libraries:

Libraries which replace the standard mode line are liable to conflict
with delight's treatment of major modes, as such libraries invariably
need to call `format-mode-line', which otherwise happens only in
circumstances in which delight wishes to show the original mode-name.

These libraries (or custom advice) can prevent this by let-binding
`delight-mode-name-inhibit' to nil around calls to `format-mode-line'
which will ensure that the delighted `mode-name' is displayed.

* Configuration via use-package:

The popular `use-package' macro supports delight.el so you can also
delight modes as part of your package configurations.  See its README
file for details.