

## In-class assigments


### In-class 1

Enter the following data into excel


| ID | X | Y |
|-----|----|----|
|1 |	4.55	| 4.99 |
|2	| 4.95 |	5.65 |
| 3	| 4.64 |	5.50 |
| 4 |	4.97	| 5.10 |
| 5	| 4.00 |	4.51 |
| 6	| 4.47	| 5.13 |
| 7	| 4.87 |	5.27 |
| 8	| 4.38	| 4.53 |
| 9	| 4.87	| 5.39 |
| 10 | 4.59 |	5.08 |

 - Produce a excel file  that computes the mean and variance of each variable and the covariance between the two. Use these statistics to compute the correlation coefficient. Do not use any function other than sum and average.

Hints: To compute variance and covariance, after computing the mean of X and Y (mX and mY, respectively), compute in new columns, for each observation in th data set, the deviation from the means ( xi-mX  and yi-mY), the product of the x- and y-deviation from the means (xi-mX)(yi-mY), and the sqaured deviations (xi-mX)(yi-mX), use the sums of these mean-deviations and cross-products, and the formulas available in the slides to compute the covariance between X and Y and the correlation coefficient.

  - Verify your results using the built-in formulas of Excel (average, var, covar, correl)
  
  - Replicate your code in R, you can load the data in R using the script provided below. Repeat the process above described, that is, compute the correlation without using the `var()`, `cov()`, and `cor()`, then verify your results using those functions.

```r
 X=c(4.55,4.95, 4.64, 4.97, 4, 4.47, 4.87, 4.38, 4.87, 4.59)
 Y=c(	4.99, 5.65,	5.5,	5.1,	4.51,	5.13,	5.27,	4.53,	5.39, 5.08)
```
  - Compare the R and Excel results.
  
Turn your assigment into D2L

**Proposed solution**

```r
 meanX=mean(X)
 meanY=mean(Y)
 
 dx=(X-meanX)
 dy=(Y-meanY)
 
 # Variances
 n=length(X)
 var(X)
 sum(dx^2)/(n-1)

 var(Y)
 sum(dy^2)/(n-1) 
 
 cov(X,Y)
 sum(dx*dy)/(n-1)
 
 cov(X,Y)/sqrt(var(X)*var(Y))
 cor(X,Y)

```

### In-class 2

Using the following data:

```r
 set.seed(195021)
 n=50
 x=runif(n)
 y=x+rnorm(n)/8
 cor(x,y)
```

1) Use `cor.test` to obtain an estimate, SE, CI and a p-value for H0: r=0 Vs Ha: r>=0
2) Obtain the same results assuming that the correlation coefficient follows a normal distribution.

```r
  cor.test(x,y)
  COR=cor(x,y)
  SE=sqrt((1-COR^2)/(n-2))
  tStat=COR/SE
  
  # three ways of doing the CI
  COR+c(-1,1)*1.96*SE # note, due to rounding, the two CIs are sligltly different
  qnorm(mean=COR,sd=SE,p=c(.025,.975))
  COR+qnorm(p=c(0.025,.975))*SE 
  
  # p-value
  H0=0
  tStat=(COR-H0)/SE
  
  ## Two ways of getting a one-sided p-value
  pnorm(q=tStat,lower.tail=FALSE)
  pnorm(q=COR,mean=0,sd=SE,lower.tail=FALSE)
  
  # For two-sided pvalue multiply by 2
```

### In-class 3

1) Using the data simulated for in-class 2:
  - calculate a 90% and a 95% CI for the correlation using Fisher's Z-transform (do not use cor.test)
  - caluclate a one-sided p-value for H0: Cor=0 and Cor=0.8 (do not use cor.test)
  - repeat your analysis using cor.test() and compare your results

Sumbit your file with the script for the above problem.

 **90% CI** (for 95% change alpha below)
```r
 alpha=0.1
 
 set.seed(195021)
 n=50
 x=runif(n)
 y=x+rnorm(n)/8
 
 COR=cor(x,y)
 
 Z= 0.5*log((1+COR)/(1-COR))
 SE.Z=sqrt(1/(n-3))  

# CI for Z
 CI.Z= qnorm(mean=Z,sd=SE.Z, p=c(alpha/2,1-alpha/2))
 
 # Now we map it back to the COR scale
   zInv=function(z){ (exp(2*z)-1)/(1+exp(2*z))}
   
 CI.R.FISHER=zInv(CI.Z)
 
 # Compare with
  cor.test(x,y,conf.level=0.9) 
```
**P-value for H0: COR>0**

A correlation =0 corresponds to a Z-transform of zero.
```r
 pnorm(mean=0,sd=SE.Z,q=Z,lower.tail=FALSE)
 cor.test(x,y,alternative='greater')
```

**P-value for H0: COR>0.8**
```r
 Z.H0=0.5*log((1+0.8)/(1-0.8))
 pnorm(mean=Z.H0,sd=SE.Z,q=Z,lower.tail=FALSE)

```

### In-class 4

Using Galton's data set compute and report:
  - Correlation between offspring height and mid-parental hight (report point estimate, p-value and 95% CI)
  - The estimated intercept and slope from the regression of offsping height on mid-parental height (use the `lm()` R-function).
  - Summarize your conclusions in no more than 2 sentences.
  

 **Proposed Solution**
 
```r
  DATA=read.table('https://raw.githubusercontent.com/gdlc/EPI809/master/GALTON.txt',header=TRUE)
  
  cor.test(DATA$Child,DATA$Midparent)
  
  tmp=lm(Child~Midparent,data=DATA)
  summary(tmp)

```

- The estimated correlation between an offspring's height and the midparent height was 0.46 (95% CI 0.406,0.508), and it is statistically signficantly different than zero.

-  The average change in the expected offspring height per inch change in mid-parental height is 0.646 inches. The estimated slope is significantly different than zero.


### In-class  5

Reproducing results from `lm()`

Using the following data:

```r
  DATA=read.table('https://raw.githubusercontent.com/gdlc/EPI809/master/GALTON.txt',header=TRUE)
  DATA=DATA[1:15,]

```

i) Estimate the regression of offspring height on mid-parental height using lm, report: Estimates, SEs, and p-values (hint: use `summary()`).

ii) Used the methods discussed in class to reproduce the t-statistic and the p-values produced by `lm()` starting from the estimates and SE produced by lm(). hint: to get the estimates you can use `summary(fm)$coef`, the first column contains estimates and the 2nd one SEs.

iii) Repeat (i) and (ii) using the first 100 rows of DATA instead of the top 15. How did SE and p-values changed?

**Proposed solution**

```r
  DATA=read.table('https://raw.githubusercontent.com/gdlc/EPI809/master/GALTON.txt',header=TRUE)
  
  # i)
  fm=lm(Child~Midparent,data=DATA[1:15,])
  summary(fm)
  
  # ii)
  est=coef(fm)[2]
  SE=summary(fm)$coef[2,2]
  tStat=est/SE
  df=15-length(coef(fm))
  pVal_norm=2*pnorm(q=abs(tStat),lower.tail=FALSE) # for n=15 we should use the t-dist not the normal
  pVal_t=2*pt(q=abs(tStat),df=df,lower.tail=FALSE)
  
  pVal_t
  pVal_norm
  
  # iii)
   fm2=lm(Child~Midparent,data=DATA[1:100,])
   summary(fm2)
 ```
 
 With larger sample size, the SE gets smaller, and the p-value becomes smaller than 0.01.


### In-class  6: ANOVA

For the [chicken data set](https://stat.ethz.ch/R-manual/R-devel/library/datasets/html/chickwts.html) reproduce the results of the ANOVA table, that is, use the data and the fitted model to determine the model, total, and residual DF, and the corresponding sum of squares. Compute the MS-squares, the F-statistic, and the corresponding p-value.

```r
  library(stats)
  data(chickwts)
  fm=lm(weight~feed,data=chickwts)
  anova(fm)
```

**Proposed solution**

```r
  # Sum of squares
   y=chickwts$weight
   TSS=sum((y-mean(y))^2) # total SS (SS after fitting H0)
   RSS=sum(residuals(fm)^2) # residual SS (SS after fitting Ha)
   MSS=TSS-RSS   # model SS
  
  # Degrees of freedom
   p=length(coef(fm)) # number of parameters
   n=length(y) # sample size
   MODEL.DF=p-1
   RES.DF=n-p
   TOTAL.DF=n-1
   
   ANOVA=data.frame('Source'=c('Model','Residual','Total'),'SS'=c(MSS,RSS,TSS),'DF'=c(MODEL.DF,RES.DF,TOTAL.DF))
   ANOVA$MS=c(ANOVA$SS[1]/ANOVA$DF[1],ANOVA$SS[2]/ANOVA$DF[2] ,NA)
   ANOVA$F=c(ANOVA$MS[1]/ANOVA$MS[2],NA,NA)
   ANOVA$pVal=c(pf(q=ANOVA$F[1],df1=ANOVA$DF[1],df2=ANOVA$DF[2],lower.tail=FALSE),NA,NA)
   
   # Compare
   ANOVA
   
   # with 
   anova(fm)
```



### In-class  7: Coding of factors and prediction equations

1) Using the wages data set:

  - Fit a linear regresssion on ethnicity using dummy coding (i.e., two dummy variables and one reference category, the default behavior of lm) and derive from the output the average wage of each group.
  - Verify your results by fitting the same model using a means-model parameterization.
  
 2) Extend the model of question 1 by adding education:
 
  - Fit the model
  - Report the estimated prediction equations for each ethnic group in the form of intercept+education\*slope
  
 3) Repeat # 2 with means coding (i.e., using wage~education -1)
 
 4) Repeat #3 adding Ethnicituy. What is the predicted wage for a female hispanic with 12 years of eucation?  

**Proposed solution**


```r
 #1#
   DATA=read.table('https://raw.githubusercontent.com/gdlc/EPI809/master/wages.txt',header=TRUE)
   fm=lm(wage~sex,data=DATA) # by default lm uses a dummy coding, with intercept
   
   # mean for female
   coef(fm)[1]
   
   # mean for males
   sum(coef(fm))
   
   # Verifying
   mean(DATA$wage[DATA$sex=='Male'])
   mean(DATA$wage[DATA$sex=='Female'])
   
   fm2=lm(wage~sex-1,data=DATA) # using `-1` in the formula instructs lm to use the means-model parameterization
   coef(fm2)
   
  #2#
   fm=lm(wage~sex+education,data=DATA)
   IntF=coef(fm)[1]
   IntM=sum(coef(fm)[1:2])
   slope=coef(fm)[3]
   
   # Equation for Female:   IntF + Education*slope
   # Equation for Male:     IntM + Education*slope
   
   #3# 
    fm2=lm(wage~sex+education-1,data=DATA)
    IntF=coef(fm2)[1]
    IntM=coef(fm2)[2]
    slope=ceof(fm2)[3]
    # Equation for Female:   IntF + Education*slope
    # Equation for Male:     IntM + Education*slope    
    
   #4# 
    fm3=lm(wage~sex+education+ethnicity-1,data=DATA)
    
    slope=coef(fm3)[3]
    
    IntFW=coef(fm3)[1]
    IntFB=sum(coef(fm3)[c(1,5)])
    IntFH=sum(coef(fm3)[c(1,4)])
 
    IntMW=IntFW+coef(fm3)[2]
    IntMB=IntFB+coef(fm3)[2]
    IntMH=IntFH+coef(fm3)[2]
    
    # Prediction for female hispanic, 12 yr of education
    IntFH+slope*12
    predict(fm3,newdata=data.frame(education=12,ethnicity='Hispanic',sex='Female'))
    
```

### In-class  8: Estimation of error variance and diagnosis

Using the [gout](https://raw.githubusercontent.com/gdlc/EPI809/master/gout.txt) data set (use `sep=' '`):

  -  For each of the blood biomarkers (UricAcid Creatinine BMI SBP Glucose HDL LDL Triglycerides) fit a linear regression on sex, age and ethnicity.
  - For each of them, estimate the error variance and an approximate R-squared (1- esitmated error variance / var(y) ). Verify your results by comparing with the results of `summary(fm)$r.squared`.
  - Conduct diagnosis analysis. Identify and report possible outliers and whether you think some of the traits may need to be transformed.

**Proposed solution**

*Error variance and R-sq.*

```r
 DATA=read.table(file='https://raw.githubusercontent.com/gdlc/EPI809/master/gout.txt',header=TRUE,sep=' ')
 
  
 y=DATA$UricAcid
 fm=lm(y~Sex+Age+Race,data=DATA)
 summary(fm)
 
 eHat=residuals(fm)
 n=length(eHat)
 p=length(coef(fm))
 
 TSS=sum((y-mean(y))^2)
 RSS=sum(eHat^2)
 varE=RSS/(n-p)
 1-varE/var(y)
 
 summary(fm)$r.squared
 SS.Reg=TSS-RSS
 RSq=SS.Reg/TSS
 RSq

```
*Diagnosis*

```r
 traits=c('UricAcid','Creatinine', 'BMI', 'SBP', 'Glucose', 'HDL','LDL', 'Triglycerides')
 

 for(i in traits){
   y=DATA[,i]
   fm=lm(y~Sex+Age+Race,data=DATA)
   plot(fm,which=2,main=i) 
   Sys.sleep(5)
 }
```

### In-class 9: Testing in the MLR model

** Using the gout data set test the following research questions**: 

    - (9.1) Does BMI, race, age, or sex have any effect on SBP?
    - (9.2) Afer accounting for differences due to race and sex, does BMI or age have any effect on SBP?
    - (9.3) After accounting for differences due to sex, age, and race, does BMI has an effect on SBP?

For each of the above questions: identify the null and alternativy hypothesis, identify an adequate test (i.e., t or F test),  implement the test, and state your conclusions.
 
**Proposed solution**

**9.1** 

This amounts to test the model as a whole. Rejection implies that at least one of the effects is different than zero. We use ANOVA+Ftest
 
```r
 DATA=read.table(file='https://raw.githubusercontent.com/gdlc/EPI809/master/gout.txt',header=TRUE,sep=' ')
 HA=lm(SBP~BMI+Race+Age+Sex,data=DATA)
 H0=lm(SBP~1,data=DATA)
 anova(H0,HA)
```

**9.2**

This is an instance of the *long-versus-short* regressions. The null includes the effects we want to contidion on, the alternative includes those effects plus the ones we want to test.

```r
 HA=lm(SBP~BMI+Race+Age+Sex,data=DATA)
 H0=lm(SBP~Race+Sex,data=DATA)
 anova(H0,HA)

```

**9.3**

This is a 1DF-test, the alternative is HA from the previous examples, the null dorps just BMI from HA. The p-value can be obtained using ANOVA+Ftest or directly from the t-test.'

```r
 HA=lm(SBP~Race+Age+Sex+BMI,data=DATA)
 H0=lm(SBP~Race+Age+Sex,data=DATA)
 anova(H0,HA)
 summary(HA)
 
```

### In-class 10: Multiple testing

Using the testing procedure we discussed in class (last slide), test which of the factors included in the following model have significant effects on Uric Acid.

Describe the procedure you are following, and state your conclussions.

```r
 fm=lm(UricAcid~Creatinine+BMI+SBP+Sex+Age+Race,data=DATA)
```


