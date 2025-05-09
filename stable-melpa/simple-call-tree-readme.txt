
Bitcoin donations gratefully accepted: 1AmWPmshr6i9gajMi1yqHgx7BYzpPKuzMz

;; Introduction:
This library is based on simple-call-tree.el by Alex Schroeder, but you
do not need that library to use it (this is a replacement).
It displays a buffer containing a call tree for functions in source
code files. You can easily & quickly navigate the call tree, displaying
the code in another window, and apply query-replace or other commands
to individual functions.

When the command `simple-call-tree-display-buffer' or `simple-call-tree-current-function'
is executed a call tree for the functions in the current buffer will be created,
and in the latter case the point in the call tree is placed on the header
closest to its position in the original buffer.
If called with a prefix arg the user is also prompted for other files to include
in the call tree.
By default the call tree is displayed in a buffer called *Simple Call Tree: <BUFNAME>*,
where <BUFNAME> is the name of the buffer that the call tree was created from.
There is a dedicated menu in the menu-bar showing various commands
and their keybindings. Most of these commands are self explanatory
so try them out.

;; Navigation:
You can navigate the call tree either by moving through consecutive
headers (n/p or N/P keys) or by jumping to main branches (j for branch
corresponding to function at point, and J to prompt for a function).
When you jump to a branch, it is added to `simple-call-tree-jump-ring',
and you can navigate your jump history using the </> keys.
You can also add the function under point to the jump-ring with the . key.
If you use a negative prefix (e.g. C--) before pressing j then the branch
jumped to will not be added to the jump-ring.
If you have fm.el (available here: http://www.damtp.cam.ac.uk/user/sje30/emacs/fm.el)
you can press f to toggle follow mode on/off.

;; Display
Normally child branches correspond to functions/variables called by the parent
branch. However, if you invert the tree by pressing i then the child branches
will correspond to functions that call the parent branch.
You can sort the tree in various different ways, and change the depth of the tree.
You can narrow the tree to the function at point by pressing /
You can display extra information such as docstrings next to each item in the tree
by pressing C-c C-n, and customize the types of information that can be displayed
(see `simple-call-tree-notes-functions').

;; Exporting:
The tree can be exported in its current state with the `simple-call-tree-export-org-tree'
command, and you can alter the types of links with the `simple-call-tree-org-link-style' option.
This may be useful for project management.

;; Refactoring
You can perform `query-replace' or `query-replace-regexp' on the function at
point by pressing % or C-%, or any other arbitrary command by pressing !
This may be useful when refactoring.
