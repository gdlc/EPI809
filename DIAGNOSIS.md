
After we fit the model we often check the residuals to:

  - Detect departures from linearity

  - Identify heteroskedasticity (i.e., heterogeneous error variance)

  - Identify departures from normality, outliers, and influential points

```r

Y=read.table('https://raw.githubusercontent.com/gdlc/EPI809/master/wages.txt',header=T)

y=Y$wage
x=Y$education

fm=lm(y~x)

## Predictions, residuals, and standarized residuals
 eHat=residuals(fm)
 yHat=predict(fm)
 eHatSTD=rstandard(fm)

## Standarized residuals versus predictors
 plot(eHatSTD~yHat)

## Histograms and quantile-quantile plots
# Data
 hist(y,30)
 qqnorm(scale(y));abline(a=0,b=1)

# Standarized residuals
 hist(eHatSTD,30)
 qqnorm(eHatSTD);abline(a=0,b=1)

## Default diagnostic plots for lm
 plot(fm)

# Residual Vs fitted values
# Look for: departures from linear patterns
#           heterogeneous error variances



```
