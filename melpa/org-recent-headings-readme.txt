This package keeps a list of recently used Org headings and lets
you quickly choose one to jump to by calling one of these commands:

The list is kept by advising functions that are commonly called to
access headings in various ways.  You can customize this list in
`org-recent-headings-advise-functions'.  Suggestions for additions
to the default list are welcome.

Note: This probably works with Org 8 versions, but it's only been
tested with Org 9.

This package makes use of handy functions and settings in
`recentf'.
