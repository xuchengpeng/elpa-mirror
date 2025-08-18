
`hotdesk-mode' provides unopinionated per-frame buffer lists that adapt with
your workflow.

A frame may be assigned a label; a buffer may be assigned multiple labels.
When a frame and buffer have the same label, they are linked.

Buffer labelling is automatic and naturally captures your usage, producing
useful frame buffer associations without configuration.  This offers an
efficient workflow in multi-frame environments, giving you curated buffer
lists you can navigate, preserve and manage per frame(s).

A customisable predicate function can prevent combinations of labels and
buffers (a rudimentary permission system).  The mode also provides utilities
and UI editors for fine-tuning label assignments.

Usage:

  (require 'hotdesk)
  (hotdesk-mode 1)

See README.md or the project URL for configuration and advanced commands.
