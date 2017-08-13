# Latexmkmod

This script is a modification of the  [latexmk tool](http://personal.psu.edu/jcc8//software/latexmk-jcc/). It is backwards compatible with the original script.

## New features

### Transforming custom dependencies names
Use `set_cus_dep_transform('custom_transform_cus_dep')`. Subroutine `custom_transform_cus_dep` will be called with two arguments: a file name and a boolean flag (0 or 1). (For example, if you write `\includegraphics{inc/img/file.pdf}` and you have a custom dependency that converts svgs to pdfs the first argument will be `inc/img/file.svg` string)

If the flag is 0 return either a modified source file name or the same source file name. It is useful when your source files (svgs for example) are in different folder than destination files (pdfs). You should return `myfolder/file.svg` then.

If the flag is 1 it necessary to decide if the handling of this depedency is intercepted. If you don't want to do this return an empty string. Else return either a modified file name or the same source file name or an array of a new source file name and a new destination file name.

### Intercepting dependency handling
It is possible to intercept all custom dependencies handling. To do that add an 'all' dependency handler: `add_cus_dep('all_cus_dep')`. All dependencies which handling needs to be intercepted (see previous section) go to this function. This is useful when your dependency sources have many different extensions (when they have only one extension it is better to use `add_cus_dep`). For example, your source files have .c, .cpp, .h, .py, etc extensions and you want to change encoding of all of them.

### Extra arguments
Custom dependency subroutines (including a hanlder for 'all' dependencies) now have two extra (`local`) arguments `$cusdep_source` and `$cusdep_dest`.

### Custom command line options
It is possible to handle command line options that come after `-r` option and don't exist in original latexmk. To do that, use `set_cus_option_handler('my_option_handler')` code. This subroutine will be called with three arguments: `$type`, `$arg`, `$argv`.

If `$type` is 0, `$arg` is an option, which is unknown to latexmk. Return either 0 if you want to handle it or 1 if it is a bad option. `$argv` contains command line arguments specified after this option, you can modify it.

If `$type` is 1, `$arg` is a filename. Return either a new file name or the same file name, or an empty string to skip it. `$argv` contains command line arguments specified after this file name, you can modify it.

If `$type` is 2, that means all command line options have been processed. You can do something depending on received options and file names.

If you call `set_cus_option_handler('my_option_handler', 'my_help_print')`, a subroutine `my_help_print` will be called when the `--help` option is specified. You can print a description of your custom options in this function.

### Bug fixes
There are some bugs fixed. For example the bug with not all the files cleaned with `-c` flag.

## Installation

To run this script on Windows you need to install Perl and add the directory containing perl.exe to `PATH`. Perl is usually included in TeX Live, on my PC it is in `c:\texlive\2017\tlpkg\tlperl\bin` folder. Then use the command `perl latexmkmod.pl foo.tex` in Windows terminal (cmd.exe).

Another way of installation, that makes it possible to run latexmkmod without typing "perl" (just `latexmkmod foo.tex`):

 1. Copy `latexmkmod.pl` to the `TEXMFLOCAL`\scripts directory (C:\texlive\texmf-local\scripts on my PC) Use `kpsewhich -var-value TEXMFLOCAL` to show your `TEXMFLOCAL` variable. If there is no 'scripts' directory create it.
 2. Copy runscript.exe to the latexmkmod.exe in the same directory (C:\texlive\2017\bin\win32, it is usually in `PATH`)
 3. Run `mktexlsr`from the same directory as Administrator
