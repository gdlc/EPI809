## This entry contains scripts we used in class, with very little editing....

#### Jan 25th, Monte Carlo Study, correlation coefficient

```r
## Population
 N=1e6
 X=rnorm(N)
 length(X)
 head(X)
  hist(X,30)

 Y=0.8*X + rnorm(N)
 rho=cor(X,Y) # true population parameter
 
## Let's estimate rho using 20 samples
 n=20
 tmp=sample(1:N,size=n)
 y=Y[tmp]
 x=X[tmp]
 cor(x,y) # point estimate
 
## Monte Carlo experiment: repeat the above many times, each time
##                         using a different sample from the pop.


B=1000
COR=rep(NA,B)

for(i in 1:B){
   tmp=sample(1:N,size=n)
   x=X[tmp]
   y=Y[tmp]
   COR[i]=cor(x,y)
}


## Sampling distribution
 hist(COR,50)
 abline(v=rho,col=2,lwd=2) # true pop. parameter
 
## Is the SE correct?
  sqrt((1-rho^2)/(n-2))
  sd(COR)
```
