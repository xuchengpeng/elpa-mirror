This mode is used for editing spec files used for building RPM packages.

Put this in your .emacs file to enable autoloading of rpm-spec-mode,
and auto-recognition of ".spec" files:

 (autoload 'rpm-spec-mode "rpm-spec-mode.el" "RPM spec mode." t)
 (setq auto-mode-alist (append '(("\\.spec" . rpm-spec-mode))
                               auto-mode-alist))
------------------------------------------------------------
