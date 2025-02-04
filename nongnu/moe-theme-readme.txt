1 moe-theme
═══════════

  <a
  href="<https://raw.github.com/kuanyui/moe-theme.el/master/pics/moe-theme.png>"><img
  src="<https://raw.github.com/kuanyui/moe-theme.el/master/pics/moe-theme.png>"
  width="720" height="401"/></a>


1.1 Screenshot
──────────────

  <a
  href="<https://raw.github.com/kuanyui/moe-theme.el/master/pics/dark01.png>"><img
  src="pics/dark01.png" width="355" height="192"/></a> <a
  href="<https://raw.github.com/kuanyui/moe-theme.el/master/pics/light01.png>"><img
  src="pics/light01.png" width="355" height="192"/></a> <a
  href="<https://raw.github.com/kuanyui/moe-theme.el/master/pics/dark02.png>"><img
  src="pics/dark02.png" width="355" height="192"/></a> <a
  href="<https://raw.github.com/kuanyui/moe-theme.el/master/pics/light02.png>"><img
  src="pics/light02.png" width="355" height="192"/></a> <a
  href="<https://raw.github.com/kuanyui/moe-theme.el/master/pics/dark03.png>"><img
  src="pics/dark03.png" width="355" height="192"/></a> <a
  href="<https://raw.github.com/kuanyui/moe-theme.el/master/pics/light03.png>"><img
  src="pics/light03.png" width="355" height="192"/></a> <a
  href="<https://raw.github.com/kuanyui/moe-theme.el/master/pics/dark04.png>"><img
  src="pics/dark04.png" width="355" height="192"/></a> <a
  href="<https://raw.github.com/kuanyui/moe-theme.el/master/pics/light04.png>"><img
  src="pics/light04.png" width="355" height="192"/></a> <a
  href="<https://raw.github.com/kuanyui/moe-theme.el/master/pics/dark05.png>"><img
  src="pics/dark05.png" width="355" height="192"/></a> <a
  href="<https://raw.github.com/kuanyui/moe-theme.el/master/pics/light05.png>"><img
  src="pics/light05.png" width="355" height="192"/></a> <a
  href="<https://raw.github.com/kuanyui/moe-theme.el/master/pics/mode-line-preview.png>"><img
  src="pics/mode-line-preview.png" width="710" height="182"/></a>


1.2 What Special?
─────────────────

  Most basic:
  1. Optimized for terminal's 256 color palettes.
  2. Dark & Light

  And more:
  1. Carefully-considered & reasonable colors
  2. Delightful & good-looking color palettes™
  3. Customizable
     • Optional `Monokai' / `Tomorrow' for syntax-highlighting (or
       totally customize by yourself)
     • Mode-line / Powerline color
     • Titles font sizes for .
  4. Fully-supported for each modes:
     • Diff / EDiff
     • Dired / Dired+
     • ERC / rcirc
     • Eshell / Ansi-term
     • Gnus / Message
     • Helm / ido
     • Org-mode / Agenda / calfw
     • Magit / Git-commit / Git-gutter
     • Markdown-mode / ReStructText-mode
     • Auto-complete-mode / Company
     • Rainbow-delimiters
     • Swoop
     • Twittering-mode
     • undo-tree / Neotree
     • Ruby / Haskell / CPerl / Tuareg / Web-mode
     • ……etc


1.3 Requirements
────────────────

  • Emacs 25.3 or above.
  • 256-colors (or higher) terminal.


1.4 Download
────────────

1.4.1 Via package.el
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  `Moe-theme' is available in [MELPA] repository now, so you can install
  `moe-theme' easily with `M-x' `list-packages'.


[MELPA] <https://github.com/milkypostman/melpa>


1.4.2 Manually
╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Download the archive of `moe-theme' (or `git clone' it) to
  `~/.emacs.d/moe-theme.el' and extract it. Then, add these to your init
  file:

  ┌────
  │ ;;customize theme
  │ (add-to-list 'custom-theme-load-path "~/.emacs.d/moe-theme.el/")
  │ (add-to-list 'load-path "~/.emacs.d/moe-theme.el/")
  │ (require 'moe-theme)
  └────


1.5 Customizations
──────────────────

  It's impossible to satisfy everyone with one fixed theme, but
  `moe-theme' provide some easy ways to customize itself.

  There's a full customization example:

  ┌────
  │ ;; If you want to use powerline, (require 'powerline) must be
  │ ;; before (require 'moe-theme).
  │ (add-to-list 'load-path "~/.emacs.d/PATH/TO/powerline/")
  │ (require 'powerline)
  │ 
  │ ;; Moe-theme
  │ (add-to-list 'custom-theme-load-path "~/.emacs.d/PATH/TO/moe-theme/")
  │ (add-to-list 'load-path "~/.emacs.d/PATH/TO/moe-theme/")
  │ (require 'moe-theme)
  │ 
  │ ;; Show highlighted buffer-id as decoration. (Default: nil)
  │ (setq moe-theme-highlight-buffer-id t)
  │ 
  │ ;; Resize titles (optional).
  │ (setq moe-theme-resize-title-markdown '(1.5 1.4 1.3 1.2 1.0 1.0))
  │ (setq moe-theme-resize-title-org '(1.5 1.4 1.3 1.2 1.1 1.0 1.0 1.0 1.0))
  │ (setq moe-theme-resize-title-rst '(1.5 1.4 1.3 1.2 1.1 1.0))
  │ 
  │ ;; Choose a color for modeline.(Default: blue)
  │ (setq moe-theme-modeline-color 'cyan)
  │ 
  │ ;; Finally, apply moe-theme now.
  │ ;; Choose what you like, (moe-light) or (moe-dark)
  │ (moe-light)
  └────

  If you have any question about settings, go on and read following
  README to get more detailed information first.

        **** Note

        *Notice that the file `moe-theme.el' is NOT a theme file,
        but it provide the ability for customization
        `moe-dark-theme' & `moe-light-theme'.*

        So, if you just want to use `load-theme' to apply *ONLY*
        `moe-theme' itself and *without customizations*, you can
        skip "Customizations" chapter and just use this:

        ┌────
        │ (add-to-list 'custom-theme-load-path "~/.emacs.d/PATH/TO/moe-theme/")
        │ 
        │ (load-theme 'moe-dark t)
        │ ;;or
        │ (load-theme 'moe-light t)
        └────


1.5.1 Resize Titles
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  You may want to resize titles in `markdown-mode', `org-mode', or
  `ReStructuredText-mode':

  ┌────
  │ ;; Resize titles
  │ (setq moe-theme-resize-title-markdown '(2.0 1.7 1.5 1.3 1.0 1.0))
  │ (setq moe-theme-resize-title-org '(2.2 1.8 1.6 1.4 1.2 1.0 1.0 1.0 1.0))
  │ (setq moe-theme-resize-title-rst '(2.0 1.7 1.5 1.3 1.1 1.0))
  └────

        Markdown should have 6 items; org has 9 items; rst has 6
        items.

        Make sure that these resizing settings should be placed
        *before* `(moe-dark)' or `(moe-light)'.

  The values should be lists. Larger the values, larger the fonts. If
  you don't like this, just leave them nil, and all the titles will be
  the same size.


1.5.2 Change Color of Mode-line (or Powerline)
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  ┌────
  │ (setq moe-theme-modeline-color 'orange)
  │ ;; (Available colors: blue, orange, green ,magenta, yellow, purple, red, cyan, w/b.)
  └────

  You can also use `M-x' `moe-theme-modeline-select-color' to change
  color interactively.

  Or `M-x' `moe-theme-modeline-random-color' to have a good luck.


◊ 1.5.2.1 Powerline support

  Now `moe-theme' supports [Powerline]. Run `powerline-moe-theme' if
  `powerline' installed.

  ┌────
  │ (powerline-moe-theme)
  └────


  [Powerline] <https://github.com/milkypostman/powerline>


1.5.3 Switch between light and dark theme by time of daylight
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  This package also contains some rudimentary functionality for swapping
  between the `moe-light' and `moe-dark' themes automatically, to match
  the time of day. To enable it, you can use the following:

  ┌────
  │ (require 'moe-theme-switcher)
  │ (setq calendar-latitude +25
  │       calendar-longitude +121
  │       moe-theme-switch-by-sunrise-and-sunset t)
  │ (moe-theme-switcher-mode 1)
  └────

  For packages that provides more sophisticated switching functionality,
  see [circadian.el] or [theme-changer].


[circadian.el] <https://github.com/guidoschmidt/circadian.el>

[theme-changer] <https://github.com/hadronzoo/theme-changer>


1.6 Frenquently Asked Problems
──────────────────────────────

1.6.1 No 256-Color Output?
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  If your terminal emulator doesn't render 256-color output correctly,
  set its environment variable `TERM' to `xterm-256color'. For example:

  • If you are using `bash' or `zsh', add following line into your
    `~/.bashrc' or `~/.zshrc':

    ┌────
    │ export TERM=xterm-256color
    └────

  • Or if you are using `Konsole', navigate to `Edit Current Profile
    General > Environment > Edit' and add the following line:

    ┌────
    │ TERM=xterm-256color
    └────

  • If you're using `tmux' and it cannot display in 256-color correctly,
    add this to `~/.tmux.conf', too:

    ┌────
    │ set -g default-terminal "screen-256color"
    └────


1.6.2 Parenthesis Is Hard To Read?
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  I recommend set the value of `show-paren-style' to `expression' for
  better visual experience:

  ┌────
  │ (show-paren-mode t)
  │ (setq show-paren-style 'expression)
  └────


1.7 Changelog
─────────────

  Notable changes from earlier versions of `moe-theme':


1.7.1 v1.1.0
╌╌╌╌╌╌╌╌╌╌╌╌

  • `moe-theme-switcher' is now a minor mode

    In previous versions, `moe-theme-switcher' was enabled automatically
    by just loading the module. As it has now been turned into a minor
    mode, users will need to update their configuration to enable this
    mode. See [this section] for details on how to do this.


[this section] See section 1.5.3


1.8 Known Issues
────────────────

  • If you add `(moe-dark)' or `(moe-light)' to your init file, the
    color of `buffer-id' would be incorrect after startuping CLI
    Emacs(but if you `M-x moe-dark/light' again, it would be corrected
    immediately). I don't know why, but this issue doesn't occur in GUI
    version Emacs.  (Tested on GNU Emacs 24.3.90.1 2014-04-11)
  • When using `moe-light' and typing characters under terminal emulator
    (e.g. Konsole) with IM (e.g. fcitx), the string embedded in Emacs
    may be very insignificant (But as you output the word from IM, it
    turns normal).


1.9 License
───────────

  `moe-theme.el' (include images) is released under GPL v3.
