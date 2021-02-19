# Homework EPI 809, Module 1

## HW1: Correlation and Linear Regression

**Due  Wed., Feb. 10, 2021 in D2L**

**1: Correlation**
  
  **1.1.** Using the [gout](https://github.com/gdlc/EPI809/blob/master/gout.txt) data set produce the following summary statistics:
  
    * Mean, SD, quartiles and range for serum uruate (SU) and for Age.
    * A histogram of each the variables listed above.
    * A scatter plot of SU versus Age
    * Summarize in no more than three sentences your findings.
   
  **1.2.** Report the correlation between SU and Age, p-values for two-sided and a one-sded (HA r>=0) test, and 95 and 99% CI for the correlation. For the CI report both a CI assuming normality and one Fisher's Z-transform.

 
**2: Linear Regression-I**

  **2.1.** Using the gout data set estimate the regression of SU on Age, report parameter estimates, SE and p-values (Hint: use the `fm<-lm()` function and apply the `summary(fm)` function to the object returned by `lm()`.
  
  **2.2.** Does Age has a significant effect on SU?
  
  **2.3.** Explain with your own words the interpretation of each of the parameters of the model.
  
  **2.4.** Report the predicted SU for individuals of 40, 50, 60, and 70 years.
  
  **2.5.** Summarize your findings (no more than 3 sentences).


**3: Linear Regression II**

Using the data provided in Table  11.1 (p 459), reproduce the results of Figure 11.1 of the textbook, including:
  
  **3.1.** Fit the model and report parameter estimates
  
  **3.2.** Using the fitted model report an ANOVA Table (Hint: apply the `anova()` function to the model fitted to obtain your answer in 3.1). Your results should match those in Table 11.3 of the textbook.
  
  **3.3.** Using R reproduce the plot of Figure 11.1
  
  
  
## HW2: Multiple Linear Regression

Due Feb 24th in D2L

In the HW you will use the [gout](https://github.com/gdlc/EPI809/blob/master/gout.txt) data set available in our github repository. 


**1.  Preliminary, descriptive analysis**

Present:
- A histogram of serum urate (labeled as UricAcid) and one historgram for the logarithm of serum urate. 
- Boxplots of log-urate versus sex and race.
- Scatter plot of log-urate versus age.

Summarize your findings. 

**2.	Regression analysis**

2.1.	The goal is to study the effect of sex, race and age on serum urate. Would you recommend using serum urate or log-urate as response? Justify

2.2.	Regress urate (or log-urate if you decided to use the transformation) on sex, race and age using a multiple regression model, and conduct an F test to test whether at least one of the factors/covariates included in the model have an effect on the response. Summarize your conclusions.

2.3.	If you find that at least one factor/covariate had a non-null effect, test individual factors and summarize your findings (hint: for 1-df test you can use the t-test, which yields results equivalent to the 1-DF F-test).

**3.	Testing long and short regression**

A researcher wants to test whether either race, sex, or both have an effect on urate after accounting for differences attributable to age.

3.1.	Describe the null and alternative hypothesis involved
3.2.  What are the model, residual, and total df of the test? Explain.
3.3.	Implement the test
3.4.	Summarize your results

**4.	Diagnostics** 

Present diagnostic analysis for the long regression `y~sex+race+age`, where y is either serum urate or log-urate. Summarize your findings.

**5.	Consider expanding the long regression of 2-4 to model sex-by-race interactions, sex-by-age and race-by-age interactions.** 

5.1.	Present the null and alternative hypotheses that you plan to use to test the above interactions.
5.2.	Implement the test and present your results
5.3.	Summarize your findings
