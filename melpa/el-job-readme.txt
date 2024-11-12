Imagine you have a function you'd like to run on a long list of inputs.  You
could run (mapcar #'FN INPUTS), but that hangs Emacs until done.

This library gives you the tools to split up the inputs and run the function
in many subprocesses (one per CPU core), then merges their outputs and
passes it back to the current Emacs.  In the meantime, current Emacs does
not hang at all.

The only public API is the function `el-job-launch'.
