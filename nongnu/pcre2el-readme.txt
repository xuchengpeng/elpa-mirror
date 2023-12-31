1 pcre2el: convert between PCRE, Emacs and rx regexp syntax
═══════════════════════════════════════════════════════════

1.1 Overview
────────────

  `pcre2el' or `rxt' (RegeXp Translator or RegeXp Tools) is a utility
  for working with regular expressions in Emacs, based on a
  recursive-descent parser for regexp syntax. In addition to converting
  (a subset of) PCRE syntax into its Emacs equivalent, it can do the
  following:

  • convert Emacs syntax to PCRE
  • convert either syntax to `rx', an S-expression based regexp syntax
  • untangle complex regexps by showing the parse tree in `rx' form and
    highlighting the corresponding chunks of code
  • show the complete list of strings (productions) matching a regexp,
    provided the list is finite
  • provide live font-locking of regexp syntax (so far only for Elisp
    buffers – other modes on the TODO list)


1.2 Usage
─────────

  Enable `rxt-mode' or its global equivalent `rxt-global-mode' to get
  the default key-bindings. There are three sets of commands: commands
  that take a PCRE regexp, commands which take an Emacs regexp, and
  commands that try to do the right thing based on the current
  mode. Currently, this means Emacs syntax in `emacs-lisp-mode' and
  `lisp-interaction-mode', and PCRE syntax everywhere else.

  The default key bindings all begin with `C-c /' and have a mnemonic
  structure: `C-c / <source> <target>', or just `C-c / <target>' for the
  "do what I mean" commands. The complete list of key bindings is given
  here and explained in more detail below:

  • "Do-what-I-mean" commands:
    `C-c / /'
          `rxt-explain'
    `C-c / c'
          `rxt-convert-syntax'
    `C-c / x'
          `rxt-convert-to-rx'
    `C-c / ′'
          `rxt-convert-to-strings'

  • Commands that work on a PCRE regexp:
    `C-c / p e'
          `rxt-pcre-to-elisp'
    `C-c / %'
          `pcre-query-replace-regexp'
    `C-c / p x'
          `rxt-pcre-to-rx'
    `C-c / p s'
          `rxt-pcre-to-sre'
    `C-c / p ′'
          `rxt-pcre-to-strings'
    `C-c / p /'
          `rxt-explain-pcre'

  • Commands that work on an Emacs regexp:
    `C-c / e /'
          `rxt-explain-elisp'
    `C-c / e p'
          `rxt-elisp-to-pcre'
    `C-c / e x'
          `rxt-elisp-to-rx'
    `C-c / e s'
          `rxt-elisp-to-sre'
    `C-c / e ′'
          `rxt-elisp-to-strings'
    `C-c / e t'
          `rxt-toggle-elisp-rx'
    `C-c / t'
          `rxt-toggle-elisp-rx'


1.2.1 Interactive input and output
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  When used interactively, the conversion commands can read a regexp
  either from the current buffer or from the minibuffer. The output is
  displayed in the minibuffer and copied to the kill-ring.

  • When called with a prefix argument (`C-u'), they read a regular
    expression from the minibuffer literally, without further processing
    – meaning there's no need to double the backslashes if it's an Emacs
    regexp.  This is the same way commands like `query-replace-regexp'
    read input.

  • When the region is active, they use they the region contents, again
    literally (without any translation of string syntax).

  • With neither a prefix arg nor an active region, the behavior depends
    on whether the command expects an Emacs regexp or a PCRE one.

    Commands that take an Emacs regexp behave like `C-x C-e': they
    evaluate the sexp before point (which could be simply a string
    literal) and use its value. This is designed for use in Elisp
    buffers. As a special case, if point is *inside* a string, it's
    first moved to the string end, so in practice they should work as
    long as point is somewhere within the regexp literal.

    Commands that take a PCRE regexp try to read a Perl-style delimited
    regex literal *after* point in the current buffer, including its
    flags. For example, putting point before the `m' in the following
    example and doing `C-c / p e' (`rxt-pcre-to-elisp') displays
    `\(?:bar\|foo\)', correctly stripping out the whitespace and
    comment:

    ┌────
    │ $x =~ m/  foo   |  (?# comment) bar /x
    └────


    The PCRE reader currently only works with `/ ... /' delimiters. It
    will ignore any preceding `m', `s', or `qr' operator, as well as the
    replacement part of an `s' construction.

    Readers for other PCRE-using languages are on the TODO list.

  The translation functions display their result in the minibuffer and
  copy it to the kill ring. When translating something into Elisp
  syntax, you might need to use the result either literally (e.g. for
  interactive input to a command like `query-replace-regexp'), or as a
  string to paste into Lisp code.  To allow both uses,
  `rxt-pcre-to-elisp' copies both versions successively to the
  kill-ring. The literal regexp without string quoting is the top
  element of the kill-ring, while the Lisp string is the
  second-from-top. You can paste the literal regexp somewhere by doing
  `C-y', or the Lisp string by `C-y M-y'.


1.2.2 Syntax conversion commands
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  `rxt-convert-syntax' (`C-c / c') converts between Emacs and PCRE
  syntax, depending on the major mode in effect when called.
  Alternatively, you can specify the conversion direction explicitly by
  using either `rxt-pcre-to-elisp' (`C-c / p e') or `rxt-elisp-to-pcre'
  (`C-c / e p').

  Similarly, `rxt-convert-to-rx' (`C-c / x') converts either kind of
  syntax to `rx' form, while `rxt-convert-pcre-to-rx' (`C-c / p x') and
  `rxt-convert-elisp-to-rx' (`C-c / e x') convert to `rx' from a
  specified source type.

  In Elisp buffers, you can use `rxt-toggle-elisp-rx' (`C-c / t' or `C-c
  / e t') to switch the regexp at point back and forth between string
  and `rx' syntax. Point should either be within an `rx' or
  `rx-to-string' form or a string literal for this to work.


1.2.3 PCRE mode (experimental)
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  If you want to use emulated PCRE regexp syntax in all Emacs commands,
  try `pcre-mode', which uses Emacs's advice system to make all commands
  that read regexps using the minibuffer use emulated PCRE syntax.  It
  should also work with Isearch.

  This feature is still fairly experimental.  It may fail to work or do
  the wrong thing with certain commands.  Please report bugs.

  `pcre-query-replace-regexp' was originally defined to do query-replace
  using emulated PCRE regexps, and is now made somewhat obsolete by
  `pcre-mode'.  It is bound to `C-c / %' by default, by analogy with
  `M-%'.  Put the following in your `.emacs' if you want to use
  PCRE-style query replacement everywhere:

  ┌────
  │ (global-set-key [(meta %)] 'pcre-query-replace-regexp)
  └────


1.2.4 Explain regexps
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  When syntax-highlighting isn't enough to untangle some gnarly regexp
  you find in the wild, try the 'explain' commands: `rxt-explain' (`C-c
  / /'), `rxt-explain-pcre' (`C-c / p') and `rxt-explain-elisp' (`C-c /
  e'). These display the original regexp along with its pretty-printed
  `rx' equivalent in a new buffer.  Moving point around either in the
  original regexp or the `rx' translation highlights corresponding
  pieces of syntax, which can aid in seeing things like the scope of
  quantifiers.

  I call them "explain" commands because the `rx' form is close to a
  plain syntax tree, and this plus the wordiness of the operators
  usually helps to clarify what is going on.  People who dislike Lisp
  syntax might disagree with this assessment.


1.2.5 Generate all matching strings (productions)
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Occasionally you come across a regexp which is designed to match a
  finite set of strings, e.g. a set of keywords, and it would be useful
  to recover the original set. (In Emacs you can generate such regexps
  using `regexp-opt'). The commands `rxt-convert-to-strings' (`C-c /
  ′'), `rxt-pcre-to-strings' (`C-c / p ′') or `rxt-elisp-to-strings'
  (`C-c / e ′') accomplish this by generating all the matching strings
  ("productions") of a regexp.  (The productions are copied to the kill
  ring as a Lisp list).

  An example in Lisp code:

  ┌────
  │ (regexp-opt '("cat" "caterpillar" "catatonic"))
  │    ;; => "\\(?:cat\\(?:atonic\\|erpillar\\)?\\)"
  │ (rxt-elisp-to-strings "\\(?:cat\\(?:atonic\\|erpillar\\)?\\)")
  │     ;; => '("cat" "caterpillar" "catatonic")
  └────


  For obvious reasons, these commands only work with regexps that don't
  include any unbounded quantifiers like `+' or `*'. They also can't
  enumerate all the characters that match a named character class like
  `[[:alnum:]]'. In either case they will give a (hopefully meaningful)
  error message. Due to the nature of permutations, it's still possible
  for a finite regexp to generate a huge number of productions, which
  will eat memory and slow down your Emacs. Be ready with `C-g' if
  necessary.


1.2.6 RE-Builder support
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  The Emacs RE-Builder is a useful visual tool which allows using
  several different built-in syntaxes via `reb-change-syntax' (`C-c
  TAB'). It supports Elisp read and literal syntax and `rx', but it can
  only convert from the symbolic forms to Elisp, not the other way. This
  package hacks the RE-Builder to also work with emulated PCRE syntax,
  and to convert transparently between Elisp, PCRE and rx syntaxes. PCRE
  mode reads a delimited Perl-like literal of the form `/ ... /', and it
  should correctly support using the `x' and `s' flags.


1.2.7 Use from Lisp
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  Example of using the conversion functions:
  ┌────
  │ (rxt-pcre-to-elisp "(abc|def)\\w+\\d+")
  │    ;; => "\\(\\(?:abc\\|def\\)\\)[_[:alnum:]]+[[:digit:]]+"
  └────


  All the conversion functions take a single string argument, the regexp
  to translate:

  • `rxt-pcre-to-elisp'
  • `rxt-pcre-to-rx'
  • `rxt-pcre-to-sre'
  • `rxt-pcre-to-strings'
  • `rxt-elisp-to-pcre'
  • `rxt-elisp-to-rx'
  • `rxt-elisp-to-sre'
  • `rxt-elisp-to-strings'


1.3 Bugs and Limitations
────────────────────────

1.3.1 Limitations on PCRE syntax
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  PCRE has a complicated syntax and semantics, only some of which can be
  translated into Elisp. The following subset of PCRE should be
  correctly parsed and converted:

  • parenthesis grouping `( .. )', including shy matches `(?: ... )'
  • backreferences (various syntaxes), but only up to 9 per expression
  • alternation `|'
  • greedy and non-greedy quantifiers `*', `*?', `+', `+?', `?' and `??'
    (all of which are the same in Elisp as in PCRE)
  • numerical quantifiers `{M,N}'
  • beginning/end of string `\A', `\Z'
  • string quoting `\Q .. \E'
  • word boundaries `\b', `\B' (these are the same in Elisp)
  • single character escapes `\a', `\c', `\e', `\f', `\n', `\r', `\t',
    `\x', and `\octal digits' (but see below about non-ASCII characters)
  • character classes `[...]' including Posix escapes
  • character classes `\d', `\D', `\h', `\H', `\s', `\S', `\v', `\V'
    both within character class brackets and outside
  • word and non-word characters `\w' and `\W' (Emacs has the same
    syntax, but its meaning is different)
  • `s' (single line) and `x' (extended syntax) flags, in regexp
    literals, or set within the expression via `(?xs-xs)' or `(?xs-xs:
    .... )' syntax
  • comments `(?# ... )'

  Most of the more esoteric PCRE features can't really be supported by
  simple translation to Elisp regexps. These include the different
  lookaround assertions, conditionals, and the "backtracking control
  verbs" `(* ...)' . OTOH, there are a few other syntaxes which are
  currently unsupported and possibly could be:

  • `\L', `\U', `\l', `\u' case modifiers
  • `\g{...}' backreferences


1.3.2 Other limitations
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌

  • The order of alternatives and characters in char classes sometimes
    gets shifted around, which is annoying.
  • Although the string parser tries to interpret PCRE's octal and
    hexadecimal escapes correctly, there are problems with matching
    8-bit characters that I don't use enough to properly understand,
    e.g.:
    ┌────
    │ (string-match-p (rxt-pcre-to-elisp "\\377") "\377") => nil
    └────

    A fix for this would be welcome.

  • Most of PCRE's rules for how `^', `\A', `$' and `\Z' interact with
    newlines are not implemented, since they seem less relevant to
    Emacs's buffer-oriented rather than line-oriented model.  However,
    the different meanings of the `.' metacharacter *are* implemented
    (it matches newlines with the `/s' flag, but not otherwise).

  • Not currently namespace clean (both `rxt-' and a couple of `pcre-'
    functions).


1.3.3 TODO:
╌╌╌╌╌╌╌╌╌╌╌

  • Python-specific extensions to PCRE?
  • Language-specific stuff to enable regexp font-locking and explaining
    in different modes. Each language would need two functions, which
    could be kept in an alist:

    1. A function to read PCRE regexps, taking the string syntax into
       account. E.g., Python has single-quoted, double-quoted and raw
       strings, each with different quoting rules.  PHP has the kind of
       belt-and-suspenders solution you would expect: regexps are in
       strings, /and/ you have to include the `/ ...  /' delimiters!
       Duh.

    2. A function to copy faces back from the parsed string to the
       original buffer text. This has to recognize any escape sequences
       so they can be treated as a single character.


1.4 Internal details
────────────────────

  Internally, `rxt' defines an abstract syntax tree data type for
  regular expressions, parsers for Elisp and PCRE syntax, and
  "unparsers" from to PCRE, rx, and SRE syntax. Converting from a parsed
  syntax tree to Elisp syntax is a two-step process: first convert to
  `rx' form, then let `rx-to-string' do the heavy lifting.  See
  `rxt-parse-re', `rxt-adt->pcre', `rxt-adt->rx', and `rxt-adt->sre',
  and the section beginning "Regexp ADT" in pcre2el.el for details.

  This code is partially based on Olin Shivers' reference SRE
  implementation in scsh, although it is simplified in some respects and
  extended in others. See `scsh/re.scm', `scsh/spencer.scm' and
  `scsh/posixstr.scm' in the `scsh' source tree for details. In
  particular, `pcre2el' steals the idea of an abstract data type for
  regular expressions and the general structure of the string regexp
  parser and unparser. The data types for character sets are extended in
  order to support symbolic translation between character set
  expressions without assuming a small (Latin1) character set. The
  string parser is also extended to parse a bigger variety of
  constructions, including POSIX character classes and various Emacs and
  Perl regexp assertions. Otherwise, only the bare minimum of scsh's
  abstract data type is implemented.


1.5 Soapbox
───────────

  Emacs regexps have their annoyances, but it is worth getting used to
  them. The Emacs assertions for word boundaries, symbol boundaries, and
  syntax classes depending on the syntax of the mode in effect are
  especially useful. (PCRE has `\b' for word-boundary, but AFAIK it
  doesn't have separate assertions for beginning-of-word and
  end-of-word). Other things that might be done with huge regexps in
  other languages can be expressed more understandably in Elisp using
  combinations of `save-excursion' with the various searches (regexp,
  literal, skip-syntax-forward, sexp-movement functions, etc.).

  There's not much point in using `rxt-pcre-to-elisp' to use PCRE
  notation in a Lisp program you're going to maintain, since you still
  have to double all the backslashes.  Better to just use the converted
  result (or better yet, the `rx' form).


1.6 History and acknowledgments
───────────────────────────────

  This was originally created out of an answer to a stackoverflow
  question:
  <http://stackoverflow.com/questions/9118183/elisp-mechanism-for-converting-pcre-regexps-to-emacs-regexps>

  Thanks to:

  • Wes Hardaker (hardaker) for the initial inspiration and subsequent
    hacking
  • priyadarshan for requesting RX/SRE support
  • Daniel Colascione (dcolascione) for a patch to support Emacs's
    explicitly-numbered match groups
  • Aaron Meurer (asmeurer) for requesting Isearch support
  • Philippe Vaucher (silex) for a patch to support
    `ibuffer-do-replace-regexp' in PCRE mode
