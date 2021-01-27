## Correlation

**Reading the data and producing descritptive statistics & plots**
```r
 DATA=read.table('~/Dropbox/STATCOMP/2018/wage.txt',header=T)
 dim(DATA)  # returns number of rows and columns
 str(DATA)  # describe the data type and structure
 head(DATA) # shows the first rows
 tail(DATA) # same for last rows
 summary(DATA) # summaries by variable

```

```r
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

**Inference**


*Correlation coefficient and it's SE*

```r
 COR=cor(x,y)
 V=(1-COR^2)/(n-2)
 SE=sqrt(V)
 cor.test(x,y)
```

*Confidence interval and p-values assuming normality*

```r
 # t-statistics
  tstat=COR/SE 
 
 # CI assuming normality 
  CI.NORM= qnorm(mean=COR,sd=SE,p=c(.025,.975))
   
  CI.NORM
   
 # P-value

```

*Using Fisher's Z-transform*

```r
 # CI based on Fisher's z-transform
  Z= 0.5*log((1+COR)/(1-COR))
  SE.Z=sqrt(1/(n-3))

  # CI for Z
   CI.Z= Z+c(-1,1)*SE.Z*qnorm(.975) # aprox. 1.96
  	
   zInv=function(z,n){
    	r=(exp(2*z)-1)/(1+exp(2*z))
    	return(r)
    
	}	
 CI.R.FISHER=zInv(CI.Z)
 rbind(CI.NORM,CI.R.FISHER)	
```
**Note**: compare the results obtained with Fisher's Z-transform and those from `cor.test(x,y)`.


