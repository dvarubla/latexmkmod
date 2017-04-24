This script is a modification of the  [original latexmk tool](http://personal.psu.edu/jcc8//software/latexmk-jcc/).

New features (mainly for out of source builds):

 1. It is possible to intercept all custom dependencies handling. To do that add an 'all' dependency handler: `add_cus_dep('all_cus_dep')`.
 2. Transforming custom dependencies source names: `set_cus_dep_transform('custom_transform_cus_dep')`. This subroutine will be called with two arguments: a source file name and a boolean flag. The flag indicates that it necessary to decide if the handling of this depedency is intercepted. If you don't want to do this and the flag is set — return an empty string, else return a modified file name (or the same file name).
 3. Custom dependencies subroutines now have two extra arguments `$cusdep_source` and `$cusdep_dest`.

The bug with not all the files cleaned with `-c` flag is also fixed.

Installation on Windows with Texlive:

 1. Copy `latexmkmod.pl` to the `TEXMFLOCAL`\scripts directory (C:\texlive\texmf-local\scripts on my PC) Use `kpsewhich -var-value TEXMFLOCAL` to show your `TEXMFLOCAL` variable. If there is no 'scripts' directory create it.
 2. Copy runscript.exe to the latexmkmod.exe in the same directory (C:\texlive\2016\bin\win32, it is usually in `PATH`)
 3. Run `mktexlsr` in cmd.exe (you may need to run it as Administrator)