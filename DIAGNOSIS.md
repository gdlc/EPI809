
After we fit the model we often check the residuals to:

  - Detect departures from linearity

  - Identify heteroskedasticity (i.e., heterogeneous error variance)

  - Identify departures from normality, outliers, and influential points

```r

Y=read.table('~/Dropbox/809/datasets/wages.txt',header=T)

y=Y$Wage
x=Y$Education

fm=lm(y~x)



eHat=residuals(fm)
yHat=predict(fm)

# Residual Vs fitted values
# Look for: departures from linear patterns
#           heterogeneous error variances

plot(eHat~yHat)

# Are residuals normal?
hist(eHat,30)
qqnorm(eHat)

# Some of these plots are provided by the plot.lm() function

plot(fm)

```
