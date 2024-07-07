
This package extends and configures `dired' with features found in
almost all 'file managers', and also some unique features:

  * Resilient dedicated dual-pane frame.
    * similar look to 'midnight commander'.
    * intelligent recovery of manually altered frame configuration
    * exit diredc/dired cleanly and totally
  * Navigable directory history
    * backward, forward, or to a direct history entry
  * File quick-preview mode
    * inspired by, and similar to, midnight commander's "C-x q"
    * customizable exclusion criteria to suppress undesirable files
      (eg. binaries)
    * optionally view magit status buffers for repository roots
  * Current file's supplemental information in minibuffer (optional)
    * eg. output from 'getfattr', 'getfacl', 'stat', 'exif'.
  * Multiple panel views
    * inspired by, and similar to, midnight commander's "M-t"
      * superior configurability
      * directly choose a specific panel view, or toggle to next
  * Extensive and easy-to-use sort options
    * including options not in 'ls': sort by chmod, owner, group
  * Swap panels (use "M-u")
    * inspired by, and similar to, midnight commander's "C-u"
      * a TRUE and complete swap (including history entries)
  * Trash management
    * per xfreedesktop standard
    * restore trashed files to their original locations
    * empty the trash, along with its administrative overhead
    * view trash summary information
  * Navigate 'up' n parent directories
  * Launch persistent asynchronous processes for files
    * Processes will survive even after exiting Emacs.
  * Quick shell window
    * choose your default shell / terminal emulation mode
    * choose your default shell program
    * easily opt for pre-configured alternatives
    * useful pre-defined shell variables
      * $d1, $d2  dired-directory in this/other pane
      * $f1, $f2  current dired file in this/other pane
      * $t1, $t2  tagged elements in this other pane
        * as a shell array variable, if supported by the shell
      * $INSIDE_DIREDC  value of variable 'diredc--version'
  * Bookmark support
  * Edit dired buffers (really `wdired-mode', not `diredc')
  * Set both panels to same directory (use "=" or "C-u =")
    * inspired by 'midnight commander's "M-i"
  * Fontify filenames based upon their names or extensions
    * fontify 'executable' suffix symbol
  * Optional "drilled-down" view of "sparse" paths (use "}", "{")
    * ie. ./paths/with/only/single/entries
    * Uses a 'diredc'-patched version of external package
      'dired-collapse' (https://github.com/Fuco1/dired-hacks)
      pending pull-request merges.

Bonus customization features
  * Customize colors for chmod bits (font-lock)
  * toggle display of "hidden" or "undesirable" files (dired-omit mode)
  * highlight current line (hl-line-mode)
    * current buffer highlights with a unique face.
  * don't wrap long lines (toggle-truncate-lines)
  * to disable:
    * option 1: M-x customize-variable diredc-bonus-configuration
    * option 2: (setq diredc-bonus-configuration nil)

