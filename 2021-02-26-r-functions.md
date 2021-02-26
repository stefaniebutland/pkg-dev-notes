R-Ladies EL, Chicago workshop with Stephanie Kirmer

[slides](https://skirmer.github.io/presentations/functions_with_r.html#1)

[repo](https://github.com/skirmer/functions_r)

basic function: `possible_dog_part1.R`

Logical, numeric, character or string, closure (function)

## Arguments, what goes in
order and naming is important

see `possible_dog_part2.R`
```
dogCheck <- function(height_cm, has_fur) {
  if(has_fur == TRUE){
    if(height_cm < 112 & height_cm > 9){
      possible_dog <- TRUE
    }
  } else {
    possible_dog <- FALSE
  }
  return(possible_dog)
}
```


```
> source('~/R/r-functions/possible_dog_part2.R')
> dogCheck(50, TRUE)
[1] TRUE
```

but if you enter in different order and not specify the value of the argument
```
> dogCheck(TRUE,50)
[1] FALSE
```

so assign things explicitly like
```
> dogCheck(has_fur = TRUE,height_cm = 50)
[1] TRUE
```

## Returns, what comes out
see `possible_dog_part3.R`

note if you assign `dog <- dogCheck(height_cm = 50, has_fur = TRUE)` (or something like that) you can look at the individual returned elements of the list

## Guardrails: Documentation

See slide #18; see `possible_dog_part4.R`
Roxygen2 format

need to alternate doc - function - doc - function

```
#' Explain what function does in a single line
#' @param a Type. Explain role of one of the arguments (here @param means argument)
#' @param b Type. Explain role of one of the arguments
#'
#' @return Type. Description of what it represents
#'
#' @examples 
#' \dontrun{myFunction(a = 5, b = 3)}
```

## Guardrails: Type Check
slide 21; see `possible_dog_part5.R`

## Guardrails: Tests
slide 22; see `possible_dog_part6.R`
`library(testthat)`

test for the right stuff, but also test error messages for when they happen

hmmm part6.R  in RStudio shows me `Run Tests` option, but not option to run a subset of the code. Highlight code and `Cmd-return` lets you run the code

## Environments
slide 25

`d <<- a*b` using double arrow puts it in global envt (outside your function)









