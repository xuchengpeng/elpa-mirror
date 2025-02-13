                      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━
                       AUTH-SOURCE XOAUTH2 PLUGIN
                      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━


                               2024-11-08





1 Introduction
══════════════

  This package provides a global minor mode for enabling support for
  xoauth2 authentication with auth-source.  OAuth 2.0, which stands for
  “Open Authorization”, is a standard designed to allow a website or
  application to access resources hosted by other web apps on behalf of
  a user.  The OAuth 2.0 Authorization Protocol Extensions (xoauth2)
  extend the OAuth 2.0 Authentication Protocol and the JSON Web Token
  (JWT) to enable server-to-server authentication.  More info please
  check out [this stackoverflow answer].


[this stackoverflow answer]
<https://stackoverflow.com/a/76389679/2337550>


2 Installation
══════════════

  `auth-source-xoauth2-plugin' is on [GNU ELPA], and you can install it
  with `package-install' (see also [the Emacs document on how to use
  package-install]).  Or you can clone the repository or simply download
  the `auth-source-xoauth2-plugin.el' file and put it anywhere in your
  Emacs' `load-path'.

  Then add the following lines in your Emacs configuration:

  ┌────
  │ (require 'auth-source-xoauth2-plugin)
  │ (auth-source-xoauth2-plugin-mode t)
  └────

  or with use-package:

  ┌────
  │ (use-package auth-source-xoauth2-plugin
  │   :custom
  │   (auth-source-xoauth2-plugin-mode t))
  └────

  After enabling, smtpmail should be supported.  To enable this in Gnus
  nnimap, you should also set `(nnimap-authenticator xoauth2)' in the
  corresponding account settings in `gnus-secondary-select-methods' as
  the following:

  ┌────
  │ (nnimap "account_name"
  │ 	...
  │ 	(nnimap-authenticator xoauth2)
  │ 	...
  │ 	)
  └────

  To disable, just toggle the minor mode off by calling `M-x
  auth-source-xoauth2-plugin-mode' again.

  When xoauth2 authentication is enabled, it will try to get the
  following data from the auth-source entry: `auth-url', `token-url',
  `scope', `client-id', `client-secret', `redirect-uri', and optionally
  `state'.  An example Authinfo entry (in JSON format as
  `~/.authinfo.json.gpg') for an Gmail account may look like below (you
  need to fill in the data between `<' and `>' based on your account
  settings):

  ┌────
  │ [
  │   ...
  │   {
  │     "machine": "<your_imap_address_or_email>",
  │     "login": "<your_email>",
  │     "port": "imaps",
  │     "auth": "xoauth2",
  │     "auth-url": "https://accounts.google.com/o/oauth2/auth",
  │     "token-url": "https://accounts.google.com/o/oauth2/token",
  │     "client-id": "<the_client_id_from_your_app>",
  │     "client-secret": "<the_client_secret_from_your_app>",
  │     "redirect-uri": "https://oauth2.dance/",
  │     "scope": "https://mail.google.com"
  │   },
  │   ...
  │ ]
  └────

  It will then use `oauth2.el' to retrieve the access-token with those
  information, use it to construct the oauth2 authentication string, and
  let `auth-source' do the rest.


[GNU ELPA]
<https://elpa.gnu.org/packages/auth-source-xoauth2-plugin.html>

[the Emacs document on how to use package-install]
<https://www.gnu.org/software/emacs/manual/html_node/emacs/Package-Installation.html>


3 Comparison with other xoauth2 implementations
═══════════════════════════════════════════════

3.1 auth-source-xoauth2
───────────────────────

  This plugin takes inspirations from [auth-source-xoauth2] to advice
  the auth-source-search backends to add xoauth2 access-token for
  authentication.  The implementation is independent and reuses many
  existing facilities in `auth-source.el', where auth-source-xoauth2
  reimplemented most of the required functions itself.

  `auth-source-xoauth2-plugin' also makes use of `oauth2.el' and its
  storage for storing temporary/ephemeral data tokens, where
  `auth-source-xoauth2' implemented its own storage.


[auth-source-xoauth2] <https://github.com/ccrusius/auth-source-xoauth2>


4 Debugging
═══════════

  In case you encounter any issues, you may consider enabling verbose
  messages to help debugging.  `auth-source-xoauth2-plugin' uses the
  same convention as `auth-source' for outputing verbose messages.  You
  may do the following:

  ┌────
  │ (setq auth-source-debug t)
  └────

  and check the `*Message*' buffer for logs.  You can enable even more
  verbose log by the following:

  ┌────
  │ (setq auth-source-debug 'trivia)
  └────

  NOTE: \'trivia will include your tokens for authentication in your
  `*Message*' buffer so be careful not to share the log with untrusted
  entities.


5 Notes on Implementation
═════════════════════════

  `auth-source' uses the `secret' field in auth-source file as password
  for authentication, including xoauth2.  To decide which authentication
  method to use (e.g. plain password vs xoauth2), this plugin inspects
  the `auth' field from the auth-source entry, and if the value is
  `xoauth2', it will try to gather data and get the access token for use
  of xoauth2 authentication; otherwise, it will fallback to the default
  authentication method.

  This package uses an advice to switch the auth-source search result
  from the `password' to the `access-token' it got, which in turn will
  be used to construct the xoauth2 authentication string, currently in
  nnimap-login and smtpmail-try-auth-method.  To enable xoauth2 support
  in smtpmail, it adds \'xoauth2 to \'smtpmail-auth-supported (if it is
  not already in the list) using `add-to-list' so that xoauth2 is tried
  first.

  Note that currently `auth-source' requires the searched entry must
  have `secret' field set in the entry, which is not necessarily true
  when using xoauth2.  Therefore in the advice it temporarily disables
  checking for `:secret' perform the search in the backend, and ensure
  that `secret' contains the generated access-token before returning.
