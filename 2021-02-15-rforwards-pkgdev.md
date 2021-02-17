# Packages in a nutshell. A Forwards Package Development module

[Emma Rand's Day 1 slides](https://3mmarand.github.io/workshops/package-dev-modules/slides/01-packages-in-a-nutshell/packages-in-a-nutshell.html) Packages in a nutshell

[Mine Çetinkaya-Rundel's Day 2 slides](http://bit.ly/pkg-dev-2) Setting up your system

[Emma Rand's Day 3 slides](http://bit.ly/pkg-dev-3) Your first package

Day 3 slides

# Day 1 Packages in a nutshell

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

# Day 2 Setting up your system
Forwards Package Development module
[Day 2 slides Mine Çetinkaya-Rundel](http://bit.ly/pkg-dev-2)


- Installing released packages from CRAN using `install.packages()` -- binary
- Installing development packages using `devtools::install_github()` -- source
- Once installed, binary and source packages are identical

`remotes::install_github()` installs fewer dependencies; a recent thing when big devtools was broken to smaller packages

Verify system setup. Run the following in the Console.

> `install.packages(devtools)`
> `library(devtools)`
> `has_devel()`
Need Xcode. If Xcode is installed, get msg `## Your system is ready to build packages!`

## Using git and GitHub
> `git --version`
`git version 2.30.0`

Note you can put your GitHub PAT in 1Password (i.e. don't reveal it)

*** fix this. see [Get help with GitHub personal access tokens](https://usethis.r-lib.org/reference/github-token.html)
```
> git_sitrep()
Git config (global)
● Name: 'Stefanie Butland'
● Email: 'stefaniebutland@gmail.com'
● Vaccinated: TRUE
● Default Git protocol: 'https'
GitHub
● Default GitHub host: 'https://github.com'
● Personal access token for 'https://github.com': '<discovered>'
● GitHub user: 'stefaniebutland'
● Token scopes: 'repo, workflow'
x Token may be mis-scoped: 'repo' and 'user' are highly recommended scopes
  The 'workflow' scope is needed to manage GitHub Actions workflow files
  If you are troubleshooting, consider this
x Can't retrieve registered email address(es)
  If you are troubleshooting, check GitHub host, token, and token scopes
Git repo for current project
ℹ No active usethis project
```

`usethis::create_package()` will create a folder with the package name you choose, including .Rproj fileand a Git pane to commit


# Day 3 Your first package

```
> usethis::create_package("~/Desktop/mypackage")
✓ Creating '/Users/stefaniebutland/Desktop/mypackage/'
✓ Setting active project to '/Users/stefaniebutland/Desktop/mypackage'
✓ Creating 'R/'
✓ Writing 'DESCRIPTION'
Package: mypackage
Title: What the Package Does (One Line, Title Case)
Version: 0.0.0.9000
Authors@R (parsed):
    * First Last <first.last@example.com> [aut, cre] (YOUR-ORCID-ID)
Description: What the package does (one paragraph).
License: `use_mit_license()`, `use_gpl3_license()` or friends to
    pick a license
Encoding: UTF-8
LazyData: true
Roxygen: list(markdown = TRUE)
RoxygenNote: 7.1.1
✓ Writing 'NAMESPACE'
✓ Writing 'mypackage.Rproj'
✓ Adding '^mypackage\\.Rproj$' to '.Rbuildignore'
✓ Adding '.Rproj.user' to '.gitignore'
✓ Adding '^\\.Rproj\\.user$' to '.Rbuildignore'
✓ Opening '/Users/stefaniebutland/Desktop/mypackage/' in new RStudio session
✓ Setting active project to '<no active project>'
```
Creates RStudio project and the files above.

```
> usethis::use_r("animal_sounds")
✓ Setting active project to '/Users/stefaniebutland/Desktop/mypackage'
● Modify 'R/animal_sounds.R'
● Call `use_test()` to create a matching test file
```

in the .R file, cp paste:
```
animal_sounds <- function(animal, sound) {
  assertthat::assert_that(
    assertthat::is.string(animal),
    assertthat::is.string(sound)
  )
  paste0("The ", animal, " goes ", sound, "!")
}
```
(creating functions not covered in this workshop)

To run this, you don't use `source(file)`, use `devtools::loadl_all()` = Cmd shift L

routine: edit, Cmd-shift-L (load all), run with cmd `animal_sounds` which is the name of the function, change things, load all, run etc 

Run `devtools::check()`. Output lots of stuff plus the following: 
```
> checking DESCRIPTION meta-information ... WARNING
  Non-standard license specification:
    `use_mit_license()`, `use_gpl3_license()` or friends to pick a
    license
  Standardizable: FALSE

> checking dependencies in R code ... WARNING
  '::' or ':::' import not declared from: ‘assertthat’
  
  0 errors ✓ | 2 warnings x | 0 notes ✓
  ```
  
Add a license with `usethis::use_mit_license("Emma Rand")` and it automatically updates the DESCRIPTION file and .Rbuildignrore.

re-run `devtools::check()` now that you've changed DESC and license. 

```
── R CMD check results ─────────── mypackage 0.0.0.9000 ────
Duration: 27.9s

> checking dependencies in R code ... WARNING
  '::' or ':::' import not declared from: ‘assertthat’

> checking DESCRIPTION meta-information ... NOTE
  Malformed Description field: should contain one or more complete sentences.

0 errors ✓ | 1 warning x | 1 note x
```

## Use git and GitHub
`usethis::use_git("initial commit")` then `usethis::use_github()` creates a public repo as long as your GitHub is set up.




## Summary
- `usethis::create_package()` starts a package with infrastructure as an RStudio Project
- functions go in .R files in the \R directory and usethis::use_r() will open a file for editing
- devtools::load_all() loads your package for testing and we use it often
- devtools::check() runs the command line tool R CMD check from within R and checks your package in in working order
- DESCRIPTION contains metadata about your package and its presence is the defining feature of a package
- usethis::use_mit_license() is a helper for adding a MIT license (others are available)
- there is a lot more to package documentation and package dependencies!
- usethis::use_git() and `usethis::use_github()` or `usethis::use_github(private = TRUE)` are helper functions for version control


## Documentation

Put your cursor anywhere in the code, RStudio > Code > Insert roxygen skeleton. Then `> devtools::document()` and look for it with `?animal_sounds` and you get a man page you can edit!






  
  


