                            ━━━━━━━━━━━━━━━
                             ACTIVITIES.EL
                            ━━━━━━━━━━━━━━━


Inspired by Genera's and KDE's concepts of "activities", this library
allows the user to select an "activity", the loading of which restores a
window configuration and/or frameset, along with the buffers shown in
each window.  Saving an activity saves the state for later restoration.
Switching away from an activity saves the last-used state for later
switching back to, while still allowing the activity's initial or
default state to be restored on demand.  Resuming an activity loads the
last-used state, or the initial/default state when a universal argument
is provided.

The implementation uses the bookmark system to save buffers' states–that
is, any major mode that supports the bookmark system is compatible.  A
buffer whose major mode does not support the bookmark system (or does
not support it well enough to restore useful state) is not compatible
and can't be fully restored, or perhaps not at all; but solving that is
as simple as implementing bookmark support for the mode, which is
usually trivial.

Integration with Emacs's `tab-bar-mode' is provided: a window
configuration or frameset can be restored to a window or set of frames,
or to a tab or set of tabs.

Various hooks are (or will be–feedback is welcome) provided, both
globally and per-activity, so that the user can define functions to be
called when an activity is saved, restored, or switched from/to.  For
example, this could be used to limit the set of buffers offered for
switching to within an activity, or to track the time spent in an
activity.


1 Installation
══════════════

  Until this library is available from a package archive, it's
  recommended to install it using [Quelpa]:

  1. Install [quelpa-use-package] (which can be installed directly from
     MELPA).
  2. Add this form to your init file (which includes a recommended
     configuration):

  ┌────
  │ (use-package activities
  │   :quelpa (activities :fetcher github :repo "alphapapa/activities.el")
  │ 
  │   :bind
  │   (("C-x C-a l" . activities-list)
  │    ("C-x C-a a" . activities-resume)
  │    ;; For convenience, we also bind `activities-resume' to "C-a", so the
  │    ;; user need not lift the Control key.  This makes it easier to
  │    ;; quickly switch between activities.
  │    ("C-x C-a C-a" . activities-resume)
  │    ("C-x C-a RET" . activities-switch)
  │    ("C-x C-a g" . activities-revert)
  │    ("C-x C-a n" . activities-new)
  │    ("C-x C-a s" . activities-suspend)
  │    ;; Alias for `activities-suspend'.
  │    ("C-x C-a C-k" . activities-kill))
  │ 
  │   :init
  │   ;; Automatically save activities' states when Emacs is idle and upon
  │   ;; exit.
  │   (activities-mode)
  │   ;; Open activities in `tab-bar' tabs (otherwise frames are used, but
  │   ;; the author doesn't test that as much).
  │   (activities-tabs-mode))
  └────

  If you choose to install it otherwise, you'll need to load both the
  `activities' and `activities-tabs' libraries, or ensure that the
  autoloads are generated properly.


[Quelpa] <https://framagit.org/steckerhalter/quelpa>

[quelpa-use-package]
<https://framagit.org/steckerhalter/quelpa-use-package#installation>


2 Usage
═══════

2.1 Activities
──────────────

  For the purposes of this library, an "activity" is a window
  configuration and its associated buffers.  When an activity is
  "resumed," its buffers are recreated and loaded into the window
  configuration, which is loaded into a frame or tab.

  From the user's perspective, an "activity" should be thought of as
  something like, "reading my email," "working on my Emacs library,"
  "writing my book," "working for this client," etc.  The user arranges
  a set of windows and buffers according to what's needed, then saves it
  as a new activity.  Later, when the user wants to return to doing that
  activity, the activity is "resumed," which restores the activity's
  last-seen state, allowing the user to pick up where the activity was
  left off; but the user may also revert the activity to its default
  state, which may be used as a kind of entry point to doing the
  activity in general.


2.2 Compatibility
─────────────────

  This library is designed to not interfere with other workflows and
  tools; it is intended to coexist and allow integration with them.  For
  example, when `activities-tabs-mode' is enabled, non-activity-related
  tabs are not affected by it; and the user may close any tab using
  existing tab commands, regardless of whether it is associated with an
  activity.


2.3 Modes
─────────

  `activities-mode'
        Automatically saves activities' states when Emacs is idle and
        when Emacs exits.  Should be enabled while using this package
        (otherwise you would have to manually call
        `activities-save-all', which would defeat much of the purpose of
        this library).
  `activities-tabs-mode'
        Causes activities to be managed as `tab-bar' tabs rather than
        frames (the default).  (/This is what the author uses; bugs
        present when this mode is not enabled are less likely to be
        found, so please report them./)


2.4 Workflow
────────────

  An example of a workflow using activities:

  1. Arrange windows in a tab according to an activity you're
     performing.
  2. Call `activities-new' (`C-x C-a n') to save the activity under a
     name.
  3. Perform the activity for a while.
  4. Change window configuration, change tab, close the tab, or even
     restart Emacs.
  5. Call `activities-resume' (`C-x C-a C-a') to resume the activity
     where you left off.
  6. Return to the original activity state with `activities-revert'
     (`C-x C-a g').
  7. Rearrange windows and buffers.
  8. Call `activities-new' with a universal prefix argument (`C-u C-x
     C-a n') to redefine an activity's default state.
  9. Suspend the activity with `activities-suspend' (`C-x C-a s') (which
     saves its last state and closes its frame/tab).


2.5 Commands
────────────

  `activities-list' (`C-x C-a l')
        List activities in a `vtable' buffer in which they can be
        managed with various commands.
  `activities-new' (`C-x C-a n')
        Define a new activity whose default state is the current frame's
        or tab's window configuration.  With prefix argument, overwrite
        an existing activity (thereby updating its default state to the
        current state).
  `activities-suspend' (`C-x C-a s')
        Save an activity's state and close its frame or tab.
  `activities-kill' (`C-x C-a C-k')
        Alias for `activities-suspend'.
  `activities-resume' (`C-x C-a C-a')
        Resume an activity, switching to a new frame or tab for its
        window configuration, and restoring its buffers.  With prefix
        argument, restore its default state rather than its last.
  `activities-revert' (`C-x C-a g')
        Revert an activity to its default state.
  `activities-switch' (`C-x C-a RET')
        Switch to an already-active activity.
  `activities-discard'
        Discard an activity permanently.
  `activities-save-all'
        Save all active activities' states.  (`activities-mode' does
        this automatically, so this command should rarely be needed.)


2.6 Bookmarks
─────────────

  When option `activities-bookmark-store' is enabled, an Emacs bookmark
  is stored when a new activity is made.  This allows the command
  `bookmark-jump' (`C-x r b') to be used to resume an activity (helping
  to universalize the bookmark system).


3 FAQ
═════

  How is this different from [Burly.el] or [Bufler.el]?
        Burly is a well-polished tool for restoring window and frame
        configurations, which could be considered an incubator for some
        of the ideas furthered here.  Bufler's `bufler-workspace'
        library uses Burly to provide some similar functionality, which
        is at an exploratory stage.  `activities' hopes to provide a
        longer-term solution more suitable for integration into Emacs.

  How does this differ from "workspace" packages?
        Yes, there are many Emacs packages that provide "workspace"-like
        features in one way or another.  To date, only Burly and Bufler
        seem to offer the ability to restore one across Emacs sessions.
        As mentioned, `activities' is intended to be more refined and
        easier to use (e.g. automatically saving activities' states when
        `activities-mode' is enabled).  Comparisons to other packages
        are left to the reader; suffice to say that `activities' is
        intended to provide what other tools haven't, in an idiomatic,
        intuitive way.  (Feedback is welcome.)

  How does this differ from the built-in `desktop-mode'?
        As best this author can tell, `desktop-mode' saves and restores
        one set of buffers, with various options to control its
        behavior.  It does not use `bookmark' internally, which prevents
        it from restoring non-file-backed buffers.  As well, it is not
        intended to be used on-demand to switch between sets of buffers,
        windows, or frames (i.e. "activities").

  "Activities" haven't seemed to pan out for KDE.  Why would they in Emacs?
        KDE Plasma's Activities system requires applications that can
        save and restore their state through Plasma, which only (or
        mostly only?) KDE apps can do, limiting the usefulness of the
        system.  However, Emacs offers a coherent environment, similar
        to Lisp machines of yore, and its `bookmark' library offers a
        way for any buffer's major mode to save and restore state, if
        implemented (which many already are).

  Why did a buffer not restore correctly?
        Most likely because that buffer's major mode does not support
        Emacs bookmarks (which `activities' uses internally to save and
        restore buffer state).  But many, if not most, major modes do;
        and for those that don't, implementing such support is usually
        trivial (and thereby benefits Emacs as a whole, not just
        `activities').  So contact the major mode's maintainer and ask
        that `bookmark' support be implemented.

  Why did I get an error?
        Because `activities' is at an early stage of development and
        some of these features are not simple to implement.  But it's
        based on Burly, which has already been through much bug-fixing,
        so it should proceed smoothly.  Please report any bugs you find.


[Burly.el] <https://github.com/alphapapa/burly.el>

[Bufler.el] <https://github.com/alphapapa/bufler.el/>


4 Changelog
═══════════

4.1 v0.3
────────

  *Additions*
  ⁃ Command `activities-list' lists activities in a `vtable' buffer in
    which they can be managed.
  ⁃ Offer current activity name by default when redefining an activity
    with `activities-new'.
  ⁃ Record times at which activities' states were updated.


4.2 v0.2
────────

  *Additions*
  ⁃ Offer current `project' name by default for new activities.  (Thanks
    to [Joseph Turner].)
  ⁃ Use current activity as default for various completions.  (Thanks to
    [Joseph Turner].)

  *Fixes*
  ⁃ Raise frame after selecting it.  (Thanks to [JD Smith] for
    suggesting.)


[Joseph Turner] <https://breatheoutbreathe.in>

[JD Smith] <https://github.com/jdtsmith>


4.3 v0.1.3
──────────

  *Fixes*
  ⁃ Autoloads.
  ⁃ Command aliases.


4.4 v0.1.2
──────────

  *Fixes*
  ⁃ Some single-window configurations were not restored properly.


4.5 v0.1.1
──────────

  *Fixes*
  ⁃ Silence message about non-file-visiting buffers.


4.6 v0.1
────────

  Initial release.
