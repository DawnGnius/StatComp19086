# Summary on writting R package

> Read "Writing R Extensions" for more information.
>
> Read r-pkgs.org for details.

## Create a package

1. load all
    use `devtools::load_all()` in R console.
2. check()
   use `devtools::check()` in R console.
3. Edit `man` floder (if modification is needed)

    1. Edit your `r` or `cpp` file correctly.

    > See slides AR3_.pdf page 4.

    1. using `devtools::document()`

    > This will create documents in `man` .Rd files and create NAMESPACE file.
    > Before use `document()`, you should delete NAMESPACE.

4. Edit `vignettes` floder (if modification is needed)

    > See above.

## Install and Use

```r
# install from remote
devtools::install_github("DawnGnius/StatComp19086", build_vignettes=TRUE)
# using `build_vignettes`, we can force installing package with vignettes.

# install from local
devtools::install("DawnGnius/StatComp19086", build_vignettes=TRUE)
# using `build_vignettes`, we can force installing package with vignettes.

# load this package
library(StatComp19086)

# remove from your lib
remove.packages("StatComp19086", lib="~/R/win-library/3.6")
```

## Vignettes Preparation and Development Cycle

This is a vignette rmarkdown file. 
> Reference: https://r-pkgs.org/vignettes.html

### To create your first vignette, run:

`usethis::use_vignette("my-vignette")`
This will:

1. Create a vignettes/ directory.

2. Add the necessary dependencies to DESCRIPTION (i.e. it adds knitr to the `Suggests` and `VignetteBuilder` fields).

3. Draft a vignette, `vignettes/my-vignette.Rmd`. 

### Create a package bundle with the vignettes included

There are 2 methods:

1. `devtools::build_vignettes()` Build all vignettes from the console.

2. `devtools::build()` Create a package bundle with the vignettes included.

Note that 
```
#> devtools::build()
#>Building the package will delete...
#>  'C:/Users/Gnius/Documents/Code/R/StatComp-R/inst/doc'
#>Are you sure?
```

Note that
RStudio’s `Build & reload` does not build vignettes to save time. 
Similarly, `devtools::install_github()` (and friends) will not build vignettes by default because they’re time consuming and may require additional packages. 
You can force building with `devtools::install_github(build_vignettes = TRUE)`. 
This will also install all suggested packages.

## Bugs

1. cpp function cannot be called, after installing from local.

    > add `@useDynLib StatComp19086, .registration = TRUE` into cpp files befor `@export` line
    >
    > add `@importFrom Rcpp evalCpp` into any one cpp file
    >
    > add `@exportPattern "^[[:alpha:]]+"` into any one cpp file
    >
    > add `Encoding: UTF-8` into DESCRIPTION
    >
    > `devtools::document()` will auto create/update .Rd files in `man` floder and also `NAMESPACE`
    >
    >> Please, Check `Rcpp::Rcpp.package.skeleton()` for detials.
