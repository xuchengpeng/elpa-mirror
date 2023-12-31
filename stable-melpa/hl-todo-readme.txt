Highlight TODO and similar keywords in comments and strings.

You can either explicitly turn on `hl-todo-mode' in certain buffers
or use the global variant `global-hl-todo-mode', which enables
the local mode based on each buffer's major-mode and the options
`hl-todo-include-modes' and `hl-todo-exclude-modes'.  By default
`hl-todo-mode' is enabled for all buffers whose major-mode derive
from either `prog-mode' or `text-mode', except `org-mode'.

This package also provides commands for moving to the next or
previous keyword, to invoke `occur' with a regexp that matches all
known keywords, and to insert a keyword.  If you want to use these
commands, then you should bind them in `hl-todo-mode-map', e.g.:

  (keymap-set hl-todo-mode-map "C-c p" #'hl-todo-previous)
  (keymap-set hl-todo-mode-map "C-c n" #'hl-todo-next)
  (keymap-set hl-todo-mode-map "C-c o" #'hl-todo-occur)
  (keymap-set hl-todo-mode-map "C-c i" #'hl-todo-insert)
