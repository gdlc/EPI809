Data and analyses from example 13.5 (chapter 13) of the book

**Data
```{r}
DATA <- matrix(nrow = 2, ncol = 2, byrow = T, data = c(13, 4987, 7, 9993))
colnames(DATA) <- c('MI=Yes', 'MY=No')
rownames(DATA) <- c('OC-use=Yes', 'OC=use=No')
print(DATA)
```
**Column 1 Risk Estimates
```{r}
p1 <- DATA[1,1] / DATA[1,2]
p2 <- DATA[2,1] / DATA[2,2]
n1 <- DATA[1,1] + DATA[1,2]
n2 <- DATA[2,1] + DATA[2,2]
ASE <- sqrt(p1*(1-p1)/n1 + p2*(1-p2)/n2)
DIFF <- p1-p2
CI95l <- DIFF - qnorm(1-0.05/2) * ASE
CI95u <- DIFF + qnorm(1-0.05/2) * ASE
data.frame(DIFF, ASE, CI95l, CI95u)
```
**Risk Difference Test asuming normality
```{r}
Z <- DIFF / ASE
Z
pnorm(Z, lower.tail = F)
pnorm(Z, lower.tail = F) + pnorm(-Z)
```
**With "correction"
```{r}
corr <- (1/n1 + 1/n2)/2
CI95l - corr
CI95u + corr
```
**Confidence limits for the Relative Risk
```{r}
RR <- p1 / p2
ASE_log <- sqrt(1 / DATA[1,1] - 1/ n1 + 1 / DATA[2,1] - 1 / n2)
exp(log(RR) - qnorm(1-0.05/2) * ASE_log)
exp(log(RR) + qnorm(1-0.05/2) * ASE_log)
```
**Relative Risk Test
```{r}
RR_Z <- log(RR) / RR_ASE
RR
RR_Z
pnorm(RR_Z, lower.tail = F)
pnorm(RR_Z, lower.tail = F) + pnorm(-RR_Z)
```
**Odds Ratio and Relative Risks
```{r}
(OR <- (p1/(1-p1)) / (p2/(1-p2)))
exp(log(OR) - qnorm(1-0.05/2) * ASE_log)
exp(log(OR) + qnorm(1-0.05/2) * ASE_log)
```
**Another example: (with packages)%**
```{r}
DATA2 <- matrix(nrow = 2, ncol = 2, byrow = T, data = c(40, 1960, 10, 990))
colnames(DATA2) <- c('Disease=Yes', 'Disease=No')
rownames(DATA2) <- c('Group=Current', 'Group=Ex')
print(DATA2)
```
**Column 1 Risk Estimates
```{r}
diff_prop <- prop.test(DATA2, correct = F)
(DIFF <- diff_prop$estimate[1] - diff_prop$estimate[2])
diff_prop$conf.int
(ASE <- ((diff_prop$estimate[1] - diff_prop$estimate[2] - diff_prop$conf.int[1]) / 1.96))
```
**Risk Difference Test
```{r}
Z <- DIFF / ASE
pnorm(Z, lower.tail = F)
pnorm(Z, lower.tail = F) + pnorm(-Z)
```
**Relative Risk Test
```{r}
library(epitools)
riskratio.wald(DATA2, rev = 'b')$measure
riskratio.wald(DATA2, rev = 'b')$p.value
```
**Odds Ratio
```{r}
oddsratio.fisher(DATA2)$measure
oddsratio.fisher(DATA2)$p.value
```
