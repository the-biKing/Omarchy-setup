# Try to Achieve XeLaTeX Mandarin Rendering in Omarchy

## Starting Point

* [Discussion on Omarchy GitHub](https://github.com/basecamp/omarchy/discussions/1720#discussioncomment-14599474)

---

## latexmk Document

From [latexmk manual](https://texdoc.org/serve/latexmk.man1.pdf/0):

```text
$pdf_mode [0]  
If zero, do NOT generate a pdf version of the document. If equal to 1, generate a pdf
version of the document using pdflatex, using the command specified by the $pdflatex
variable. If equal to 2, generate a pdf version of the document from the ps file, by using
the command specified by the $ps2pdf variable. If equal to 3, generate a pdf version of
the document from the dvi file, using the command specified by the $dvipdf variable.
If equal to 4, generate a pdf version of the document using lualatex, using the command
specified by the $lualatex variable. If equal to 5, generate a pdf version (and an xdv
version) of the document using xelatex, using the commands specified by the $xelatex
and xdvipdfmx variables.
In $pdf_mode=2, it is ensured that .dvi and .ps files are also made. In $pdf_mode=3,
it is ensured that a .dvi file is also made. But this may be overridden by the document.
```

* **For XeLaTeX**, `$pdf_mode` should be `5`:

```perl
$pdf_mode = 5;                   # xelatex mode
$synctex = 1;
$interaction = "nonstopmode";
$aux_dir = "build";
$out_dir = "build";
$cleanup_includes_generated = 1;
$xelatex = 'xelatex -file-line-error -shell-escape -synctex=1 %O %S';
```

---

## Vimtex Document

From [Vimtex documentation](https://github.com/lervag/vimtex/blob/master/doc/vimtex.txt):

### `g:vimtex_compiler_latexmk`

This dictionary allows customization of the `vimtex-compiler-latexmk` compiler. User values take precedence over defaults.

```vim
let g:vimtex_compiler_latexmk = {
      \ 'aux_dir' : '',
      \ 'out_dir' : '',
      \ 'callback' : 1,
      \ 'continuous' : 1,
      \ 'executable' : 'latexmk',
      \ 'hooks' : [],
      \ 'options' : [
      \   '-verbose',
      \   '-file-line-error',
      \   '-synctex=1',
      \   '-interaction=nonstopmode',
      \ ],
      \}
```

> **Note:**
> Options like `-pdf` or `-lualatex` **should NOT** be added here. Use `g:vimtex_compiler_latexmk_engines` instead.

---

### `g:vimtex_compiler_latexmk_engines`

Defines a mapping between TeX program directives and compiler engines. The `_` key defines the default engine.

```vim
let g:vimtex_compiler_latexmk_engines = {
      \ '_'                : '-pdf',
      \ 'pdfdvi'           : '-pdfdvi',
      \ 'pdfps'            : '-pdfps',
      \ 'pdflatex'         : '-pdf',
      \ 'luatex'           : '-lualatex',
      \ 'lualatex'         : '-lualatex',
      \ 'xelatex'          : '-xelatex',
      \ 'context (pdftex)' : '-pdf -pdflatex=texexec',
      \ 'context (luatex)' : '-pdf -pdflatex=context',
      \ 'context (xetex)'  : '-pdf -pdflatex=''texexec --xtx''',
      \}
```

> **Note:**
> If the TeX program directive is not specified in the main project file, the compiler engine is deduced from `$pdf_mode`.

---

## Possible Modification in `latex.lua`

```lua
-- Compile with latexmk into ./build with synctex + nonstop mode
vim.g.vimtex_compiler_method = "latexmk"

vim.g.vimtex_compiler_latexmk_engines = {
  _ = "-xelatex",
  xelatex = "-xelatex",
}

vim.g.vimtex_compiler_latexmk = {
  build_dir = "build",
  options = {
    "-interaction=nonstopmode",
    "-synctex=1",
    "-file-line-error",
    "-shell-escape",
  },
}
```

