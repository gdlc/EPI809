## Multiple Linear Regression

**Example 1:** Fitting models for factors with multiple levels.

The following example presents a regression of wage on ethnicity, this factor has 3 levels (Black, Hispanic, and White). 
If we create three dummy variables (B/H/W), the sum  of the three dummies is always equal to 1–these three variables are colinear. Thus, we cannot fit a model with an intercept and three dummies. There are two standard parameterizations used for these models: (a) The means model uses one dummy per group without including an intercept. (b) The dummy coding fits a model with an intercept plus q-1 dummies, where q is the number of levels of the factor. The two models are statistically equivalent; however the interpretation of the parameters differ.

```r
 fname='https://raw.githubusercontent.com/gdlc/EPI809/master/wages.txt'
 DATA=read.table(file=fname,header=T)
 
 attach(DATA) # attachs the variables to the environment...
 
 ## Creating the dummy variables
 B=ifelse(DATA$ethnicity=='Black',1,0)
 H=ifelse(DATA$ethnicity=='Hispanic',1,0)
 W=ifelse(DATA$ethnicity=='Other',1,0)
 
 
 ## Means model: three dummies, no intercept
 fm1=lm(wage~B+H+W-1) #'-1' tells lm to exclude the intercept
 fm2=lm(wage~ethnicity-1,data=DATA) #'-1' tells lm to exclude the intercept
 summary(fm1)
 summary(fm2)
 
 
 # In the 'means model' parameter estimates are just the means of the group 
 # (if we had covariates these would be group-specific intercepts)
 mean(wage[B==1])
 mean(wage[H==1])
 mean(wage[W==1])
 
 
 ## Dummy coding: intercept + two dummy variables (the default in lm())
 fm3=lm(wage~ethnicity,data=DATA)

 # fm1 and fm2 are just two parameterization of the same model:
 plot(predict(fm1),predict(fm3))
 plot(residuals(fm1),residuals(fm3))
 
 sum(residuals(fm1)^2))
 sum(residuals(fm2)^2))
 sum(residuals(fm3)^2))
 

```

Challenge: recover the mean of each group from the parameter estimates of `fm2`


**Setting the order of the levels of a factor**

By default, R orders the levels of a factor based on alphabetical order. The first level is then picked as the reference group.  

```r
  ethnicity=DATA$ethnicity
  str(ethnicity) # a character variable
  
  ethnicity=factor(ethnicity) # a factor with three levels
  
  levels(ethnicity) # the order of the levels.
  
  wage=DATA$wage
  fm=lm(wage~ethnicity)
  
  summary(fm)
```

You can tell R which one should be the reference group by chainging the order of the levels of a factor.

```r
 levels(ethnicity)=c('Black','Other','Hispanic')
 fm=lm(wage~ethnicity)
 summary(fm)
```


**How does lm construct the contrasts (dummy variables in this case)?**

The `model.matrix` function create the incidence matrix used by `lm`.

```r
  X=model.matrix(~ethnicity)
  head(X)
  
  X=model.matrix(~ethnicity-1)
  head(X)
```
Once the incidence matrix is crated, the columns of `W` are treated as quantitative variables.

**Example 2:** A model with factors (i.e., discrete predictors) and covariates (i.e., quantitative predictors.

```r
 fm4=lm(Wage~ethnicity+Education,data=DATA)
 summary(fm4)
```

