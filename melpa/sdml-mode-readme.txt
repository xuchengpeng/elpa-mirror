
This package provides a tree-sitter based major mode for SDML.


        ___          _____          ___
       /  /\        /  /::\        /__/\
      /  /:/_      /  /:/\:\      |  |::\
     /  /:/ /\    /  /:/  \:\     |  |:|:\    ___     ___
    /  /:/ /::\  /__/:/ \__\:|  __|__|:|\:\  /__/\   /  /\
   /__/:/ /:/\:\ \  \:\ /  /:/ /__/::::| \:\ \  \:\ /  /:/
   \  \:\/:/~/:/  \  \:\  /:/  \  \:\~~\__\/  \  \:\  /:/
    \  \::/ /:/    \  \:\/:/    \  \:\         \  \:\/:/
     \__\/ /:/      \  \::/      \  \:\         \  \::/
       /__/:/        \__\/        \  \:\         \__\/
       \__\/          Domain       \__\/          Language
        Simple                      Modeling



Installing

Install is easiest from MELPA, here's how with `use-package`.

`(use-package sdml-mode)'

Or, interactively; `M-x package-install RET sdml-ispell RET'


Usage

Once installed the major mode should be used for any file ending in `.sdm'
or `.sdml' with highlighting and indentation support.

Abbreviations and Skeletons

This package creates a new `abbrev-table', named `sdml-mode-abbrev-table', which
provides a number of useful skeletons for the following.  `abbrev-mode' is enabled
by `sdml-mode' and when typing one of the abbreviations below type space to
expand.

Typing `d t SPCA' will prompt for a name and expand into the SDML declaration
`datatype MyName ← opaque _' where the underscore character represents the new
cursor position.

Declarations: mo=module, dt=datatype, en=enum, ev=event, pr=property,
  st=structure, un=union

Annotation Properties: pal=skos:altLabel, pdf=skos:definition,
  ped=skos:editorialNote, ppl=skos:prefLabel, pco=rdfs:comment

Constraints: ci=informal, cf=formal, all=universal, any=existential

Datatypes: db=boolean, dd=decimal, df=double, dh=binary, di=integer,
  sd=string, du=unsigned

Interactive Commands


`sdml-mode-validate-current-buffer' (\\[sdml-mode-validate-current-buffer]) to
validate and show errors for the current buffer's module.

Adding this as a save-hook allows validation on every save of a buffer.

`(add-hook 'after-save-hook 'sdml-validate-current-buffer)'

`sdml-mode-validate-file' (\\[sdml-mode-validate-file]) to
validate and show errors for a specified file name.

`sdml-mode-current-buffer-dependency-tree' (\\[sdml-mode-current-buffer-dependency-tree])
to display the dependencies of the current buffer's module as a textual tree.

`sdml-mode-current-buffer-dependency-graph' (\\[sdml-mode-current-buffer-dependency-graph])
to display the dependencies of the current buffer's module as an SVG directed graph.


Debug

`\\[tree-sitter-debug-mode]' -- open tree-sitter debug view
`\\[tree-sitter-query-builder]' -- open tree-sitter query builder

Extensions

`flycheck-sdml' -- Integrate the lisp-based linter with sdml-mode.
`ob-sdml' -- Support SDML org-mode Babel blocks
`sdml-fold' -- Provide code-folding for SDML source.
`sdml-ispell' -- Provide spell checking for specific SDML Tree-Sitter nodes.
