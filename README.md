
<!-- README.md is generated from README.Rmd. Please edit that file -->

# logitr <a href='https://jhelvy.github.io/logitr/'><img src='man/figures/logitr-hex.png' align="right" height="139" /></a>

<!-- badges: start -->

[![](https://lifecycle.r-lib.org/articles/figures/lifecycle-stable.svg)](https://lifecycle.r-lib.org/articles/stages.html#stable)
[![CRAN
status](https://www.r-pkg.org/badges/version/logitr)](https://CRAN.R-project.org/package=logitr)
[![Travis build
status](https://travis-ci.com/jhelvy/logitr.svg?branch=master)](https://travis-ci.com/jhelvy/logitr)
[![](http://cranlogs.r-pkg.org/badges/grand-total/logitr?color=blue)](https://cran.r-project.org/package=logitr)
<!-- badges: end -->

Estimation of multinomial (MNL) and mixed logit (MXL) models in R with
“Preference” space or “Willingness-to-pay” (WTP) space [utility
parameterizations](https://jhelvy.github.io/logitr/articles/utility_models.html).

The latest version includes support for:

-   Homogeneous multinomial logit (MNL) models
-   Heterogeneous mixed logit (MXL) models (with normal and log-normal
    parameter distributions).
-   Preference space utility parameterization.
-   WTP space utility parameterization.
-   An option to run a multistart optimization loop that uses different
    random starting points in each iteration (useful for non-convex
    problems like MXL models or models with WTP space
    parameterizations).
-   Computing and comparing WTP from both preference space and WTP space
    models.
-   Simulating the expected shares of a set of alternatives using an
    estimated model.

Note: MXL models assume uncorrelated heterogeneity covariances and are
estimated using maximum simulated likelihood based on the algorithms in
Kenneth Train’s book [*Discrete Choice Methods with Simulation, 2nd
Edition (New York: Cambridge University Press,
2009)*](https://eml.berkeley.edu/books/choice2.html).

## Installation

You can install {logitr} from CRAN:

``` r
install.packages("logitr")
```

or you can install the development version of {logitr} from
[GitHub](https://github.com/jhelvy/logitr):

``` r
# install.packages("remotes")
remotes::install_github("jhelvy/logitr")
```

Load the library with:

``` r
library(logitr)
```

## Basic Usage

View the [basic
usage](https://jhelvy.github.io/logitr/articles/basic_usage.html) page
for details on how to use **logitr** to estimate models.

## Author, Version, and License Information

-   Author: *John Paul Helveston*
    [www.jhelvy.com](http://www.jhelvy.com/)
-   Date First Written: *Sunday, September 28, 2014*
-   Most Recent Update: March 23, 2021
-   License:
    [MIT](https://github.com/jhelvy/logitr/blob/master/LICENSE.md)

## Citation Information

If you use this package for in a publication, I would greatly appreciate
it if you cited it - you can get the citation by typing
`citation("logitr")` into R:

``` r
citation("logitr")
#> 
#> To cite logitr in publications use:
#> 
#>   John Paul Helveston (2021). logitr: Random utility logit models with
#>   preference and willingness to pay space parameterizations. R package
#>   version 0.1.2.
#> 
#> A BibTeX entry for LaTeX users is
#> 
#>   @Manual{,
#>     title = {logitr: Random Utility Logit Models with Preference and Willingness to Pay Space Parameterizations},
#>     author = {John Paul Helveston},
#>     year = {2020},
#>     note = {R package version 0.1.2},
#>     url = {https://jhelvy.github.io/logitr/},
#>   }
```
