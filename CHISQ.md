## Chi-square test for independence of two Bernoulli random variables.

**Data**
```r
OC=rbind(c(2537,683),c(8747,1498))
rownames(OC)=c('BC','BCF')
colnames(OC)=c('<30','30+')
```

First we estimate the joint and the marginal distribtuions

```r
# (Empirical) Joint distribution
 OF=OC/sum(OC)

# (Empirical) Marginal for BC
 mBC=rowSums(OF)

# (Empirical) Marginal for Age strata
 mAge=colSums(OF)
```

Now we compute the expected frecuencies under independence

```r
 EF=OC
 EF[]=NA
 EF[1,1]=mBC[1]*mAge[1]
 EF[1,2]=mBC[1]*mAge[2]
 EF[2,1]=mBC[2]*mAge[1]
 EF[2,2]=mBC[2]*mAge[2]
```

And with this, we compute the expected counts

```r
 EC=EF*N
```

Next we compute the chi-square statistics `CHISQ=sum((EC-OC)^2/EC)`

```r
 CHISQ=0

 for(i in 1:2){
	for(j in 1:2){
	  CHISQ=CHISQ+((EC[i,j]-OC[i,j])^2)/EC[i,j]
	}
 }
```

Finally, we compute the p-value

```r
 pchisq(q=CHISQ,df=1,lower.tail=F)
```

[Back](https://github.com/gdlc/EPI809/blob/master/README.md)
