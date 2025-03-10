                   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
                    POPPER: POPUP BUFFERS FOR EMACS
                   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━


Popper is a minor-mode to tame the flood of ephemeral windows Emacs
produces, while still keeping them within arm's reach.

Designate any buffer to "popup" status, and it will stay out of your
way.  Disimss or summon it easily with one key. Cycle through all your
"popups" or just the ones relevant to your current buffer. Group popups
automatically so you're presented with the most relevant ones. Useful
for many things, including toggling display of REPLs, documentation,
compilation or shell output: any buffer you need instant access to but
want kept out of your way!

There is a [detailed demo of Popper here]. [Note (10/2021): This demo is
quite out of date at this point but covers the basics.]

You can pre-designate any buffer (by name or major-mode) as a popup, and
the status will be automatically applied when Emacs creates it.

By default, your popups are displayed in a non-obtrusive way, but Popper
respects window rules for buffers that you might have in
`display-buffer-alist' or created using a window management package like
`shackle.el'. Popper summons windows defined by the user as "popups" by
simply calling `display-buffer'.


[detailed demo of Popper here]
<https://www.youtube.com/watch?v=E-xUNlZi3rI>


0.0.1 Toggle a popup:
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Here I toggle a REPL for quick access.

  <https://user-images.githubusercontent.com/8607532/135746327-c400aaf9-4aa1-4b6e-8b0a-0dd58c2690bb.mp4>


0.0.2 Cycle through all your popups:
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Here I cycle through all "popup buffers" in quick succession. My popup
  buffers are the usual suspects: help buffers, REPLs, grep and occur
  buffers, shell and compilation output, log buffers etc.

  <https://user-images.githubusercontent.com/8607532/135746363-aa3c3a25-cc9d-4907-a85f-07ea0d764238.mp4>

  Note that popup buffers are indicated here by the marker "POP" in
  their modelines.


0.0.3 Or jump to them instantly with hinting
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  You can see your popups in the echo area and jump to them with a key.

  <https://user-images.githubusercontent.com/8607532/135746395-dfe3b3e8-9d5a-4309-b521-9555a34bb73d.mp4>


0.0.4 Group your popups according to context
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  With grouping turned on, I'm only shown the popups relevant to the
  current context (in this case the Popper project).

  <https://user-images.githubusercontent.com/8607532/135746404-d8673390-d220-46fe-9b57-9dc81458cecd.mp4>

  The context can be anything, see below. Projectile, Perspective and
  Project.el are supported out of the box.


0.0.5 Turn a regular window into a popup:
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  <https://user-images.githubusercontent.com/8607532/135746418-21d32c74-e1f1-48f3-ba19-792c7cb2a51a.mp4>

  Or promote a popup to regular window status.


0.0.6 Popper respects your display buffer settings
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  <https://user-images.githubusercontent.com/8607532/135746477-93f8fc3d-4806-4901-beae-904059584e72.mp4>

  And windows open the way you have specified them to: in reused
  windows, side windows, new or child frames, etc. All display-buffer
  actions are supported except for displaying in popups in new frames
  and in atomic windows.


0.0.7 … you can toggle all your popups at once:
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  <file:images/popper-toggle-all.png>


1 Usage
═══════

  Turn on `popper-mode'.

  • Turn any buffer into a popup (or vice-versa) with
    `popper-toggle-type'.

  There are two commands for displaying popups, you can bind them as
  convenient:

  • `popper-toggle': Show/hide the latest popup. Does more with prefix
    args.
  • `popper-cycle': Cycle through your popups in sequence.

  To automatically designate buffers as popups, see the customization
  section. Additionally, you can kill an open popup buffer with
  `popper-kill-latest-popup'.

  If you want the echo-area hints, turn on `popper-echo-mode'.


2 Setup
═══════

  `popper' is available on GNU ELPA, so you can install it with `M-x
  package-install RET popper RET'.


2.1 With `use-package'
──────────────────────

  ┌────
  │ (use-package popper
  │   :ensure t ; or :straight t
  │   :bind (("C-`"   . popper-toggle)
  │ 	 ("M-`"   . popper-cycle)
  │ 	 ("C-M-`" . popper-toggle-type))
  │   :init
  │   (setq popper-reference-buffers
  │ 	'("\\*Messages\\*"
  │ 	  "Output\\*$"
  │ 	  "\\*Async Shell Command\\*"
  │ 	  help-mode
  │ 	  compilation-mode))
  │   (popper-mode +1)
  │   (popper-echo-mode +1))                ; For echo area hints
  └────
  See the Customization section for details on specifying buffer types
  as popups.


2.2 Without `use-package'
─────────────────────────

  ┌────
  │ (require 'popper)
  │ (setq popper-reference-buffers
  │       '("\\*Messages\\*"
  │ 	"Output\\*$"
  │ 	"\\*Async Shell Command\\*"
  │ 	help-mode
  │ 	compilation-mode))
  │ (global-set-key (kbd "C-`") 'popper-toggle)  
  │ (global-set-key (kbd "M-`") 'popper-cycle)
  │ (global-set-key (kbd "C-M-`") 'popper-toggle-type)
  │ (popper-mode +1)
  │ 
  │ ;; For echo-area hints
  │ (require 'popper-echo)
  │ (popper-echo-mode +1)
  └────
  See the Customization section for details on specifying buffer types
  as popups.


3 Customization
═══════════════

  To get started, customize this variable:

  • `popper-reference-buffers': List of buffers to treat as popups. Each
    entry in the list can be a regexp (string) to match buffer names
    against or a major-mode (symbol) to match buffer major-modes
    against.

    Example:

    ┌────
    │ (setq popper-reference-buffers
    │       '("\\*Messages\\*"
    │ 	"Output\\*$"
    │ 	help-mode
    │ 	compilation-mode))
    └────

    Will treat the following as popups: The Messages buffer, any buffer
    ending in "Output*", and all help and compilation buffers.

    *Note: Because of how some shell buffers are initialized in Emacs,
     you may need to supply both the name and major mode to match them
     consistently*. Take your pick:

    ┌────
    │ ;; Match eshell, shell, term and/or vterm buffers
    │ (setq popper-reference-buffers
    │       (append popper-reference-buffers
    │ 	      '("^\\*eshell.*\\*$" eshell-mode ;eshell as a popup
    │ 		"^\\*shell.*\\*$"  shell-mode  ;shell as a popup
    │ 		"^\\*term.*\\*$"   term-mode   ;term as a popup
    │ 		"^\\*vterm.*\\*$"  vterm-mode  ;vterm as a popup
    │ 		)))
    └────

    As of v0.40, Popper also supports classifying a buffer as a popup
    based on any user supplied predicate. This predicate (function) is
    called with the buffer as argument and returns `t' if it should be
    considered a popup. Here is an example with a predicate:

    ┌────
    │ (setq popper-reference-buffers
    │       '("\\*Messages\\*"
    │ 	help-mode
    │ 	(lambda (buf) (with-current-buffer buf
    │ 		   (and (derived-mode-p 'fundamental-mode)
    │ 			(< (count-lines (point-min) (point-max))
    │ 			   10)))))))
    └────

    This list includes the the Messages and `help-mode' buffers from
    before, along with a predicate: any buffer derived from the major
    mode `fundamental-mode' that has fewer than 10 lines will be
    considered a popup.

    Note that for performance reasons, predicates that classify a buffer
    as a popup are /only run when the buffer is created/. Thus
    dynamically changing a buffer's popup status based on its changing
    state is not possible (yet).

    There are other customization options, check the `popper' group.

    Here is an example of how I use Popper:

  <https://user-images.githubusercontent.com/8607532/135748097-268f5aae-ad42-44fa-9435-b63b960d45cf.mp4>

  In this example:
  • Popup buffers have no modelines.
  • My popups are grouped by project, so I only see popups relevant to
    the current one.
  • I use the echo-area hints to select popups with the number keys.
  • These hints have their buffer names truncated so they're easier to
    read.
  • My popups show up in different ways on screen depending on my
    display-buffer settings: Help windows on the right, REPLs and
    command output at the bottom, grep buffers at the top etc.

    This section details these (and other) customization options.


3.1 Grouping popups by context
──────────────────────────────

  Popper can group popups by "context", so that the popups available for
  display are limited to those that are relevant to the context in which
  `popper-toggle' or `popper-cycle' is called. For example, when cycling
  popups from a project buffer, you may only want to see the popups
  (REPLs, help buffers and compilation output, say) that were spawned
  from buffers in that project. This is intended to approximate DWIM
  behavior, so that the most relevant popup in any context is never more
  than one command away.

  Built in contexts include projects as defined in Emacs' built in
  `project.el' and `projectile', using `perspective' names (from
  `persp.el'), as well as the default directory of a buffer. To set
  this, customize `popper-group-function' or use one of

  ┌────
  │ (setq popper-group-function #'popper-group-by-project) ; project.el projects
  │ 
  │ (setq popper-group-function #'popper-group-by-projectile) ; projectile projects
  │ 
  │ (setq popper-group-function #'popper-group-by-directory) ; group by project.el
  │ 							 ; project root, with
  │ 							 ; fall back to
  │ 							 ; default-directory
  │ (setq popper-group-function #'popper-group-by-perspective) ; group by perspective
  └────

  You can also provide a custom function that takes no arguments, is
  executed in the context of a popup buffer and returns a string or
  symbol that represents the group/context it belongs to. This function
  will group all popups under the symbol `my-popup-group':

  ┌────
  │ (defun popper-group-by-my-rule ()
  │   "This function should return a string or symbol that is the
  │ name of the group this buffer belongs to. It is called with each
  │ popup buffer as current, so you can use buffer-local variables."
  │ 
  │   'my-popup-group)
  │ 
  │ (setq popper-group-function #'popper-group-by-my-rule)
  └────


3.2 Managing popup placement
────────────────────────────

  In keeping with the principle of least surprise, all popups are shown
  in the same location: At the bottom of the frame. You can customize
  `popper-display-function' to change how popups are displayed.

  However this means you can't have more than one popup open at a
  time. You may also want more control over where individual popups
  appear. For example, you may want an IDE-like set-up, with all help
  windows open on the right, REPLs on top and compilation windows at the
  bottom. This is best done by customizing Emacs'
  `display-buffer-alist'. Since this is a [singularly confusing task], I
  recommend using `popper' with a package that locks window placements,
  /e.g./ [Shackle].


[singularly confusing task]
<https://www.gnu.org/software/emacs/manual/html_node/elisp/The-Zen-of-Buffer-Display.html#The-Zen-of-Buffer-Display>

[Shackle] <https://depp.brause.cc/shackle/>

3.2.1 Default popup placement:
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  ┌────
  │ (setq popper-display-control t)  ;This is the DEFAULT behavior
  └────
  You can customize `popper-display-function' to show popups any way
  you'd like.  Any `display-buffer' [action function] can work, or you
  can write your own. For example, setting it as
  ┌────
  │ (setq popper-display-function #'display-buffer-in-child-frame)
  └────
  will cause popups to be displayed in a child frame.


[action function]
<https://www.gnu.org/software/emacs/manual/html_node/elisp/Buffer-Display-Action-Functions.html>


3.2.2 Popup placement controlled using `display-buffer-alist' or `shackle.el':
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  If you already have rules in place for how various buffers should be
  displayed, such as by customizing `display-buffer-alist' or with
  `shackle.el', popper will respect them once you set
  `popper-display-control' to nil:

  ┌────
  │ (use-package shackle
  │  ;; -- shackle rules here --
  │  )
  │ 
  │ (use-package popper
  │ ;; -- popper customizations here--
  │ 
  │ :config
  │ (setq popper-display-control nil))
  └────


3.3 Suppressing popups
──────────────────────

  Popper can suppress popups when they are first created. The buffer
  will be registered in the list of popups but will not show up on your
  screen. Instead, a message ("Popup suppressed: $buffer-name") will be
  printed to the echo area. You can then raise it using `popper-toggle'
  or `popper-cycle' at your convenience. It behaves as a regular popup
  from that point on:

  <https://user-images.githubusercontent.com/8607532/132929265-37eee976-131f-4631-9bad-73090bf17231.mp4>

  This is generally useful to keep buffers that are created as a side
  effect from interrupting your work.

  To specify popups to auto-hide, use a cons cell with the `hide' symbol
  when specifying `popup-reference-buffers':

  ┌────
  │ (setq popper-reference-buffers
  │     '(("Output\\*$" . hide)
  │       (completion-list-mode . hide)
  │       occur-mode
  │       "\\*Messages\\*"))
  └────

  This assignment will suppress all buffers ending in `Output*' and the
  Completions buffer. The other entries are treated as normal popups.

  You can combine the hiding feature with predicates for classifying
  buffers as popups:

  ┌────
  │ (defun popper-shell-output-empty-p (buf)
  │   (and (string-match-p "\\*Async Shell Command\\*" (buffer-name buf))
  │        (= (buffer-size buf) 0)))
  │ 
  │ (add-to-list 'popper-reference-buffers
  │ 	     '(popper-shell-output-empty-p . hide))
  └────

  This assignment will suppress display of the async shell command
  output buffer, but only when there is no output (stdout). Once it is
  hidden it will be treated as a popup on par with other entries in
  `popper-reference-buffers'.


3.4 Mode line and Echo area customization
─────────────────────────────────────────

  • To change the modeline string used by Popper (the default is "POP"),
    customize `popper-mode-line'. You can disable the modeline entirely
    by setting it to nil.
  • You can change the keys used to access popups when using
    `popper-echo-mode' by customizing the `popper-echo-dispatch-keys'
    variable. To retain the display while removing the keymap, set this
    variable to `nil'.
  • You can change the number of minibuffer lines used for display by
    `popper-echo-mode' by customizing `popper-echo-lines'.
  • If you want to change the buffer names displayed in the echo area in
    some way (such as to color them by mode or truncate long names), you
    can customize the variable `popper-echo-transform-function'.


4 Alternatives
══════════════

  Packages like [Term Toggle] and [eshell toggle] give you an easy way
  to access a "dropdown" terminal. Popper can be used for this almost
  trivially, but it's a much more general solution for buffer management
  and access.

  Packages like [Shackle] help with specifying how certain buffers
  should be displayed, but don't give you an easy way to access them
  beyond calling display-buffer. Popper is mainly concerned with the
  latter and is thus more or less orthogonal to Shackle. Moreover, most
  window management packages for Emacs are opinionated in how windows
  should be displayed, or provide an additional API to customize this
  (e.g. [Popwin]). While Popper defaults to displaying popups a certain
  way, it tries to stay out of the business of display rules and focuses
  on providing one-key access to the buffers you're most likely to need
  next.


[Term Toggle] <https://github.com/amno1/emacs-term-toggle>

[eshell toggle] <https://github.com/4DA/eshell-toggle>

[Shackle] <https://depp.brause.cc/shackle/>

[Popwin] <https://github.com/emacsorphanage/popwin>


5 Technical notes
═════════════════

  `popper' uses a buffer local variable (`popper-popup-status') to
  identify if a given buffer should be treated as a popup. Matching is
  always by buffer and not window, so having two windows of a buffer,
  one treated as a popup and one as a regular window, isn't possible
  (although you can do this with indirect clones). In addition, it
  maintains an alist of popup windows/buffers for cycling through.

  By default, it installs a single rule in `display-buffer-alist' to
  handle displaying popups. If `popper-display-control' is set to `nil',
  this rule is ignored. You can change how the popups are shown by
  customizing `popper-display-function', the function used by
  `display-buffer' to display popups, although you are better off
  customizing `display-buffer-alist' directly or using Shackle.
