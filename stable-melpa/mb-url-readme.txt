<!-- markdown-link-check-disable -->
[![MELPA](http://melpa.org/packages/mb-url-badge.svg)](http://melpa.org/#/mb-url)
[![MELPA Stable](http://stable.melpa.org/packages/mb-url-badge.svg)](http://stable.melpa.org/#/mb-url)
[![CI](https://github.com/dochang/mb-url/actions/workflows/ci.yml/badge.svg)](https://github.com/dochang/mb-url/actions/workflows/ci.yml)
[![CircleCI](https://dl.circleci.com/status-badge/img/gh/dochang/mb-url/tree/master.svg?style=svg)](https://dl.circleci.com/status-badge/redirect/gh/dochang/mb-url/tree/master)
[![Average time to resolve an issue](http://isitmaintained.com/badge/resolution/dochang/mb-url.svg)](http://isitmaintained.com/project/dochang/mb-url "Average time to resolve an issue")
[![Percentage of issues still open](http://isitmaintained.com/badge/open/dochang/mb-url.svg)](http://isitmaintained.com/project/dochang/mb-url "Percentage of issues still open")
[![Issues](https://img.shields.io/github/issues/dochang/mb-url.svg)](https://github.com/dochang/mb-url)
[![Pull Requests](https://img.shields.io/github/issues-pr/dochang/mb-url.svg)](https://github.com/dochang/mb-url)
[![GitHub](https://img.shields.io/github/license/dochang/mb-url)](https://github.com/dochang/mb-url/blob/master/LICENSE)
[![Say Thanks!](https://img.shields.io/badge/say-thanks-green)](https://saythanks.io/to/dochang)
<!--
See the following issues for details.

<https://github.com/BlitzKraft/saythanks.io/issues/60>
<https://github.com/BlitzKraft/saythanks.io/issues/103>
-->
<!-- markdown-link-check-enable -->

Multiple Backends for URL package.

This package provides several backends for `url-retrieve' &
`url-retrieve-synchronously', which replace the internal implementation.

The motivation of this package is I can't connect HTTPS url behind proxy
(Related bugs: [#11788][], [#12636][], [#18860][], [msg00756][], [#10][]).

[#11788]: http://debbugs.gnu.org/cgi/bugreport.cgi?bug=11788
[#12636]: http://debbugs.gnu.org/cgi/bugreport.cgi?bug=12636
[#18860]: http://debbugs.gnu.org/cgi/bugreport.cgi?bug=18860
[msg00756]: https://lists.gnu.org/archive/html/help-gnu-emacs/2015-08/msg00756.html
[#10]: http://debbugs.gnu.org/cgi/bugreport.cgi?bug=10

Notice:

As the URL package has supported HTTPS over proxies supporting CONNECT since
Emacs 26, this package is no longer recommended.  But it can still be used
in Emacs 26+.

Installation:

`mb-url' is available on [MELPA] and [el-get].

[MELPA]: https://melpa.org/
[el-get]: https://github.com/dimitri/el-get

To install `mb-url' from git repository, clone the repo, then add the repo
dir into `load-path'.

`mb-url' depends on `cl-lib';  The test code also depends on `s'.

Backends:

Currently only support `url-http'.

`url-http`:

Install `mb-url-http-around-advice' to use `mb-url-http' backends.

```elisp
(advice-add 'url-http :around 'mb-url-http-around-advice)
```

All backend functions receive `(name url buffer default-sentinel)', return a
process.

`mb-url-http-backend' indicates the current backend.  If the backend is
`nil', which means no backend, `url-http' will be called.

E.g.,

```elisp
(setq mb-url-http-backend 'mb-url-http-curl)
```

NOTE: All clients are required to support automatically decompressing
response bodies.

#### [cURL][]

[cURL]: http://curl.haxx.se/

##### `mb-url-http-curl'

cURL backend for `url-http'.

##### `mb-url-http-curl-program'

cURL program.

##### `mb-url-http-curl-switches'

cURL switches.

#### [HTTPie][]

[HTTPie]: http://httpie.org/

##### `mb-url-http-httpie'

HTTPie backend for `url-http'.

##### `mb-url-http-httpie-program'

HTTPie program.

##### `mb-url-http-httpie-switches'

HTTPie switches.

##### `mb-url-http-httpie-supported-content-encoding-list'

Content encodings which HTTPie supports to decode.

If your HTTPie supports to decode a encoding like `br`, put the encoding
into this list.

By default, HTTPie supports to decode `gzip` and `deflate`.

##### `mb-url-http-httpie-content-encoding-fix-function'

A function to fix the value of `Content-Encoding` for HTTPie.  Default
value is `mb-url-http-httpie-delete-content-encoding-from-list'.

License:

GPLv3

Acknowledgements:

<https://github.com/nicferrier/curl-url-retrieve>
