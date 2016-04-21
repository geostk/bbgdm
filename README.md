#BBGDM
[![Travis-CI Build Status](https://travis-ci.org/skiptoniam/bbgdm.svg?branch=master)](https://travis-ci.org/skiptoniam/bbgdm)

BBGDM is a R package for running Generalized Dissimilarity Models with Bayesian Bootstrap for parameter estimation. To install package run the following command in your R terminal
```r
install.packages(c('devtools'))
devtools::install_github('skiptoniam/bbgdm')
```
### Load the required libaries, we need vegan for the dune dataset.
```r
library(bbgdm)
library(vegan)
```

### Run bbgdm on the famous dune meadow data
The dune meadow vegetation data, dune, has cover class values of 30 species on 20 sites.
Make the abundance data presence/absence.
```r
data(dune)
data(dune.env)
dune_pa <- ifelse(dune>0,1,0)
```

### Fit a bbgdm
Now we have a species by sites matrix of simulated data and a set data for a one dimensional gradient.
```r
form <- ~1+A1
fm1 <- bbgdm(form,dune_pa, dune.env,family="binomial",link='logit',
             dism_metric="number_non_shared",spline_type = 'ispline',
             nboot=100, geo=FALSE,optim.meth='nlmnib')
```
### Plot response curves
```r
plotResponse(fm1,plotdim = c(1,1))
```
### Plot diagnostics
```r
bbgdm.check(fm1)
```
### Run 'Wald-like' test on parameters
```r
bbgdm.wald.test(fm1)
```
