## Analysis of Variance (ANOVA) in the Linear Regression Model

The following code shows how to perform Analysis of Variance (ANOVA) in R using the `lm()` and `anova()` functions.
The examples use a simple linear regression model; however, the same approach can be used for models involving more than one predictor (i.e., multiple linnear regressions).


```r
# Reading the data and checking the resulting data matches what we expect
 Y=read.table('~/Dropbox/809/datasets/galton.txt',header=T)
 # To show results more clearly, I'll use a subset of the data..
 Y=Y[1:20,]
 head(Y)
 dim(Y)
 str(Y)

# Fitting a linear regression model
 fm=lm(Child~Midparent,data=Y)

# Parameter estimates
 summary(fm)

# Analysis of Variance
anova(fm)
```

Now, let's replicate all the ANOVA results

```r
 # The total sum of squares is the sum of the squared deviations of the response variable from its mean
  TSS=sum((Y$Child-mean(Y$Child))^2)
  
 # Then we can compute the residual sum of squares (i.e., variability not explained by the model)
 
 # 1st retrieve the fitted coefficients
  muHat=coef(fm)[1]
  bHat=coef(fm)[2]
 
 # Next, compute predictions and model residuals
  yHat=muHat+Y$Midparent*bHat
  eHat=Y$Child-yHat
 # Now, compute the residual sum of squares
  RSS=sum(eHat^2)

 # The amount of variability explained by the model (i.e., the model sum of squares) is TSS-RSS
  MSS=TSS-RSS
  # Also equal to: sum((yHat-mean(Y$Child))^2)

 # F-test
   n=nrow(Y)
   modelDF=length(coef(fm))-1
   resDF=n-modelDF-1
   F=(MSS/modelDF)/(RSS/resDF)
   pVal=pf(F,df1=modelDF,df2=resDF,lower.tail=F)
   
 # Now compare with the results from the anova function
  anova(fm)
  TSS
  RSS
  MSS
  MSS/modelF
  RSS/resDF
  F
  pVal
 
 # The model R-squared is the proportion of variance explained by the model, that is
  R2=MSS/TSS
  
 # Compare with the summary function
   summary(fm)
   R2
   

```
