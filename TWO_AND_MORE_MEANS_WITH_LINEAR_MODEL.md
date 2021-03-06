## Comparinng two means using a linear model


You studied in EPI 808 how to test for differences in the means of two or more groups, here we show how the same can be done using a linear model. 

To do this we create a *dummy* variable, that is a categorical variable that take value 0 for one group (e.g., female) and one for the other group (e.g., male).



```r
# use file='https://raw.githubusercontent.com/gdlc/EPI809/master/wages.txt' 
Y=read.table(file='~/Dropbox/EPI_809/datasets/wages.txt',header=T)


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
 vM=var(yM)/nM # variance of the sample mean (eq. 5.9, pp 168)
 vF=var(yF)/nF # same as above
 vDiff=vM+vF   # because male and female are independent, the variance of the difference
 			   # is the sum of the variances.
 
# t-statistic
SE.D=sqrt(vDiff)
tStat=D/SE.D

## Now with a linear model
 fm=lm(Wage~Sex,data=Y)
 summary(fm)
 
 round(c(D,SE.D,tStat,pt(abs(tStat),df=nM+nF-2,lower.tail=F)),4)

```

### More than two means

The following [data set](https://stat.ethz.ch/R-manual/R-devel/library/datasets/html/chickwts.html) contains the weight of 71 chickens that were randomly allocated to 5 different feed suplements.

```r
 library(stats)
 data(chickwts)
 head(chickwts)
 table(chickwts$feed)
 boxplot(weight~feed,data=chickwts)
```

**What happens if we fit a linear model?**

When a predictor variable (feed in the example below) is of type character or factor, `lm()` picks a reference group, and creates a dummy variable for each of the other groups. The mean of the reference group is represented by the intercept. The mean of each of the other groups is represented by the sum of the intercept plus the corresponding regression coefficient.

```r
 fm=lm(weight~feed,data=chickwts)
 summary(fm)
 mean(chickwts$weight[chickwts$feed==horsebean])
 coef(fm)[1]+coef(fm)[2]
 
 anova(fm)
```


**The model.matrix() function**

In a linear model, the incidence matrix for effects, is a matrix whose columns contain the predictors (either dummy variables or continous covariates). If the model involves *p* parameter and the sample size is *n*, the incidence matrix has *n* rows and *p* columns. 

The function `lm(y~sex+age+...,data=...)` takes the variables on the right-hand-side of the formula (`~sex+age+...`) and produces the incidence matrix for the model. By default (unlesss `-1`, e.g., `~age+sex-1`, is used in the formula, the first column of the incidence matrix is a vector of ones, which is the incidence vector for the intercetp. The remaining columns are either covariates or dummy-variables associated to factors in the model (e.g. sex, ethnicity). The following example illustrates how `model.matrix()` works.

```r
 X=model.matrix(~feed,data=chickwts)
 dim(X)
 head(X,20)
 
 fm=lm(weight~feed,data=chickwts)
 summary(fm)

```
