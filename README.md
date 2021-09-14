
[![](https://www.r-pkg.org/badges/version/bacondecomp?color=green)](https://cran.r-project.org/package=bacondecomp)
![](https://github.com/evanjflack/bacondecomp/workflows/R-CMD-check/badge.svg)
[![Build
Status](https://travis-ci.com/evanjflack/bacondecomp.svg?branch=master)](https://travis-ci.com/evanjflack/bacondecomp)
[![Coverage
status](https://codecov.io/gh/evanjflack/bacondecomp/branch/master/graph/badge.svg)](https://codecov.io/github/evanjflack/bacondecomp?branch=master)
[![Example Jupyter
Notebook](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/evanjflack/bacondecomp/master?filepath=index.ipynb)

<!-- README.md is generated from README.Rmd. Please edit that file -->

# bacondecomp

`bacondecomp` is a package with tools to perform the Goodman-Bacon
decomposition for differences-in-differences with variation in treatment
timing. The decomposition can be done with and without time-varying
covariates.

## Installation

You can install `bacondecomp 0.1.1` from CRAN:

``` r
install.packages("bacondecomp")
```

You can install the development version of `bacondecomp` from GitHub:

``` r
library(devtools)
install_github("evanjflack/bacondecomp")
```

## Functions

-   `bacon()`: calculates all 2x2 differences-in-differences estimates
    and weights for the Bacon-Goodman decomposition.

## Data

-   `math_refom`: Aggregated data from Goodman (2019, JOLE)
-   `castle`: Data from Cheng and Hoekstra (2013, JHR)
-   `divorce:` Data from Stevenson and Wolfers (2006, QJE)

## Example

This is a basic example which shows you how to use the bacon() function
to decompose the two-way fixed effects estimate of the effect of an
education reform on future earnings following Goodman (2019, JOLE).

``` r
library(bacondecomp)
#> Loading required package: fixest
#> fixest 0.9.0, BREAKING changes! (Permanently remove this message with fixest_startup_msg(FALSE).) 
#> - In i():
#>     + the first two arguments have been swapped! Now it's i(factor_var, continuous_var) for interactions. 
#>     + argument 'drop' has been removed (put everything in 'ref' now).
#> - In feglm(): 
#>     + the default family becomes 'gaussian' to be in line with glm(). Hence, for Poisson estimations, please use fepois() instead.
df_bacon <- bacon(incearn_ln ~ reform_math,
                  data = bacondecomp::math_reform,
                  id_var = "state",
                  time_var = "class")

library(ggplot2)

ggplot(df_bacon) +
  aes(x = weight, y = estimate, shape = factor(type)) +
  geom_point() +
  geom_hline(yintercept = 0) +
  labs(x = "Weight", y = "Estimate", shape = "Type")
```

<img src="man/figures/README-example-1.png" width="100%" />

## References

Goodman-Bacon, Andrew. 2018. “Difference-in-Differences with Variation
in Treatment Timing.” National Bureau of Economic Research Working Paper
Series No. 25018. doi: 10.3386/w25018.

[Paper
Link](https://cdn.vanderbilt.edu/vu-my/wp-content/uploads/sites/2318/2019/07/29170757/ddtiming_7_29_2019.pdf)
