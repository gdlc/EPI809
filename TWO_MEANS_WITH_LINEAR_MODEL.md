## Comparinng two means using a linear model


You studied in EPI 808 how to test for differences in the mean of two groups, here we show how the same can be done
using a linear model. To do this we create a *dummy* variable, that is a categorical variable that take value 0 for one group (e.g., female) and 
one for the other group (e.g., male).



```r

Y=read.table('~/Dropbox/809/datasets/wages.txt',header=T)

# data by group
 yM=Y$Wage[Y$Sex==0]
 yF=Y$Wage[Y$Sex==1]

# Sample size by group
 nM=length(yM)
 nF=length(yF)

# Mean by group and difference
 mM=mean(yM)
 mF=mean(yF)
 D=mF-mM
 
# Variance of the sample mean and of the difference
 vM=var(yM)/nM # variance of the sample mean (eq. 6.2, pp 168)
 vF=var(yF)/nF # same as above
 vDiff=vM+vF   # because male and female are independent, the variance of the difference
 			   # is the sum of the variances.
 
# t-statistic
SE.D=sqrt(vDiff)
tStat=D/SE.D


## Now with a linear model
 fm=lm(Wage~Sex,data=Y)
 summary(fm)
 

```
