# Pilot Study on Sampling
Repository for storing materials and data from the pilot study on sampling to accompany ["Sources of Philosophical Intuitions: Towards a Model of Intuition Generation"](https://github.com/DominikDziedzic/IntuitionGenerationProject) project.

This study demonstrates the causal relationship between value- and probability-guided judgments, and responses. David Kaplan’s ‘Carnap vs. Agnew’ case (1978, p. 239) concerning the reference of demonstratives has been converted to experimental materials and presented to participants in one of four conditions.

**The online survey tool used in this study can be accessed here: [On the Use of English Demonstrative "That" (Sampling)](http://kognilab.pl/ls3/index.php/914295?newtest=Y&lang=en).**

## The Extended List of References

To view the extended list of references on the sampling mechanism, visit: [**List of References**](https://github.com/DominikDziedzic/IntuitionGenerationProject/blob/main/List%20of%20References%20-%20Towards%20a%20Model%20of%20Intuition%20Generation.pdf).

---

Content of the repository (after opening each file, right-click and select Save as):
- **Raw data** 
  - [in .txt]() 
  - [in .csv]()
- **Experimental materials**
  -  [in .docx]()
  -  [in .pdf]() 
- [**Source code in .R**]()

# Analysis

The results of statistical analyses are presented below: 
- [**Frequentist Linear Regression**](https://github.com/DominikDziedzic/PilotStudySampling#frequentist-linear-regression)
- [**Bayesian Linear Regression**](https://github.com/DominikDziedzic/PilotStudySampling#bayesian-linear-regression)
- **Mediation Analysis**
  - [Frequentist Variant](https://github.com/DominikDziedzic/PilotStudySampling#frequentist-variant)
  - [Bayesian Variant](https://github.com/DominikDziedzic/PilotStudySampling#bayesian-variant)
- [**References**](https://github.com/DominikDziedzic/PilotStudySampling#references)

## Frequentist Linear Regression

### Required packages
Run the following code in R to install the required packages:
``` r
install.packages("")
install.packages("")
install.packages("")
```

Load the required packages:
``` r
library()
library()
library()
```

### Import data
Download raw data files in [.txt]() or [.csv format]() and run the following in R to import data:
``` r
# a) if in .txt:
data <- read.delim(file.choose())
# b) if in .csv:
data <- read.csv(file.choose(), sep = ";")

attach(data) # attach your data, so that objects in the database can be accessed by giving their names
```

Let's review the dataset:
``` r
str(data)
# 'data.frame':	59 obs. of  5 variables:
#  $ valence        : int  1 1 1 1 1 1 1 1 1 1 ...
#  $ probability    : int  1 1 1 1 1 1 1 1 1 1 ...
#  $ reference      : int  1 0 1 0 1 1 1 1 1 1 ...
#  $ sub_valence    : int  TODO
#  $ sub_probability: int  TODO
```
The dataset consists of two IVs (i.e., `valence` and `probability`) and the DV (`reference` = responses to a scenario). The length of the dataset is 59. Assignment to conditions was random, with 13 participants in the good-probable condition, 16 in the good-improbable, 14 in the bad-probable, and 16 in the bad-improbable. Definitions of the conditions are as follow:
- In the **good-probable condition**, the participant reads the following scenario:
  - Suppose that, without turning and looking, David points to the place on his wall which has long been occupied by a picture of Rudolf Carnap, a famous philosopher, and he says: “**That** is a picture of one of the greatest philosophers of the twentieth century, and now it is yours — it is your birthday present”. But unbeknownst to David, someone has replaced his picture of Carnap with a valuable portrait of Marquis de Lafayette, a French aristocrat, a general in the American Revolutionary War, and a dignified figure but not a philosopher at all. You have seen paintings that look so valuable hanging on people’s walls. Indeed, many people have such paintings in their homes in the town where David lives. Lafayette is an important figure in the history of the town — wounded during a battle near the town, he still managed to organize a successful retreat saving many lives — and many people in the town display his portrait to this day.
- In the **good-improbable condition**, the last three sentences are replaced with:
  - You have never seen anything that looked so valuable hanging on someone’s wall. Indeed, no one has such paintings in their homes in the town where David lives. Given the technique and execution, you do not have the slightest doubt that the painting was produced a long time ago and that, in all probability, the work is authentic.
- In the **bad-probable condition**, the participant reads:
  - Suppose that, without turning and looking, David points to the place on his wall which has long been occupied by a picture of Rudolf Carnap, a famous philosopher, and he says: “**That** is a picture of one of the greatest philosophers of the twentieth century, and now it is yours — it is your birthday present”. But unbeknownst to David, someone has replaced his picture of Carnap with a cheap and nasty picture of you that has been photoshopped to show you in a patently offensive way. You have seen pictures that look so offensive hanging on people’s walls. Indeed, many people have such pictures in their homes in the town where David lives — many people make such crude jokes nowadays.
- In the **bad-improbable condition**, the last two sentences are replaced with:
  - You have never seen anything that would look so offensive hanging on someone’s wall. Indeed, no one has such pictures in their homes in the town where David lives. The picture is so offensive that you can’t help but wonder why anyone would produce such an image in the first place.

In all four conditions, the participant answers the question: _When David uses the expression “That”, is he talking about: (A) The picture of Rudolf Carnap? (B1) The portrait of Marquis de Lafayette? / (B2) The picture of you?_ (The choice between B1 and B2 depends on the assigned condition — good or bad).

The two remaining variables — `sub_valence` and `sub_variability` — are relevant for mediation analysis (see below).

Compute the model with interaction
``` r
summary(lm(reference ~ valence * probability, data = data))
# Residuals:
#     Min      1Q  Median      3Q     Max 
# -0.8461 -0.4286 -0.1250  0.5000  0.8750 

# Coefficients:
                    Estimate Std. Error t value Pr(>|t|)  
# (Intercept)          0.12500    0.11115   1.125   0.2656  
# valence              0.37500    0.15718   2.386   0.0205 *
# probability          0.30357    0.16270   1.866   0.0674 .
# valence:probability  0.04258    0.23244   0.183   0.8553  
# ---
# Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

# Residual standard error: 0.4446 on 55 degrees of freedom
# Multiple R-squared:  0.2577,	Adjusted R-squared:  0.2172 
# F-statistic: 6.363 on 3 and 55 DF,  p-value: 0.0008822
```

Since the `valence` × `probability` type interaction is insignificant let's try and compute the model without interaction:
``` r
summary(lm(reference ~ valence + probability, data = data))
model <- lm(reference ~ valence + probability, data = data)
# Residuals:
#     Min      1Q  Median      3Q     Max 
# -0.8342 -0.4397 -0.1153  0.4903  0.8847 

# Coefficients:
#             Estimate Std. Error t value Pr(>|t|)   
# (Intercept)  0.11526    0.09677   1.191  0.23863   
# valence      0.39447    0.11479   3.436  0.00112 **
# probability  0.32443    0.11519   2.817  0.00669 **
# ---
# Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

# Residual standard error: 0.4407 on 56 degrees of freedom
# Multiple R-squared:  0.2572,	Adjusted R-squared:  0.2307 
# F-statistic: 9.696 on 2 and 56 DF,  p-value: 0.0002423
```
The effects of `valence` and `probability` are significant (p < 0.01). Compute eta squared values by running:
``` r
effectsize::eta_squared(model)
# Parameter   | Eta2 (partial) |       90% CI
# -------------------------------------------
# valence     |           0.17 | [0.05, 0.32]
# probability |           0.12 | [0.02, 0.27]
```

And beta coefficients (standarized coefficients):
``` r
dataStand <- lapply(data, scale) # standardizes all variables
modelStand <- lm(reference ~ valence + probability, data = dataStand)
beta_coef <- coef(modelStand)
beta_coef
#   (Intercept)       valence   probability 
# -1.009122e-16  3.958397e-01  3.244347e-01
```
The linear relationship between reference (DV) and `valence` and `probability` (IVs) is positive and significant. Reference judgments were significantly correlated both with `valence` (_b_ = 0.394, _t_(56) = 3.436, _p_ < 0.01, eta^2 = 0.17,  beta = 3.958) and `probability` (_b_ = 0.324, _t_(56) = 2.817, _p_ < 0.01, eta^2 = 0.12, beta = 3.244). Together, the two variables explain R^2 = 0.25% of the variability in reference judgments, which is a large effect (Cohen, 1988).

## Bayesian Linear Regression

## Mediation Analysis

### Frequentist Variant

### Bayesian Variant

## References
- Cohen J. (1988). Statistical Power Analysis for the Behavioral Sciences, 2nd Ed. Hillsdale, NJ: Laurence Erlbaum Associates.
- 
