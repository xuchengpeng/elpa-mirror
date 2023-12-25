
Consult-dir implements commands to easily switch between "active"
directories. The directory candidates are collected from user bookmarks,
projectile project roots (if available), project.el project roots and recentf
file locations. The `default-directory' variable not changed in the process.

Call `consult-dir' from the minibuffer to choose a directory with completion
and insert it into the minibuffer prompt, shadowing or deleting any existing
directory. The file name input is retained. This lets the user switch to
distant directories very quickly when finding files, for instance.

Call `consult-dir' from a regular buffer to choose a directory with
completion and then interactively find a file in that directory. The command
run with this directory is configurable via `consult-dir-default-command' and
defaults to `find-file'.

Call `consult-dir-jump-file' from the minibuffer to asynchronously find a
file anywhere under the directory that is currently in the prompt. This can
be used with `consult-dir' to quickly switch directories and find files at an
arbitrary depth under them. `consult-dir-jump-file' uses `consult-find' under
the hood.

To use this package, bind `consult-dir' and `consult-dir-jump-file' under the
`minibuffer-local-completion-map' or equivalent, and `consult-dir' to the global map.

(define-key minibuffer-local-completion-map (kbd "C-x C-d") #'consult-dir)
(define-key minibuffer-local-completion-map (kbd "C-x C-j") #'consult-dir-jump-file)
(define-key global-map (kbd "C-x C-d") #'consult-dir)

Directory sources configuration:
- To make recent directories available, turn on `recentf-mode'.

- To make projectile projects available, turn on projectile-mode and
configure `consult-dir-project-list-function'. Note that Projectile is NOT
required to install this package.

- To make project.el projects available, configure
`consult-dir-project-list-function'.

To change directory sources or their ordering, customize
`consult-dir-sources'.
