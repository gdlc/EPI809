## Multiple Linear Regression

**Example 1:** Fitting models for factors with multiple levels.

The following example presents a regression of wage on ethnicity, this factor has 3 levels (Black, Hispanic, and White). 
If we create three dummy variables (B/H/W), the sum  of the three dummies is always equal to 1–these three variables are colinear. Thus, we cannot fit a model with an intercept and three dummies. There are two standard parameterizations used for these models: (a) The means model uses one dummy per group without including an intercept. (b) The dummy coding fits a model with an intercept plus q-1 dummies, where q is the number of levels of the factor. The two models are statistically equivalent; however the interpretation of the parameters differ.

```r
 DATA=read.table('~/Desktop/wages2.txt',header=T)
 
 ## Creating the dummy variables
 B=ifelse(DATA$ethnicity=='B',1,0)
 H=ifelse(DATA$ethnicity=='H',1,0)
 W=ifelse(DATA$ethnicity=='W',1,0)
 
 
 ## Means model: three dummies, no intercept
 fm1=lm(wage~B+H+W-1) #'-1' tells lm to exclude the intercept
 fm2=lm(Wage~ethnicity,data=DATA) #'-1' tells lm to exclude the intercept
 summary(fm1)
 
 # In the 'means model' parameter estimates are just the means of the group 
 # (if we had covariates these would be group-specific intercepts)
 mean(DATA$Wage[B==1])
 mean(DATA$Wage[H==1])
 mean(DATA$Wage[W==1])
 
 
 ## Dummy coding: intercept + two dummy variables
 fm2=lm(Wage~ethnicity,data=DATA)

 # fm1 and fm2 are just two parameterization of the same model:
 plot(predict(fm1),predict(fm2))
 plot(residuals(fm1),residuals(fm2))
 anova(fm1)
 anova(fm2)
```

Challenge: recover the mean of each group from the parameter estimates of `fm2`

**How does lm construct the contrasts (dummy variables in this case)?**

The `model.matrix` function create the incidence matrix used by `lm`.

```r
  W=model.matrix(~ethnicity,data=DATA)
  head(W)
  
  W=model.matrix(~ethnicity-1,data=DATA)
  head(W)
```
Once the incidence matrix is crated, the columns of `W` are treated as quantitative variables.

**Example 2:** A model with factors (i.e., discrete predictors) and covariates (i.e., quantitative predictors.

```r
 fm3=lm(Wage~ethnicity+Education,data=DATA)
 summary(fm3)
 
```
**Example 3:** ANOVA in the Multiple Linear Regression

```r

 anova(fm3)
 
 anova(fm2,fm3)
 
 fm3b=m(Wage~Education+ethnicity,data=DATA)
 
 summary(fm3b)
 anova(fm2,fm3b)

```

