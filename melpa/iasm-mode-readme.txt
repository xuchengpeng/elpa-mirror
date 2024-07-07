Inspired by Justine Tunney's disaster.el (http://github.com/jart/disaster‎).

iasm provides a simple interactive interface objdump and ldd which
facilitates assembly exploration. It also provides tools to speed up the
edit-compile-disasm loop.

This mode currently only supports Linux because it relies rather heavily on
objdump and ldd. It also hasn't been tested for other CPU architectures or
other unixes so expect some of the regexes to spaz out in colourful ways.

Note that this is my first foray into elisp so monstrosities abound. Go forth
at your own peril. If you wish to slay the beasts that lurk within or simply
add a few functionalities, contributions are more then welcome. See the todo
section for ideas.
