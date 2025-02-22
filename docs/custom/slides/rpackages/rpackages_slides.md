---
title: "Building R Packages"
author: Thomas Girke
date: May 25, 2021
output: 
  ioslides_presentation:
    keep_md: yes
    logo: ./images/ucr_logo.png
    widescreen: yes
    df_print: paged
    smaller: true
subtitle: "Short Overview with Hands-on Example" 
bibliography: bibtex.bib
---

<!---
- ioslides manual: 
   https://bookdown.org/yihui/rmarkdown/ioslides-presentation.html

- Compile from command-line
Rscript -e "rmarkdown::render('rpackages_slides.Rmd'); knitr::knit('rpackages_slides.Rmd', tangle=TRUE)"
-->

<!---
  Note: following css chunks are required for scrolling support beyond slide boundaries
-->

<style>
slides > slide {
  overflow-x: auto !important;
  overflow-y: auto !important;
}
</style>

<style type="text/css">
pre {
  max-height: 300px;
  overflow-y: auto;
}

pre[class] {
  max-height: 300px;
}
</style>

<style type="text/css">
.scroll-300 {
  max-height: 300px;
  overflow-y: auto;
  background-color: inherit;
}
</style>

## Online Sign-in Form

<br/><br/>
<br/><br/>
<br/><br/>

<p style='text-align: center;'> __The Sign in Form is [here](https://bit.ly/3ufjfYA)__ </p>


## How to Navigate this Slide Show?

<br/>

- This __ioslides__ presentation contains scrollable slides. 
- Which slides are scrollable, is indicated by a tag at the bottom of the corresponding slides stating: 

<p style='text-align: center;'> __[ Scroll down to continue ]__ </p>

- The following single character keyboard shortcuts enable alternate display modes of __ioslides__:
    - `f`: enable fullscreen mode
    - `w`: toggle widescreen mode
    - `o`: enable overview mode
    - `h`: enable code highlight mode
- Pressing Esc exits all of these modes. Additional details can be found [here](https://bookdown.org/yihui/rmarkdown/ioslides-presentation.html).

# Outline

- <div class="white">__Overview__</div>
- Package Development with `R Base` _et al._
- Package Development with `devtools` _et al._
- References


## Overview

### Motivation for building R packages

1. Organization
    - Consolidate functions with related utilties in single place  
    - Interdepencies among less complex functions make coding more efficient
    - Minimizes duplications
2. Documentation
    - Help page infrastructure improves documentation of functions
    - Big picture of utilties provided by package vignettes (manuals) 
3. Sharability 
    - Package can be easier shared with colleagues and public
    - Increases code accessibilty for other users

## Package development environments

<br/><br/>
This following introduces two approaches for building R packages: 
<br/><br/>

1. `R Base` and related functionalities 
2. `devtools` and related packages (_e.g._ `usethis`, `roxygen2` and `sinew`)

<br/><br/>
The sample code provided below creates for each method a simple test package
that can be installed and loaded on a user's system. The instructions for the
second appoach are more detailed since it is likely to provide the most
practical soluton for newer users of R.

# Outline

- Overview
- <div class="white">Package Development with `R Base` _et al._<div class="white">
- Package Development with `devtools` _et al._
- References


## R Base _et al._ 

- R packages can be built with the `package.skeleton` function. The most
  comprehensive documentation on package development is provided by the [Writing R Extensions](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Package-Dependencies)
  page on CRAN. 
- The basic workflow example below will create a directory named `mypackage`
  containing the skeleton of the package for all functions, methods and classes
  defined in the R script(s) passed on to the `code_files` argument.
- The basic structure of the package directory is described
  [here](http://cran.fhcrc.org/doc/manuals/R-exts.html#Package-structure).
- The package directory will also contain a file named `Read-and-delete-me`
  with instructions for completing the package:


```r
## Download R script (here pkg_build_fct.R) containing two sample functions                                                                                                         
download.file("https://raw.githubusercontent.com/tgirke/GEN242/main/content/en/manuals/rprogramming/helper_functions/pkg_build_fct.R", "pkg_build_fct.R")                           
## Build package skeleton based on functions in pkg_build_fct.R                                                                                                                     
package.skeleton(name="mypackage", code_files=c("pkg_build_fct.R")) 
```

- Once a package skeleton is available one can build the package from the
  command-line (Linux/OS X). 
- This will create a tarball of the package with its version number encoded in
  the file name. Subequently, the package tarball needs to be checked for
  errors with:

<p style='text-align: right;'> __[ Scroll down to continue ]__ </p>
<br/><br/>
<br/><br/>


```r
system("R CMD build mypackage")
system("R CMD check mypackage_1.0.tar.gz") 
```

Install package from source

```r
install.packages("mypackage_1.0.tar.gz", repos=NULL) 
```

# Outline

- Overview
- Package Development with `R Base` _et al._
- <div class="white">Package Development with `devtools` _et al._<div class="white">
- References

## R `devtools` _et al._ 

Several package develpment routines of the traditional method outlined above
are manual, such as updating the NAMESPACE file and documenting functions in
separate help (`*.Rd`) files. This process can be simplified and partially
automated by taking advantage of a more recent R package development
environment composed of several helper packages including `devtools`,
`usethis`, `roxygen2` and `sinew` [@Wickham_undated-ei]. Many books and web sites document this process
in more detail. Here is a small selection of useful online documentation about 
R package development:

+ Book: [R Packages](https://r-pkgs.org/index.html) by Hadley Wickham and Jenny Bryan   
+ [My First R Package](https://tinyheero.github.io/jekyll/update/2015/07/26/making-your-first-R-package.html) by Fong Chun Chan
+ [How to Creat an R Package, Easy Mode](https://www.amitkohli.com/2020/01/07/2020-01-07-how-to-create-an-r-package-my-way/) by Amit Kohli
+ [Package Development Cheat Sheet](https://rawgit.com/rstudio/cheatsheets/master/package-development.pdf)
+ Automating `roxygen2` documentation with `sinew` by Jonathan Sidi: [Blog](https://yonicd.github.io/2017-09-18-sinew/) and [CRAN](https://cran.r-project.org/web/packages/sinew/index.html)

## Workflow for building R packages 

The following outlines the basic workflow for building, testing and extending R packages with 
the package development environment functionalities outlined above. 

### (a) Create package skeleton


```r
library("devtools"); library("roxygen2"); library("usethis"); library(sinew) # If not availble install these packages with 'install.packages(...)'
create("myfirstpkg") # Creates package skeleton. The chosen name (here myfirstpkg) will be the name of the package.
setwd("myfirstpkg") # Set working directory of R session to package directory 'myfirstpkg'
use_mit_license() # Add license information to description file (here MIT). To look up alternatives, do ?use_mit_license
```

### (b) Add R functions 

Next, R functions can be added to `*.R` file(s) under the R directory of the new
package. Several functions can be organized in one `*.R` file, each in its own
file or any combination. For demonstration purposes, the following will
download an R file (`pkg_build_fct.R` from [here](https://bit.ly/3hK0mdH)) defining two functions (named:`myMAcomp` 
and `talkToMe`) and save it to the R directory of the package.

<p style='text-align: right;'> __[ Scroll down to continue ]__ </p>
<br/><br/>


```r
download.file("https://raw.githubusercontent.com/tgirke/GEN242/main/content/en/manuals/rprogramming/helper_functions/pkg_build_fct.R", "R/pkg_build_fct.R")
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


```r
load_all() # Loads package in a simulated way without installing it. 
writeLines(makeOxygen(myMAcomp), "myroxylines") # This creates a 'myroxylines' file in current directory. Delete this file after adding its content to the corresponding functions.
```

### (d) Autogenerate help files 

The `document` function autogenerates for each function one `*.Rd` file in the
`man` directory of the package.  The content in the `*.Rd` help files is based
on the information in the roxygen comment lines generated in the
previous step. In addition, all relevant export/import instructions are added
to the `NAMESPACE` file.  Importantly, when using roxygen-based documentation in a package
then the `NAMESPACE` and `*.Rd` files should not be manually edited since this information
will be lost during the automation routines provided by `roxygen2`.


```r
document() # Auto-generates/updates *.Rd files under man directory (here: myMAcomp.Rd and talkToMe.Rd)
tools::Rd2txt("man/myMAcomp.Rd") # Renders Rd file from source
tools::checkRd("man/myMAcomp.Rd") # Checks Rd file for problems
```

### (e) Add a vignette

A vignette template can be auto-generated with the `use_vignette` function from
the `usethis` package. The `*.Rmd` source file of the vignette will be located
under a new `vignette` directory. Additional vignettes can be manually added to
this directory as needed.


```r
use_vignette("introduction", title="Introduction to this package")
```

### (f) Check, install and build package

Now the package can be checked for problems. All warnings and errors should be
addressed prior to submission to a public repository.  After this it can be
installed on a user's system with the `install` command. In addition, the
`build` function allows to assemble the package in a `*.tar.gz` file. The
latter is often important for sharing packages and/or submitting them to public 
repositories.  


```r
setwd("..") # Redirect R session to parent directory
check("myfirstpkg") # Check package for problems, when in pkg dir one can just use check()
# remove.packages("myfirstpkg") # Optional. Removes test package if already installed
install("myfirstpkg", build_vignettes=TRUE) # Installs package  
build("myfirstpkg") # Creates *.tar.gz file for package required to for submission to CRAN/Bioc
```

### (g) Using the new package

After installing and loading the package its functions, help files and
vignettes can be accessed as follows. 


```r
library("myfirstpkg")
library(help="myfirstpkg")
?myMAcomp
vignette("introduction", "myfirstpkg")
```

Another very useful development function is `test` for evaluating the test code of a package.

### (h) Share package on GitHub

To host and share the new package `myfirstpkg` on GitHub, one can use the following steps:

1. Create an empty target GitHub repos online (_e.g._ named `mypkg_repos`) as outlined [here](https://girke.bioinformatics.ucr.edu/GEN242/tutorials/github/github/#exercise). 
2. Clone the new GitHub repos to local system with `git clone https://github.com/<github_username>/<repo name>` (here from command-line)
3. Copy the root directory of the package into the downloaded repos with `cp myfirstpkg mypkg_repos`  
4. Next `cd` into `mypkg_repos`, and then add all files and directories of the package to the staging area with `git add -A :/`. 
5. Commit and push the changes to GitHub with: `git commit -am "first commit"; git push`. 
6. After this the package should be life on the corresponding GitHub web page.
7. Assuming the package is public, it can be installed directly from GitHub by anyone as shown below (from within R). Installs of
   private packages require a personal access token (PAT) that needs to be assigned to the `auth_token` argument. PATs can be created
   [here](https://github.com/settings/tokens).


```r
devtools::install_github("<github_user_name>/<mypkg_repos>", subdir="myfirstpkg") # If the package is in the root directory of the repos, then the 'subdir' argument can be dropped.
```

# Outline

- Overview
- Package Development with `R Base` _et al._
- Package Development with `devtools` _et al._
- <div class="white">References<div class="white">

## Session Info


```r
sessionInfo()
```

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
##  [1] LC_CTYPE=en_US.UTF-8       LC_NUMERIC=C              
##  [3] LC_TIME=en_US.UTF-8        LC_COLLATE=en_US.UTF-8    
##  [5] LC_MONETARY=en_US.UTF-8    LC_MESSAGES=en_US.UTF-8   
##  [7] LC_PAPER=en_US.UTF-8       LC_NAME=C                 
##  [9] LC_ADDRESS=C               LC_TELEPHONE=C            
## [11] LC_MEASUREMENT=en_US.UTF-8 LC_IDENTIFICATION=C       
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## loaded via a namespace (and not attached):
##  [1] digest_0.6.27     R6_2.5.0          jsonlite_1.7.2    magrittr_2.0.1   
##  [5] evaluate_0.14     rlang_0.4.11      stringi_1.6.2     jquerylib_0.1.4  
##  [9] bslib_0.2.5.1     rmarkdown_2.8     tools_4.1.0       stringr_1.4.0    
## [13] xfun_0.23         yaml_2.2.1        compiler_4.1.0    htmltools_0.5.1.1
## [17] knitr_1.33        sass_0.4.0
```

## References



