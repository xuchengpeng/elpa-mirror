timeout is a small Elisp library that provides higher order functions to
throttle or debounce Elisp functions.  This is useful for corralling
over-eager code that:
(i) is slow and blocks Emacs, and
(ii) does not provide customization options to limit how often it runs,

To throttle a function FUNC to run no more than once every 2 seconds, run
(timeout-throttle! 'func 2.0)

To debounce a function FUNC to run after a delay of 0.3 seconds, run
(timeout-debounce! 'func 0.3)

To create a new throttled or debounced version of FUNC instead, run

(timeout-throttle 'func 2.0)
(timeout-debounce 'func 0.3)

You can bind this via fset or defalias:

(defalias 'throttled-func (timeout-throttle 'func 2.0))
(fset     'throttled-func (timeout-throttle 'func 2.0))

The interactive spec and documentation of FUNC is carried over to the new
function.
