## Correlation

**Reading the data and producing descritptive statistics & plots**
```r
 DATA=read.table('~/Dropbox/STATCOMP/2018/wage.txt',header=T)
 str(DATA)
 hist(DATA$Wage,30)
 table(DATA$Education)
 summary(DATA$Wage)
 plot(Wage~Education,data=DATA,col='blue')
```

**Assessing co-variability using deviations from the mean**

```r
 x=DATA$Education
 y=DATA$Wage
 meanX=mean(x)
 meanY=mean(y)
 dx=(x-meanX)
 dy=(y-meanY)
 
 signColor=ifelse(sign(dx)*sign(dy)==1,'blue','red')

 plot(Wage~Education,data=DATA,col=signColor);
 abline(h=meanY,lwd=1.5,lty=2)
 abline(v=meanX,lwd=1.5,lty=2)
 
 sum(sign(dx)*sign(dy)==1)
 sum(sign(dx)*sign(dy)== -1)

```

**Variance, covariance and correlation**

```r
 dx=(x-meanX)
 dy=(y-meanY)
 
 # Variances
 n=length(dx)
 var(x)
 sum(dx^2)/(n-1)

 var(y)
 sum(dy^2)/(n-1) 
 
 cov(x,y)
 sum(dx*dy)/(n-1)
 
 cov(x,y)/sqrt(var(x)*var(y))
 cor(x,y)

```
