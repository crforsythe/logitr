
<!-- README.md is generated from README.Rmd. Please edit that file -->

# logitr

**logitr** estimates multinomial (MNL) and mixed logit (MXL) models in
R. Models can be estimated using “Preference” space or
“Willingness-to-pay” (WTP) space utility parameterizations. The
current version includes support for:

  - Homogeneous multinomial logit (MNL) models
  - Heterogeneous mixed logit (MXL) models (with normal and log-normal
    parameter distributions).
  - Preference space utility parameterization.
  - WTP space utility parameterization.
  - An optional multistart optimization that uses different random
    starting points in each iteration (useful for non-convex problems
    like MXL models or models with WTP space parameterizations).
  - A simulation function for computing the expected shares of a set of
    alternatives using an estimated model.

MXL models assume uncorrelated heterogeneity covariances and are
estimated using maximum simulated likelihood based on the algorithms in
[Kenneth Train’s](http://eml.berkeley.edu/~train/) book [*Discrete
Choice Methods with Simulation, 2nd Edition (New York: Cambridge
University Press, 2009)*](http://eml.berkeley.edu/books/choice2.html).

## Installation

The current version is not yet on CRAN, but you can install it from
Github using the **devtools** library:

    devtools::install_github('jhelvy/logitr')

## Required Libraries

**logitr** requires the
[**nloptr**](https://cran.r-project.org/web/packages/nloptr/index.html)
library. This is because `nloptr()` allows for both the objective and
gradient functions to be computed in a single function. This speeds up
computation time considerably because both the objective and gradient
functions require many of the same calculations (e.g. computing
probabilities).

# Estimating models

The `logitr()` function estimates multinomial logit (MNL) and mixed
logit (MXL) models using “Preference” space or “Willingness-to-pay
(WTP)” space [utility
parameterizations](articles/utility_models.html). The basic usage for
estimating models is as follows:

``` r
model <- logitr(
  data,
  choiceName,
  obsIDName,
  parNames,
  priceName = NULL,
  randPars = NULL,
  randPrice = NULL,
  modelSpace = "pref",
  weightsName = NULL,
  options = list()
)
```

## Data format

The `data` argument is set to the name of the `data.frame` storing the
choice observations. The `data.frame` must be arranged such that each
row is an alternative from a choice observation. The choice observations
do not have to be symmetric (i.e. each choice observation could have a
different number of alternatives). The data must include columns for
each of the following arguments in the `logitr()` function:

  - `choiceName`: A dummy variable that identifies which alternative was
    chosen (`1` = chosen, `0` = not chosen).
  - `obsIDName`: A sequence of numbers that identifies each unique
    choice occasion. For example, if the first three choice occasions
    had 2 alternatives each, then the first 9 rows of the `obsID`
    variable would be `1, 1, 2, 2, 3, 3`.
  - `parNames`: The names of the variables that will be used as model
    covariates. For WTP space models, do **not** include the price
    variable in `parNames` - this is provided separately with the
    `priceName` argument.

## Preference space models

Unlike similar packages like
[**mlogit**](https://cran.r-project.org/web/packages/mlogit/index.html),
**logitr** does not support the formula type input that uses a symbolic
description of the model to be estimated. Parameters are simply
described as a vector of the column names in the `data.frame` provided
to the `data` argument.

The `logitr()` function assumes that the deterministic part of the
utility function is linear in parameters, i.e.:

Accordingly, each parameter in the `parNames` argument is an additive
part of \(v_{j}\). For example, if the observed utility was
\(v_{j} = \beta x_{j} - \alpha p_{j}\), where \(p_{j}\) is price and
\(x_{j}\) is size, then the `parNames` argument should be `c("price",
"size")`, and the `data.frame` used for the `data` argument should have
columns called `price` and `size`.

## WTP space models

For models estimated in the WTP space, the `modelSpace` argument should
be set to `modelSpace = "wtp"`. The `parNames` argument should **not**
contain the name of the column for the price attribute. This is provided
separately with the `priceName` argument. For example, if the observed
utility was \(v_{j} = \lambda \left(\omega' x_{j} - p_{j}\right)\),
where \(p_{j}\) is price and \(x_{j}\) is size, then the `parNames`
argument should be `"size"`, and the `priceName` argument should be
`"price"`.

Since WTP space models are non-convex, it is recommended that you use a
multi-start search to run the optimization loop multiple times
(controlled by `numMultiStarts` in the `options` argument) to search for
different local minima.

## Mixed logit models

To estimate a mixed logit model, use the `randPars` argument to denote
which parameters will be modeled with a distribution. The current
package version supports normal and log-normal distributions. For
example, if you wanted to model a covariate called `"size"` with a
normal distribution, set `randPars = c(size = "n")`. Likewise, if you
wanted it to be log-normally distributed, set `randPars = c(size =
"ln")`.

Since mixed logit models are non-convex, it is recommended that you use
a multi-start search to run the optimization loop multiple times
(controlled by `numMultiStarts` in the `options` argument) to search for
different local minima. Note that mixed logit models can take a long
time to estimate, so setting large number for `numMultiStarts` could
take hours or more to complete.

## View results

Use the `summary()` function to print a summary of the results from an
estimated model. The function will print the following based on the
model settings:

  - For a single model run, it prints some summary information,
    including the model space (Preference or WTP), log-likelihood value
    at the solution, and a summary table of the model coefficients.
  - For MXL models, the function also prints a summary of the random
    parameters.
  - If you set `keepAllRuns = TRUE` in the `options` argument,
    `summary()` will print a summary of all the multistart runs followed
    by a summary of the best model (as determined by the largest
    log-likelihood value).

Use `statusCodes()` to print a description of each status code from the
`nloptr` optimization routine.

## Computing and comparing WTP

For models in the preference space, you can get a summary table of the
implied WTP by using:

    wtp(model_pref, priceName)

To compare the WTP between two equivalent models in the preference space
and WTP spaces, use:

    wtpCompare(model_pref, model_wtp, priceName)

## Simulate shares

Once you have estimated a model, you can use it to simulate the expected
shares of a particular set of alternatives. This can be done using the
function `simulateShares()`. The simulation reports the expected share
as well as a confidence interval for each alternative:

``` r
shares = simulateShares(model, alts, priceName = NULL, alpha = 0.025)
```

# Author, Version, and License Information

  - Author: *John Paul Helveston*
    [www.jhelvy.com](http://www.jhelvy.com/)
  - Date First Written: *Sunday, September 28, 2014*
  - Most Recent Update: *Thursday, Oct 22, 2020*
  - License: GPL-3
  - Latest Version: 1.2.0

# Citation Information

If you use this package for in a publication, I would greatly appreciate
it if you cited it. You can get the citation information by typing this
into R:

    citation('logitr')
