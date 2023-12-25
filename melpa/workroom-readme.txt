Workroom provides named "workrooms" (or workspaces), somewhat
similar to multiple desktops in GNOME.

Each workroom has own set of buffers, allowing you to work on
multiple projects without getting lost in all buffers.

Each workroom also has its own set of views.  Views are just named
window configurations.  They allow you to switch to another window
configuration without losing your well-planned window setup.

You can also bookmark a workroom to restore them at a later time,
possibly in another Emacs session.  You can also save your
workrooms in your desktop.

Usage
═════

There is always a workroom named "master", which contains all live
buffers.  Removing any buffer from this workroom kills that buffer.
You can't kill this workroom, but you can customize the variable
`workroom-default-room-name' to change its name.

All the useful commands can be called with following key sequences:

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 Key          Command
───────────────────────────────────────────
 `C-x c s'    `workroom-switch'
 `C-x c S'    `workroom-switch-view'
 `C-x c d'    `workroom-kill'
 `C-x c D'    `workroom-kill-view'
 `C-x c C-d'  `workroom-kill-with-buffers'
 `C-x c r'    `workroom-rename'
 `C-x c R'    `workroom-rename-view'
 `C-x c c'    `workroom-clone'
 `C-x c C'    `workroom-clone-view'
 `C-x c m'    `workroom-bookmark'
 `C-x c M'    `workroom-bookmark-multiple'
 `C-x c b'    `workroom-switch-to-buffer'
 `C-x c a'    `workroom-add-buffer'
 `C-x c k'    `workroom-kill-buffer'
 `C-x c K'    `workroom-remove-buffer'
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Here the prefix key sequence is `C-x c', but you can customize
`workroom-command-map-prefix' to change it.

You might want to remap ~switch-to-buffer~, ~kill-buffer~ and other
commands with Workroom-aware commands by adding something like the
following to your init file:

┌────
│ (global-set-key [remap switch-to-buffer]
│                 #'workroom-switch-to-buffer)
│ (global-set-key [remap kill-buffer] #'workroom-kill-buffer)
└────

You can save all your workroom in your desktop by enabling
`workroom-desktop-save-mode' mode.

You can create a workroom containing only your project buffer with
`workroom-switch-to-project-workroom'.  You can also enable
`workroom-auto-project-workroom-mode', it'll switch to (creating if
needed) the project's workroom when you open a file.

If you want to completely automate managing workroom buffer list,
check out the docstrings of `workroom-buffer-manager-function',
`workroom-set-buffer-manager-function' and
`workroom-buffer-manager-data'.
