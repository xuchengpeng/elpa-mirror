Ghub provides basic support for using the APIs of various Git forges
from Emacs packages.  Originally it only supported the Github REST
API, but now it also supports the Github GraphQL API as well as the
REST APIs of Gitlab, Gitea, Gogs and Bitbucket.

Ghub abstracts access to API resources using only a handful of basic
functions such as `ghub-get'.  These are convenience wrappers around
`ghub-request'.

When accessing Github, then Ghub handles the creation and storage of
access tokens using a setup wizard to make it easier for users to get
started.  The tokens for other forges have to be created manually.

Ghub is intentionally limited to only provide these two essential
features — basic request functions and guided setup — to avoid being
too opinionated, which would hinder wide adoption.  It is assumed that
wide adoption would make life easier for users and maintainers alike,
because then all packages that talk to forge APIs could be configured
the same way.

Please consult the manual (info "ghub") for more information.
