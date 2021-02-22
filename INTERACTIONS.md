## Interactions in Linear models

### 1) Factor-by-factor

Is the sex-wage gap different in the south and north regions?


```r
 Y=read.table('~/Dropbox/809/datasets/wages2.txt',header=T)

 fm0=lm(Wage~sex+Region,data=Y)
 fm1=lm(Wage~sex+Region+sex*Region,data=Y)


 # compare ANOVA results with t-test from fm1
  anova(fm0,fm1)
  summary(fm1)
  
  library(car)
  Anova(fm1,type='III')


 # Same as fm2, using a "means" model
  attach(Y)
  sex_region<-paste0(sex,'-',Region)
  fm2=lm(Wage~sex_region-1)
 
  library(agricolae)
  TukeyHSD(aov(Wage~sex_region-1),which='sex_region')
  
```

### 2) Factor-by-covariate

```r

 fm0=lm(Wage~sex+Region+sex*Region+Education,data=Y)
 fm1=lm(Wage~sex+Region+sex*Region+Education+sex*Education+Region*Education,data=Y)
 
 
 # compare ANOVA results with t-test from fm1
  anova(fm0,fm1)
  summary(fm1)
  Anova(fm1,type='III')


 # Same as fm2, using a "means" model
  attach(Y)
  sex_region<-paste0(sex,'-',Region)
  fm2=lm(Wage~sex_region-1)
 
  library(agricolae)
  TukeyHSD(aov(Wage~sex_region-1),which='sex_region')
  
```
