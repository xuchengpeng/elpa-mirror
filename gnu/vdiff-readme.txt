[[https://melpa.org/#/vdiff][file:https://melpa.org/packages/vdiff-badge.svg]] [[evil-vdiff-test][https://github.com/justbur/emacs-vdiff/workflows/evil-vdiff-test/badge.svg]]

* vdiff

A tool like vimdiff for Emacs 

** Table of Contents                                                    :TOC:
- [[#vdiff][vdiff]]
  - [[#introduction][Introduction]]
  - [[#recent-significant-changes][Recent (Significant) Changes]]
  - [[#screenshot][Screenshot]]
  - [[#installation-and-usage][Installation and Usage]]
  - [[#hydra][Hydra]]
  - [[#further-customization][Further customization]]

** Introduction

   vdiff compares two or three buffers on the basis of the output from the diff
   tool. The buffers are kept synchronized so that as you move through one of
   the buffers the top of the active buffer aligns with the corresponding top of
   the other buffer(s). This is similar to how ediff works, but in ediff you use
   a third "control buffer" to move through the diffed buffers. The key
   difference is that in vdiff you are meant to actively edit one of the buffers
   and the display will update automatically for the other buffer. Similar to
   ediff, vdiff provides commands to "send" and "receive" hunks from one buffer
   to the other as well as commands to traverse the diff hunks, which are useful
   if you are trying to merge changes. In contrast to ediff, vdiff also provides
   folding capabilities to fold sections of the buffers that don't contain
   changes. This folding occurs automatically. Finally, you are encouraged to
   bind a key to `vdiff-hydra/body', which will use hydra.el (in ELPA) to create
   a convenient transient keymap containing most of the useful vdiff commands.

   This functionality is all inspired by (but not equivalent to) the vimdiff
   tool from vim.

   Contributions and suggestions are very welcome.

** Recent (Significant) Changes
   - [2019-02-26] If the region is active when changes are sent to other
     buffers, only lines in the intersection of the region and any hunks are
     sent. This allows sending individual lines, similar to how individual lines
     can be staged in magit.
   - [2018-04-17] Add option to use various git diff algorithms. See
     =vdiff-diff-algorithm= for options.
   - [2017-05-17] Split =vdiff-magit.el= into [[https://github.com/justbur/emacs-vdiff-magit][separate repository]]. 
   - [2017-02-01] Added magit integration functions in =vdiff-magit.el=.
   - [2016-07-25] Added three-way diff support. See =vdiff-buffers3= and =vdiff-files3=.
   
** Screenshot

*** Basic two file diff with refined hunks
[[./img/leuven.png]]

*** Three file diff with targets for sending changes
[[./img/leuven3.png]]

** Installation and Usage
   
vdiff is available in MELPA, which is the recommended way to install it and keep
it up to date. To install it you may do =M-x package-install RET vdiff RET=.

To start a vdiff session, the main entry points are

| Command                | Description                                                       |
|------------------------+-------------------------------------------------------------------|
| =vdiff-buffers=        | Diff two open buffers                                             |
| =vdiff-files=          | Diff two files                                                    |
| =vdiff-buffers3=       | Diff three open buffers                                           |
| =vdiff-files3=         | Diff three files                                                  |
| =vdiff-current-file=   | Like =ediff-current-file= (Diff buffer with disk version of file) |
| =vdiff-merge-conflict= | Use vdiff to resolve merge conflicts in current file              |
   
After installing you can bind the commands to your preferred key prefix like this

#+BEGIN_SRC emacs-lisp
(require 'vdiff)
(define-key vdiff-mode-map (kbd "C-c") vdiff-mode-prefix-map)
#+END_SRC

which will bind most of the commands under the =C-c= prefix when vdiff-mode is
active. Of course you can pick whatever prefix you prefer. With the =C-c= prefix
the commands would be

*** Basics
    
| Key     | Command                 | Description                        |
|---------+-------------------------+------------------------------------|
| =C-c g= | =vdiff-switch-buffer=   | Switch buffers at matching line    |
| =C-c n= | =vdiff-next-hunk=       | Move to next hunk in buffer        |
| =C-c p= | =vdiff-previous-hunk=   | Move to previous hunk in buffer    |
| =C-c h= | =vdiff-hydra/body=      | Enter vdiff-hydra                  |

*** Viewing and Transmitting Changes Between Buffers

| Key     | Command                            | Description                         |
|---------+------------------------------------+-------------------------------------|
| =C-c r= | =vdiff-receive-changes=            | Receive change from other buffer    |
| =C-c R= | =vdiff-receive-changes-and-step=   | Same as =C-c r= then =C-c n=        |
| =C-c s= | =vdiff-send-changes=               | Send this change(s) to other buffer |
| =C-c S= | =vdiff-send-changes-and-step=      | Same as =C-c s= then =C-c n=        |
| =C-c f= | =vdiff-refine-this-hunk=           | Highlight changed words in hunk     |
| =C-c x= | =vdiff-remove-refinements-in-hunk= | Remove refinement highlighting      |
| (none)  | =vdiff-refine-this-hunk-symbol=    | Refine based on symbols             |
| (none)  | =vdiff-refine-this-hunk-word=      | Refine based on words               |
| =C-c F= | =vdiff-refine-all-hunks=           | Highlight changed words             |
| (none)  | =vdiff-refine-all-hunks-symbol=    | Refine all based on symbols         |
| (none)  | =vdiff-refine-all-hunks-word=      | Refine all based on words           |

*** Folds

| Key     | Command                            | Description                         |
|---------+------------------------------------+-------------------------------------|
| =C-c N= | =vdiff-next-fold=                  | Move to next fold in buffer         |
| =C-c P= | =vdiff-previous-fold=              | Move to previous fold in buffer     |
| =C-c c= | =vdiff-close-fold=                 | Close fold at point or in region    |
| =C-c C= | =vdiff-close-all-folds=            | Close all folds in buffer           |
| =C-c t= | =vdiff-close-other-folds=          | Close all other folds in buffer     |
| =C-c o= | =vdiff-open-fold=                  | Open fold at point or in region     |
| =C-c O= | =vdiff-open-all-folds=             | Open all folds in buffer            |

*** Ignoring case and whitespace

| Key       | Command                   | Description             |
|-----------+---------------------------+-------------------------|
| =C-c i c= | =vdiff-toggle-case=       | Toggle ignoring of case |
| =C-c i w= | =vdiff-toggle-whitespace= | Toggle ignoring of case |

*** Saving, Updating and Exiting

| Key     | Command                 | Description                  |
|---------+-------------------------+------------------------------|
| =C-c w= | =vdiff-save-buffers=    | Save both buffers            |
| =C-c u= | =vdiff-refresh=         | Force diff refresh           |
| (none)  | =vdiff-restore-windows= | Restore window configuration |
| =C-c q= | =vdiff-quit=            | Quit vdiff                   |

Evil-mode users might prefer something like the following to use a comma as a
prefix in normal state.

#+BEGIN_SRC emacs-lisp
(require 'vdiff)
(require 'evil)
(evil-define-key 'normal vdiff-mode-map "," vdiff-mode-prefix-map)
#+END_SRC

vimdiff-like binding are provided by [[https://github.com/emacs-evil/evil-collection][evil-collection]]'s [[https://github.com/emacs-evil/evil-collection/blob/master/evil-collection-vdiff.el][evil-collection-vdiff.el]]

** Hydra

Using the [[https://github.com/abo-abo/hydra][hydra package]], =vdiff-hydra= allows quick movement and changes to be
made in the buffer. By default it lives on the =h= command in the prefix
map. Bind =vdiff-hydra/body= directly to customize this key binding.

[[file:img/hydra.png]]


** Further customization
   
The current customization options and their defaults are
   
#+BEGIN_SRC emacs-lisp
  ;; Whether to lock scrolling by default when starting vdiff
  (setq vdiff-lock-scrolling t)

  ;; diff program/algorithm to use. Allows choice of diff or git diff along with
  ;; the various algorithms provided by these commands. See
  ;; `vdiff-diff-algorithms' for the associated command line arguments.
  (setq vdiff-diff-algorithm 'diff)

  ;; diff3 command to use. Specify as a list where the car is the command to use
  ;; and the remaining elements are the arguments to the command.
  (setq vdiff-diff3-command '("diff3"))

  ;; Don't use folding in vdiff buffers if non-nil.
  (setq vdiff-disable-folding nil)

  ;; Unchanged lines to leave unfolded around a fold
  (setq vdiff-fold-padding 6)

  ;; Minimum number of lines to fold
  (setq vdiff-min-fold-size 4)

  ;; If non-nil, allow closing new folds around point after updates.
  (setq vdiff-may-close-fold-on-point t)

  ;; Function that returns the string printed for a closed fold. The arguments
  ;; passed are the number of lines folded, the text on the first line, and the
  ;; width of the buffer.
  (setq vdiff-fold-string-function 'vdiff-fold-string-default)

  ;; Default syntax table class code to use for identifying "words" in
  ;; `vdiff-refine-this-change'. Some useful options are
  ;; 
  ;; "w"   (default) words
  ;; "w_"  symbols (words plus symbol constituents)
  ;; 
  ;; For more information see
  ;; https://www.gnu.org/software/emacs/manual/html_node/elisp/Syntax-Class-Table.html
  (setq vdiff-default-refinement-syntax-code "w")

  ;; If non-nil, automatically refine all hunks.
  (setq vdiff-auto-refine nil)

  ;; How to represent subtractions (i.e., deleted lines). The
  ;; default is full which means add the same number of (fake) lines
  ;; as those that were removed. The choice single means add only one
  ;; fake line. The choice fringe means don't add lines but do
  ;; indicate the subtraction location in the fringe.
  (setq vdiff-subtraction-style 'full)

  ;; Character to use for filling subtraction lines. See also
  ;; `vdiff-subtraction-style'.
  (setq vdiff-subtraction-fill-char ?-)
#+END_SRC

