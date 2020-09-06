# The gretl *fsboost* package

Package for computing forward-stagewise shrinkage and selection regressions.

# Introduction
So called shrinkage and/ or selection estimators such as Ridge or Lasso among others are known to handle such issues by imposing an additional restriction to an otherwise ordinary least square setting. Another alternative estimation approach is the so called forward-stagewise  regression approach (*fsboost* henceforth).

*fsboost* is a simple strategy for constructing a sequence of sparse regression estimates: Initially set all coefficients to zero, and iteratively update the coefficient (by a small amount, depending on the learning rate) of the variable that achieves (under quadratic loss) the maximal absolute  correlation with the current residual.
Learning from the residuals has some connection to an approach known as *boosting* in the machine-learning community.

**References**
- Hastie, T., Taylor, J., Tibshirani R. and Walther G. (2007): "Forward stagewise regression and the monotone lasso", *Electronic Journal of Statistics*, Vol. 1, 1-29.
- Tibshirani, R. (2015): "A General Framework for Fast Stagewise Algorithms", *Journal of Machine Learning Research*, 16, 2543-2588.


# Features
- Support for linear regression.
- Simple API.
- Plot coefficient paths.
- GUI access through the gretl menu.

# Detailed help file
A detailed help file can be found here: https://github.com/atecon/fsboost/blob/master/docs/fsboost.pdf

# Installation and usage
Get the package from the gretl package server and install it:
```
pkg install fsboost
```
## GUI interface
Once the package is installed, the user can access the GUI interface via the "Model --> Other linear models --> Forward Stagewise" menu. The interface will look like this:

![sample](https://github.com/atecon/fsboost/raw/master/docs/gui.png)

## Simple scripting example
Here is a sample script on how to use it (see also: https://raw.githubusercontent.com/atecon/fsboost/master/src/fsboost_sample.inp):

First, load the package and open a the well-known cross-sectional data set ```mroz87.gdt``` (723 observations). In this example, we 'model' the hourly wages of women, ```WW```, by means of 17 features (exogenous variables). The ```fsreg()``` function calles the the linear regression computation. All relevant output is stored in the returned bundle (a kind of struct data type) named ```B``` here:
```
clear
set verbose off
include fsboost.gfn

list RHS = const dataset
RHS -= LHS WW     # drop endogenous variable

bundle B = fsreg(WW, RHS, opts)    # Run estimation
print B                            # Print content of the returned bundle
```
Estimation does not even take half a second.

Once the computation is finished, the user can print the summary results by means of the ```print_fsboost_results()``` function:
```
print_fsboost_results(B)
```

The printed table looks like this:
```
Forward-stagewise regression results (no inference)
-------------------------------------------------------

             coefficient    std. error   z    p-value
  ---------------------------------------------------
  const      -1.24238           NA       NA     NA   
  LFP         2.60587           NA       NA     NA   
  WHRS       -0.000284255       NA       NA     NA   
  WE          0.132787          NA       NA     NA   
  RPWG        0.494871          NA       NA     NA   
  FAMINC      1.02652e-05       NA       NA     NA   
  MTR        -0.644518          NA       NA     NA   

  Learning rate = 0.0002
  Number of iterations = 4964
  Correl. w. residuals = -0.0578633
  S.E. of regression = 2.18792
  R-squared = 0.544504
  R-squared alt. = 0.547703
```

The list of the *active set* (variable's with non-zero coefficients) can be retrieved from the resulting model bundle:

```
list X_final = B.X_final    # Retrieve list of selected regressors
eval varnames(X_final)      # Print names of selected regressors
```

The estimated coefficient paths can be plotted through the function ```plot_coefficient_paths()```:
```
plot_coefficient_paths(B)
```

The resulting plot looks similar to the following one:

![sample](https://github.com/atecon/fsboost/raw/master/docs/coeff_path.png)

For more details on information available read the pdf help.


# Changelog

### v0.1, September 2020
- initial release
