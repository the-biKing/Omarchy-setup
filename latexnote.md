# Try to achieve xelatex mandarin rendering in Omarchy


- [Starting Point](https://github.com/basecamp/omarchy/discussions/1720#discussioncomment-14599474)

- [latexmk document](https://texdoc.org/serve/latexmk.man1.pdf/0)
    >$pdf_mode [0]  
    If zero, do NOT generate a pdf version of the document. If equal to 1, generate a pdf
    version of the document using pdflatex, using the command specified by the $pdflatex
    variable. If equal to 2, generate a pdf version of the document from the ps file, by using
    the command specified by the $ps2pdf variable. If equal to 3, generate a pdf version of
    the document from the dvi file, by using the command specified by the $dvipdf variable.
    If equal to 4, generate a pdf version of the document using lualatex, using the command
    specified by the $lualatex variable. If equal to 5, generate a pdf version (and an xdv
    version) of the document using xelatex, using the commands specified by the $xelatex
    and xdvipdfmx variables.
    In $pdf_mode=2, it is ensured that .dvi and .ps files are also made. In $pdf_mode=3, it is
    ensured that a .dvi file is also made. But this may be overridden by the document.  

    For xelatex mode should be 5

    ```
    $pdf_mode = 5;                   # xelatex mode
    $synctex = 1;
    $interaction = "nonstopmode";
    $aux_dir = "build";
    $out_dir = "build";
    $cleanup_includes_generated = 1;
    $xelatex = 'xelatex -file-line-error -shell-escape -synctex=1 %O %S';
    ```
- [Vimtex document](https://github.com/lervag/vimtex/blob/master/doc/vimtex.txt)  
      **g:vimtex_compiler_latexmk**  
      This dictionary allows customization of the |vimtex-compiler-latexmk|
      compiler. The values set by the user will take precedence over the default
      values.

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

    options ~
    This is a list of options that are passed to `latexmk`. The default
    options should work well for most people.

    Note: Options like `-pdf` or `-lualatex` should NOT be added to this list.
          These are options used to specify the LaTeX processor/engine, see
          instead |g:vimtex_compiler_latexmk_engines|.

    Note: Options may also be specified indirectly to `latexmk` through both
          a global and a project specific `.latexmkrc` file. One should know,
          though, that options specified on the command line has priority, and
          so if one wants to override one of the above default options, then
          one has to set this key to a list that contains the desired options.
  
    **g:vimtex_compiler_latexmk_engines**  
        Defines a map between TeX program directive (|vimtex-tex-program|) and
        compiler engine. This is used by |vimtex-compiler-latexmk| to define the
        LaTeX program. The `_` key defines the default engine.  
        Note: If the TeX program directive is not specified within the main project
              file, and if `$pdf_mode` is added to a project-specific `.latexmkrc`
              file, then the compiler engine will be deduced from the value of
              `$pdf_mode`. The supported values of `$pdf_mode` are 1 (pdflatex), 4
              (lualatex) and 5 (xelatex). See the latexmk documentation for details.
    
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
  - Possible modification in latex.lua
    ```
    -- Compile with latexmk into ./build with synctex + nonstop mode
      vim.g.vimtex_compiler_method = "latexmk"
      vim.g.vimtex_compiler_latexmk_engine ={
      "xelatex"          : "-xelatex"
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
