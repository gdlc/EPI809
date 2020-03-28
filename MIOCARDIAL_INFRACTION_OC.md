Data and analyses from example 13.5 (chapter 13) of the book



**Data**

```r

 DATA=matrix(nrow=2,ncol=2,byrow=T,data=c(13,4987,7,9993))
 colnames(DATA)=c('MI=Yes','MY=No')
 rownames(DATA)=c('OC-use=Yes','OC=use=No')
 print(DATA)
```



**Row, col and total %**

```r
TOT_PCT=100*DATA/sum(DATA)

colsums=colSums(DATA)
rowsums=rowSums(DATA)
ROW_PCT=DATA
COL_PCT=DATA

ROW_PCT[1,]=100*DATA[1,]/rowsums[1]
ROW_PCT[2,]=100*DATA[2,]/rowsums[2]

COL_PCT[,1]=100*DATA[,1]/colsums[1]
COL_PCT[,2]=100*DATA[,2]/colsums[2]


print(round(TOT_PCT,2))
print(round(ROW_PCT,2))
print(round(COL_PCT,2))
```
