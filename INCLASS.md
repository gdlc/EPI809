

## In-class assigments


#### In-class 1

Enter the following data into excel or R


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

Produce a excel file or an R-script that computes the mean and variance of each variable and the covariance between the two.

Use only sums, differences, products and power functions (i.e., do not use the excel functions `average()`, `correl()`, etc. or the R-functions `mean()`, `var()`, `cov()` )

Turn your assigment into D2L

**Proposed solution**

```r
X=c(4.55, 4.95, 4.64, 4.97, 4, 4.47, 4.87, 4.38, 4.87, 4.59)

Y=c(4.99, 5.65, 5.5, 5.1, 4.51, 5.13, 5.27, 4.53, 5.39, 5.08)

mX=mean(X)
mY=mean(Y)
dX=X-mX
dY=Y-mY
dXdY=dX*dY
sum(dXdY)/(length(X)-1)

cov(X,Y)

```

#### In-class 2

**Proposed solution**


Read into the R-environment the [gout data set]().
 
1.1 Provide univariate summary statistics for all the variables.
1.2 Provide a scatter plot of serum urate (su) versus age).
1.3 Use basic basic operators `+`, `^2`, `-`, and `/` to compute the mean and variance of `su` and `age`, and the covaraince between the two.

```r


```
