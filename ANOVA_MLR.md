<div id="menu" />

## ANOVA and Testing

### Topics:
 
  - [1) Testing the model as a whole](#whole-model)
  - [2) Long versus Short regression](#long-short)
  - [3) 1-DF Test](#1DF)
  - [4) Sequential ANOVA](#sequentialANOVA)
  - [5) Testing individual factors using Type-III SS](#type-III)


<div id="whole-model" />

### 1) Testing the model as a whole 

Does education, region, marital status, experience, union, sex, or ethnicity have any effect on wages?

**Model (i.e., alternative hypothesis)**

```r
 Y=read.table(file='https://raw.githubusercontent.com/gdlc/EPI809/master/wages.txt',header=TRUE)
 HA=lm(wage~education+region+married+experience+union+sex+ethnicity,data=Y)
```

**Null hypothesis**: 

H0: all coefficients, except the intercept, are equal to zero.

```r
 H0=lm(wage~1,data=Y)
```

**ANOVA-table**:

```r
anova(HO,HA)
```

**Reproducing the ANOVA table**:

```r
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
[Back to menu](#menu)

<div id="long-short" />


## 2) Long Vs Short Regression

Do experience and education have an effect on wages aftera ccountin for differences in wages due to sex and region?

```r
 Y=read.table('https://raw.githubusercontent.com/gdlc/EPI809/master/wages.txt',header=T)
 head(Y)
 Y$region=factor(Y$region,levels=c('North','South'))

 fmL=lm(wage~region+sex+experience+education,data=Y)
 fmS=lm(wage~region+sex,data=Y)
 anova(fmS,fmL)

```
[Back to menu](#menu)

<div id="1DF" />

### 3) 1DF Test: F- and t-test


Research question: Does sex have an effect on wages aftera accountig for differences due to region, experience and education?

```r
 Y=read.table('https://raw.githubusercontent.com/gdlc/EPI809/master/wages.txt',header=T)
 head(Y)
 Y$region=factor(Y$region,levels=c('North','South'))
 HA=lm(wage~region+experience+education+sex,data=Y)
 H0=lm(wage~region+experience+education,data=Y)
 
 # F-test
 anova(H0,HA)
 
 # Compare with the t-test for sex
 summary(HA)
 
 # The squared of the t-statistic should be equal to the F-statistic of the ANOVA table
 summary(HA)$coef[5,3]^2
```
[Back to menu](#menu)


<div id="sequentialANOVA" />


### 4) Sequential ANOVA

When we apply the `anova` function to a fitted model we obtain a decomposition of the variance for each factor. By default, R performs sequential ANOVA. Sequential (or type-I) ANOVA is not invariant to the order in which predictors enter in the model. The following example reproduces the sequential ANOVA produce by `anova()` using sequential regressions. 

```r
 rm(list=ls()) # cleans the environment
 Y=read.table(file='https://raw.githubusercontent.com/gdlc/EPI809/master/wages.txt',header=T)

 fm=lm(wage~education+sex+ethnicity,data=Y)
 anova(fm)
```

**The following code shows how a sequential (Type-I) ANOVA can be reproduced by adding one factor at a time**

```r

 fm0=lm(wage~1,data=Y)
 fm1=lm(wage~education,data=Y)
 fm2=lm(wage~education+sex,data=Y)
 fm3=lm(wage~education+sex+ethnicity,data=Y)
 
 RSS0=sum(residuals(fm0)^2)
 RSS1=sum(residuals(fm1)^2) 
 RSS2=sum(residuals(fm2)^2)
 RSS3=sum(residuals(fm3)^2)
 
 anova(fm)
 
 #compare Sum of Squares from anova(fm) with this
 
 RSS0-RSS1
 RSS1-RSS2
 RSS2-RSS3
 
```

However, as noted, if you change the order of the variables in the model, the Type-I (sequential) SS will change.



<div id="type-III" />

### 5) Type-III SS

Type-III sum of squares quantifies the variability a factor can explain, after accountig for all the other factors in the model.
ANOVA tables computed using Type-III SS can be used to test individual factors, in each test, the null hypothesis accounts for all the other factors.


```r
#install.packages(pkg='car',repos='https://cran.r-project.org/')

Y=read.table('https://raw.githubusercontent.com/gdlc/EPI809/master/wages.txt',header=T)
head(Y)
Y$region=factor(Y$region,levels=c('North','South'))
fm=lm(wage~education+sex+ethnicity,data=Y)

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

For 1DF tests the p-values derived from an F-test based Type-III SS is equivalent to the t-test

```r
 summary(fm)
 Anova(fm)
```

[Back to menu](#menu)

