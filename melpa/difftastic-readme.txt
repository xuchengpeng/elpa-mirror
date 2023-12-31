The difftastic is designed to integrate difftastic - a structural diff tool
- (https://github.com/wilfred/difftastic) into your Emacs workflow,
enhancing your code review and comparison experience.  This package
automatically displays difftastic's output within Emacs using faces from
your user theme, ensuring consistency with your overall coding environment.

Configuration

To configure the `difftastic` commands in `magit-diff` prefix, use the
following code snippet in your Emacs configuration:

(require 'difftastic)

;; Add commands to a `magit-difftastic'
(eval-after-load 'magit-diff
  '(transient-append-suffix 'magit-diff '(-1 -1)
     [("D" "Difftastic diff (dwim)" difftastic-magit-diff)
      ("S" "Difftastic show" difftastic-magit-show)]))
(add-hook 'magit-blame-read-only-mode-hook
          (lambda ()
            (kemap-set magit-blame-read-only-mode-map
                       "D" #'difftastic-magit-show)
            (kemap-set magit-blame-read-only-mode-map
                       "S" #'difftastic-magit-show)))

Or, if you use `use-package':

(use-package difftastic
  :demand t
  :bind (:map magit-blame-read-only-mode-map
         ("D" . difftastic-magit-show)
         ("S" . difftastic-magit-show))
  :config
  (eval-after-load 'magit-diff
    '(transient-append-suffix 'magit-diff '(-1 -1)
       [("D" "Difftastic diff (dwim)" difftastic-magit-diff)
        ("S" "Difftastic show" difftastic-magit-show)])))

Usage

There are four commands to interact with difftastic:

- `difftastic-magit-diff' - show the result of 'git diff ARG' with
  difftastic.  It tries to guess ARG, and ask for it when can't. When called
  with prefix argument it will ask for ARG.

- `difftastic-magit-show' - show the result of 'git show ARG' with
  difftastic.  It tries to guess ARG, and ask for it when can't. When called
  with prefix argument it will ask for ARG.

- `difftastic-files' - show the result of 'difft FILE-A FILE-B'.  When
  called with prefix argument it will ask for language to use, instead of
  relaying on difftastic's detection mechanism.

- `difftastic-buffers' - show the result of 'difft BUFFER-A BUFFER-B'.
  Language is guessed based on buffers modes.  When called with prefix
  argument it will ask for language to use.
