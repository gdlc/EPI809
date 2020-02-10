## Example of F-test using Type-III ANOVA, followed by testing of mulitple differences (contrasts) using Tukey's Honest Significance Difference

The code requires two packages `car` and `agricolae`, if you have not installed these packages, remove the comment `#` in the first two lines and run those lines.


```r
#install.packages(pkg='car',repos='https://cran.r-project.org/')
#install.packages(pkg='agricolae',repos='https://cran.r-project.org/')
library(car)
library(agricolae)

Y=read.table('~/Dropbox/809/datasets/wages2.txt',header=T)

# We can fit the model using lm, or aov which stands for analysis of variance
fm=lm(Wage~sex+ethnicity,data=Y)
fm2=aov(Wage~sex+ethnicity,data=Y)

summary(fm2)
anova(fm)

## Type-III SS and F-test
Anova(fm,type='III')

TukeyHSD(fm2,which='ethnicity')

```
