This entry shows how to perform Analysis of Variance in the Multiple Linear Regression Model.


**The `anova` function applied to multiple-linear regression models**

```r
Y=read.table(file='https://raw.githubusercontent.com/gdlc/EPI809/master/wages.txt',header=TRUE)
fm=lm(wage~education+region+married+experience+union+sex+ethnicity,data=Y)
summary(fm)

```

**Testing the model as a whole*: 

H0: all coefficients, except the intercept, are equal to zero.

```r
 fm0=lm(wage~1,data=Y)
 anova(fm0,fm)
 
 # Reproducing the ANOVA table
 y=Y$wage
 yHat=predict(fm)
 eHatHa=residuals(fm)
 eHatH0=residuals(fm0)
 n=length(yHat) # discuss what happens when there are NAs
 
 # Sum of Squares
 resSSH0=sum(eHatH0^2) # total SS  
 resSSHa=sum(eHatHa^2) # residual SS
 SSm=resSSH0-resSSHa   # model SS

 # Degree of freedom
  resDFH0=n-length(coef(fm0))
  resDFHa=n-length(coef(fm))
  modelDF=resDFH0-resDFHa

 # F-statistic and p-value
  FStat=(SSm/modelDF)/(resSSHa/resDFHa)
  PVal=pf(q=FStat,df1=modelDF,df2=resDFHa,lower.tail=F)
  
  SSm
  RSS
  modelDF
  resDFHa
  FStat
  PVal
  
  anova(fm0,fm)
  
```

**Sequential ANOVA**

When we apply the `anova` function to a fitted model we obtain a decomposition of the variance for each factor. By default, R performs sequential ANOVA. Sequential (or type-I) ANOVA is not invariant to the order in which predictors enter in the model. The following example reproduces the sequential ANOVA produce by `anova()` using sequential regressions. 

```r
rm(list=ls()) # cleans the environment
Y=read.table(file='https://raw.githubusercontent.com/gdlc/EPI809/master/wages.txt',header=T)

fm0=lm(wage~1,data=Y)
fm=lm(wage~education+sex+ethnicity,data=Y)
anova(fm0,fm)

anova(fm)

## Sequential anova
 # Null hypothesis
 eHat0=residuals(fm0) # Intercept only

 # Add education
 fm1=lm(eHat0~education,data=Y) # Intercept plus education
 eHat1=residuals(fm1)
 sum(eHat0^2)-sum(eHat1^2) # variance explained by education

 # Add sex
 fm2=lm(wage~education+sex,data=Y) # Intercept+Education + sec
 eHat2=residuals(fm2)
 sum(eHat1^2)-sum(eHat2^2) # variance explained by sex that was not 
                           # explained by education.
 # Add ethnicity
 fm3=lm(eHat2~education+sex+ethnicity,data=Y)
 eHat3=residuals(fm3)
 sum(eHat2^2)-sum(eHat3^2)
 
 sum(eHat3^2) # final RSS

```

What happens if we change the order we enter variables in the model?

```r
fm=lm(Wage~Education+sex+ethnicity,data=Y)
fm2=lm(Wage~sex+ethnicity+Education,data=Y)
anova(fm0,fm)
anova(fm0,fm2)
anova(fm)
anova(fm2)
```

### Type-III SS 

The ANOVA with sequential SS is not invariant with respect to the order in which we entered predictors in the model. To test the significance of each factor, we may want to do it by enetering each factor last in the model. This is called the type-III SS. When we do this, we test whether a particular factor has an effect, after considering all the other factors in the model. The "short" regression or null hypothesis is a model that excludes one factor and the "long" regression is the model that includes all the predictors we are considering.


```r
#install.packages(pkg='car',repos='https://cran.r-project.org/')
library(car)
anova(fm) # produces sequential ANOVA
Anova(fm,type='III') # SS when each factor is entered last

# Let's verify

# Education
 fm=lm(wage~education+sex+ethnicity,data=Y)
 fm0=lm(wage~sex+ethnicity,data=Y)
 sum(residuals(fm0)^2)-sum(residuals(fm)^2)
 
# sex
 fm=lm(wage~education+sex+ethnicity,data=Y)
 fm0=lm(wage~education+ethnicity,data=Y)
 sum(residuals(fm0)^2)-sum(residuals(fm)^2)
 
# Ethnicity
 fm=lm(wage~education+sex+ethnicity,data=Y)
 fm0=lm(wage~education+sex,data=Y)
 sum(residuals(fm0)^2)-sum(residuals(fm)^2)

# RSS
 sum(residual(fm)^2)

```


## Short Vs Long Regression

```r

Y=read.table('https://raw.githubusercontent.com/gdlc/EPI809/master/wages.txt',header=T)
head(Y)
Y$region=factor(Y$region,levels=c('North','South'))
fm0=lm(Wage~1)
fmL=lm(wage~region+sex+experience+education,data=Y)
fmS=lm(wage~region+sex,data=Y)
anova(fmL)
anova(fmS)
anova(fmS,fmL)

```
