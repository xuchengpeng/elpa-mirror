1 org-mime
══════════

  [https://travis-ci.org/org-mime/org-mime.svg?branch=master]
  [file:https://elpa.nongnu.org/nongnu/org-mime.svg]
  [file:http://melpa.org/packages/org-mime-badge.svg]
  [file:http://stable.melpa.org/packages/org-mime-badge.svg]

  This program sends HTML email using Org-mode HTML export.

  This approximates a WYSiWYG HTML mail editor from within Emacs, and
  can be useful for sending tables, fontified source code, and inline
  images in email.

  Tested on Emacs 25.1, 26.1, 27

  Screenshot: <file:screenshot.png>


[https://travis-ci.org/org-mime/org-mime.svg?branch=master]
<https://travis-ci.org/org-mime/org-mime>

[file:https://elpa.nongnu.org/nongnu/org-mime.svg]
<https://elpa.nongnu.org/nongnu/org-mime.html>

[file:http://melpa.org/packages/org-mime-badge.svg]
<http://melpa.org/#/org-mime>

[file:http://stable.melpa.org/packages/org-mime-badge.svg]
<http://stable.melpa.org/#/org-mime>


2 Setup
═══════

  ┌────
  │ (require 'org-mime)
  │ ;; for gnus – this is set by default
  │ (setq org-mime-library 'mml)
  │ ;; OR for Wanderlust (WL)
  │ ;; (setq org-mime-library 'semi)
  └────


3 Usage
═══════

3.1 `M-x org-mime-htmlize'
──────────────────────────

  Run `M-x org-mime-htmlize' from within a mail composition buffer to
  export either the entire buffer or just the active region to html, and
  embed the results into the buffer as a text/html mime section.

  Export a portion of an email body composed using `mml-mode' to html
  using `org-mode'.  If called with an active region only export that
  region, otherwise export the entire body.

  Warning: There has been some concern voiced over the potential
  complexity of email resulting from calling this function on an active
  region resulting in multiple `multipart/alternative' sections in the
  same email. Please see [this email thread] for a discussion of the
  potential pitfalls of this approach. Speaking from personal experience
  this has not been a problem for the author.

  You could use `org-mime-edit-mail-in-org-mode' edit mail in a special
  editor with `org-mode'.

  After `org-mime-htmlize', you can always run
  `org-mime-revert-to-plain-text-mail' restore the original plain text
  mail.


[this email thread] <http://thread.gmane.org/gmane.emacs.orgmode/23617>


3.2 `M-x org-mime-org-buffer-htmlize'
─────────────────────────────────────

  `org-mime-org-buffer-htmlize' can be called from within an Org-mode
  buffer to export either the whole buffer or the narrowed subtree or
  active region to HTML, and open a new email buffer including the
  resulting HTML content as an embedded mime section.

  Export the current org-mode buffer to HTML using `org-export-as-html'
  and package the results into an email handling with appropriate MIME
  encoding.

  The following key bindings are suggested, which bind the C-c M-o key
  sequence to the appropriate org-mime function in both email and
  Org-mode buffers,
  ┌────
  │ (add-hook 'message-mode-hook
  │ 	  (lambda ()
  │ 	    (local-set-key (kbd "C-c M-o") 'org-mime-htmlize)))
  │ (add-hook 'org-mode-hook
  │ 	  (lambda ()
  │ 	    (local-set-key (kbd "C-c M-o") 'org-mime-org-buffer-htmlize)))
  └────


3.3 `M-x org-mime-org-subtree-htmlize'
──────────────────────────────────────

  `org-mime-org-subtree-htmlize' is similar to
  `org-mime-org-buffer-htmlize' but works on subtree. It can also read
  subtree properties MAIL_SUBJECT, MAIL_TO, MAIL_CC, MAIL_BCC, and
  MAIL_IN_REPLY_TO. Here is the sample of subtree:
  ┌────
  │ * mail one
  │  :PROPERTIES:
  │  :MAIL_SUBJECT: mail title
  │  :MAIL_TO: person1@gmail.com
  │  :MAIL_CC: person2@gmail.com
  │  :MAIL_BCC: person3@gmail.com
  │  :MAIL_IN_REPLY_TO: <MESSAGE-ID>
  │  :END:
  │ some text here ...
  └────


4 Tips
══════

4.1 Embed image into mail body
──────────────────────────────

  Use below syntax,
  ┌────
  │ [[/full/path/to/your.jpg]]
  └────


4.2 CSS style customization
───────────────────────────

  Email clients will often strip all global CSS from email messages. In
  the case of web-based email readers this is essential in order to
  protect the CSS of the containing web site. To ensure that your CSS
  styles are rendered correctly they must be included in the actual body
  of the elements to which they apply.

  For those who use color themes with Dark backgrounds it is useful to
  set a dark background for all exported code blocks and example
  regions. This can be accomplished with the following,

  ┌────
  │ (add-hook 'org-mime-html-hook
  │ 	  (lambda ()
  │ 	    (org-mime-change-element-style
  │ 	     "pre" (format "color: %s; background-color: %s; padding: 0.5em;"
  │ 			   "#E6E1DC" "#232323"))))
  │ 
  │ ;; the following can be used to nicely offset block quotes in email bodies
  │ (add-hook 'org-mime-html-hook
  │ 	  (lambda ()
  │ 	    (org-mime-change-element-style
  │ 	     "blockquote" "border-left: 2px solid gray; padding-left: 4px;")))
  └────

  Below code renders text between "#" in red color,
  ┌────
  │ (add-hook 'org-mime-html-hook
  │ 	  (lambda ()
  │ 	    (while (re-search-forward "#\\([^#]*\\)#" nil t)
  │ 	      (replace-match "<span style=\"color:red\">\\1</span>"))))
  └────
  For other customization options see the org-mime customization group.


4.3 Beautify quoted mail when replying
──────────────────────────────────────

  It already works out of box. Currently it emulates Gmail's style.


4.4 Export options
──────────────────

  To avoid exporting TOC, you can setup `org-mime-export-options' which
  overrides Org default settings (but still inferior to file-local
  settings),
  ┌────
  │ (setq org-mime-export-options '(:with-latex imagemagick
  │ 				:section-numbers nil
  │ 				:with-author nil
  │ 				:with-toc nil))
  └────
  Or just setup your export options in org buffer/subtree.

  `org-mime-export-options' will override your export options if it's
  NOT nil.


4.5 Latex export problem
────────────────────────

  Please double check your org and latex setup. See
  <https://github.com/org-mime/org-mime/issues/33> for technical
  details.

  You can also modify the variable
  `org-mime-org-html-with-latex-default'.


4.6 fix exported plain text and html
────────────────────────────────────

  By default both the plain text and html are exported into the email.

  The exported plain text could be modified in
  `org-mime-plain-text-hook'. For example, below code removes "\\",
  ┌────
  │ (add-hook 'org-mime-plain-text-hook
  │ 	  (lambda ()
  │ 	    (while (re-search-forward "\\\\" nil t)
  │ 	      (replace-match ""))))
  └────

  The exported HTML could be modified in `org-mime-html-hook'. For
  example, below code renders text between "#" in red color,
  ┌────
  │ (add-hook 'org-mime-html-hook
  │ 	  (lambda ()
  │ 	    (while (re-search-forward "#\\([^#]*\\)#" nil t)
  │ 	      (replace-match "<span style=\"color:red\">\\1</span>"))))
  └────

  Surely you can fix the exported HTML in `org-mode'. For example, One
  issue of `org-mode' is [unwanted numbers in displaymath and equation].

  Thibault Marin provided [a patch] to fix the `org-mode'.

  In summary, this package gives you freedom to hack the plain text part
  or html part of the email.

  If you prefer a more "elegant" way, you could always investigate the
  `org-mode' instead.


[unwanted numbers in displaymath and equation]
<https://github.com/org-mime/org-mime/issues/38>

[a patch]
<https://lists.gnu.org/archive/html/emacs-orgmode/2019-11/msg00016.html>


4.7 Keep gpg signatures outside of multipart
────────────────────────────────────────────

  `org-mime-find-html-start' gives user a chance to tweak the region
  beginning to htmlize,
  ┌────
  │ (setq org-mime-find-html-start
  │       (lambda (start)
  │ 	(save-excursion
  │ 	  (goto-char start)
  │ 	  (search-forward "<#secure method=pgpmime mode=sign>")
  │ 	  (+ (point) 1))))
  └────


4.8 ASCII export options for text/plain
───────────────────────────────────────

  Use `org-mime-export-ascii' to export the org-mode file as ASCII for
  the `text/plain' section of the email message. The default is to
  export the original unmodified org-mode file.

  ASCII export options:
  • plain text
    ┌────
    │ (setq org-mime-export-ascii 'ascii)
    └────
  • latin1
    ┌────
    │ (setq org-mime-export-ascii 'latin1)
    └────
  • utf-8
    ┌────
    │ (setq org-mime-export-ascii 'utf-8)
    └────


4.9 Prompt for confirmation if message has no HTML
──────────────────────────────────────────────────

  If you plan to run `org-mime-htmlize' on all your email, you may want
  a confirmation if it appears you're sending an email without multipart
  content. To do this, add a hook to `message-send-hook' to your init
  file:

  ┌────
  │ (add-hook 'message-send-hook 'org-mime-confirm-when-no-multipart)
  └────


5 Support legacy Emacs versions
═══════════════════════════════

  • 0.1.6 is the last version to support Emacs 24


6 Development
═════════════

  • Patches are always welcomed
  • You can `(setq org-mime-debug t)' to enable the log
  • Make sure your code has minimum dependency and works on Emacs
    versions we support


7 Credits
═════════

  • org-mime was developed by Eric Schulte with much-appreciated help
    and discussion from everyone on the [using orgmode to send html
    mail] thread especially Eric S. Fraga for adding WL support.
  • [Anthony Cowley] fixed many bugs for exporting
  • [Matt Price] improved handling of mail headers (CC, BCC …)


[using orgmode to send html mail]
<https://lists.gnu.org/archive/html/emacs-orgmode/2010-03/msg00500.html>

[Anthony Cowley] <https://github.com/acowley>

[Matt Price] <https://github.com/titaniumbones>


8 Report bug
════════════

  You need provides the version of Emacs and Org-mode you are using.

  We also need exact steps to reproduce the issue.


9 Licence
═════════

  Documentation from the <http://orgmode.org/worg/> website (either in
  its HTML format or in its Org format) is licensed under the [GNU Free
  Documentation License version 1.3] or later. The code examples and css
  style sheets are licensed under the [GNU General Public License v3 or
  later].


[GNU Free Documentation License version 1.3]
<http://www.gnu.org/copyleft/fdl.html>

[GNU General Public License v3 or later]
<http://www.gnu.org/licenses/gpl.html>
