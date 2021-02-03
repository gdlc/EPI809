

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

ii) Used the methods discussed in class to reproduce the t-statistic and the p-values produced by `lm()` starting from the estimates and SE produced by lm(). hint: to get the estimates you can use `ssummary(fm)$coef`, the first column contains estimates and the 2nd one SEs.

iii) Repeat (i) and (ii) using the first 100 rows of DATA instead of the top 15. How did SE and p-values changed?


