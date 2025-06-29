1 el-job
════════

  Imagine you have a function you'd like to run on a long list of
  inputs.  You could run `(mapcar #'FN INPUTS)', but that hangs Emacs
  until done.

  This library lets you run the same function in many subprocesses (one
  per CPU core), each with their own split of the `INPUTS' list, then
  merge their outputs and pass it back to the current Emacs.

  In the meantime, current Emacs does not hang at all.

  Best of all, it completes /faster/ than `(mapcar #'FN INPUTS)', owing
  to the use of all CPU cores!

  For real-world usage, search for `el-job' in the source of
  [org-mem.el].


[org-mem.el] <https://github.com/meedstrom/org-mem/blob/main/org-mem.el>

1.1 Design rationale
────────────────────

  I want to shorten the round-trip as much as possible, *between the
  start of an async task and having the results*.

  For example, say you have some lisp that collects completion
  candidates, and you want to run it asynchronously because the lisp you
  wrote isn't always fast enough to avoid the user's notice, but you'd
  still like it to return as soon as possible.


1.1.1 Processes stay alive
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  In the above example, a user might only delay a fraction of a second
  between opening the minibuffer and beginning to type, so there's scant
  room for overhead like spinning up subprocesses that load a bunch of
  libraries before getting to work.

  Thus, el-job keeps idle subprocesses for up to 30 seconds after a job
  finishes, awaiting more input.

  An aesthetic drawback is cluttering your task manager with many
  processes named "emacs".

  Users who tend to run system commands such as `pkill emacs' may find
  that the command occasionally "does not work", because it actually
  killed an el-job subprocess, instead of the Emacs they see on screen.


1.1.2 Emacs 30 `fast-read-process-output'
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Some other libraries, like the popular [async.el], are designed around
  a custom process filter.

  Since Emacs 30, it's a good idea to instead use the /built-in/ process
  filter when performance is critical, and el-job does so.  Quoting
  [NEWS.30]:

  ┌────
  │ ** The default process filter was rewritten in native code.
  │ The round-trip through the Lisp function
  │ 'internal-default-process-filter' is skipped when the process filter is
  │ the default one.  It is reimplemented in native code, reducing GC churn.
  │ To undo this change, set 'fast-read-process-output' to nil.
  └────


[async.el] <https://github.com/jwiegley/emacs-async/>

[NEWS.30]
<https://github.com/emacs-mirror/emacs/blob/master/etc/NEWS.30>


1.2 News 2.4
────────────

  • Jobs must now have `:inputs'.  If `:inputs' nil and there was
    nothing queued, `el-job-launch' will no-op and return the symbol
    `inputs-were-empty'.


1.3 News 2.3
────────────

  • Some renames to follow Elisp convention
    • `el-job:timestamps' and friends now `el-job-timestamps'.


1.4 News 2.1
────────────

  • DROP SUPPORT Emacs 28
    • It likely has not been working for a while anyway.  Maybe works on
      the [v0.3 branch], from 0.3.26+.


[v0.3 branch] <https://github.com/meedstrom/el-job/tree/v0.3>


1.5 News 2.0
────────────

  • Jobs must now have `:id' (no more anonymous jobs).
  • Pruned many code paths.


1.6 News 1.1
────────────

  • Changed internals so that all builds of Emacs can be expected to
    perform similarly well.


1.7 News 1.0
────────────

  • No longer keeps processes alive forever.  All jobs are kept alive
    for up to 30 seconds of disuse, then reaped.
  • Pruned many code paths.
  • Many arguments changed, and a few were removed.  Consult the
    docstring of `el-job-launch' again.


1.8 Limitations
───────────────

  1. The return value from the `:funcall-per-input' function must always
     be a list with a fixed length, where the elements are also lists.

     For example, org-mem passes `:funcall-per-input
     #'org-mem-parser--parse-file' to el-job, and if you look in
     [org-mem-parser.el] for the defun of `org-mem-parser--parse-file',
     it always returns a list of 5 items:

     ┌────
     │ (list (if missing-file (list missing-file)) ; List of 0 or 1 item
     │       (if file-mtime (list file-mtime))     ; List of 0 or 1 item
     │       found-entries                         ; List of many items
     │       org-node-parser--found-links          ; List of many items
     │       (if problem (list problem))))         ; List of 0 or 1 item
     └────

     It may look clunky to return sub-lists of only one item, but you
     could consider it a minor expense in exchange for simpler library
     code.

  2. Some data types cannot be exchanged with the children: those whose
     printed form look like `#<...>'.  For example, `#<buffer
     notes.org>', `#<obarray n=94311>', `#<marker at 3102 in
     README.org>'.

     IIUC, this sort of data only has meaning within the current process
     – so even if you could send it, it would not be usable by the
     recipient anyway.

  3. For now, this library tends to be applicable only to a narrow set
     of use-cases, since you can only pass one `:inputs' list which
     would tend to contain a single kind of thing, e.g. it could be a
     list of files to visit, to be split between child processes.  In
     many potential use-cases, you'd actually want multiple input lists
     and split them differently, and that's not supported yet.


[org-mem-parser.el]
<https://github.com/meedstrom/org-mem/blob/main/org-mem-parser.el>
