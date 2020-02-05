This entry shows how to perform Analysis of Variance in the Multiple Linear Regression Model.


**The `anova` function applied to multiple-linear regression models**

```r
Y=read.table(file='~/Desktop/wages2.txt',header=T)
fm=lm(Wage~Education+South+Married+Experience+Union+sex+ethnicity,data=Y)
summary(fm)
anova(fm)
```

Total and residual sum of squares 7 testing the model.

```r
 fm0=lm(Wage~1,data=Y)
 anova(fm0,fm)
 
 # Reproducing the anova table
 y=Y$Wage
 yHat=predict(fm)
 eHat=residuals(fm)
 DF.M=(1+1+1+1)+(1+1)+2 #south, married, union and sex has 2 levels, thus 1df each. 
                  # Education and Experience are covariates, 1 df each
                  # Ethnicity has 3 levels, thus 2 df. 

  SSy=sum((y-mean(y))^2) # total SS
  RSS=sum(eHat^2) # residual SS
  SSm=SSy-RSS # model SS
  DF.RES=nrow(Y)-DF-1
  FStat=(SSm/DF.M)/(RSS/DF.RES)
  PVal=pf(q=FStat,df1=DF.M,df2=DF.RES,lower.tail=F)
  
  SSm
  RSS
  M.DF
  DF.RES
  FStat
  PVal
  
```

**Sequential ANOVA**

When we apply the `anova` function to a fitted model we obtain a decomposition of the variance for each factor. 
By default, R performs sequential ANOVA. 

```r

fm0=lm(Wage~1,data=Y)
fm=lm(Wage~Education+sex+ethnicity,data=Y)
anova(fm0,fm)

anova(fm)
eHat0=residuals(fm0)

fm1=lm(eHat0~Education,data=Y)
eHat1=residuals(fm1)
sum(eHat0^2)-sum(eHat1^2)

fm2=lm(eHat1~sex,data=Y)
eHat2=residuals(fm2)
sum(eHat1^2)-sum(eHat2^2)

fm3=lm(eHat2~ethnicity,data=Y)
eHat3=residuals(fm3)
sum(eHat2^2)-sum(eHat3^2)

```

What happen if we change the order we enter variables in the model?

```r
fm=lm(Wage~Education+sex+ethnicity,data=Y)
fm2=lm(Wage~sex+ethnicity+Education,data=Y)
anova(fm0,fm)
anova(fm0,fm2)
anova(fm)
anova(fm2)
```

### Type-III SS 

The ANOVA with sequential SS is not ivariant with respect to the order in which we entered predictors in the model. To test the significance of each factor, we may want to do it bu enetering each factor last in the model. This is called the type-III SS.

```r
#install.packages(pkg='car',repos='https://cran.r-project.org/')
library(car)
anova(fm) # produces sequential ANOVA
Anova(fm,type='III') # SS when each factor is entered last

# Let's verify

# Education
 fm=lm(Wage~Education+sex+ethnicity,data=Y)
 fm0=lm(Wage~sex+ethnicity,data=Y)
 sum(residuals(fm0)^2)-sum(residuals(fm)^2)
 
# sex
 fm=lm(Wage~Education+sex+ethnicity,data=Y)
 fm0=lm(Wage~Education+ethnicity,data=Y)
 sum(residuals(fm0)^2)-sum(residuals(fm)^2)
 
# Ethnicity
 fm=lm(Wage~Education+sex+ethnicity,data=Y)
 fm0=lm(Wage~Education+sex,data=Y)
 sum(residuals(fm0)^2)-sum(residuals(fm)^2)

# RSS
 sum(residual(fm)^2)

```
