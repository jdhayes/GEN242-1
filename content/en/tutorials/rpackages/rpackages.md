---
title: "Building R Packages" 
author: "Author: Thomas Girke"
date: "Last update: 02 June, 2021" 
output:
  html_document:
    toc: true
    toc_float:
        collapsed: true
        smooth_scroll: true
    toc_depth: 3
    fig_caption: yes
    code_folding: show
    number_sections: true

fontsize: 14pt
bibliography: bibtex.bib
weight: 16
type: docs
---

<!--
- Compile from command-line
Rscript -e "rmarkdown::render('rpackages.Rmd', c('html_document'), clean=F); knitr::knit('rpackages.Rmd', tangle=TRUE)"
-->
<script type="text/javascript">
document.addEventListener("DOMContentLoaded", function() {
  document.querySelector("h1").className = "title";
});
</script>
<script type="text/javascript">
document.addEventListener("DOMContentLoaded", function() {
  var links = document.links;  
  for (var i = 0, linksLength = links.length; i < linksLength; i++)
    if (links[i].hostname != window.location.hostname)
      links[i].target = '_blank';
});
</script>

<div style="text-align: right">

Source code downloads:    
\[ [Slides](https://girke.bioinformatics.ucr.edu/GEN242/slides/slides_19/) \]    
\[ [.Rmd](https://raw.githubusercontent.com/tgirke/GEN242//main/content/en/tutorials/rpackages/rpackages.Rmd) \]    
\[ [.R](https://raw.githubusercontent.com/tgirke/GEN242//main/content/en/tutorials/rpackages/rpackages.R) \]

</div>

## Overview

### Motivation for building R packages

1.  Organization
    -   Consolidate functions with related utilties in single place  
    -   Interdepencies among less complex functions make coding more efficient
    -   Minimizes duplications
2.  Documentation
    -   Help page infrastructure improves documentation of functions
    -   Big picture of utilties provided by package vignettes (manuals)
3.  Sharability
    -   Package can be easier shared with colleagues and public
    -   Increases code accessibilty for other users

### Package development environments

This following introduces two approaches for building R packages:

1.  `R Base` and related functionalities
2.  `devtools` and related packages (*e.g.* `usethis`, `roxygen2` and `sinew`)

The sample code provided below creates for each method a simple test package
that can be installed and loaded on a user’s system. The instructions for the
second appoach are more detailed since it is likely to provide the most
practical solution for newer users of R.

1.  Traditional approach using base R functionalities
2.  R package development with helper packages: `devtools`, `usethis`, `roxygen2` and `sinew`

The sample code provided below creates for each method a simple test package
that can be installed and loaded on a user’s system. The instructions for the
second appoach are more detailed since it is likely to provide the most
practical solution for newer users of R.

## 1. R Base *et al.* Approach

R packages can be built with the `package.skeleton` function. The most
comprehensive documentation on package development is provided by the [Writing
R
Extensions](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Package-Dependencies)
page on CRAN. The basic workflow example below will create a directory named
`mypackage` containing the skeleton of the package for all functions, methods
and classes defined in the R script(s) passed on to the `code_files` argument.
The basic structure of the package directory is described
[here](http://cran.fhcrc.org/doc/manuals/R-exts.html#Package-structure).
The package directory will also contain a file named `Read-and-delete-me` with
instructions for completing the package:

``` r
## Download R script (here pkg_build_fct.R) containing two sample functions
download.file("https://raw.githubusercontent.com/tgirke/GEN242/main/content/en/tutorials/rpackages/helper_functions/pkg_build_fct.R", "pkg_build_fct.R")
## Build package skeleton based on functions in pkg_build_fct.R
package.skeleton(name="mypackage", code_files=c("pkg_build_fct.R"))
```

Once a package skeleton is available one can build the package from the
command-line (Linux/OS X). This will create a tarball of the package with its
version number encoded in the file name. Subequently, the package tarball needs
to be checked for errors with:

``` r
system("R CMD build mypackage")
system("R CMD check mypackage_1.0.tar.gz")
```

Install package from source

``` r
install.packages("mypackage_1.0.tar.gz", repos=NULL) 
```

For more details see [here](http://manuals.bioinformatics.ucr.edu/home/programming-in-r#TOC-Building-R-Packages).

## 2. R `devtools` *et al.* Approach

Several package develpment routines of the traditional method outlined above
are manual, such as updating the NAMESPACE file and documenting functions in
separate help (`*.Rd`) files. This process can be simplified and partially
automated by taking advantage of a more recent R package development
environment composed of several helper packages including `devtools`,
`usethis`, `roxygen2` and `sinew` (Wickham and Bryan, n.d.). Many books and web sites document this process
in more detail. Here is a small selection of useful online documentation about
R package development:

-   Book: [R Packages](https://r-pkgs.org/index.html) by Hadley Wickham and Jenny Bryan  
-   [My First R Package](https://tinyheero.github.io/jekyll/update/2015/07/26/making-your-first-R-package.html) by Fong Chun Chan
-   [How to Creat an R Package, Easy Mode](https://www.amitkohli.com/2020/01/07/2020-01-07-how-to-create-an-r-package-my-way/) by Amit Kohli
-   [Package Development Cheat Sheet](https://rawgit.com/rstudio/cheatsheets/master/package-development.pdf)
-   Automating `roxygen2` documentation with `sinew` by Jonathan Sidi: [Blog](https://yonicd.github.io/2017-09-18-sinew/) and [CRAN](https://cran.r-project.org/web/packages/sinew/index.html)

## Workflow for building R packages

The following outlines the basic workflow for building, testing and extending R packages with
the package development environment functionalities outlined above.

### (a) Create package skeleton

``` r
library("devtools"); library("roxygen2"); library("usethis"); library(sinew) # If not availble install these packages with 'install.packages(...)'
create("myfirstpkg") # Creates package skeleton. The chosen name (here myfirstpkg) will be the name of the package.
setwd("myfirstpkg") # Set working directory of R session to package directory 'myfirstpkg'
use_mit_license() # Add license information to description file (here MIT). To look up alternatives, do ?use_mit_license
```

### (b) Add R functions

Next, R functions can be added to `*.R` file(s) under the R directory of the new
package. Several functions can be organized in one `*.R` file, each in its own
file or any combination. For demonstration purposes, the following will
download an R file (`pkg_build_fct.R` from [here](https://raw.githubusercontent.com/tgirke/GEN242/main/content/en/tutorials/rpackages/helper_functions/pkg_build_fct.R)) defining two functions (named:`myMAcomp`
and `talkToMe`) and save it to the R directory of the package.

``` r
download.file("https://raw.githubusercontent.com/tgirke/GEN242/main/content/en/tutorials/rpackages/helper_functions/pkg_build_fct.R", "R/pkg_build_fct.R")
```

### (c) Auto-generate roxygen comment lines

The `makeOxygen` function from the `sinew` package creates `roxygen2` comment
skeletons based on the information from each function (below for `myMAcomp` example).
The roxygen comment lines need to be added above the code of each function.
This can be done by copy and paste from the R console or by writing the output
to a temporary file (below via `writeLines`). Alternatively, the `makeOxyFile`
function can be used to create a roxygenized copy of an R source file, where the
roxygen comment lines have been added above all functions automatically. Next,
the default text in the comment lines needs to be replaced by meaningful text
describing the utility and usage of each function. This editing process of documentation
can be completed and/or revised any time.

``` r
load_all() # Loads package in a simulated way without installing it. 
writeLines(makeOxygen(myMAcomp), "myroxylines") # This creates a 'myroxylines' file in current directory. Delete this file after adding its content to the corresponding functions.
```

### (d) Autogenerate help files

The `document` function autogenerates for each function one `*.Rd` file in the
`man` directory of the package. The content in the `*.Rd` help files is based
on the information in the roxygen comment lines generated in the
previous step. In addition, all relevant export/import instructions are added
to the `NAMESPACE` file. Importantly, when using roxygen-based documentation in a package
then the `NAMESPACE` and `*.Rd` files should not be manually edited since this information
will be lost during the automation routines provided by `roxygen2`.

``` r
document() # Auto-generates/updates *.Rd files under man directory (here: myMAcomp.Rd and talkToMe.Rd)
tools::Rd2txt("man/myMAcomp.Rd") # Renders Rd file from source
tools::checkRd("man/myMAcomp.Rd") # Checks Rd file for problems
```

### (e) Add a vignette

A vignette template can be auto-generated with the `use_vignette` function from
the `usethis` package. The `*.Rmd` source file of the vignette will be located
under a new `vignette` directory. Additional vignettes can be manually added to
this directory as needed.

``` r
use_vignette("introduction", title="Introduction to this package")
```

### (f) Check, install and build package

Now the package can be checked for problems. All warnings and errors should be
addressed prior to submission to a public repository. After this it can be
installed on a user’s system with the `install` command. In addition, the
`build` function allows to assemble the package in a `*.tar.gz` file. The
latter is often important for sharing packages and/or submitting them to public
repositories.

``` r
setwd("..") # Redirect R session to parent directory
check("myfirstpkg") # Check package for problems, when in pkg dir one can just use check()
# remove.packages("myfirstpkg") # Optional. Removes test package if already installed
install("myfirstpkg", build_vignettes=TRUE) # Installs package  
build("myfirstpkg") # Creates *.tar.gz file for package required to for submission to CRAN/Bioc
```

### (g) Using the new package

After installing and loading the package its functions, help files and
vignettes can be accessed as follows.

``` r
library("myfirstpkg")
library(help="myfirstpkg")
?myMAcomp
vignette("introduction", "myfirstpkg")
```

Another very useful development function is `test` for evaluating the test code of a package.

### (h) Share package on GitHub

To host and share the new package `myfirstpkg` on GitHub, one can use the following steps:

1.  Create an empty target GitHub repos online (*e.g.* named `mypkg_repos`) as outlined [here](https://girke.bioinformatics.ucr.edu/GEN242/tutorials/github/github/#exercise).
2.  Clone the new GitHub repos to local system with `git clone https://github.com/<github_username>/<repo name>` (here from command-line)
3.  Copy the root directory of the package into the downloaded repos with `cp -r myfirstpkg mypkg_repos`  
4.  Next `cd` into `mypkg_repos`, and then add all files and directories of the package to the staging area with `git add -A :/`.
5.  Commit and push the changes to GitHub with: `git commit -am "first commit"; git push`.
6.  After this the package should be life on the corresponding GitHub web page.
7.  Assuming the package is public, it can be installed directly from GitHub by anyone as shown below (from within R). Installs of
    private packages require a personal access token (PAT) that needs to be assigned to the `auth_token` argument. PATs can be created
    [here](https://github.com/settings/tokens).

``` r
devtools::install_github("<github_user_name>/<mypkg_repos>", subdir="myfirstpkg") # If the package is in the root directory of the repos, then the 'subdir' argument can be dropped.
```

## Session Info

``` r
sessionInfo()
```

    ## R version 4.1.0 (2021-05-18)
    ## Platform: x86_64-pc-linux-gnu (64-bit)
    ## Running under: Debian GNU/Linux 10 (buster)
    ## 
    ## Matrix products: default
    ## BLAS:   /usr/lib/x86_64-linux-gnu/blas/libblas.so.3.8.0
    ## LAPACK: /usr/lib/x86_64-linux-gnu/lapack/liblapack.so.3.8.0
    ## 
    ## locale:
    ##  [1] LC_CTYPE=en_US.UTF-8       LC_NUMERIC=C               LC_TIME=en_US.UTF-8       
    ##  [4] LC_COLLATE=en_US.UTF-8     LC_MONETARY=en_US.UTF-8    LC_MESSAGES=en_US.UTF-8   
    ##  [7] LC_PAPER=en_US.UTF-8       LC_NAME=C                  LC_ADDRESS=C              
    ## [10] LC_TELEPHONE=C             LC_MEASUREMENT=en_US.UTF-8 LC_IDENTIFICATION=C       
    ## 
    ## attached base packages:
    ## [1] stats     graphics  grDevices utils     datasets  methods   base     
    ## 
    ## other attached packages:
    ## [1] ggplot2_3.3.3    limma_3.48.0     BiocStyle_2.20.0
    ## 
    ## loaded via a namespace (and not attached):
    ##  [1] bslib_0.2.5.1       compiler_4.1.0      pillar_1.6.1        BiocManager_1.30.15
    ##  [5] jquerylib_0.1.4     tools_4.1.0         digest_0.6.27       jsonlite_1.7.2     
    ##  [9] evaluate_0.14       lifecycle_1.0.0     tibble_3.1.2        gtable_0.3.0       
    ## [13] pkgconfig_2.0.3     rlang_0.4.11        DBI_1.1.1           yaml_2.2.1         
    ## [17] blogdown_1.3        xfun_0.23           withr_2.4.2         stringr_1.4.0      
    ## [21] dplyr_1.0.6         knitr_1.33          generics_0.1.0      sass_0.4.0         
    ## [25] vctrs_0.3.8         tidyselect_1.1.1    grid_4.1.0          glue_1.4.2         
    ## [29] R6_2.5.0            fansi_0.4.2         rmarkdown_2.8       bookdown_0.22      
    ## [33] purrr_0.3.4         magrittr_2.0.1      codetools_0.2-18    scales_1.1.1       
    ## [37] htmltools_0.5.1.1   ellipsis_0.3.2      assertthat_0.2.1    colorspace_2.0-1   
    ## [41] utf8_1.2.1          stringi_1.6.2       munsell_0.5.0       crayon_1.4.1

## References

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-Wickham_undated-ei" class="csl-entry">

Wickham, Hadley, and Jennifer Bryan. n.d. “R Packages.” <https://r-pkgs.org/index.html>. <https://r-pkgs.org/index.html>.

</div>

</div>
