lua-mode provides support for editing Lua, including automatic
indentation, syntactical font-locking, running interactive shell,
Flymake checks with luacheck, interacting with `hs-minor-mode' and
online documentation lookup.

The following variables are available for customization (see more via
`M-x customize-group lua`):

- Var `lua-indent-level':
  indentation offset in spaces
- Var `lua-indent-string-contents':
  set to `t` if you like to have contents of multiline strings to be
  indented like comments
- Var `lua-indent-nested-block-content-align':
  set to `nil' to stop aligning the content of nested blocks with the
  open parenthesis
- Var `lua-indent-close-paren-align':
  set to `t' to align close parenthesis with the open parenthesis,
  rather than with the beginning of the line
- Var `lua-mode-hook':
  list of functions to execute when lua-mode is initialized
- Var `lua-documentation-url':
  base URL for documentation lookup
- Var `lua-documentation-function': function used to
  show documentation (`eww` is a viable alternative for Emacs 25)

These are variables/commands that operate on the Lua process:

- Var `lua-default-application':
  command to start the Lua process (REPL)
- Var `lua-default-command-switches':
  arguments to pass to the Lua process on startup (make sure `-i` is there
  if you expect working with Lua shell interactively)
- Cmd `lua-start-process': start new REPL process, usually happens automatically
- Cmd `lua-kill-process': kill current REPL process

These are variables/commands for interaction with the Lua process:

- Cmd `lua-show-process-buffer': switch to REPL buffer
- Cmd `lua-hide-process-buffer': hide window showing REPL buffer
- Var `lua-always-show': show REPL buffer after sending something
- Cmd `lua-send-buffer': send whole buffer
- Cmd `lua-send-current-line': send current line
- Cmd `lua-send-defun': send current top-level function
- Cmd `lua-send-region': send active region
- Cmd `lua-restart-with-whole-file': restart REPL and send whole buffer

To enable on-the-fly linting, make sure you have the luacheck
program installed (available from luarocks) and activate
`flymake-mode'.

See "M-x apropos-command ^lua-" for a list of commands.
See "M-x customize-group lua" for a list of customizable variables.
