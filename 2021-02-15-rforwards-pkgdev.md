# Packages in a nutshell. A Forwards Package Development module

[Emma's slides](https://3mmarand.github.io/workshops/package-dev-modules/slides/01-packages-in-a-nutshell/packages-in-a-nutshell.html)

Some commands:

```
> R.version
               _                           
platform       x86_64-apple-darwin17.0     
arch           x86_64                      
os             darwin17.0                  
system         x86_64, darwin17.0          
status                                     
major          4                           
minor          0.3                         
year           2020                        
month          10                          
day            10                          
svn rev        79318                       
language       R                           
version.string R version 4.0.3 (2020-10-10)
nickname       Bunny-Wunnies Freak Out     

> R.home()
[1] "/Library/Frameworks/R.framework/Resources"
> list.files(R.home())
 [1] "bin"          "COPYING"      "doc"         
 [4] "etc"          "fontconfig"   "include"     
 [7] "Info.plist"   "lib"          "library"     
[10] "man1"         "modules"      "R"           
[13] "Rscript"      "share"        "SVN-REVISION"
[16] "tests"   

> .Library
[1] "/Library/Frameworks/R.framework/Resources/library"

> dir(.Library)
[1] "/Library/Frameworks/R.framework/Resources/library"

>  .libPaths()
[1] "/Library/Frameworks/R.framework/Versions/4.0/Resources/library"

> dim(installed.packages())
[1] 182  16

> View(installed.packages())
shows pkgs in table format

> colnames(installed.packages())
 [1] "Package"               "LibPath"              
 [3] "Version"               "Priority"             
 [5] "Depends"               "Imports"              
 [7] "LinkingTo"             "Suggests"             
 [9] "Enhances"              "License"              
[11] "License_is_FOSS"       "License_restricts_use"
[13] "OS_type"               "MD5sum"               
[15] "NeedsCompilation"      "Built"   
```

- imports don't need to be loaded with `library` statemetn
- depends are needed by pkg to work
- suggests - for examples used in pkg

## Scripts vs Packages
### Script
- .R
- library() calls
- docs in # comments
- source()

### Package
- define reusable components
- a folder rather than single file
- DESCRIPTION file makes it a package
  - here you list depends, your name, pkg descr, license
  - from https://r-pkgs.org/description.html, "“Every package must have a DESCRIPTION. In fact, it’s the defining feature of a package (RStudio and devtools consider any directory containing DESCRIPTION to be a package)”
- NAMESPACE makes packages available for your pkg to use
- documentation Roxygen2 formats code commends in Rd format which is tricky to write in
- install and restart

## Explore a package
See [demo package](https://github.com/forwards/workshops/tree/master/Chicago2019/demoPackage) by Stephanie Kirmer 
- devtools creates packages infrastructure
- `#'` is Roxygen format for comment

## Package states
There are five states a package can be in. [helpful diagram](https://github.com/hadley/r-pkgs/blob/master/diagrams/installation.png)
- source
  - you write this
  - has specific dir structure with e.g. DESCRIPTION, R/ dir
- bundled
  - pkg files compressed to single file like .tar.gz (you don't normally need to make one)
- binary
  - single file pkg distribution e.g. from CRAN `install.packages()
- installed
  - binary pkg that has been uncompressed into a pkg library
  - `R CMD INSTALL` powers all pkg installation
- in-memory
  - you use this
  - if pkg is installed, `library()` makes its fcn available by loading pkg into memory and attaching it to the search path. 
  - when developing a pkg you don't use `library()`, use `devtools::load_all()` to load source pkg directly into memory
  
## Workflow
  
Will need `devtools::loadl_all()` when developing a pkg. `devtools::load_all()` is to package development as interactive “stepping through” code is to script development. And you'll use restart and install once in a while. RStudio’s Install & Restart is to package development as source() is to script development.

## Summary of workshop 1 of 3
- packages allow you to generalise, test, document and share your code
- packages come from CRAN and GitHub (mainly) but some other specialised repositories
- they live in libraries (folders) on your pc
- you write source, submit bundles, install binaries (mainly), and use in-memory
- we use devtools::load_all() during development a lot

## Resources
- [Writing R Extensions](https://cran.r-project.org/doc/manuals/R-exts.html) 1999–2020 R Core Team
- [R Packages](https://r-pkgs.org/) (Wickham and Bryan, 2020)
- R Packages Ch2 "[The whole game](https://r-pkgs.org/whole-game.html)" 
- Hillary Parker's [Writing an R package from scratch](https://hilaryparker.com/2014/04/29/writing-an-r-package-from-scratch/)
- Karl Broman's [R package primer a minimal tutorial](https://kbroman.org/pkg_primer/)
  
  
  


