Introduction to Item Response Theory
================
Eric R. Schuler, Ph.D.
2022-09-16

**Title:** Introduction to Item Response Theory in R

**Speaker:** Eric R. Schuler, Ph.D.

**Description:** This workshop will cover the basics of item response
theory (IRT) in R. Specifically, we will cover:

1.  What exactly is IRT
2.  Assumptions of IRT
3.  What is a Rasch model
4.  Running and interpreting a Rasch model
5.  What is a 2-pl and 3-pl model
6.  Running and interpreting a 2-pl and 3-pl model
7.  Storing the individual’s ability score (latent score)

This workshop assumes familiarity with R (if not, please see our
On-Demand Workshop on Using R). Additional resources will be provided.

We will now check to see if the packages we need are already installed
(if any are missing they will be added)

# What is item response theory (IRT)?

Item response theory (IRT), sometimes called modern test theory or
latent trait theory, is used for psychological measurement to understand
a person’s ability on a trait as we cannot directly measure the trait we
use items to reflect that trait. In IRT, a person’s level on a trait is
estimated from their responses on test item. Within the IRT model the
trait level and properties of the item are considered in the
individual’s response to an item (Embretson & Reise, 2013, p. 40). There
are additional parameters which we will discuss later like the slope of
the item characteristic curve. If the slope is steeper it will do a
better job at differentiating a person’s trait score or $\theta$.

IRT does not make the same assumptions as classical test theory and is
more flexible. Embretson and Reise (2013, p. 15) compare the measurement
rules of classical test theory and IRT:

Assumptions for classical test theory (Embretson & Reise, 2013, p. 43):

- The expected value for error over persons is zero
- Error is not related to other variables (e.g., true score, other error
  scores, other true scores)
- Errors are normally distributed within persons and homogenous across
  persons

Assumptions for IRT (Embretson & Reise, 2013, p. 45 and 48):

- Item characteristic curves (ICCs) have a specified form (we will go
  over that in a bit)
- Local independence has been obtained (no further relationships remain
  between the items when the model parameters are controlled) – This
  assumption is difficult to assess and typically omitted in practice
  (Mair, 2019 p. 99)

IRT has become widely used in educational psychology, testing and
measurement (i.e., aptitude tests) both nationally and internationally.

Old Rules (Classical Test theory):

- Rule 1: The standard error of measurement applies to all scores in a
  particular population.
- Rule 2: Longer tests are more reliable than shorter tests.
- Rule 3: Comparing test scores across multiple forms is optimal when
  the forms are parallel.
- Rule 4: Unbaised estimates of item properties depend on having
  representative samples.
- Rule 5: Test scores obtain meaning by comparing their position in a
  norm group.
- Rule 6: Interval scale properties are achieved by obtaining normal
  score distributions.
- Rule 7: Mixed item formats leads to unbalanced impact on test total
  scores.
- Rule 8: Change scores cannot be meaningfully compared when initial
  score levels differ.
- Rule 9: Factor analysis on binary items produce artifacts rather than
  factors.
- Rule 10: Item stimulus features are unimportant compared to
  psychometric properties.

New Rules (under IRT):

- Rule 1: The standard error of measurement differs across scores (or
  response patterns), but generalizes across populations.
- Rule 2: Shorter tests can be more reliable than longer tests.
- Rule 3: Comparing test scores across multiple forms is optimal when
  test difficulty levels vary between persons.
- Rule 4: Unbiased estimates of item properties maybe obtained from
  unrepresentative samples.
- Rule 5: Test scores have meaning when they are compared for distance
  from items.
- Rule 6: Interval scale properties are achieved by applying justifiable
  measurement models.
- Rule 7: Mixed item formats can yield optimal test scores.
- Rule 8: Change scores can be meaningfully compared when initial score
  levels differ.
- Rule 9: Factor analysis on raw item data yields a full information
  factor analysis.
- Rule 10: Item stimulus features can be directly related to
  psychometric properties.

I would highly recommend Chapter 2 of Embretson and Reise’s (2013) “Item
response theory for psychologists” for a deeper discussion on the rules
and additional benefits of using IRT.

# Rasch Models (1-PL)

One of the most basic IRT models is the Rasch model, sometimes called a
one-parameter logistic model (1-PL). The Rasch model was developed in
1960 and has a dependent variable that is dichotomous (yes/no or
correct/incorrect). In the Rasch model, the independent variables are
the individual’s latent trait score ($\theta$), the level of difficulty
for the item ($\beta$), which are combined additively and the item’s
difficulty is subtracted from the person’s ability (Embretson & Reise,
2013 p. 48-49). In a Rasch model, the discrimination parameter, $\alpha$
is constant.

Formula for a Rasch model is:

$$P_{ij} (\theta_j, b_i) = exp(\theta_j - b_i) / 1 + exp(\theta_j - b_i) $$
In this equation $\theta$ is the individual’s underlying ability, or
their latent score. The probability of getting the item correct is
depicted as a probability (s-curve of sigmoid/ogive). Ability scores
typically range from -3 to +3 in application. The higher the person’s
ability, the higher probability of getting the item correct. The $b_i$
is the difficulty parameter, which is set on the same metric of ability
can be higher or lower than 0 on the x-axis. Difficulty determines how
the item behaves on the ability trait. Items tha are difficult are
shifted to the right (hard items) while easier items are shifted to the
left). The point of the difficulty is determined at where the median
probability to get the item correct is.

As a visual, we have the Item characteristic curve. The y-axis is the
probability of yes/correct endorsement while the x-axis is the latent
trait score.

``` r
fit <- RM(Mobility)
plotICC(fit,2)
abline(v = 0, col = "grey")
abline(h = .5, col = "grey")
```

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-1-1.png)

We can see that the median of the s-curve is a little to the left of the
0 so it is a bit easier. If a person had a latent trait score of 0, they
would have a probability to endorse around 70%.

``` r
summary(fit)
```


    Results of RM estimation: 

    Call:  RM(X = Mobility) 

    Conditional log-likelihood: -7340.702 
    Number of iterations: 25 
    Number of parameters: 7 

    Item (Category) Difficulty Parameters (eta): with 0.95 CI:
           Estimate Std. Error lower CI upper CI
    Item 2   -0.678      0.037   -0.750   -0.606
    Item 3   -3.910      0.046   -4.000   -3.820
    Item 4   -1.094      0.037   -1.166   -1.022
    Item 5    2.732      0.062    2.611    2.853
    Item 6    1.666      0.048    1.572    1.761
    Item 7    3.375      0.074    3.231    3.519
    Item 8    2.221      0.054    2.114    2.328

    Item Easiness Parameters (beta) with 0.95 CI:
                Estimate Std. Error lower CI upper CI
    beta Item 1    4.312      0.048    4.218    4.406
    beta Item 2    0.678      0.037    0.606    0.750
    beta Item 3    3.910      0.046    3.820    4.000
    beta Item 4    1.094      0.037    1.022    1.166
    beta Item 5   -2.732      0.062   -2.853   -2.611
    beta Item 6   -1.666      0.048   -1.761   -1.572
    beta Item 7   -3.375      0.074   -3.519   -3.231
    beta Item 8   -2.221      0.054   -2.328   -2.114

Individuals who have higher degrees of knowledge (or higher scores on a
trait) will have a higher $\theta$. They will have a higher probability
of endorsing items correctly (yes/correct) even when the item difficulty
is higher. It is good to have items that are spread across difficulty to
better capture an individual’s $\theta$.

Let’s get started!

Load in the data, we will be using the data examples from Paek & Cole
(2019) “Using R for Item Response Theory Model Applications”. As we will
only be focusing on Rasch, 2-PL and 3-PL models, all of the data
examples will be dichotomous in nature. If you are using Likert scaled
responses, IRT can be done but you would want to consider using what is
called a graded response model or GRM as it would be considered
polytomous or more than two response options.

``` r
rasch <- read.csv("rasch_example.csv")
```

This data are 500 responses for test-takers who either had a 0 for the
question (incorrect or no) or a 1 (correct or yes). There were 20
dichtomous questions in the dataset.

View the data

``` r
head(rasch)
```

      MC1 MC2 MC3 MC4 MC5 MC6 MC7 MC8 MC9 MC10 MC11 MC12 MC13 MC14 MC15 MC16 MC17
    1   0   0   0   1   0   1   0   1   0    0    1    1    1    1    0    0    1
    2   0   1   0   0   1   0   0   0   1    1    1    1    1    0    1    1    0
    3   1   1   0   0   0   1   1   0   0    0    1    1    1    1    1    1    0
    4   0   0   1   0   1   0   1   1   0    0    1    1    1    1    0    0    0
    5   0   0   1   0   0   0   0   1   0    1    1    0    0    0    0    1    0
    6   1   1   1   1   1   1   0   1   0    1    0    0    1    1    0    1    1
      MC18 MC19 MC20
    1    0    1    0
    2    0    1    0
    3    0    1    0
    4    0    0    0
    5    0    1    0
    6    0    1    1

I prefer using the glimpse function

``` r
glimpse(rasch)
```

    Rows: 500
    Columns: 20
    $ MC1  <int> 0, 0, 1, 0, 0, 1, 0, 0, 0, 1, 1, 1, 1, 1, 1, 0, 1, 0, 0, 0, 0, 0,…
    $ MC2  <int> 0, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 0,…
    $ MC3  <int> 0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 1, 1, 1, 1, 1, 0, 1, 0, 0, 1, 1, 0,…
    $ MC4  <int> 1, 0, 0, 0, 0, 1, 0, 0, 1, 0, 1, 1, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0,…
    $ MC5  <int> 0, 1, 0, 1, 0, 1, 1, 1, 1, 0, 1, 1, 0, 1, 0, 0, 1, 1, 0, 1, 0, 0,…
    $ MC6  <int> 1, 0, 1, 0, 0, 1, 0, 1, 1, 0, 1, 1, 1, 1, 0, 1, 1, 1, 0, 0, 1, 0,…
    $ MC7  <int> 0, 0, 1, 1, 0, 0, 0, 0, 1, 0, 0, 1, 0, 1, 1, 0, 0, 0, 0, 1, 0, 1,…
    $ MC8  <int> 1, 0, 0, 1, 1, 1, 0, 0, 0, 1, 0, 1, 1, 1, 0, 0, 1, 1, 0, 1, 1, 0,…
    $ MC9  <int> 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 1, 1, 0,…
    $ MC10 <int> 0, 1, 0, 0, 1, 1, 0, 0, 0, 0, 0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 1, 0,…
    $ MC11 <int> 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,…
    $ MC12 <int> 1, 1, 1, 1, 0, 0, 0, 0, 1, 1, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 1,…
    $ MC13 <int> 1, 1, 1, 1, 0, 1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 0, 1, 0, 1, 1, 1, 1,…
    $ MC14 <int> 1, 0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0, 1, 1, 0, 1, 1, 0,…
    $ MC15 <int> 0, 1, 1, 0, 0, 0, 0, 0, 1, 0, 0, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0,…
    $ MC16 <int> 0, 1, 1, 0, 1, 1, 0, 0, 0, 0, 0, 1, 1, 1, 0, 1, 0, 0, 0, 1, 1, 1,…
    $ MC17 <int> 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1,…
    $ MC18 <int> 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 1, 1, 0, 1, 0, 0, 0, 1, 0,…
    $ MC19 <int> 1, 1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0,…
    $ MC20 <int> 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 1, 0, 0, 0, 0, 1, 0, 0,…

# Assumptions of IRT

One of the assumptions of IRT models are that the models are
uni-dimensional in nature. There are multidimensional IRT models that
allow for multiple dimensions to be assessed within an IRT framework but
that is outside of the scope of this workshop. To assess the assumption
of uni-dimensionality

**Approach 1:**

Categorical principal component analysis, or princals, is a method for
assessing dimesionality. Specifically, we can use dimension reduction
techniques to assess the number of underlying dimensions (Mair, 2019,
p. 96).

Fitting a two-dimensional Princals solution and then we plot the
corresponding loadings. We would have unidimensionality if all the
arrows are approximately pointing in the same direction.

``` r
prin_assess <- princals(rasch)
plot(prin_assess)
```

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-6-1.png)

**Approach 2:**

We will run an exploratory factor analysis (EFA) on the tetrachoric
correlation matrix and then assess for very simple structure, minimum
average partial, and the Bayesian information criteria.

Extract the tetrachoric correlations

``` r
tc_rasch <- polychoric(rasch)$rho
```

Obtain the eigen values

``` r
evals <- eigen(tc_rasch)$values
```

Look at the scree plot

``` r
scree(tc_rasch, factors =FALSE)
```

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-9-1.png)

We will now check for very simple structure (VSS)

First we will look at a visual

``` r
rasch_vss <- vss(tc_rasch, fm = "ml",
               n.obs =nrow(rasch), 
               plot =TRUE)
```

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-10-1.png)

Now looking at the VSS, MAP, and BIC

``` r
rasch_vss
```


    Very Simple Structure
    Call: vss(x = tc_rasch, fm = "ml", n.obs = nrow(rasch), plot = TRUE)
    VSS complexity 1 achieves a maximimum of 0.72  with  1  factors
    VSS complexity 2 achieves a maximimum of 0.74  with  2  factors

    The Velicer MAP achieves a minimum of 0.01  with  1  factors 
    BIC achieves a minimum of  -245.97  with  2  factors
    Sample Size adjusted BIC achieves a minimum of  13.62  with  8  factors

    Statistics by number of factors 
      vss1 vss2   map dof chisq    prob sqresid  fit RMSEA  BIC SABIC complex
    1 0.72 0.00 0.011 170   814 1.1e-84    12.9 0.72 0.087 -243   297     1.0
    2 0.58 0.74 0.015 151   692 3.0e-70    11.7 0.74 0.085 -246   233     1.5
    3 0.54 0.71 0.018 133   595 1.4e-59    10.8 0.77 0.083 -232   190     1.7
    4 0.38 0.68 0.022 116   495 3.2e-48     9.6 0.79 0.081 -226   142     2.1
    5 0.38 0.66 0.027 100   391 5.3e-36     8.6 0.81 0.076 -231    87     2.2
    6 0.33 0.59 0.031  85   308 5.1e-27     7.8 0.83 0.072 -220    50     2.4
    7 0.31 0.55 0.037  71   241 2.2e-20     7.2 0.84 0.069 -200    25     2.7
    8 0.33 0.60 0.043  58   190 6.1e-16     6.4 0.86 0.067 -170    14     2.5
      eChisq  SRMR eCRMS eBIC
    1    805 0.065 0.069 -251
    2    681 0.060 0.067 -257
    3    580 0.055 0.066 -247
    4    437 0.048 0.061 -284
    5    338 0.042 0.058 -283
    6    251 0.036 0.054 -277
    7    198 0.032 0.053 -243
    8    155 0.029 0.052 -206

**Approach 3:**

Here we will use an item factor analysis, or IFA, to fit a one-factor
and two-factor model and compare the nested model using using a
likeilihood ration test and AIC/BIC.

Fit a one-factor model

``` r
rasch_fit1 <- mirt(rasch,1,verbose = FALSE)
```

Fit a two-factor model

``` r
rasch_fit2 <- mirt(rasch,2,verbose = FALSE)
```

Now compare using LR, AIC, and BIC

``` r
anova(rasch_fit1,rasch_fit2, verbose=FALSE)
```

           AIC    SABIC       HQ      BIC    logLik     X2  df    p
    1 11931.51 11973.14 11997.67 12100.10 -5925.758    NaN NaN  NaN
    2 11940.12 12001.51 12037.69 12188.78 -5911.059 29.398  19 0.06

Based on these, we can feel fairly confident that we have
unidimensionality.

# Rasch Models

A Rasch model is the most basic IRT model that can be run. It allows us
to assess the item difficulty for each item (minus the first item due to
identification within the estimation process) and after the model is run
we can also extract the estimates latent ability score ($\theta$) for
each individual.

We will start with running a Rasch model using the RM() from the eRm
package. The eRM package uses conditional maximum likelihood for its
estimator. Running the model is sometimes called ‘calibrating’

``` r
rm <- RM(rasch)
```

let’s take a look at the output, which provides the difficulty of the
items with 95% CI.

``` r
summary(rm)
```


    Results of RM estimation: 

    Call:  RM(X = rasch) 

    Conditional log-likelihood: -4530.381 
    Number of iterations: 16 
    Number of parameters: 19 

    Item (Category) Difficulty Parameters (eta): with 0.95 CI:
         Estimate Std. Error lower CI upper CI
    MC2    -1.138      0.102   -1.338   -0.937
    MC3    -0.344      0.096   -0.531   -0.156
    MC4     0.175      0.097   -0.014    0.365
    MC5    -0.009      0.096   -0.197    0.179
    MC6    -0.324      0.096   -0.512   -0.137
    MC7     0.828      0.103    0.625    1.030
    MC8     0.244      0.097    0.054    0.434
    MC9     0.850      0.104    0.647    1.054
    MC10    0.717      0.102    0.518    0.917
    MC11   -1.485      0.109   -1.698   -1.272
    MC12   -1.294      0.105   -1.500   -1.089
    MC13   -1.665      0.113   -1.886   -1.443
    MC14    1.025      0.107    0.816    1.234
    MC15    0.794      0.103    0.593    0.996
    MC16   -0.353      0.096   -0.541   -0.165
    MC17    0.839      0.104    0.636    1.042
    MC18    0.568      0.100    0.372    0.764
    MC19   -0.420      0.096   -0.608   -0.232
    MC20    0.516      0.099    0.321    0.711

    Item Easiness Parameters (beta) with 0.95 CI:
              Estimate Std. Error lower CI upper CI
    beta MC1    -0.475      0.099   -0.669   -0.281
    beta MC2     1.138      0.102    0.937    1.338
    beta MC3     0.344      0.096    0.156    0.531
    beta MC4    -0.175      0.097   -0.365    0.014
    beta MC5     0.009      0.096   -0.179    0.197
    beta MC6     0.324      0.096    0.137    0.512
    beta MC7    -0.828      0.103   -1.030   -0.625
    beta MC8    -0.244      0.097   -0.434   -0.054
    beta MC9    -0.850      0.104   -1.054   -0.647
    beta MC10   -0.717      0.102   -0.917   -0.518
    beta MC11    1.485      0.109    1.272    1.698
    beta MC12    1.294      0.105    1.089    1.500
    beta MC13    1.665      0.113    1.443    1.886
    beta MC14   -1.025      0.107   -1.234   -0.816
    beta MC15   -0.794      0.103   -0.996   -0.593
    beta MC16    0.353      0.096    0.165    0.541
    beta MC17   -0.839      0.104   -1.042   -0.636
    beta MC18   -0.568      0.100   -0.764   -0.372
    beta MC19    0.420      0.096    0.232    0.608
    beta MC20   -0.516      0.099   -0.711   -0.321

Item 1 is omitted so that the model can be identifiable and the other
parameters estimated.

Assessing global model fit using the Andersen’s LR test

``` r
lrtest <- LRtest(rm)
lrtest
```


    Andersen LR-test: 
    LR-value: 18.829 
    Chi-square df: 19 
    p-value:  0.468 

Based on this we would not reject the Rasch model.

Martin-Lof LR test which is another test of goodness of fit. This test
splits the data at the median to compare subgroups to see if the item
set is homogeneous (uni-dimensional).

``` r
MLoef(rm, splitcr="median")
```


    Martin-Loef-Test (split criterion: median)
    LR-value: 73.739 
    Chi-square df: 99 
    p-value: 0.973 

Also non-significant

We can also split the data and asses on two subgroups to see if there
are parameter invariance (grouping based on the median raw score). We
are using a Bonferonni correction.

``` r
Waldtest(rm)
```


    Wald test on item level (z-values):

              z-statistic p-value
    beta MC1       -0.461   0.645
    beta MC2       -1.132   0.258
    beta MC3        2.029   0.042
    beta MC4        0.267   0.789
    beta MC5       -0.607   0.544
    beta MC6       -0.558   0.577
    beta MC7       -0.264   0.792
    beta MC8        1.746   0.081
    beta MC9        1.259   0.208
    beta MC10      -0.422   0.673
    beta MC11       1.233   0.218
    beta MC12      -0.409   0.683
    beta MC13       0.114   0.909
    beta MC14      -0.279   0.780
    beta MC15      -0.958   0.338
    beta MC16       0.308   0.758
    beta MC17       0.497   0.619
    beta MC18      -2.017   0.044
    beta MC19       0.911   0.362
    beta MC20      -0.743   0.457

We can also visualize this split and goodness of fit (it is a bit messy
though with a lot of items)

``` r
plotGOF(lrtest, conf=list())
```

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-20-1.png)

We can also assess infit and outfit mean square and the corresponding
t-statistics to see how well the items performed. We are looking for
values of outfit and infit closer to 1.

``` r
person_p <- person.parameter(rm)
itemfit(person_p)
```


    Itemfit Statistics: 
           Chisq  df p-value Outfit MSQ Infit MSQ Outfit t Infit t Discrim
    MC1  496.025 496   0.491      0.998     0.964   -0.006  -0.818   0.415
    MC2  467.645 496   0.815      0.941     0.960   -0.632  -0.862   0.369
    MC3  519.186 496   0.228      1.045     1.051    0.730   1.323   0.326
    MC4  517.946 496   0.240      1.042     1.027    0.694   0.671   0.362
    MC5  493.319 496   0.526      0.993     0.965   -0.104  -0.896   0.427
    MC6  456.953 496   0.895      0.919     0.952   -1.323  -1.268   0.435
    MC7  496.138 496   0.490      0.998     1.000    0.007   0.012   0.378
    MC8  575.209 496   0.008      1.157     1.072    2.402   1.734   0.312
    MC9  526.885 496   0.163      1.060     1.085    0.730   1.640   0.296
    MC10 470.243 496   0.791      0.946     0.982   -0.677  -0.360   0.405
    MC11 475.051 496   0.743      0.956     0.985   -0.365  -0.253   0.306
    MC12 483.687 496   0.645      0.973     0.996   -0.238  -0.055   0.324
    MC13 444.427 496   0.953      0.894     0.972   -0.844  -0.444   0.306
    MC14 495.232 496   0.501      0.996     1.004   -0.007   0.088   0.359
    MC15 445.023 496   0.951      0.895     0.958   -1.306  -0.832   0.422
    MC16 488.310 496   0.589      0.983     1.000   -0.262   0.007   0.381
    MC17 449.613 496   0.933      0.905     0.935   -1.153  -1.285   0.432
    MC18 440.998 496   0.964      0.887     0.920   -1.602  -1.794   0.475
    MC19 468.213 496   0.810      0.942     0.986   -0.910  -0.366   0.392
    MC20 439.163 496   0.968      0.884     0.929   -1.702  -1.615   0.465

Extracting parameters and show them from easiest to most difficult

``` r
betas <- -coef(rm)
round(sort(betas),2)
```

    beta MC13 beta MC11 beta MC12  beta MC2 beta MC19 beta MC16  beta MC3  beta MC6 
        -1.66     -1.49     -1.29     -1.14     -0.42     -0.35     -0.34     -0.32 
     beta MC5  beta MC4  beta MC8  beta MC1 beta MC20 beta MC18 beta MC10 beta MC15 
        -0.01      0.18      0.24      0.47      0.52      0.57      0.72      0.79 
     beta MC7 beta MC17  beta MC9 beta MC14 
         0.83      0.84      0.85      1.02 

We can now look at the Item Characteristic Curve for each item.

The x-axis is the latent trait (standardized) and the y-axis is the
probability of getting the item correct. Each of the items has the same
slope (or discrimination parameter) as it is held constant in a Rasch
model

The plotINFO function produces item and test information.

``` r
plotINFO(rm)
```

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-23-1.png)

These ICCs help determine the behavior of items (i.e., endorsement
probability) along the latent trait (Mair, 2019, p. 104)

``` r
plotICC(rm)
```

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-24-1.png)

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-24-2.png)

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-24-3.png)

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-24-4.png)

``` r
abline(v = -0.18, col = "grey")
abline(h = .5, col = "grey")
```

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-24-5.png)

Let’s look at item MC1

``` r
plotICC(rm, item.subset = "MC1")
abline(v = -0.18, col = "grey")
abline(h = .5, col = "grey")
```

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-25-1.png)

We can also have all the items visible, which will allow us to see if
all the ICCs are parallel (needed for Rasch) and all have a slope of 1.
We can also see what is the easiest and most difficult items.

``` r
plotjointICC(rm, cex=.6, 
             xlab = "Knowledge",
             main = "ICCs Test Items")
```

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-26-1.png)

The Person-item map, or Wright map, highlights the histogram of person
ability scores and the dots are the location of the item difficulties.
The item difficulty and person ability scores are on the same scale so
they can be interpreted the same. We can use this to look at how items
and person spread along the ability scale.

``` r
plotPImap(rm, cex.gen = .55)
```

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-27-1.png)

We can also sort it by difficulty

``` r
plotPImap(rm, cex.gen = .55, sorted = TRUE)
```

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-28-1.png)

Now that we have looked at the difficulties of the items and assessed
fit. We can finally extract estimated latent scores, which can be used
in other analyses

``` r
rasch_person <- person.parameter(rm)
rasch_person
```


    Person Parameters:

     Raw Score    Estimate Std.Error
             0 -4.13917665        NA
             1 -3.27618884 1.0485744
             2 -2.48246503 0.7755980
             3 -1.97443879 0.6609142
             4 -1.58229250 0.5962479
             5 -1.25249043 0.5548278
             6 -0.96096550 0.5266119
             7 -0.69445265 0.5069887
             8 -0.44458232 0.4936153
             9 -0.20540769 0.4852587
            10  0.02782571 0.4813295
            11  0.25932828 0.4816830
            12  0.49331943 0.4865721
            13  0.73455417 0.4967088
            14  0.98906283 0.5134706
            15  1.26526690 0.5393832
            16  1.57659159 0.5792832
            17  1.94734756 0.6435163
            18  2.43132323 0.7592744
            19  3.19894491 1.0357701
            20  4.03542850        NA

We can also visualize the theta to raw score

``` r
plot(rasch_person)
```

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-30-1.png)

Now we can add the theta (latent score for each person on the knowledge
trait) to the dataset.

``` r
rasch$theta <- rasch_person$theta.table[,1]
```

Now we will run a t-test to look at differences in knowledge by sex

We will pretend we have a factor in the dataset, first creating it

``` r
rasch$sex <- rep(c("Male","Female"),250)
rasch$sex <- factor(rasch$sex)
```

Look at the descriptive statistics

``` r
describeBy(rasch$theta, group = rasch$sex)
```


     Descriptive statistics by group 
    group: Female
       vars   n  mean  sd median trimmed  mad   min  max range skew kurtosis   se
    X1    1 250 -0.11 1.2  -0.21   -0.15 1.08 -4.14 4.04  8.17 0.22     0.86 0.08
    ------------------------------------------------------------ 
    group: Male
       vars   n  mean   sd median trimmed  mad   min  max range skew kurtosis   se
    X1    1 250 -0.14 1.12  -0.21   -0.18 1.12 -2.48 4.04  6.52 0.46     0.45 0.07

Compare differences in latent knowledge by sex

``` r
knowledge_sex <-t.test(theta~sex,
                       data=rasch)
knowledge_sex
```


        Welch Two Sample t-test

    data:  theta by sex
    t = 0.28807, df = 496.12, p-value = 0.7734
    alternative hypothesis: true difference in means between group Female and group Male is not equal to 0
    95 percent confidence interval:
     -0.1741471  0.2339876
    sample estimates:
    mean in group Female   mean in group Male 
              -0.1106016           -0.1405218 

# 2-PL Model

A 2-PL extends the Rasch model by not assuming that each item has the
same discrimination ($\alpha$). This allows us to have an additional
parameter in the model.

Formula for a 2-PL model:

$$P_{ij} (\theta_j, b_i, a_i) = exp[a_i(\theta_j - b_i)] / 1 + exp[a_i(\theta_j - b_i)] $$

$\theta$ = ability

\$b_i \$ = difficulty parameter

\$a_i \$ = discrimination parameter (unfixed)

First let’s load in the dataset example

``` r
twopl <- read.csv("2pl_example.csv")
glimpse(twopl)
```

    Rows: 1,000
    Columns: 20
    $ IT1  <int> 0, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 1, 0, 1, 1, 0, 0, 1, 1, 1, 0,…
    $ IT2  <int> 1, 1, 0, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 1, 1, 0, 1, 1, 1, 0, 0,…
    $ IT3  <int> 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 1, 0,…
    $ IT4  <int> 0, 0, 0, 1, 1, 0, 1, 1, 0, 0, 0, 1, 0, 1, 1, 0, 0, 0, 0, 0, 1, 0,…
    $ IT5  <int> 0, 1, 0, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    $ IT6  <int> 0, 1, 0, 1, 0, 1, 0, 1, 0, 0, 1, 0, 0, 0, 0, 1, 0, 1, 1, 0, 0, 1,…
    $ IT7  <int> 0, 0, 0, 0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1, 0,…
    $ IT8  <int> 1, 0, 1, 0, 0, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 0,…
    $ IT9  <int> 1, 1, 0, 0, 1, 1, 0, 1, 0, 0, 0, 1, 0, 0, 1, 1, 1, 0, 1, 1, 0, 0,…
    $ IT10 <int> 0, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 0, 0, 0,…
    $ IT11 <int> 1, 1, 0, 0, 1, 1, 0, 0, 0, 1, 1, 0, 1, 0, 1, 1, 0, 1, 1, 1, 1, 0,…
    $ IT12 <int> 0, 1, 0, 0, 1, 1, 1, 1, 0, 0, 1, 0, 1, 0, 1, 0, 0, 1, 1, 1, 1, 0,…
    $ IT13 <int> 0, 1, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 1, 1, 0, 1, 0, 0, 0,…
    $ IT14 <int> 1, 1, 0, 1, 0, 1, 0, 1, 1, 0, 0, 0, 0, 0, 0, 1, 0, 1, 1, 0, 0, 0,…
    $ IT15 <int> 1, 0, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 0,…
    $ IT16 <int> 1, 1, 0, 0, 1, 1, 1, 0, 1, 0, 0, 0, 0, 1, 1, 0, 0, 0, 0, 1, 0, 0,…
    $ IT17 <int> 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 0,…
    $ IT18 <int> 1, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0, 1, 1, 0, 1, 0, 0, 1, 1, 0, 1, 0,…
    $ IT19 <int> 1, 1, 1, 1, 1, 1, 0, 0, 0, 1, 0, 0, 0, 0, 1, 1, 0, 1, 1, 1, 1, 0,…
    $ IT20 <int> 1, 1, 0, 0, 0, 1, 1, 1, 0, 1, 0, 1, 1, 0, 0, 1, 0, 0, 1, 0, 1, 0,…

This data are 1000 responses for test-takers who either had a 0 for the
question (incorrect or no) or a 1 (correct or yes). There were 20
dichtomous questions in the dataset.

Normally we would want to screen the data and assess the
unidimensionality assumption. For time we are going to skip this as we
have already covered it.

To run a 2pl model, we would have the data and then use \~z1 to denote
unidimensionality. The IRT.param= TRUE portion is to produce the item
difficulty.

``` r
two_pl <- ltm(twopl ~z1, IRT.param = TRUE)
```

Let’s check convergence, if we get a 0 that means it converged normally.

``` r
two_pl$conv
```

    [1] 0

If we look at the summary we can see the item difficulty and item
discrimination coefficients

``` r
summary(two_pl)
```


    Call:
    ltm(formula = twopl ~ z1, IRT.param = TRUE)

    Model Summary:
      log.Lik      AIC     BIC
     -10853.7 21787.39 21983.7

    Coefficients:
                  value std.err   z.vals
    Dffclt.IT1  -0.4560  0.0641  -7.1119
    Dffclt.IT2  -0.7904  0.0604 -13.0862
    Dffclt.IT3  -0.2897  0.0883  -3.2805
    Dffclt.IT4   0.0331  0.0963   0.3436
    Dffclt.IT5   0.6624  0.0803   8.2438
    Dffclt.IT6  -0.2176  0.0685  -3.1766
    Dffclt.IT7   1.5875  0.1620   9.8022
    Dffclt.IT8  -1.4029  0.1684  -8.3325
    Dffclt.IT9  -0.2936  0.0671  -4.3764
    Dffclt.IT10 -1.4936  0.1475 -10.1240
    Dffclt.IT11 -0.3253  0.0711  -4.5742
    Dffclt.IT12 -0.5684  0.0527 -10.7793
    Dffclt.IT13  1.0854  0.1477   7.3491
    Dffclt.IT14 -0.4220  0.0600  -7.0353
    Dffclt.IT15  0.1383  0.0501   2.7606
    Dffclt.IT16  0.6708  0.0964   6.9581
    Dffclt.IT17 -1.3818  0.0783 -17.6489
    Dffclt.IT18 -0.7385  0.1321  -5.5887
    Dffclt.IT19 -1.2712  0.0905 -14.0447
    Dffclt.IT20 -1.0685  0.1018 -10.4922
    Dscrmn.IT1   1.5075  0.1274  11.8320
    Dscrmn.IT2   2.2296  0.1935  11.5196
    Dscrmn.IT3   0.8651  0.0902   9.5940
    Dscrmn.IT4   0.7377  0.0849   8.6901
    Dscrmn.IT5   1.2135  0.1123  10.8034
    Dscrmn.IT6   1.2163  0.1082  11.2461
    Dscrmn.IT7   0.9723  0.1113   8.7367
    Dscrmn.IT8   0.7766  0.0933   8.3246
    Dscrmn.IT9   1.2887  0.1123  11.4796
    Dscrmn.IT10  0.9876  0.1074   9.1913
    Dscrmn.IT11  1.1915  0.1067  11.1631
    Dscrmn.IT12  2.5046  0.2149  11.6559
    Dscrmn.IT13  0.7368  0.0893   8.2486
    Dscrmn.IT14  1.6713  0.1381  12.1016
    Dscrmn.IT15  2.2445  0.1896  11.8387
    Dscrmn.IT16  0.9449  0.0968   9.7645
    Dscrmn.IT17  2.5925  0.2723   9.5216
    Dscrmn.IT18  0.6624  0.0832   7.9651
    Dscrmn.IT19  1.6730  0.1569  10.6615
    Dscrmn.IT20  1.1574  0.1116  10.3683

    Integration:
    method: Gauss-Hermite
    quadrature points: 21 

    Optimization:
    Convergence: 0 
    max(|grad|): 0.021 
    quasi-Newton: BFGS 

Inspect the coefficients (rounded to three decimal places)

- “Dffclt” is the item difficulty
- “Dscrmn” is the item discrimination

``` r
round(coef(two_pl),3)
```

         Dffclt Dscrmn
    IT1  -0.456  1.508
    IT2  -0.790  2.230
    IT3  -0.290  0.865
    IT4   0.033  0.738
    IT5   0.662  1.214
    IT6  -0.218  1.216
    IT7   1.587  0.972
    IT8  -1.403  0.777
    IT9  -0.294  1.289
    IT10 -1.494  0.988
    IT11 -0.325  1.191
    IT12 -0.568  2.505
    IT13  1.085  0.737
    IT14 -0.422  1.671
    IT15  0.138  2.244
    IT16  0.671  0.945
    IT17 -1.382  2.592
    IT18 -0.739  0.662
    IT19 -1.271  1.673
    IT20 -1.068  1.157

plot the first 5 items for ICC

``` r
plot(two_pl, item = 1:5, legend =TRUE)
```

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-40-1.png)

Items 6 to 10

``` r
plot(two_pl, item = 6:10, legend =TRUE)
```

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-41-1.png)

Items 11 to 15

``` r
plot(two_pl, item = 11:15, legend =TRUE)
```

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-42-1.png)

If we look back at the coefficients

``` r
coef(two_pl)
```

              Dffclt    Dscrmn
    IT1  -0.45597511 1.5075340
    IT2  -0.79038680 2.2296120
    IT3  -0.28967553 0.8651201
    IT4   0.03307719 0.7377238
    IT5   0.66238583 1.2135482
    IT6  -0.21762419 1.2163393
    IT7   1.58748321 0.9723258
    IT8  -1.40288805 0.7766165
    IT9  -0.29360847 1.2887284
    IT10 -1.49356848 0.9875957
    IT11 -0.32526846 1.1914515
    IT12 -0.56840731 2.5046326
    IT13  1.08538948 0.7368284
    IT14 -0.42201296 1.6713488
    IT15  0.13832301 2.2444783
    IT16  0.67078011 0.9449118
    IT17 -1.38181547 2.5924943
    IT18 -0.73853536 0.6624472
    IT19 -1.27117265 1.6730142
    IT20 -1.06848021 1.1574098

Items that have good discrimination would be between 0.8 to 2.5 (de
Ayala, 2009). Items that have discrimination less than .8 would have
item characteristic curves that are too flat, while discrimination
scores higher than 2.5 would mean that the the items discriminate only
within a narrow range of the trait.

We can also assess the goodness of fit using the mirt package, so we
would reject the Rasch model.

``` r
twopl.2 <-mirt(twopl, 1,itemtype='2PL')
```


    Iteration: 1, Log-Lik: -11111.818, Max-Change: 0.83540
    Iteration: 2, Log-Lik: -10901.119, Max-Change: 0.38992
    Iteration: 3, Log-Lik: -10870.437, Max-Change: 0.18239
    Iteration: 4, Log-Lik: -10860.796, Max-Change: 0.11287
    Iteration: 5, Log-Lik: -10857.172, Max-Change: 0.06837
    Iteration: 6, Log-Lik: -10855.450, Max-Change: 0.05071
    Iteration: 7, Log-Lik: -10853.983, Max-Change: 0.02214
    Iteration: 8, Log-Lik: -10853.850, Max-Change: 0.01295
    Iteration: 9, Log-Lik: -10853.771, Max-Change: 0.00785
    Iteration: 10, Log-Lik: -10853.676, Max-Change: 0.00301
    Iteration: 11, Log-Lik: -10853.659, Max-Change: 0.00307
    Iteration: 12, Log-Lik: -10853.648, Max-Change: 0.00245
    Iteration: 13, Log-Lik: -10853.621, Max-Change: 0.00278
    Iteration: 14, Log-Lik: -10853.619, Max-Change: 0.00124
    Iteration: 15, Log-Lik: -10853.617, Max-Change: 0.00121
    Iteration: 16, Log-Lik: -10853.615, Max-Change: 0.00054
    Iteration: 17, Log-Lik: -10853.615, Max-Change: 0.00058
    Iteration: 18, Log-Lik: -10853.614, Max-Change: 0.00042
    Iteration: 19, Log-Lik: -10853.614, Max-Change: 0.00038
    Iteration: 20, Log-Lik: -10853.614, Max-Change: 0.00032
    Iteration: 21, Log-Lik: -10853.614, Max-Change: 0.00031
    Iteration: 22, Log-Lik: -10853.614, Max-Change: 0.00027
    Iteration: 23, Log-Lik: -10853.614, Max-Change: 0.00026
    Iteration: 24, Log-Lik: -10853.614, Max-Change: 0.00023
    Iteration: 25, Log-Lik: -10853.614, Max-Change: 0.00022
    Iteration: 26, Log-Lik: -10853.614, Max-Change: 0.00020
    Iteration: 27, Log-Lik: -10853.614, Max-Change: 0.00019
    Iteration: 28, Log-Lik: -10853.614, Max-Change: 0.00017
    Iteration: 29, Log-Lik: -10853.614, Max-Change: 0.00017
    Iteration: 30, Log-Lik: -10853.614, Max-Change: 0.00015
    Iteration: 31, Log-Lik: -10853.614, Max-Change: 0.00015
    Iteration: 32, Log-Lik: -10853.614, Max-Change: 0.00013
    Iteration: 33, Log-Lik: -10853.614, Max-Change: 0.00013
    Iteration: 34, Log-Lik: -10853.614, Max-Change: 0.00012
    Iteration: 35, Log-Lik: -10853.614, Max-Change: 0.00011
    Iteration: 36, Log-Lik: -10853.614, Max-Change: 0.00053
    Iteration: 37, Log-Lik: -10853.614, Max-Change: 0.00049
    Iteration: 38, Log-Lik: -10853.614, Max-Change: 0.00040
    Iteration: 39, Log-Lik: -10853.614, Max-Change: 0.00032
    Iteration: 40, Log-Lik: -10853.614, Max-Change: 0.00007

``` r
M2(twopl.2)
```

                M2  df         p      RMSEA RMSEA_5   RMSEA_95      SRMSR       TLI
    stats 189.6136 170 0.1443064 0.01074661       0 0.01838579 0.02510003 0.9975107
                CFI
    stats 0.9977727

Model-data fit and model comparison (move up in code)

We will compare the results of a Rasch model with that of the 2-PL and
assess using a likelihood ratio. (Runs but creates an error in R
markdown for knitting)

``` r
mod.1pl <- rasch(twopl)
mod.1pl
```


    Call:
    rasch(data = twopl)

    Coefficients:
     Dffclt.IT1   Dffclt.IT2   Dffclt.IT3   Dffclt.IT4   Dffclt.IT5   Dffclt.IT6  
         -0.508       -1.062       -0.221        0.033        0.675       -0.208  
     Dffclt.IT7   Dffclt.IT8   Dffclt.IT9  Dffclt.IT10  Dffclt.IT11  Dffclt.IT12  
          1.375       -1.020       -0.295       -1.303       -0.312       -0.788  
    Dffclt.IT13  Dffclt.IT14  Dffclt.IT15  Dffclt.IT16  Dffclt.IT17  Dffclt.IT18  
          0.769       -0.494        0.201        0.579       -1.990       -0.467  
    Dffclt.IT19  Dffclt.IT20       Dscrmn  
         -1.528       -1.036        1.204  

    Log.Lik: -11043.02

``` r
anova(mod.1pl,two_pl)
```


     Likelihood Ratio Table
                 AIC     BIC   log.Lik    LRT df p.value
    mod.1pl 22128.04 22231.1 -11043.02                  
    two_pl  21787.39 21983.7 -10853.70 378.65 19  <0.001

We can see based on the results that we would reject the null of the
Rasch in favor of the 2-PL.

Extracting estimated latent scores

``` r
ltm::factor.scores(two_pl,method = "EAP")
```


    Call:
    ltm(formula = twopl ~ z1, IRT.param = TRUE)

    Scoring Method: Expected A Posteriori

    Factor-Scores for observed response patterns:
        IT1 IT2 IT3 IT4 IT5 IT6 IT7 IT8 IT9 IT10 IT11 IT12 IT13 IT14 IT15 IT16 IT17
    1     0   0   0   0   0   0   0   0   0    0    0    0    0    0    0    0    0
    2     0   0   0   0   0   0   0   0   0    0    0    0    0    0    0    0    0
    3     0   0   0   0   0   0   0   0   0    0    0    0    0    0    0    0    0
    4     0   0   0   0   0   0   0   0   0    0    0    0    0    0    0    0    1
    5     0   0   0   0   0   0   0   0   0    0    0    0    0    0    0    1    0
    6     0   0   0   0   0   0   0   0   0    0    0    0    0    0    1    0    0
    7     0   0   0   0   0   0   0   0   0    0    0    0    1    0    0    0    0
    8     0   0   0   0   0   0   0   0   0    0    0    0    1    0    0    1    0
    9     0   0   0   0   0   0   0   0   0    0    0    0    1    0    0    1    0
    10    0   0   0   0   0   0   0   0   0    0    0    1    0    0    0    0    0
    11    0   0   0   0   0   0   0   0   0    0    0    1    0    0    0    0    1
    12    0   0   0   0   0   0   0   0   0    0    1    0    0    0    0    0    0
    13    0   0   0   0   0   0   0   0   0    0    1    0    0    0    0    1    0
    14    0   0   0   0   0   0   0   0   0    0    1    0    0    1    0    0    1
    15    0   0   0   0   0   0   0   0   0    0    1    1    0    0    0    0    0
    16    0   0   0   0   0   0   0   0   0    1    0    0    0    0    0    0    0
    17    0   0   0   0   0   0   0   0   0    1    0    0    0    0    0    0    1
    18    0   0   0   0   0   0   0   0   0    1    0    0    0    0    0    0    1
    19    0   0   0   0   0   0   0   0   0    1    0    0    0    1    0    0    0
    20    0   0   0   0   0   0   0   0   0    1    0    0    1    1    0    0    1
    21    0   0   0   0   0   0   0   0   0    1    1    0    0    0    0    0    0
    22    0   0   0   0   0   0   0   0   0    1    1    0    0    0    0    0    1
    23    0   0   0   0   0   0   0   0   0    1    1    0    0    0    1    0    0
    24    0   0   0   0   0   0   0   0   1    0    0    0    0    0    0    0    0
    25    0   0   0   0   0   0   0   0   1    0    0    0    0    0    0    0    0
    26    0   0   0   0   0   0   0   0   1    1    0    0    0    1    0    0    0
    27    0   0   0   0   0   0   0   0   1    1    0    0    1    0    0    0    0
    28    0   0   0   0   0   0   0   1   0    0    0    0    0    0    0    0    0
    29    0   0   0   0   0   0   0   1   0    0    0    0    0    0    0    0    0
    30    0   0   0   0   0   0   0   1   0    0    0    0    0    0    0    0    0
    31    0   0   0   0   0   0   0   1   0    0    0    0    0    0    0    0    1
    32    0   0   0   0   0   0   0   1   0    0    0    0    0    0    0    1    1
    33    0   0   0   0   0   0   0   1   0    0    0    0    0    0    0    1    1
    34    0   0   0   0   0   0   0   1   0    0    0    0    1    0    0    0    0
    35    0   0   0   0   0   0   0   1   0    0    0    1    0    1    0    0    1
    36    0   0   0   0   0   0   0   1   0    1    0    0    0    0    0    0    0
    37    0   0   0   0   0   0   0   1   0    1    0    0    0    0    0    0    1
    38    0   0   0   0   0   0   0   1   0    1    0    0    0    0    0    0    1
    39    0   0   0   0   0   0   0   1   0    1    0    0    0    0    0    0    1
    40    0   0   0   0   0   0   0   1   0    1    0    0    0    0    0    1    0
    41    0   0   0   0   0   0   0   1   0    1    1    0    0    0    0    0    1
    42    0   0   0   0   0   0   0   1   0    1    1    0    0    0    0    0    1
    43    0   0   0   0   0   0   0   1   0    1    1    0    0    0    0    0    1
    44    0   0   0   0   0   0   0   1   0    1    1    0    0    1    0    0    0
    45    0   0   0   0   0   0   0   1   1    0    0    0    0    0    0    0    1
    46    0   0   0   0   0   0   0   1   1    0    0    0    0    1    0    0    0
    47    0   0   0   0   0   0   0   1   1    0    0    0    1    0    0    0    1
    48    0   0   0   0   0   0   0   1   1    0    1    0    0    1    0    0    0
    49    0   0   0   0   0   0   0   1   1    1    0    0    0    0    0    0    1
    50    0   0   0   0   0   0   0   1   1    1    0    0    0    0    0    0    1
    51    0   0   0   0   0   0   0   1   1    1    0    0    0    1    0    0    0
    52    0   0   0   0   0   0   0   1   1    1    1    0    0    0    0    1    1
    53    0   0   0   0   0   0   1   0   1    0    0    0    0    0    0    0    0
    54    0   0   0   0   0   0   1   1   0    1    1    0    0    0    0    0    0
    55    0   0   0   0   0   0   1   1   1    1    0    0    1    0    0    0    0
    56    0   0   0   0   0   1   0   0   0    0    0    0    0    0    0    0    0
    57    0   0   0   0   0   1   0   0   0    0    0    0    1    0    0    0    1
    58    0   0   0   0   0   1   0   0   0    1    0    0    0    1    0    0    0
    59    0   0   0   0   0   1   0   0   0    1    0    0    0    1    0    0    1
    60    0   0   0   0   0   1   0   0   0    1    1    0    0    0    0    1    1
    61    0   0   0   0   0   1   0   0   1    1    1    1    0    1    1    1    1
    62    0   0   0   0   0   1   0   1   0    0    0    0    0    0    0    0    0
    63    0   0   0   0   0   1   0   1   0    0    0    0    0    0    0    0    1
    64    0   0   0   0   0   1   0   1   0    0    0    1    0    0    0    0    0
    65    0   0   0   0   0   1   0   1   0    0    1    1    0    0    1    0    1
    66    0   0   0   0   0   1   0   1   0    1    0    0    0    0    0    0    1
    67    0   0   0   0   0   1   0   1   0    1    0    1    0    1    1    1    1
    68    0   0   0   0   0   1   0   1   0    1    1    0    1    0    0    0    1
    69    0   0   0   0   0   1   0   1   1    0    0    1    0    1    0    1    1
    70    0   0   0   0   0   1   0   1   1    1    0    0    0    0    0    0    0
    71    0   0   0   0   0   1   0   1   1    1    1    1    0    1    0    0    1
    72    0   0   0   0   0   1   0   1   1    1    1    1    1    1    0    0    1
    73    0   0   0   0   1   0   0   0   0    0    0    0    0    0    0    0    0
    74    0   0   0   0   1   0   0   0   0    1    0    0    0    0    1    0    0
    75    0   0   0   0   1   0   0   0   0    1    0    0    0    0    1    0    1
    76    0   0   0   0   1   0   0   0   0    1    1    0    0    1    0    0    1
    77    0   0   0   0   1   0   0   1   0    0    0    1    0    0    0    1    1
    78    0   0   0   0   1   0   0   1   0    1    0    0    0    0    0    0    0
    79    0   0   0   0   1   0   0   1   1    0    0    0    1    0    0    0    0
    80    0   0   0   0   1   1   0   0   0    0    0    0    0    0    0    1    0
    81    0   0   0   0   1   1   0   0   1    0    1    0    0    0    0    0    0
    82    0   0   0   0   1   1   0   1   0    1    0    0    0    0    0    0    0
    83    0   0   0   1   0   0   0   0   0    0    0    0    0    0    0    0    1
    84    0   0   0   1   0   0   0   0   0    0    0    0    0    1    0    0    1
    85    0   0   0   1   0   0   0   0   0    1    0    0    0    0    0    0    0
    86    0   0   0   1   0   0   0   0   0    1    0    1    0    0    0    0    0
    87    0   0   0   1   0   0   0   0   0    1    0    1    0    0    1    1    1
    88    0   0   0   1   0   0   0   0   0    1    0    1    0    1    0    0    1
    89    0   0   0   1   0   0   0   0   0    1    1    0    0    0    0    0    1
    90    0   0   0   1   0   0   0   0   1    1    0    0    0    1    0    0    1
    91    0   0   0   1   0   0   0   0   1    1    0    0    1    0    0    0    0
    92    0   0   0   1   0   0   0   0   1    1    0    1    1    1    0    0    1
    93    0   0   0   1   0   0   0   0   1    1    1    0    0    0    0    0    1
    94    0   0   0   1   0   0   0   1   0    0    0    0    0    0    0    0    1
    95    0   0   0   1   0   0   0   1   0    0    0    0    1    1    0    1    1
    96    0   0   0   1   0   0   0   1   0    0    1    1    0    0    0    1    0
    97    0   0   0   1   0   0   0   1   0    1    0    0    0    0    0    0    0
    98    0   0   0   1   0   0   0   1   0    1    0    0    0    1    0    0    0
    99    0   0   0   1   0   0   0   1   0    1    0    0    1    0    0    0    1
    100   0   0   0   1   0   0   0   1   0    1    1    1    0    0    0    0    1
    101   0   0   0   1   0   0   0   1   1    1    0    0    0    1    0    0    1
    102   0   0   0   1   0   0   0   1   1    1    0    1    0    1    0    0    1
    103   0   0   0   1   0   0   0   1   1    1    1    1    1    1    0    1    1
    104   0   0   0   1   0   1   0   0   1    0    1    1    0    0    1    0    0
    105   0   0   0   1   0   1   0   1   0    0    0    0    0    1    0    0    1
    106   0   0   0   1   0   1   0   1   0    1    1    0    0    1    0    0    0
    107   0   0   0   1   1   0   0   0   0    1    1    0    0    0    0    0    1
    108   0   0   0   1   1   0   0   1   0    0    1    0    0    0    0    0    0
    109   0   0   0   1   1   1   0   1   0    0    0    0    0    0    1    0    1
    110   0   0   1   0   0   0   0   0   0    0    0    0    0    0    0    0    0
    111   0   0   1   0   0   0   0   0   0    0    0    0    1    1    0    1    0
    112   0   0   1   0   0   0   0   0   0    0    0    1    0    1    0    0    1
    113   0   0   1   0   0   0   0   0   0    0    1    0    0    0    0    0    0
    114   0   0   1   0   0   0   0   0   0    1    0    0    0    0    0    0    0
    115   0   0   1   0   0   0   0   0   0    1    0    0    0    0    0    1    1
    116   0   0   1   0   0   0   0   0   0    1    0    0    0    0    0    1    1
    117   0   0   1   0   0   0   0   0   0    1    0    0    0    1    0    0    0
    118   0   0   1   0   0   0   0   0   0    1    0    1    0    1    0    0    1
    119   0   0   1   0   0   0   0   0   1    0    0    1    0    1    0    0    1
    120   0   0   1   0   0   0   0   0   1    1    0    0    0    0    0    0    0
    121   0   0   1   0   0   0   0   1   0    0    0    0    0    0    0    0    0
    122   0   0   1   0   0   0   0   1   0    0    0    0    0    0    0    0    1
    123   0   0   1   0   0   0   0   1   0    0    0    0    0    1    0    0    0
    124   0   0   1   0   0   0   0   1   0    0    0    0    0    1    0    0    1
    125   0   0   1   0   0   0   0   1   0    0    0    1    0    0    0    1    1
    126   0   0   1   0   0   0   0   1   0    0    0    1    0    0    0    1    1
    127   0   0   1   0   0   0   0   1   0    1    0    1    1    1    0    0    1
    128   0   0   1   0   0   0   1   0   1    1    1    1    0    0    0    1    0
    129   0   0   1   0   0   0   1   1   0    1    0    0    0    0    0    0    1
    130   0   0   1   0   0   1   0   0   0    1    1    1    0    1    0    0    1
    131   0   0   1   0   0   1   0   0   1    1    0    0    0    0    0    0    1
    132   0   0   1   0   0   1   0   1   0    1    0    1    0    1    1    0    1
    133   0   0   1   0   0   1   0   1   0    1    0    1    1    0    0    0    1
    134   0   0   1   0   0   1   0   1   0    1    1    1    1    0    0    0    1
    135   0   0   1   0   0   1   0   1   1    1    0    1    1    1    1    0    1
    136   0   0   1   0   0   1   1   0   0    0    0    0    1    0    0    1    0
    137   0   0   1   0   0   1   1   0   0    0    0    1    0    0    0    0    1
    138   0   0   1   0   0   1   1   1   1    1    0    0    1    0    0    0    0
    139   0   0   1   0   1   1   0   0   1    1    0    1    0    1    0    0    0
    140   0   0   1   0   1   1   1   1   0    1    1    0    0    0    1    0    0
    141   0   0   1   1   0   0   0   0   0    0    0    0    0    0    0    0    0
    142   0   0   1   1   0   0   0   0   0    0    0    0    0    0    0    0    0
    143   0   0   1   1   0   0   0   0   0    0    0    0    0    0    0    1    0
    144   0   0   1   1   0   0   0   0   0    1    1    0    1    0    0    0    1
    145   0   0   1   1   0   0   0   1   0    0    0    0    0    0    0    1    1
    146   0   0   1   1   0   0   0   1   0    0    0    0    0    1    0    0    1
    147   0   0   1   1   0   0   0   1   0    0    0    0    1    0    0    0    1
    148   0   0   1   1   0   0   0   1   0    0    0    1    0    0    0    1    1
    149   0   0   1   1   0   0   0   1   0    1    0    0    0    0    0    0    1
    150   0   0   1   1   0   0   0   1   1    0    0    1    0    0    0    0    1
    151   0   0   1   1   0   0   0   1   1    1    0    0    0    0    0    0    1
    152   0   0   1   1   0   0   0   1   1    1    1    1    0    0    0    1    1
    153   0   0   1   1   0   0   0   1   1    1    1    1    1    1    1    0    0
    154   0   0   1   1   0   1   0   0   0    1    1    0    0    0    0    0    1
    155   0   0   1   1   0   1   0   0   0    1    1    1    0    0    0    0    1
    156   0   0   1   1   0   1   0   0   1    1    0    1    0    1    1    0    1
    157   0   0   1   1   0   1   0   1   0    0    0    0    0    0    0    0    0
    158   0   0   1   1   0   1   0   1   0    1    0    0    1    1    0    0    1
    159   0   0   1   1   0   1   0   1   0    1    1    1    0    1    0    0    1
    160   0   0   1   1   1   0   0   0   1    0    0    0    0    0    1    0    1
    161   0   0   1   1   1   0   0   0   1    1    0    0    0    0    1    0    1
    162   0   0   1   1   1   0   0   1   1    0    0    1    0    0    0    1    1
    163   0   0   1   1   1   0   0   1   1    1    1    1    0    0    0    1    1
    164   0   1   0   0   0   0   0   0   0    0    0    0    0    0    0    0    0
    165   0   1   0   0   0   0   0   0   0    0    0    1    0    1    0    0    0
    166   0   1   0   0   0   0   0   0   0    1    0    0    0    1    0    0    1
    167   0   1   0   0   0   0   0   0   1    0    0    0    0    0    0    0    1
    168   0   1   0   0   0   0   0   0   1    0    0    1    0    0    0    0    1
    169   0   1   0   0   0   0   0   0   1    0    1    0    0    0    0    0    1
    170   0   1   0   0   0   0   0   0   1    1    1    1    0    0    0    0    1
    171   0   1   0   0   0   0   0   0   1    1    1    1    0    1    0    0    1
    172   0   1   0   0   0   0   0   1   0    0    0    0    0    0    0    0    0
    173   0   1   0   0   0   0   0   1   0    0    0    0    0    1    0    0    0
    174   0   1   0   0   0   0   0   1   0    0    0    1    1    0    0    0    1
    175   0   1   0   0   0   0   0   1   0    1    0    0    0    0    0    0    1
    176   0   1   0   0   0   0   0   1   0    1    0    1    0    0    0    0    1
    177   0   1   0   0   0   0   0   1   0    1    0    1    0    0    0    0    1
    178   0   1   0   0   0   0   0   1   0    1    1    0    0    0    0    0    0
    179   0   1   0   0   0   0   0   1   0    1    1    0    0    0    0    0    1
    180   0   1   0   0   0   0   0   1   0    1    1    0    0    0    1    0    1
    181   0   1   0   0   0   0   0   1   1    0    0    0    0    1    0    1    1
    182   0   1   0   0   0   0   0   1   1    1    0    0    0    0    0    0    1
    183   0   1   0   0   0   0   0   1   1    1    0    0    0    1    0    0    0
    184   0   1   0   0   0   0   0   1   1    1    0    0    0    1    1    0    1
    185   0   1   0   0   0   0   0   1   1    1    0    1    0    1    1    0    1
    186   0   1   0   0   0   0   0   1   1    1    0    1    0    1    1    1    1
    187   0   1   0   0   0   0   0   1   1    1    0    1    1    1    0    0    1
    188   0   1   0   0   0   0   0   1   1    1    1    0    0    0    1    0    1
    189   0   1   0   0   0   0   0   1   1    1    1    1    0    1    1    1    1
    190   0   1   0   0   0   0   1   1   1    1    1    0    0    0    0    0    1
    191   0   1   0   0   0   1   0   0   0    0    1    1    0    0    0    0    0
    192   0   1   0   0   0   1   0   0   0    1    0    0    0    0    0    0    1
    193   0   1   0   0   0   1   0   0   0    1    0    1    1    0    0    0    1
    194   0   1   0   0   0   1   0   0   1    1    0    1    1    1    1    1    1
    195   0   1   0   0   0   1   0   0   1    1    1    1    1    0    0    1    1
    196   0   1   0   0   0   1   0   1   0    0    1    0    0    1    0    1    1
    197   0   1   0   0   0   1   0   1   0    1    0    1    0    1    1    0    1
    198   0   1   0   0   0   1   0   1   0    1    0    1    1    1    1    0    1
    199   0   1   0   0   0   1   0   1   0    1    1    1    0    0    0    0    1
    200   0   1   0   0   0   1   0   1   0    1    1    1    0    0    0    0    1
    201   0   1   0   0   0   1   0   1   0    1    1    1    0    1    0    0    1
    202   0   1   0   0   0   1   0   1   1    0    0    1    0    1    0    0    1
    203   0   1   0   0   0   1   1   0   0    1    0    1    0    1    0    0    1
    204   0   1   0   0   0   1   1   1   1    1    0    1    0    1    0    0    1
    205   0   1   0   0   0   1   1   1   1    1    1    1    1    1    1    1    1
    206   0   1   0   0   1   0   0   0   0    1    0    1    0    1    0    0    1
    207   0   1   0   0   1   0   0   0   1    0    0    1    0    1    0    0    1
    208   0   1   0   0   1   0   0   1   0    0    0    1    0    0    0    0    1
    209   0   1   0   0   1   0   0   1   0    0    1    1    0    1    0    0    1
    210   0   1   0   0   1   0   0   1   0    1    0    1    0    1    0    0    0
    211   0   1   0   0   1   0   0   1   1    1    1    0    0    1    1    1    1
    212   0   1   0   0   1   0   1   1   0    1    0    0    1    1    0    0    1
    213   0   1   0   0   1   0   1   1   1    1    1    1    1    1    0    0    1
    214   0   1   0   0   1   1   0   1   1    1    1    1    0    0    1    0    1
    215   0   1   0   0   1   1   0   1   1    1    1    1    0    0    1    0    1
    216   0   1   0   0   1   1   0   1   1    1    1    1    0    1    1    1    1
    217   0   1   0   0   1   1   1   1   1    1    0    1    0    1    0    0    1
    218   0   1   0   1   0   0   0   0   0    0    0    0    0    0    0    1    1
    219   0   1   0   1   0   0   0   0   0    0    1    0    0    0    1    0    0
    220   0   1   0   1   0   0   0   0   0    1    0    0    0    0    0    0    0
    221   0   1   0   1   0   0   0   0   0    1    0    0    1    0    0    1    1
    222   0   1   0   1   0   0   0   0   1    1    0    1    1    1    0    1    1
    223   0   1   0   1   0   0   0   0   1    1    0    1    1    1    1    0    1
    224   0   1   0   1   0   0   0   0   1    1    1    0    0    1    0    0    1
    225   0   1   0   1   0   0   0   1   0    0    0    0    0    0    0    0    1
    226   0   1   0   1   0   0   0   1   0    0    1    0    0    0    0    0    0
    227   0   1   0   1   0   0   0   1   0    1    0    0    0    0    0    0    1
    228   0   1   0   1   0   0   0   1   0    1    1    0    0    0    0    0    1
    229   0   1   0   1   0   0   0   1   0    1    1    1    0    1    1    0    1
    230   0   1   0   1   0   0   0   1   1    0    0    0    0    1    0    0    1
    231   0   1   0   1   0   0   0   1   1    1    0    0    0    1    0    0    1
    232   0   1   0   1   0   0   0   1   1    1    0    0    0    1    0    1    1
    233   0   1   0   1   0   0   0   1   1    1    0    0    1    1    1    1    1
    234   0   1   0   1   0   0   1   1   0    1    1    1    1    1    0    0    1
    235   0   1   0   1   0   0   1   1   1    1    0    1    0    1    0    0    1
    236   0   1   0   1   0   1   0   0   1    1    1    0    0    1    1    1    1
    237   0   1   0   1   0   1   0   0   1    1    1    1    0    1    1    0    1
    238   0   1   0   1   0   1   0   0   1    1    1    1    0    1    1    1    1
    239   0   1   0   1   0   1   0   1   0    1    0    0    0    0    0    0    1
    240   0   1   0   1   0   1   0   1   0    1    0    1    0    0    0    0    1
    241   0   1   0   1   0   1   0   1   0    1    0    1    1    0    0    0    1
    242   0   1   0   1   0   1   0   1   0    1    1    0    0    1    1    0    1
    243   0   1   0   1   0   1   0   1   1    1    0    0    0    0    1    0    1
    244   0   1   0   1   0   1   0   1   1    1    0    1    0    0    0    0    1
    245   0   1   0   1   0   1   0   1   1    1    1    0    0    0    0    1    1
    246   0   1   0   1   1   0   0   1   0    0    1    0    0    0    0    0    1
    247   0   1   0   1   1   0   0   1   0    1    1    0    1    1    0    0    1
    248   0   1   0   1   1   0   0   1   0    1    1    1    1    1    0    0    1
    249   0   1   0   1   1   0   0   1   1    1    1    1    0    1    1    1    1
    250   0   1   0   1   1   0   1   1   0    0    0    0    0    0    1    1    1
    251   0   1   0   1   1   1   0   0   0    0    1    1    1    1    0    0    1
    252   0   1   0   1   1   1   0   1   0    1    1    1    1    0    1    0    1
    253   0   1   0   1   1   1   0   1   1    0    1    1    0    1    1    1    1
    254   0   1   0   1   1   1   0   1   1    1    0    1    0    1    1    0    1
    255   0   1   0   1   1   1   0   1   1    1    0    1    1    1    0    0    1
    256   0   1   0   1   1   1   0   1   1    1    1    1    1    1    1    0    1
    257   0   1   0   1   1   1   1   1   0    1    0    0    0    0    1    1    1
    258   0   1   0   1   1   1   1   1   1    1    1    1    1    1    1    1    1
    259   0   1   1   0   0   0   0   0   0    0    0    1    0    0    0    0    1
    260   0   1   1   0   0   0   0   0   0    1    1    0    0    0    0    0    0
    261   0   1   1   0   0   0   0   0   0    1    1    0    0    1    0    0    1
    262   0   1   1   0   0   0   0   0   0    1    1    1    0    0    0    0    1
    263   0   1   1   0   0   0   0   0   1    0    0    1    0    1    0    0    0
    264   0   1   1   0   0   0   0   0   1    1    0    0    0    0    0    0    0
    265   0   1   1   0   0   0   0   0   1    1    0    0    0    0    0    0    1
    266   0   1   1   0   0   0   0   0   1    1    0    1    1    1    1    1    1
    267   0   1   1   0   0   0   0   0   1    1    1    0    0    0    1    0    1
    268   0   1   1   0   0   0   0   1   0    0    0    0    0    0    0    0    1
    269   0   1   1   0   0   0   0   1   0    0    0    1    0    1    0    0    1
    270   0   1   1   0   0   0   0   1   0    0    1    0    0    1    0    0    1
    271   0   1   1   0   0   0   0   1   0    1    0    0    0    0    0    0    1
    272   0   1   1   0   0   0   0   1   0    1    0    0    0    1    0    0    1
    273   0   1   1   0   0   0   0   1   0    1    0    0    1    1    0    0    0
    274   0   1   1   0   0   0   0   1   0    1    0    1    1    0    1    0    1
    275   0   1   1   0   0   0   0   1   0    1    1    0    0    1    1    0    1
    276   0   1   1   0   0   0   0   1   1    0    1    0    0    1    1    1    1
    277   0   1   1   0   0   0   0   1   1    1    0    0    1    0    0    0    1
    278   0   1   1   0   0   0   0   1   1    1    0    1    0    0    0    0    1
    279   0   1   1   0   0   0   0   1   1    1    1    1    0    0    0    0    1
    280   0   1   1   0   0   0   1   1   0    0    0    0    0    1    0    0    1
    281   0   1   1   0   0   0   1   1   1    0    1    1    0    1    0    1    1
    282   0   1   1   0   0   0   1   1   1    1    1    1    0    1    0    0    1
    283   0   1   1   0   0   1   0   0   0    1    1    0    0    0    1    1    1
    284   0   1   1   0   0   1   0   0   0    1    1    0    1    0    0    0    1
    285   0   1   1   0   0   1   0   0   1    1    0    1    0    1    0    0    1
    286   0   1   1   0   0   1   0   0   1    1    0    1    0    1    0    1    1
    287   0   1   1   0   0   1   0   0   1    1    1    1    0    1    1    0    1
    288   0   1   1   0   0   1   0   1   0    1    0    0    0    1    0    0    0
    289   0   1   1   0   0   1   0   1   0    1    0    0    1    1    0    0    1
    290   0   1   1   0   0   1   0   1   0    1    0    0    1    1    0    1    1
    291   0   1   1   0   0   1   0   1   0    1    0    1    0    0    0    0    1
    292   0   1   1   0   0   1   0   1   0    1    1    0    0    0    0    0    1
    293   0   1   1   0   0   1   0   1   0    1    1    0    0    0    0    0    1
    294   0   1   1   0   0   1   0   1   0    1    1    0    1    1    0    0    1
    295   0   1   1   0   0   1   0   1   0    1    1    1    0    1    1    0    1
    296   0   1   1   0   0   1   0   1   1    0    1    0    0    1    0    0    1
    297   0   1   1   0   0   1   0   1   1    1    0    0    0    0    1    0    1
    298   0   1   1   0   0   1   0   1   1    1    0    0    1    1    0    0    1
    299   0   1   1   0   0   1   0   1   1    1    0    1    0    0    0    0    1
    300   0   1   1   0   0   1   0   1   1    1    0    1    0    1    0    1    1
    301   0   1   1   0   0   1   0   1   1    1    0    1    0    1    1    1    1
    302   0   1   1   0   0   1   0   1   1    1    0    1    1    1    1    0    1
    303   0   1   1   0   0   1   0   1   1    1    1    1    0    0    1    1    1
    304   0   1   1   0   0   1   0   1   1    1    1    1    0    1    0    0    1
    305   0   1   1   0   0   1   0   1   1    1    1    1    1    0    1    0    1
    306   0   1   1   0   0   1   1   0   1    1    0    0    1    1    1    0    1
    307   0   1   1   0   0   1   1   1   1    0    1    1    0    1    0    0    1
    308   0   1   1   0   0   1   1   1   1    1    0    0    0    1    1    0    1
    309   0   1   1   0   0   1   1   1   1    1    1    1    0    1    0    1    1
    310   0   1   1   0   1   0   0   0   1    1    1    0    0    0    0    0    1
    311   0   1   1   0   1   0   0   0   1    1    1    1    0    1    1    0    1
    312   0   1   1   0   1   0   0   1   0    1    0    1    0    0    0    0    1
    313   0   1   1   0   1   0   0   1   1    1    0    0    0    1    0    0    1
    314   0   1   1   0   1   0   0   1   1    1    1    1    1    1    1    0    1
    315   0   1   1   0   1   0   1   1   1    0    1    1    0    1    0    0    1
    316   0   1   1   0   1   1   0   0   0    1    0    0    0    1    0    1    1
    317   0   1   1   0   1   1   0   0   1    1    0    1    1    0    1    0    1
    318   0   1   1   0   1   1   0   0   1    1    1    1    0    1    1    1    1
    319   0   1   1   0   1   1   0   0   1    1    1    1    1    1    0    1    1
    320   0   1   1   0   1   1   0   1   0    1    1    0    0    1    0    0    1
    321   0   1   1   0   1   1   0   1   1    1    1    1    0    1    0    0    1
    322   0   1   1   0   1   1   0   1   1    1    1    1    0    1    1    0    1
    323   0   1   1   0   1   1   0   1   1    1    1    1    0    1    1    1    1
    324   0   1   1   1   0   0   0   0   0    0    0    0    0    0    0    0    1
    325   0   1   1   1   0   0   0   0   0    1    1    0    0    0    0    0    1
    326   0   1   1   1   0   0   0   0   1    0    1    1    0    0    0    0    1
    327   0   1   1   1   0   0   0   0   1    1    0    1    0    0    0    0    1
    328   0   1   1   1   0   0   0   1   0    0    0    1    1    1    0    0    1
    329   0   1   1   1   0   0   0   1   0    1    1    1    0    0    1    1    1
    330   0   1   1   1   0   0   0   1   1    1    0    1    0    0    1    1    1
    331   0   1   1   1   0   0   0   1   1    1    0    1    0    1    0    0    1
    332   0   1   1   1   0   0   0   1   1    1    1    1    0    1    0    0    1
    333   0   1   1   1   0   0   0   1   1    1    1    1    1    1    0    0    0
    334   0   1   1   1   0   0   1   1   0    1    0    1    0    1    1    1    1
    335   0   1   1   1   0   1   0   0   0    0    0    1    1    1    0    0    1
    336   0   1   1   1   0   1   0   0   0    1    1    1    0    0    1    0    1
    337   0   1   1   1   0   1   0   0   1    0    0    0    0    1    0    0    1
    338   0   1   1   1   0   1   0   0   1    0    1    1    0    1    1    0    1
    339   0   1   1   1   0   1   0   0   1    1    0    1    0    0    0    0    1
    340   0   1   1   1   0   1   0   0   1    1    0    1    0    1    0    1    1
    341   0   1   1   1   0   1   0   1   0    0    0    1    0    1    0    1    1
    342   0   1   1   1   0   1   0   1   0    1    0    1    1    1    0    0    1
    343   0   1   1   1   0   1   0   1   1    0    1    0    0    0    0    0    1
    344   0   1   1   1   0   1   0   1   1    0    1    1    0    1    0    0    1
    345   0   1   1   1   0   1   0   1   1    1    0    0    0    1    1    0    1
    346   0   1   1   1   0   1   0   1   1    1    0    1    0    1    0    0    1
    347   0   1   1   1   0   1   0   1   1    1    1    0    0    1    0    0    1
    348   0   1   1   1   0   1   0   1   1    1    1    0    1    0    0    0    1
    349   0   1   1   1   0   1   0   1   1    1    1    1    0    1    0    0    1
    350   0   1   1   1   0   1   0   1   1    1    1    1    0    1    1    0    1
    351   0   1   1   1   0   1   0   1   1    1    1    1    0    1    1    1    1
    352   0   1   1   1   0   1   1   0   1    1    1    1    0    1    1    0    1
    353   0   1   1   1   0   1   1   1   1    0    0    0    1    1    0    0    1
    354   0   1   1   1   0   1   1   1   1    1    0    1    1    1    1    0    1
    355   0   1   1   1   1   0   0   0   1    1    0    1    0    1    1    1    1
    356   0   1   1   1   1   0   0   1   1    1    0    1    0    0    1    0    1
    357   0   1   1   1   1   0   0   1   1    1    1    1    0    1    0    0    1
    358   0   1   1   1   1   0   1   1   1    1    0    0    1    1    1    0    1
    359   0   1   1   1   1   1   0   0   1    1    1    1    0    0    0    1    1
    360   0   1   1   1   1   1   0   0   1    1    1    1    1    1    0    1    1
    361   0   1   1   1   1   1   0   1   0    0    0    1    0    0    0    1    1
    362   0   1   1   1   1   1   0   1   0    0    0    1    0    1    0    0    1
    363   0   1   1   1   1   1   0   1   0    1    1    0    0    1    0    0    1
    364   0   1   1   1   1   1   0   1   0    1    1    1    1    1    0    1    1
    365   0   1   1   1   1   1   0   1   1    1    0    1    0    0    0    0    1
    366   0   1   1   1   1   1   0   1   1    1    0    1    0    1    0    0    1
    367   0   1   1   1   1   1   0   1   1    1    0    1    0    1    1    0    1
    368   0   1   1   1   1   1   0   1   1    1    0    1    1    0    0    0    1
    369   0   1   1   1   1   1   0   1   1    1    1    0    1    1    1    0    1
    370   0   1   1   1   1   1   0   1   1    1    1    1    0    0    0    1    1
    371   0   1   1   1   1   1   0   1   1    1    1    1    0    1    1    0    1
    372   0   1   1   1   1   1   0   1   1    1    1    1    0    1    1    1    1
    373   0   1   1   1   1   1   0   1   1    1    1    1    0    1    1    1    1
    374   0   1   1   1   1   1   0   1   1    1    1    1    1    1    0    1    1
    375   0   1   1   1   1   1   1   1   1    1    1    1    1    1    1    1    1
    376   0   1   1   1   1   1   1   1   1    1    1    1    1    1    1    1    1
    377   1   0   0   0   0   0   0   0   0    0    0    0    0    0    0    0    0
    378   1   0   0   0   0   0   0   0   0    0    0    0    0    1    0    1    0
    379   1   0   0   0   0   0   0   0   0    0    1    0    0    0    0    0    1
    380   1   0   0   0   0   0   0   0   0    0    1    0    1    1    1    1    1
    381   1   0   0   0   0   0   0   0   0    1    0    0    0    0    0    0    0
    382   1   0   0   0   0   0   0   0   0    1    0    0    0    0    0    1    1
    383   1   0   0   0   0   0   0   0   0    1    0    0    1    0    0    0    1
    384   1   0   0   0   0   0   0   0   0    1    0    1    0    1    0    1    1
    385   1   0   0   0   0   0   0   0   0    1    0    1    1    1    0    1    1
    386   1   0   0   0   0   0   0   0   1    1    0    0    0    1    0    0    1
    387   1   0   0   0   0   0   0   1   0    0    0    0    0    0    0    0    0
    388   1   0   0   0   0   0   0   1   0    0    0    1    1    0    0    0    1
    389   1   0   0   0   0   0   0   1   0    0    1    1    1    0    0    0    0
    390   1   0   0   0   0   0   0   1   0    1    0    0    0    0    1    0    1
    391   1   0   0   0   0   0   0   1   0    1    0    0    1    0    0    1    1
    392   1   0   0   0   0   0   0   1   0    1    0    1    0    0    0    0    1
    393   1   0   0   0   0   0   0   1   0    1    1    0    0    0    0    0    1
    394   1   0   0   0   0   0   0   1   0    1    1    0    1    0    0    0    1
    395   1   0   0   0   0   0   0   1   1    0    0    0    0    0    0    0    0
    396   1   0   0   0   0   0   0   1   1    0    0    1    0    0    0    0    0
    397   1   0   0   0   0   0   0   1   1    1    1    1    0    1    0    1    1
    398   1   0   0   0   0   0   1   0   0    1    0    0    0    0    1    0    1
    399   1   0   0   0   0   0   1   0   0    1    0    0    0    1    1    0    1
    400   1   0   0   0   0   0   1   1   0    1    1    0    0    0    0    0    0
    401   1   0   0   0   0   1   0   0   0    1    0    0    1    0    0    0    1
    402   1   0   0   0   0   1   0   0   0    1    0    0    1    0    1    0    1
    403   1   0   0   0   0   1   0   0   1    1    1    1    0    1    0    0    1
    404   1   0   0   0   0   1   0   1   0    0    1    0    0    0    1    0    1
    405   1   0   0   0   0   1   0   1   0    1    1    1    0    1    0    0    1
    406   1   0   0   0   0   1   0   1   1    0    0    0    0    1    0    1    0
    407   1   0   0   0   0   1   0   1   1    1    0    0    0    0    0    0    1
    408   1   0   0   0   0   1   1   1   0    1    0    1    0    0    0    0    1
    409   1   0   0   0   1   0   0   0   0    0    1    0    0    0    0    0    1
    410   1   0   0   0   1   0   0   0   0    1    1    0    1    0    0    0    0
    411   1   0   0   0   1   0   0   1   0    1    0    0    1    1    0    0    1
    412   1   0   0   0   1   0   0   1   0    1    0    1    0    0    0    0    1
    413   1   0   0   0   1   0   0   1   1    0    0    0    0    0    0    0    1
    414   1   0   0   0   1   0   0   1   1    1    1    1    0    1    0    0    1
    415   1   0   0   0   1   0   1   0   0    1    0    1    1    0    0    0    1
    416   1   0   0   1   0   0   0   0   0    0    0    0    0    0    0    0    0
    417   1   0   0   1   0   0   0   0   1    1    1    0    0    1    0    0    1
    418   1   0   0   1   0   0   0   1   0    0    0    0    0    0    1    0    1
    419   1   0   0   1   0   0   0   1   0    1    1    0    0    0    0    0    1
    420   1   0   0   1   0   0   0   1   0    1    1    0    0    0    0    0    1
    421   1   0   0   1   0   0   0   1   1    1    0    1    0    0    0    0    1
    422   1   0   0   1   0   0   0   1   1    1    1    1    1    1    0    1    0
    423   1   0   0   1   0   0   1   1   0    1    1    1    0    0    0    0    1
    424   1   0   0   1   0   1   0   0   0    1    0    0    0    0    0    0    1
    425   1   0   0   1   0   1   0   0   1    1    1    1    1    1    0    0    1
    426   1   0   0   1   0   1   0   1   0    0    0    0    0    0    1    0    0
    427   1   0   0   1   0   1   0   1   0    1    0    1    0    0    0    0    1
    428   1   0   0   1   0   1   0   1   0    1    1    1    0    1    0    0    1
    429   1   0   0   1   0   1   0   1   0    1    1    1    0    1    1    1    1
    430   1   0   0   1   0   1   0   1   1    1    0    1    0    1    0    1    1
    431   1   0   0   1   0   1   1   1   0    1    0    0    0    0    0    1    1
    432   1   0   0   1   0   1   1   1   1    0    1    0    0    0    0    0    1
    433   1   0   0   1   0   1   1   1   1    1    1    1    0    0    1    0    1
    434   1   0   0   1   1   1   0   0   0    1    0    0    0    1    0    0    1
    435   1   0   0   1   1   1   0   1   1    0    1    1    1    1    0    0    1
    436   1   0   0   1   1   1   0   1   1    1    1    1    0    1    1    0    1
    437   1   0   0   1   1   1   0   1   1    1    1    1    1    1    0    0    1
    438   1   0   0   1   1   1   1   0   0    1    0    1    0    0    0    0    1
    439   1   0   1   0   0   0   0   0   0    0    0    0    0    0    0    0    0
    440   1   0   1   0   0   0   0   0   0    1    0    0    0    0    0    0    1
    441   1   0   1   0   0   0   0   0   0    1    0    0    1    0    0    0    1
    442   1   0   1   0   0   0   0   0   0    1    1    0    0    0    0    0    0
    443   1   0   1   0   0   0   0   1   0    0    0    1    0    0    0    1    0
    444   1   0   1   0   0   0   0   1   0    1    0    0    0    0    0    0    1
    445   1   0   1   0   0   0   0   1   0    1    1    1    0    1    1    1    1
    446   1   0   1   0   0   0   0   1   0    1    1    1    1    0    0    0    1
    447   1   0   1   0   0   0   1   0   1    1    1    1    0    1    1    0    1
    448   1   0   1   0   0   1   0   0   0    0    1    1    0    0    0    1    1
    449   1   0   1   0   0   1   0   0   0    1    0    1    1    1    0    1    0
    450   1   0   1   0   0   1   0   0   1    1    1    1    0    0    0    1    1
    451   1   0   1   0   0   1   0   1   0    0    1    0    0    0    0    0    1
    452   1   0   1   0   0   1   0   1   1    1    0    1    0    1    0    1    1
    453   1   0   1   0   0   1   1   0   1    0    0    0    0    0    0    0    0
    454   1   0   1   0   1   0   0   0   1    1    1    0    0    1    1    0    1
    455   1   0   1   0   1   0   0   1   0    1    0    1    1    1    0    0    1
    456   1   0   1   0   1   1   0   1   0    1    0    1    0    1    1    1    1
    457   1   0   1   0   1   1   0   1   0    1    0    1    1    0    1    0    1
    458   1   0   1   0   1   1   0   1   1    1    1    1    1    1    1    0    1
    459   1   0   1   1   0   0   0   0   0    0    0    0    0    0    0    0    1
    460   1   0   1   1   0   0   0   0   1    1    0    1    0    1    0    0    1
    461   1   0   1   1   0   0   0   1   0    0    0    0    0    0    0    1    1
    462   1   0   1   1   0   0   0   1   0    0    0    1    0    1    0    1    1
    463   1   0   1   1   0   0   0   1   0    0    1    0    0    0    0    0    1
    464   1   0   1   1   0   0   1   1   0    0    1    1    0    0    0    0    1
    465   1   0   1   1   0   1   0   0   0    1    1    1    0    0    0    1    1
    466   1   0   1   1   0   1   0   0   1    1    0    1    1    1    1    0    1
    467   1   0   1   1   0   1   0   0   1    1    1    1    0    1    1    0    1
    468   1   0   1   1   0   1   0   1   1    1    0    1    1    0    1    1    1
    469   1   0   1   1   0   1   0   1   1    1    1    1    0    0    0    0    1
    470   1   0   1   1   0   1   0   1   1    1    1    1    0    1    1    0    1
    471   1   0   1   1   1   0   0   1   1    0    0    1    0    1    1    0    1
    472   1   0   1   1   1   0   1   1   1    1    0    0    1    0    0    0    1
    473   1   0   1   1   1   0   1   1   1    1    1    1    1    1    1    1    1
    474   1   0   1   1   1   1   0   1   1    0    1    1    0    0    0    0    1
    475   1   0   1   1   1   1   1   1   1    1    1    1    1    1    1    1    1
    476   1   1   0   0   0   0   0   0   0    0    1    1    0    0    0    0    1
    477   1   1   0   0   0   0   0   0   0    1    1    0    0    0    0    0    1
    478   1   1   0   0   0   0   0   0   0    1    1    1    0    1    1    1    1
    479   1   1   0   0   0   0   0   0   0    1    1    1    1    0    0    0    1
    480   1   1   0   0   0   0   0   0   1    0    0    0    0    0    0    1    0
    481   1   1   0   0   0   0   0   0   1    0    0    0    0    1    0    0    1
    482   1   1   0   0   0   0   0   0   1    1    1    0    0    0    0    0    1
    483   1   1   0   0   0   0   0   0   1    1    1    1    0    1    1    0    1
    484   1   1   0   0   0   0   0   1   0    1    0    0    0    0    0    0    1
    485   1   1   0   0   0   0   0   1   0    1    0    0    1    0    0    0    1
    486   1   1   0   0   0   0   0   1   0    1    0    0    1    0    0    0    1
    487   1   1   0   0   0   0   0   1   0    1    0    1    0    0    0    0    1
    488   1   1   0   0   0   0   0   1   0    1    0    1    0    1    0    1    1
    489   1   1   0   0   0   0   0   1   0    1    0    1    0    1    1    0    1
    490   1   1   0   0   0   0   0   1   0    1    0    1    1    0    0    0    1
    491   1   1   0   0   0   0   0   1   0    1    1    0    1    0    0    0    0
    492   1   1   0   0   0   0   0   1   0    1    1    1    0    0    1    1    1
    493   1   1   0   0   0   0   0   1   1    1    0    1    1    1    1    0    1
    494   1   1   0   0   0   0   0   1   1    1    1    0    0    1    0    0    1
    495   1   1   0   0   0   0   0   1   1    1    1    1    1    1    1    1    1
    496   1   1   0   0   0   0   1   1   0    0    0    0    0    0    0    0    0
    497   1   1   0   0   0   0   1   1   1    1    0    1    1    1    1    0    1
    498   1   1   0   0   0   1   0   0   0    0    1    1    0    0    0    1    1
    499   1   1   0   0   0   1   0   0   0    1    1    0    0    0    0    0    1
    500   1   1   0   0   0   1   0   0   0    1    1    0    0    1    0    0    0
    501   1   1   0   0   0   1   0   0   0    1    1    1    1    1    1    0    1
    502   1   1   0   0   0   1   0   0   1    1    0    1    0    0    1    1    1
    503   1   1   0   0   0   1   0   0   1    1    0    1    0    1    1    0    0
    504   1   1   0   0   0   1   0   0   1    1    1    1    1    1    1    0    1
    505   1   1   0   0   0   1   0   1   0    1    0    1    0    1    0    1    1
    506   1   1   0   0   0   1   0   1   0    1    0    1    0    1    0    1    1
    507   1   1   0   0   0   1   0   1   0    1    0    1    0    1    1    1    1
    508   1   1   0   0   0   1   0   1   0    1    1    0    0    0    0    0    1
    509   1   1   0   0   0   1   0   1   0    1    1    1    0    0    0    1    1
    510   1   1   0   0   0   1   0   1   0    1    1    1    0    0    1    0    1
    511   1   1   0   0   0   1   0   1   0    1    1    1    0    0    1    1    1
    512   1   1   0   0   0   1   0   1   0    1    1    1    1    1    0    1    1
    513   1   1   0   0   0   1   0   1   0    1    1    1    1    1    1    1    1
    514   1   1   0   0   0   1   0   1   1    0    1    1    0    1    0    0    1
    515   1   1   0   0   0   1   0   1   1    0    1    1    0    1    1    1    1
    516   1   1   0   0   0   1   0   1   1    1    0    0    0    1    0    0    1
    517   1   1   0   0   0   1   0   1   1    1    0    1    0    0    1    0    1
    518   1   1   0   0   0   1   0   1   1    1    0    1    1    0    0    1    1
    519   1   1   0   0   0   1   0   1   1    1    1    0    0    0    0    0    1
    520   1   1   0   0   0   1   0   1   1    1    1    0    0    1    1    1    1
    521   1   1   0   0   0   1   0   1   1    1    1    1    0    0    0    1    1
    522   1   1   0   0   0   1   0   1   1    1    1    1    1    1    1    0    1
    523   1   1   0   0   0   1   0   1   1    1    1    1    1    1    1    1    1
    524   1   1   0   0   0   1   0   1   1    1    1    1    1    1    1    1    1
    525   1   1   0   0   0   1   1   0   0    1    1    1    0    1    0    0    1
    526   1   1   0   0   0   1   1   0   1    1    0    1    0    0    0    1    1
    527   1   1   0   0   0   1   1   1   0    1    0    0    0    0    1    0    1
    528   1   1   0   0   0   1   1   1   0    1    1    1    0    1    0    0    1
    529   1   1   0   0   0   1   1   1   1    1    0    1    0    0    0    0    1
    530   1   1   0   0   0   1   1   1   1    1    1    0    1    0    0    0    1
    531   1   1   0   0   0   1   1   1   1    1    1    1    0    1    0    0    1
    532   1   1   0   0   0   1   1   1   1    1    1    1    0    1    1    1    1
    533   1   1   0   0   0   1   1   1   1    1    1    1    1    1    1    0    1
    534   1   1   0   0   0   1   1   1   1    1    1    1    1    1    1    1    1
    535   1   1   0   0   1   0   0   0   1    1    1    1    1    1    1    0    1
    536   1   1   0   0   1   0   0   1   0    1    0    1    0    0    0    0    0
    537   1   1   0   0   1   0   0   1   0    1    0    1    1    1    1    1    1
    538   1   1   0   0   1   0   0   1   1    1    0    1    1    1    1    0    1
    539   1   1   0   0   1   0   0   1   1    1    0    1    1    1    1    0    1
    540   1   1   0   0   1   0   0   1   1    1    1    1    0    0    0    1    1
    541   1   1   0   0   1   0   0   1   1    1    1    1    0    1    1    0    1
    542   1   1   0   0   1   0   1   1   1    0    0    0    0    1    0    0    0
    543   1   1   0   0   1   1   0   1   0    1    1    1    1    1    1    0    1
    544   1   1   0   0   1   1   0   1   1    1    0    1    0    0    0    0    1
    545   1   1   0   0   1   1   0   1   1    1    0    1    1    1    0    0    1
    546   1   1   0   0   1   1   0   1   1    1    1    0    1    0    1    1    1
    547   1   1   0   0   1   1   0   1   1    1    1    1    0    0    1    1    1
    548   1   1   0   0   1   1   0   1   1    1    1    1    0    1    1    1    1
    549   1   1   0   0   1   1   0   1   1    1    1    1    1    0    0    1    1
    550   1   1   0   0   1   1   0   1   1    1    1    1    1    1    1    0    1
    551   1   1   0   0   1   1   0   1   1    1    1    1    1    1    1    1    1
    552   1   1   0   0   1   1   1   1   0    1    1    1    1    1    1    1    1
    553   1   1   0   0   1   1   1   1   1    1    1    1    0    1    1    1    1
    554   1   1   0   0   1   1   1   1   1    1    1    1    1    1    1    1    1
    555   1   1   0   1   0   0   0   0   0    1    1    1    0    1    0    0    1
    556   1   1   0   1   0   0   0   0   1    0    1    1    0    1    1    0    1
    557   1   1   0   1   0   0   0   0   1    1    0    1    0    1    0    1    1
    558   1   1   0   1   0   0   0   0   1    1    0    1    0    1    1    0    1
    559   1   1   0   1   0   0   0   0   1    1    1    1    0    0    1    1    1
    560   1   1   0   1   0   0   0   1   0    0    0    1    0    1    0    0    1
    561   1   1   0   1   0   0   0   1   0    1    1    0    0    1    1    1    1
    562   1   1   0   1   0   0   0   1   1    0    0    0    0    0    0    0    1
    563   1   1   0   1   0   0   0   1   1    0    1    0    0    0    0    0    1
    564   1   1   0   1   0   0   0   1   1    1    0    1    0    1    0    1    1
    565   1   1   0   1   0   0   0   1   1    1    0    1    1    1    1    0    1
    566   1   1   0   1   0   0   0   1   1    1    1    0    0    1    1    1    1
    567   1   1   0   1   0   0   0   1   1    1    1    1    0    0    1    1    1
    568   1   1   0   1   0   0   0   1   1    1    1    1    0    0    1    1    1
    569   1   1   0   1   0   0   1   0   1    0    1    1    0    1    1    1    1
    570   1   1   0   1   0   0   1   1   0    1    1    1    0    1    1    0    1
    571   1   1   0   1   0   0   1   1   1    1    1    0    1    1    0    1    1
    572   1   1   0   1   0   0   1   1   1    1    1    1    1    0    0    0    1
    573   1   1   0   1   0   0   1   1   1    1    1    1    1    1    0    1    1
    574   1   1   0   1   0   0   1   1   1    1    1    1    1    1    1    0    1
    575   1   1   0   1   0   1   0   0   0    1    0    0    0    0    0    0    1
    576   1   1   0   1   0   1   0   0   0    1    0    1    0    1    0    0    0
    577   1   1   0   1   0   1   0   0   0    1    1    1    0    1    1    0    1
    578   1   1   0   1   0   1   0   0   1    1    0    1    0    1    0    1    1
    579   1   1   0   1   0   1   0   0   1    1    1    1    0    1    0    0    1
    580   1   1   0   1   0   1   0   0   1    1    1    1    0    1    1    1    1
    581   1   1   0   1   0   1   0   1   0    0    0    1    0    1    0    0    1
    582   1   1   0   1   0   1   0   1   0    0    1    1    0    0    1    0    1
    583   1   1   0   1   0   1   0   1   0    1    1    1    0    1    1    0    1
    584   1   1   0   1   0   1   0   1   0    1    1    1    1    0    1    0    1
    585   1   1   0   1   0   1   0   1   1    0    1    1    0    1    1    0    1
    586   1   1   0   1   0   1   0   1   1    0    1    1    1    0    1    0    1
    587   1   1   0   1   0   1   0   1   1    0    1    1    1    1    1    0    1
    588   1   1   0   1   0   1   0   1   1    1    0    0    0    1    1    0    1
    589   1   1   0   1   0   1   0   1   1    1    0    1    0    0    1    1    1
    590   1   1   0   1   0   1   0   1   1    1    0    1    0    1    0    1    1
    591   1   1   0   1   0   1   0   1   1    1    0    1    1    1    1    0    1
    592   1   1   0   1   0   1   0   1   1    1    1    1    0    0    1    0    1
    593   1   1   0   1   0   1   0   1   1    1    1    1    0    1    0    0    1
    594   1   1   0   1   0   1   0   1   1    1    1    1    0    1    1    0    1
    595   1   1   0   1   0   1   0   1   1    1    1    1    0    1    1    1    1
    596   1   1   0   1   0   1   0   1   1    1    1    1    0    1    1    1    1
    597   1   1   0   1   0   1   0   1   1    1    1    1    1    0    1    0    1
    598   1   1   0   1   0   1   0   1   1    1    1    1    1    0    1    1    1
    599   1   1   0   1   0   1   0   1   1    1    1    1    1    1    1    0    1
    600   1   1   0   1   0   1   0   1   1    1    1    1    1    1    1    1    1
    601   1   1   0   1   0   1   1   0   0    1    1    0    0    1    1    1    1
    602   1   1   0   1   0   1   1   0   1    1    1    1    0    1    0    1    1
    603   1   1   0   1   0   1   1   0   1    1    1    1    0    1    1    1    1
    604   1   1   0   1   0   1   1   1   0    1    0    1    1    1    1    1    1
    605   1   1   0   1   0   1   1   1   0    1    1    0    0    1    1    1    1
    606   1   1   0   1   0   1   1   1   0    1    1    1    1    1    1    1    1
    607   1   1   0   1   0   1   1   1   1    0    1    0    1    0    0    1    1
    608   1   1   0   1   0   1   1   1   1    0    1    1    1    1    0    0    1
    609   1   1   0   1   0   1   1   1   1    1    0    1    0    1    1    0    1
    610   1   1   0   1   0   1   1   1   1    1    1    1    1    1    1    1    1
    611   1   1   0   1   1   0   0   0   0    1    1    1    1    1    1    1    1
    612   1   1   0   1   1   0   0   1   0    0    0    0    0    1    0    0    1
    613   1   1   0   1   1   0   0   1   0    1    0    1    0    1    0    0    1
    614   1   1   0   1   1   0   0   1   0    1    1    1    0    1    0    1    1
    615   1   1   0   1   1   0   0   1   1    1    1    1    0    1    1    0    0
    616   1   1   0   1   1   0   0   1   1    1    1    1    1    1    1    0    1
    617   1   1   0   1   1   0   1   0   1    0    1    1    1    1    1    1    1
    618   1   1   0   1   1   0   1   1   0    0    1    1    1    1    0    0    1
    619   1   1   0   1   1   0   1   1   1    0    1    1    1    1    1    1    1
    620   1   1   0   1   1   0   1   1   1    1    1    0    0    1    0    1    1
    621   1   1   0   1   1   0   1   1   1    1    1    1    0    1    1    0    1
    622   1   1   0   1   1   1   0   0   0    1    1    1    0    1    1    0    1
    623   1   1   0   1   1   1   0   0   1    1    1    0    0    1    0    0    1
    624   1   1   0   1   1   1   0   0   1    1    1    1    0    1    0    1    1
    625   1   1   0   1   1   1   0   0   1    1    1    1    0    1    1    1    1
    626   1   1   0   1   1   1   0   1   0    0    1    1    1    0    1    0    1
    627   1   1   0   1   1   1   0   1   1    1    0    1    0    1    1    1    1
    628   1   1   0   1   1   1   0   1   1    1    1    1    0    1    0    0    1
    629   1   1   0   1   1   1   0   1   1    1    1    1    0    1    1    0    1
    630   1   1   0   1   1   1   0   1   1    1    1    1    0    1    1    1    1
    631   1   1   0   1   1   1   0   1   1    1    1    1    1    0    1    0    1
    632   1   1   0   1   1   1   0   1   1    1    1    1    1    1    1    0    1
    633   1   1   0   1   1   1   0   1   1    1    1    1    1    1    1    1    1
    634   1   1   0   1   1   1   0   1   1    1    1    1    1    1    1    1    1
    635   1   1   0   1   1   1   1   0   0    1    0    0    0    1    0    0    1
    636   1   1   0   1   1   1   1   0   1    1    1    1    0    1    1    1    1
    637   1   1   0   1   1   1   1   1   0    1    1    1    0    0    0    1    1
    638   1   1   0   1   1   1   1   1   0    1    1    1    0    1    0    1    1
    639   1   1   0   1   1   1   1   1   1    0    0    1    0    1    1    1    1
    640   1   1   0   1   1   1   1   1   1    1    1    1    0    1    0    0    1
    641   1   1   0   1   1   1   1   1   1    1    1    1    0    1    1    0    1
    642   1   1   0   1   1   1   1   1   1    1    1    1    0    1    1    1    1
    643   1   1   0   1   1   1   1   1   1    1    1    1    1    0    0    1    1
    644   1   1   0   1   1   1   1   1   1    1    1    1    1    0    1    0    1
    645   1   1   0   1   1   1   1   1   1    1    1    1    1    1    1    0    1
    646   1   1   0   1   1   1   1   1   1    1    1    1    1    1    1    1    1
    647   1   1   1   0   0   0   0   0   0    0    1    0    0    0    0    0    0
    648   1   1   1   0   0   0   0   0   0    0    1    0    1    1    0    0    1
    649   1   1   1   0   0   0   0   0   1    1    1    1    0    1    0    1    1
    650   1   1   1   0   0   0   0   1   0    0    1    0    1    1    1    1    1
    651   1   1   1   0   0   0   0   1   0    1    0    1    0    1    1    0    1
    652   1   1   1   0   0   0   0   1   0    1    1    1    0    0    0    0    1
    653   1   1   1   0   0   0   0   1   0    1    1    1    0    1    0    1    1
    654   1   1   1   0   0   0   0   1   1    0    1    1    0    0    0    1    1
    655   1   1   1   0   0   0   0   1   1    0    1    1    0    1    0    0    1
    656   1   1   1   0   0   0   0   1   1    1    0    0    0    0    0    0    1
    657   1   1   1   0   0   0   0   1   1    1    0    1    1    1    0    1    1
    658   1   1   1   0   0   0   0   1   1    1    1    0    1    1    0    1    1
    659   1   1   1   0   0   0   0   1   1    1    1    1    0    1    0    0    1
    660   1   1   1   0   0   0   0   1   1    1    1    1    0    1    0    1    1
    661   1   1   1   0   0   0   0   1   1    1    1    1    1    0    0    0    1
    662   1   1   1   0   0   0   0   1   1    1    1    1    1    1    0    0    1
    663   1   1   1   0   0   0   1   1   1    1    1    1    1    1    0    0    1
    664   1   1   1   0   0   1   0   0   0    1    0    1    0    1    0    0    1
    665   1   1   1   0   0   1   0   0   0    1    1    0    0    1    1    1    1
    666   1   1   1   0   0   1   0   0   0    1    1    1    0    0    1    0    1
    667   1   1   1   0   0   1   0   0   1    0    0    1    1    1    0    1    1
    668   1   1   1   0   0   1   0   0   1    1    0    1    0    1    1    1    1
    669   1   1   1   0   0   1   0   1   0    0    0    0    0    0    0    0    1
    670   1   1   1   0   0   1   0   1   0    1    0    0    0    1    1    0    1
    671   1   1   1   0   0   1   0   1   0    1    0    1    0    0    0    1    1
    672   1   1   1   0   0   1   0   1   0    1    0    1    0    1    0    0    1
    673   1   1   1   0   0   1   0   1   0    1    1    0    0    0    1    0    1
    674   1   1   1   0   0   1   0   1   0    1    1    1    0    0    0    0    1
    675   1   1   1   0   0   1   0   1   0    1    1    1    0    1    0    0    1
    676   1   1   1   0   0   1   0   1   0    1    1    1    0    1    1    0    1
    677   1   1   1   0   0   1   0   1   0    1    1    1    0    1    1    1    1
    678   1   1   1   0   0   1   0   1   0    1    1    1    1    1    0    1    1
    679   1   1   1   0   0   1   0   1   0    1    1    1    1    1    1    1    1
    680   1   1   1   0   0   1   0   1   1    0    0    1    0    0    0    0    1
    681   1   1   1   0   0   1   0   1   1    0    0    1    1    0    0    1    1
    682   1   1   1   0   0   1   0   1   1    0    1    1    0    1    0    0    1
    683   1   1   1   0   0   1   0   1   1    0    1    1    0    1    1    0    1
    684   1   1   1   0   0   1   0   1   1    1    0    1    0    0    0    0    1
    685   1   1   1   0   0   1   0   1   1    1    0    1    0    1    0    1    1
    686   1   1   1   0   0   1   0   1   1    1    0    1    1    1    0    0    1
    687   1   1   1   0   0   1   0   1   1    1    1    0    0    0    0    0    1
    688   1   1   1   0   0   1   0   1   1    1    1    1    0    0    1    1    1
    689   1   1   1   0   0   1   0   1   1    1    1    1    0    1    1    0    1
    690   1   1   1   0   0   1   0   1   1    1    1    1    0    1    1    1    1
    691   1   1   1   0   0   1   0   1   1    1    1    1    1    1    1    1    0
    692   1   1   1   0   0   1   0   1   1    1    1    1    1    1    1    1    1
    693   1   1   1   0   0   1   1   0   1    0    1    1    1    0    1    1    1
    694   1   1   1   0   0   1   1   0   1    1    1    1    1    1    1    1    1
    695   1   1   1   0   0   1   1   1   0    0    1    1    1    1    1    1    1
    696   1   1   1   0   0   1   1   1   0    1    0    1    0    0    0    0    1
    697   1   1   1   0   0   1   1   1   0    1    0    1    0    1    1    0    1
    698   1   1   1   0   0   1   1   1   0    1    0    1    1    1    1    1    1
    699   1   1   1   0   0   1   1   1   0    1    1    1    0    0    0    0    1
    700   1   1   1   0   0   1   1   1   1    1    0    1    0    1    1    0    1
    701   1   1   1   0   0   1   1   1   1    1    0    1    0    1    1    1    1
    702   1   1   1   0   0   1   1   1   1    1    1    0    1    1    0    0    1
    703   1   1   1   0   0   1   1   1   1    1    1    1    0    1    1    0    1
    704   1   1   1   0   0   1   1   1   1    1    1    1    0    1    1    1    1
    705   1   1   1   0   0   1   1   1   1    1    1    1    1    1    0    0    0
    706   1   1   1   0   0   1   1   1   1    1    1    1    1    1    1    0    1
    707   1   1   1   0   0   1   1   1   1    1    1    1    1    1    1    1    1
    708   1   1   1   0   1   0   0   0   1    0    1    1    1    1    1    1    1
    709   1   1   1   0   1   0   0   0   1    1    0    1    0    0    0    0    1
    710   1   1   1   0   1   0   0   1   0    0    1    0    0    0    0    1    0
    711   1   1   1   0   1   0   0   1   0    1    0    0    0    0    0    1    1
    712   1   1   1   0   1   0   0   1   0    1    1    0    0    0    0    1    1
    713   1   1   1   0   1   0   0   1   0    1    1    1    0    0    0    0    1
    714   1   1   1   0   1   0   0   1   0    1    1    1    1    1    0    1    1
    715   1   1   1   0   1   0   0   1   0    1    1    1    1    1    1    0    1
    716   1   1   1   0   1   0   0   1   1    0    1    1    1    1    1    1    1
    717   1   1   1   0   1   0   0   1   1    1    1    1    0    1    0    0    1
    718   1   1   1   0   1   0   0   1   1    1    1    1    0    1    1    0    1
    719   1   1   1   0   1   0   0   1   1    1    1    1    0    1    1    1    1
    720   1   1   1   0   1   0   0   1   1    1    1    1    1    1    1    1    1
    721   1   1   1   0   1   0   0   1   1    1    1    1    1    1    1    1    1
    722   1   1   1   0   1   0   1   0   0    1    1    1    1    1    1    0    1
    723   1   1   1   0   1   0   1   0   1    0    1    1    0    0    1    0    1
    724   1   1   1   0   1   0   1   1   0    1    0    1    0    1    0    1    1
    725   1   1   1   0   1   0   1   1   0    1    0    1    0    1    1    1    1
    726   1   1   1   0   1   0   1   1   0    1    1    1    0    1    0    1    1
    727   1   1   1   0   1   0   1   1   0    1    1    1    0    1    1    0    1
    728   1   1   1   0   1   0   1   1   1    0    1    1    0    1    1    0    1
    729   1   1   1   0   1   0   1   1   1    1    1    1    1    1    0    1    1
    730   1   1   1   0   1   1   0   0   0    0    1    1    0    1    0    1    1
    731   1   1   1   0   1   1   0   0   1    1    1    0    1    1    0    1    1
    732   1   1   1   0   1   1   0   0   1    1    1    1    1    1    0    1    1
    733   1   1   1   0   1   1   0   0   1    1    1    1    1    1    1    1    1
    734   1   1   1   0   1   1   0   1   0    0    1    1    1    1    1    0    1
    735   1   1   1   0   1   1   0   1   0    1    0    0    0    1    0    0    1
    736   1   1   1   0   1   1   0   1   0    1    0    1    1    1    0    1    1
    737   1   1   1   0   1   1   0   1   0    1    1    1    1    0    0    0    1
    738   1   1   1   0   1   1   0   1   1    0    1    0    0    0    0    0    1
    739   1   1   1   0   1   1   0   1   1    0    1    1    1    1    1    1    1
    740   1   1   1   0   1   1   0   1   1    1    0    0    0    0    1    0    1
    741   1   1   1   0   1   1   0   1   1    1    0    1    0    1    0    0    1
    742   1   1   1   0   1   1   0   1   1    1    0    1    0    1    1    1    1
    743   1   1   1   0   1   1   0   1   1    1    0    1    1    1    1    1    1
    744   1   1   1   0   1   1   0   1   1    1    1    0    1    1    1    0    1
    745   1   1   1   0   1   1   0   1   1    1    1    1    0    1    1    1    1
    746   1   1   1   0   1   1   0   1   1    1    1    1    1    1    0    1    1
    747   1   1   1   0   1   1   0   1   1    1    1    1    1    1    1    0    1
    748   1   1   1   0   1   1   0   1   1    1    1    1    1    1    1    1    1
    749   1   1   1   0   1   1   0   1   1    1    1    1    1    1    1    1    1
    750   1   1   1   0   1   1   1   0   1    1    1    1    1    1    1    1    1
    751   1   1   1   0   1   1   1   1   0    0    1    0    0    0    0    0    1
    752   1   1   1   0   1   1   1   1   0    1    1    1    0    1    1    0    1
    753   1   1   1   0   1   1   1   1   0    1    1    1    1    0    1    1    1
    754   1   1   1   0   1   1   1   1   1    1    1    1    0    1    0    0    1
    755   1   1   1   0   1   1   1   1   1    1    1    1    0    1    1    1    1
    756   1   1   1   0   1   1   1   1   1    1    1    1    1    1    1    0    1
    757   1   1   1   0   1   1   1   1   1    1    1    1    1    1    1    1    1
    758   1   1   1   1   0   0   0   0   0    1    0    1    1    1    1    0    1
    759   1   1   1   1   0   0   0   0   0    1    1    1    0    1    1    1    1
    760   1   1   1   1   0   0   0   0   0    1    1    1    1    0    0    0    1
    761   1   1   1   1   0   0   0   0   0    1    1    1    1    1    1    1    1
    762   1   1   1   1   0   0   0   0   1    1    0    1    0    1    0    0    1
    763   1   1   1   1   0   0   0   0   1    1    1    1    0    1    0    0    1
    764   1   1   1   1   0   0   0   0   1    1    1    1    0    1    1    0    1
    765   1   1   1   1   0   0   0   1   0    0    1    0    0    1    0    1    1
    766   1   1   1   1   0   0   0   1   0    1    0    0    0    0    0    0    1
    767   1   1   1   1   0   0   0   1   0    1    0    0    0    0    1    1    1
    768   1   1   1   1   0   0   0   1   0    1    0    0    0    1    0    0    1
    769   1   1   1   1   0   0   0   1   0    1    0    1    0    1    1    0    1
    770   1   1   1   1   0   0   0   1   0    1    0    1    1    0    0    1    1
    771   1   1   1   1   0   0   0   1   0    1    1    1    0    1    1    0    1
    772   1   1   1   1   0   0   0   1   0    1    1    1    1    1    1    0    1
    773   1   1   1   1   0   0   0   1   1    0    1    1    1    1    1    0    1
    774   1   1   1   1   0   0   0   1   1    1    0    1    0    0    1    0    1
    775   1   1   1   1   0   0   0   1   1    1    0    1    0    1    1    0    1
    776   1   1   1   1   0   0   0   1   1    1    0    1    1    1    1    0    1
    777   1   1   1   1   0   0   0   1   1    1    1    0    1    1    0    0    1
    778   1   1   1   1   0   0   0   1   1    1    1    1    0    0    0    0    1
    779   1   1   1   1   0   0   0   1   1    1    1    1    0    1    0    0    1
    780   1   1   1   1   0   0   0   1   1    1    1    1    0    1    1    1    1
    781   1   1   1   1   0   0   0   1   1    1    1    1    1    0    0    0    1
    782   1   1   1   1   0   0   0   1   1    1    1    1    1    0    1    0    1
    783   1   1   1   1   0   0   0   1   1    1    1    1    1    1    0    1    1
    784   1   1   1   1   0   0   1   0   0    1    1    1    0    0    1    0    1
    785   1   1   1   1   0   0   1   1   1    1    0    1    1    1    1    1    1
    786   1   1   1   1   0   0   1   1   1    1    1    0    0    1    0    1    1
    787   1   1   1   1   0   0   1   1   1    1    1    1    0    1    1    1    1
    788   1   1   1   1   0   0   1   1   1    1    1    1    0    1    1    1    1
    789   1   1   1   1   0   0   1   1   1    1    1    1    1    1    1    0    1
    790   1   1   1   1   0   1   0   0   0    1    0    1    0    1    1    1    1
    791   1   1   1   1   0   1   0   0   0    1    0    1    1    1    1    1    1
    792   1   1   1   1   0   1   0   0   0    1    1    1    0    0    0    0    1
    793   1   1   1   1   0   1   0   0   0    1    1    1    0    1    1    1    1
    794   1   1   1   1   0   1   0   0   0    1    1    1    1    1    0    1    1
    795   1   1   1   1   0   1   0   0   1    1    0    0    1    0    0    0    0
    796   1   1   1   1   0   1   0   0   1    1    1    0    1    0    0    1    1
    797   1   1   1   1   0   1   0   0   1    1    1    1    1    1    1    1    1
    798   1   1   1   1   0   1   0   1   0    0    0    1    0    1    0    0    1
    799   1   1   1   1   0   1   0   1   0    0    1    1    1    1    1    1    1
    800   1   1   1   1   0   1   0   1   0    1    0    1    0    0    0    0    1
    801   1   1   1   1   0   1   0   1   0    1    0    1    0    0    0    0    1
    802   1   1   1   1   0   1   0   1   0    1    0    1    0    1    0    0    1
    803   1   1   1   1   0   1   0   1   0    1    0    1    0    1    1    0    1
    804   1   1   1   1   0   1   0   1   0    1    0    1    1    1    0    0    1
    805   1   1   1   1   0   1   0   1   0    1    0    1    1    1    1    1    1
    806   1   1   1   1   0   1   0   1   0    1    1    1    0    1    0    1    1
    807   1   1   1   1   0   1   0   1   0    1    1    1    0    1    1    1    1
    808   1   1   1   1   0   1   0   1   0    1    1    1    1    1    0    0    1
    809   1   1   1   1   0   1   0   1   1    0    0    1    1    1    1    1    1
    810   1   1   1   1   0   1   0   1   1    0    1    1    1    1    1    1    1
    811   1   1   1   1   0   1   0   1   1    0    1    1    1    1    1    1    1
    812   1   1   1   1   0   1   0   1   1    1    0    0    0    0    1    0    1
    813   1   1   1   1   0   1   0   1   1    1    0    1    0    1    0    0    1
    814   1   1   1   1   0   1   0   1   1    1    0    1    0    1    0    1    1
    815   1   1   1   1   0   1   0   1   1    1    0    1    1    1    1    1    1
    816   1   1   1   1   0   1   0   1   1    1    1    1    0    1    0    1    1
    817   1   1   1   1   0   1   0   1   1    1    1    1    0    1    0    1    1
    818   1   1   1   1   0   1   0   1   1    1    1    1    0    1    1    0    1
    819   1   1   1   1   0   1   0   1   1    1    1    1    0    1    1    0    1
    820   1   1   1   1   0   1   0   1   1    1    1    1    0    1    1    0    1
    821   1   1   1   1   0   1   0   1   1    1    1    1    0    1    1    1    1
    822   1   1   1   1   0   1   0   1   1    1    1    1    0    1    1    1    1
    823   1   1   1   1   0   1   0   1   1    1    1    1    1    1    1    0    1
    824   1   1   1   1   0   1   0   1   1    1    1    1    1    1    1    0    1
    825   1   1   1   1   0   1   0   1   1    1    1    1    1    1    1    1    1
    826   1   1   1   1   0   1   1   0   0    1    0    1    0    1    1    1    1
    827   1   1   1   1   0   1   1   0   1    1    0    1    0    1    0    1    1
    828   1   1   1   1   0   1   1   0   1    1    0    1    0    1    1    0    1
    829   1   1   1   1   0   1   1   0   1    1    1    1    1    1    1    0    1
    830   1   1   1   1   0   1   1   1   0    1    0    1    0    0    0    1    1
    831   1   1   1   1   0   1   1   1   0    1    1    1    0    1    1    1    1
    832   1   1   1   1   0   1   1   1   0    1    1    1    0    1    1    1    1
    833   1   1   1   1   0   1   1   1   0    1    1    1    1    1    1    1    1
    834   1   1   1   1   0   1   1   1   1    0    1    1    0    1    1    1    1
    835   1   1   1   1   0   1   1   1   1    1    0    1    0    0    1    0    1
    836   1   1   1   1   0   1   1   1   1    1    0    1    0    1    1    1    1
    837   1   1   1   1   0   1   1   1   1    1    1    1    0    1    0    1    1
    838   1   1   1   1   0   1   1   1   1    1    1    1    0    1    1    1    1
    839   1   1   1   1   0   1   1   1   1    1    1    1    1    1    1    0    1
    840   1   1   1   1   0   1   1   1   1    1    1    1    1    1    1    1    1
    841   1   1   1   1   0   1   1   1   1    1    1    1    1    1    1    1    1
    842   1   1   1   1   1   0   0   0   1    0    1    1    1    1    1    0    1
    843   1   1   1   1   1   0   0   0   1    1    0    1    0    0    1    0    1
    844   1   1   1   1   1   0   0   0   1    1    1    1    0    1    1    0    1
    845   1   1   1   1   1   0   0   0   1    1    1    1    1    0    1    0    1
    846   1   1   1   1   1   0   0   1   0    1    0    1    0    0    0    1    1
    847   1   1   1   1   1   0   0   1   0    1    0    1    0    1    1    0    1
    848   1   1   1   1   1   0   0   1   0    1    1    1    0    1    0    0    1
    849   1   1   1   1   1   0   0   1   0    1    1    1    0    1    1    0    1
    850   1   1   1   1   1   0   0   1   0    1    1    1    1    1    0    0    1
    851   1   1   1   1   1   0   0   1   1    0    1    1    1    1    1    0    1
    852   1   1   1   1   1   0   0   1   1    1    0    1    0    1    0    1    1
    853   1   1   1   1   1   0   0   1   1    1    0    1    0    1    1    0    1
    854   1   1   1   1   1   0   0   1   1    1    0    1    1    0    1    1    1
    855   1   1   1   1   1   0   0   1   1    1    0    1    1    1    1    1    1
    856   1   1   1   1   1   0   0   1   1    1    1    0    0    1    1    1    1
    857   1   1   1   1   1   0   0   1   1    1    1    1    0    0    1    1    1
    858   1   1   1   1   1   0   0   1   1    1    1    1    0    1    1    0    1
    859   1   1   1   1   1   0   0   1   1    1    1    1    1    0    1    0    1
    860   1   1   1   1   1   0   0   1   1    1    1    1    1    1    0    0    1
    861   1   1   1   1   1   0   0   1   1    1    1    1    1    1    1    0    0
    862   1   1   1   1   1   0   0   1   1    1    1    1    1    1    1    0    1
    863   1   1   1   1   1   0   0   1   1    1    1    1    1    1    1    1    1
    864   1   1   1   1   1   0   1   0   1    1    1    1    1    0    1    1    1
    865   1   1   1   1   1   0   1   1   1    1    1    1    0    1    1    1    1
    866   1   1   1   1   1   0   1   1   1    1    1    1    1    1    1    1    1
    867   1   1   1   1   1   1   0   0   0    1    1    1    0    1    1    0    1
    868   1   1   1   1   1   1   0   0   1    0    1    1    0    1    1    1    1
    869   1   1   1   1   1   1   0   0   1    0    1    1    1    1    0    0    1
    870   1   1   1   1   1   1   0   0   1    1    1    1    0    1    1    1    1
    871   1   1   1   1   1   1   0   0   1    1    1    1    1    1    1    1    1
    872   1   1   1   1   1   1   0   1   0    0    0    1    1    1    1    0    1
    873   1   1   1   1   1   1   0   1   0    0    1    1    1    1    1    1    1
    874   1   1   1   1   1   1   0   1   0    1    1    1    0    0    1    0    1
    875   1   1   1   1   1   1   0   1   0    1    1    1    0    1    0    0    1
    876   1   1   1   1   1   1   0   1   0    1    1    1    0    1    0    1    1
    877   1   1   1   1   1   1   0   1   0    1    1    1    1    1    0    1    1
    878   1   1   1   1   1   1   0   1   0    1    1    1    1    1    1    0    1
    879   1   1   1   1   1   1   0   1   1    0    0    1    0    1    1    0    1
    880   1   1   1   1   1   1   0   1   1    0    1    1    0    1    1    0    1
    881   1   1   1   1   1   1   0   1   1    1    0    0    1    1    0    0    1
    882   1   1   1   1   1   1   0   1   1    1    0    1    0    1    1    0    1
    883   1   1   1   1   1   1   0   1   1    1    0    1    0    1    1    1    1
    884   1   1   1   1   1   1   0   1   1    1    0    1    1    0    1    0    1
    885   1   1   1   1   1   1   0   1   1    1    0    1    1    1    0    0    1
    886   1   1   1   1   1   1   0   1   1    1    0    1    1    1    1    0    1
    887   1   1   1   1   1   1   0   1   1    1    0    1    1    1    1    1    1
    888   1   1   1   1   1   1   0   1   1    1    1    1    0    0    1    0    1
    889   1   1   1   1   1   1   0   1   1    1    1    1    0    0    1    1    1
    890   1   1   1   1   1   1   0   1   1    1    1    1    0    1    0    0    1
    891   1   1   1   1   1   1   0   1   1    1    1    1    0    1    1    0    1
    892   1   1   1   1   1   1   0   1   1    1    1    1    0    1    1    0    1
    893   1   1   1   1   1   1   0   1   1    1    1    1    0    1    1    1    1
    894   1   1   1   1   1   1   0   1   1    1    1    1    0    1    1    1    1
    895   1   1   1   1   1   1   0   1   1    1    1    1    1    1    0    0    1
    896   1   1   1   1   1   1   0   1   1    1    1    1    1    1    1    0    1
    897   1   1   1   1   1   1   0   1   1    1    1    1    1    1    1    0    1
    898   1   1   1   1   1   1   0   1   1    1    1    1    1    1    1    1    1
    899   1   1   1   1   1   1   0   1   1    1    1    1    1    1    1    1    1
    900   1   1   1   1   1   1   1   0   0    1    1    1    0    1    1    0    1
    901   1   1   1   1   1   1   1   1   0    1    0    1    1    1    1    0    1
    902   1   1   1   1   1   1   1   1   0    1    1    1    0    0    1    0    1
    903   1   1   1   1   1   1   1   1   0    1    1    1    0    1    1    0    1
    904   1   1   1   1   1   1   1   1   0    1    1    1    1    1    1    1    1
    905   1   1   1   1   1   1   1   1   1    0    0    1    1    1    0    0    1
    906   1   1   1   1   1   1   1   1   1    0    1    1    0    0    0    0    1
    907   1   1   1   1   1   1   1   1   1    0    1    1    1    1    1    0    1
    908   1   1   1   1   1   1   1   1   1    0    1    1    1    1    1    1    1
    909   1   1   1   1   1   1   1   1   1    1    0    1    0    1    1    0    1
    910   1   1   1   1   1   1   1   1   1    1    0    1    0    1    1    0    1
    911   1   1   1   1   1   1   1   1   1    1    0    1    0    1    1    1    1
    912   1   1   1   1   1   1   1   1   1    1    0    1    1    1    1    0    1
    913   1   1   1   1   1   1   1   1   1    1    1    1    0    0    0    0    1
    914   1   1   1   1   1   1   1   1   1    1    1    1    0    0    1    0    1
    915   1   1   1   1   1   1   1   1   1    1    1    1    0    0    1    1    1
    916   1   1   1   1   1   1   1   1   1    1    1    1    0    1    1    0    1
    917   1   1   1   1   1   1   1   1   1    1    1    1    0    1    1    0    1
    918   1   1   1   1   1   1   1   1   1    1    1    1    0    1    1    1    1
    919   1   1   1   1   1   1   1   1   1    1    1    1    0    1    1    1    1
    920   1   1   1   1   1   1   1   1   1    1    1    1    1    0    0    0    1
    921   1   1   1   1   1   1   1   1   1    1    1    1    1    1    1    0    1
    922   1   1   1   1   1   1   1   1   1    1    1    1    1    1    1    1    1
    923   1   1   1   1   1   1   1   1   1    1    1    1    1    1    1    1    1
    924   1   1   1   1   1   1   1   1   1    1    1    1    1    1    1    1    1
        IT18 IT19 IT20 Obs   Exp     z1 se.z1
    1      0    0    0   2 3.256 -2.445 0.544
    2      1    0    0   2 1.118 -2.263 0.506
    3      1    1    0   1 0.294 -1.891 0.445
    4      1    1    1   1 0.097 -1.327 0.343
    5      0    1    0   1 0.057 -1.836 0.437
    6      1    0    0   1 0.009 -1.782 0.429
    7      0    0    0   1 0.261 -2.244 0.502
    8      1    0    0   1 0.008 -1.889 0.444
    9      1    1    1   1 0.002 -1.435 0.353
    10     1    0    0   1 0.032 -1.735 0.421
    11     1    0    0   1 0.022 -1.365 0.343
    12     0    1    1   1 0.049 -1.594 0.391
    13     0    0    1   1 0.010 -1.712 0.417
    14     0    1    1   1 0.013 -1.049 0.367
    15     0    1    0   1 0.008 -1.412 0.348
    16     0    0    0   1 1.455 -2.183 0.490
    17     0    0    0   1 0.372 -1.664 0.407
    18     0    1    1   1 0.170 -1.288 0.344
    19     0    1    0   1 0.053 -1.548 0.379
    20     0    0    1   1 0.007 -1.199 0.355
    21     0    0    0   2 0.187 -1.922 0.449
    22     0    1    1   1 0.059 -1.140 0.362
    23     0    1    1   1 0.002 -1.185 0.357
    24     0    0    0   1 0.254 -2.112 0.478
    25     0    1    0   1 0.084 -1.772 0.427
    26     0    0    0   1 0.017 -1.605 0.393
    27     0    0    0   1 0.018 -1.762 0.426
    28     0    0    0   2 1.576 -2.234 0.500
    29     0    1    1   1 0.194 -1.659 0.406
    30     1    0    0   1 0.617 -2.078 0.472
    31     1    0    0   1 0.198 -1.592 0.390
    32     0    0    0   1 0.041 -1.550 0.380
    33     1    1    1   1 0.019 -1.113 0.364
    34     0    1    0   1 0.052 -1.731 0.420
    35     1    1    0   1 0.011 -0.847 0.335
    36     1    0    0   1 0.384 -1.873 0.442
    37     0    0    1   1 0.201 -1.394 0.346
    38     0    1    1   1 0.193 -1.193 0.356
    39     1    1    1   1 0.147 -1.107 0.365
    40     0    0    1   1 0.035 -1.618 0.396
    41     0    1    0   1 0.079 -1.189 0.356
    42     1    0    0   1 0.055 -1.312 0.343
    43     1    1    1   1 0.064 -0.949 0.358
    44     0    0    0   1 0.017 -1.506 0.369
    45     1    1    0   1 0.039 -1.218 0.352
    46     1    0    1   1 0.006 -1.389 0.346
    47     1    0    0   1 0.007 -1.330 0.343
    48     0    1    0   1 0.003 -1.266 0.347
    49     0    0    0   1 0.071 -1.378 0.344
    50     1    1    1   1 0.058 -0.936 0.356
    51     1    0    1   1 0.007 -1.272 0.346
    52     1    0    0   1 0.003 -1.027 0.366
    53     0    1    0   1 0.003 -1.607 0.394
    54     1    0    1   1 0.002 -1.366 0.344
    55     0    1    0   1 0.001 -1.285 0.345
    56     0    0    0   1 0.265 -2.129 0.480
    57     1    1    1   1 0.005 -1.082 0.366
    58     0    0    1   1 0.010 -1.453 0.356
    59     1    1    1   1 0.017 -0.838 0.332
    60     1    0    0   1 0.002 -1.141 0.362
    61     0    1    1   1 0.001 -0.007 0.335
    62     0    0    1   1 0.066 -1.738 0.421
    63     0    0    1   1 0.044 -1.367 0.344
    64     0    0    0   1 0.009 -1.524 0.373
    65     1    0    1   1 0.001 -0.614 0.304
    66     0    0    1   1 0.052 -1.250 0.348
    67     1    1    1   1 0.003 -0.126 0.345
    68     1    0    1   1 0.003 -0.913 0.352
    69     1    1    1   1 0.002 -0.376 0.360
    70     0    1    0   1 0.014 -1.343 0.343
    71     0    1    1   1 0.014 -0.301 0.364
    72     1    1    1   1 0.008 -0.123 0.345
    73     1    0    0   1 0.038 -1.984 0.458
    74     0    1    1   1 0.001 -1.182 0.357
    75     0    1    1   1 0.001 -0.849 0.335
    76     0    0    0   1 0.002 -1.133 0.363
    77     0    1    1   1 0.001 -0.747 0.306
    78     0    0    0   1 0.039 -1.769 0.427
    79     1    0    0   1 0.001 -1.500 0.367
    80     1    0    1   1 0.000 -1.433 0.352
    81     1    1    0   1 0.000 -1.186 0.357
    82     1    1    0   1 0.003 -1.274 0.346
    83     1    1    0   1 0.049 -1.376 0.344
    84     0    1    0   1 0.016 -1.257 0.348
    85     0    0    1   1 0.116 -1.786 0.429
    86     0    0    1   1 0.009 -1.409 0.348
    87     0    1    0   1 0.000 -0.673 0.296
    88     1    1    0   1 0.008 -0.750 0.306
    89     1    1    0   1 0.021 -1.108 0.365
    90     0    1    0   1 0.008 -0.960 0.360
    91     1    0    0   1 0.003 -1.536 0.376
    92     1    1    0   1 0.001 -0.566 0.316
    93     0    0    1   1 0.007 -1.093 0.366
    94     0    0    0   1 0.105 -1.581 0.387
    95     0    1    0   1 0.001 -0.937 0.356
    96     0    1    0   1 0.001 -1.110 0.365
    97     0    0    1   3 0.091 -1.651 0.404
    98     1    0    0   1 0.014 -1.479 0.362
    99     1    1    0   1 0.013 -1.065 0.367
    100    1    1    1   1 0.021 -0.620 0.302
    101    0    1    1   1 0.016 -0.745 0.305
    102    1    1    0   1 0.009 -0.562 0.318
    103    1    1    0   1 0.001 -0.208 0.357
    104    1    1    1   1 0.000 -0.583 0.311
    105    0    1    1   1 0.008 -0.854 0.337
    106    0    1    0   1 0.002 -1.053 0.367
    107    1    0    0   1 0.002 -1.168 0.359
    108    1    1    1   1 0.001 -1.165 0.359
    109    1    1    0   1 0.000 -0.724 0.301
    110    0    0    0   1 0.560 -2.212 0.496
    111    0    0    0   1 0.001 -1.564 0.383
    112    0    0    1   1 0.003 -0.981 0.363
    113    0    1    1   1 0.017 -1.472 0.360
    114    1    0    0   1 0.141 -1.855 0.440
    115    0    0    1   1 0.012 -1.272 0.346
    116    1    0    1   1 0.008 -1.190 0.356
    117    0    1    1   1 0.013 -1.295 0.344
    118    1    1    0   1 0.009 -0.738 0.304
    119    1    1    1   1 0.004 -0.608 0.305
    120    1    1    1   1 0.011 -1.262 0.347
    121    0    0    1   1 0.120 -1.802 0.432
    122    0    1    0   1 0.084 -1.348 0.343
    123    0    1    1   1 0.012 -1.320 0.343
    124    1    0    1   1 0.012 -1.124 0.364
    125    0    1    1   1 0.005 -0.781 0.315
    126    1    1    1   1 0.005 -0.718 0.300
    127    0    1    1   1 0.007 -0.554 0.320
    128    1    1    1   1 0.000 -0.623 0.302
    129    1    1    0   1 0.006 -1.016 0.366
    130    1    1    1   1 0.008 -0.371 0.361
    131    0    0    1   1 0.007 -1.073 0.366
    132    1    1    1   1 0.007 -0.136 0.347
    133    0    1    1   1 0.006 -0.599 0.307
    134    0    0    0   1 0.001 -0.746 0.305
    135    1    1    1   1 0.005  0.097 0.347
    136    1    0    0   1 0.000 -1.408 0.348
    137    0    1    1   1 0.001 -0.736 0.303
    138    1    1    1   1 0.000 -0.798 0.320
    139    1    1    0   1 0.000 -0.638 0.299
    140    0    0    0   1 0.000 -0.896 0.348
    141    0    0    1   1 0.042 -1.809 0.433
    142    0    1    0   1 0.042 -1.716 0.417
    143    0    0    0   1 0.010 -1.849 0.439
    144    0    0    0   1 0.002 -1.203 0.354
    145    0    0    0   1 0.005 -1.346 0.343
    146    0    1    0   1 0.009 -1.045 0.367
    147    0    0    1   1 0.004 -1.233 0.350
    148    0    1    0   1 0.002 -0.824 0.328
    149    0    0    0   1 0.040 -1.341 0.343
    150    0    1    1   1 0.006 -0.681 0.296
    151    1    0    1   1 0.009 -0.945 0.358
    152    0    1    1   1 0.004 -0.345 0.363
    153    1    1    1   1 0.000 -0.119 0.344
    154    0    1    1   1 0.006 -0.790 0.318
    155    1    1    1   1 0.006 -0.487 0.339
    156    1    1    1   1 0.003  0.007 0.335
    157    0    1    0   1 0.007 -1.424 0.350
    158    1    1    1   1 0.003 -0.543 0.323
    159    0    1    0   1 0.005 -0.411 0.356
    160    0    0    1   1 0.000 -0.823 0.328
    161    0    1    1   1 0.000 -0.575 0.314
    162    1    1    1   1 0.000 -0.385 0.359
    163    0    1    1   1 0.001 -0.187 0.354
    164    0    0    1   1 0.066 -1.696 0.414
    165    1    0    0   1 0.001 -1.208 0.354
    166    0    0    0   1 0.019 -1.156 0.360
    167    1    0    1   1 0.010 -1.096 0.365
    168    0    0    0   1 0.004 -1.005 0.365
    169    0    1    0   1 0.008 -0.957 0.360
    170    1    1    1   1 0.022 -0.402 0.357
    171    1    1    1   1 0.027 -0.184 0.354
    172    0    0    1   1 0.055 -1.572 0.385
    173    0    1    0   1 0.012 -1.295 0.344
    174    0    0    1   1 0.003 -0.834 0.331
    175    0    0    1   1 0.070 -1.121 0.364
    176    0    0    0   1 0.021 -0.942 0.357
    177    1    0    0   1 0.019 -0.862 0.339
    178    1    0    0   1 0.014 -1.355 0.343
    179    1    0    0   1 0.023 -1.028 0.366
    180    1    1    1   1 0.011 -0.495 0.337
    181    0    0    1   1 0.002 -0.766 0.311
    182    0    1    1   1 0.055 -0.762 0.309
    183    1    0    1   1 0.003 -0.984 0.363
    184    1    1    1   1 0.011 -0.271 0.363
    185    1    0    1   1 0.005 -0.165 0.351
    186    1    1    1   1 0.020  0.141 0.356
    187    0    0    0   1 0.002 -0.572 0.315
    188    0    1    1   1 0.007 -0.420 0.354
    189    0    1    0   1 0.005  0.065 0.341
    190    1    1    1   1 0.005 -0.493 0.338
    191    0    1    1   1 0.001 -0.837 0.332
    192    0    0    0   1 0.021 -1.214 0.353
    193    1    1    0   1 0.004 -0.590 0.309
    194    0    0    0   1 0.000 -0.128 0.345
    195    0    1    1   1 0.002 -0.115 0.344
    196    1    0    1   1 0.001 -0.605 0.306
    197    0    0    1   1 0.003 -0.258 0.362
    198    0    1    1   1 0.009  0.028 0.337
    199    0    1    0   1 0.017 -0.533 0.326
    200    1    1    1   1 0.045 -0.310 0.364
    201    0    1    1   1 0.044 -0.179 0.353
    202    1    0    1   1 0.003 -0.425 0.353
    203    0    0    1   1 0.001 -0.516 0.331
    204    1    1    1   1 0.013  0.022 0.336
    205    1    1    1   1 0.024  0.815 0.443
    206    0    1    1   1 0.006 -0.434 0.352
    207    0    1    1   1 0.002 -0.396 0.358
    208    1    1    0   1 0.004 -0.676 0.296
    209    1    0    0   1 0.001 -0.567 0.316
    210    1    1    1   1 0.002 -0.563 0.317
    211    1    1    1   1 0.003  0.128 0.353
    212    0    1    0   1 0.000 -0.566 0.316
    213    1    1    1   1 0.004  0.267 0.384
    214    1    1    0   1 0.005  0.122 0.352
    215    1    1    1   1 0.020  0.279 0.386
    216    1    1    1   1 0.077  0.721 0.431
    217    0    1    1   1 0.004  0.086 0.345
    218    0    1    1   1 0.005 -0.912 0.351
    219    0    1    1   1 0.000 -0.924 0.354
    220    0    0    0   1 0.032 -1.604 0.393
    221    0    0    0   1 0.001 -1.056 0.367
    222    0    0    0   1 0.000 -0.474 0.343
    223    0    1    1   1 0.003  0.032 0.337
    224    0    1    0   1 0.005 -0.612 0.304
    225    0    0    1   1 0.021 -1.154 0.361
    226    0    1    0   1 0.006 -1.265 0.347
    227    0    1    1   1 0.056 -0.817 0.326
    228    1    0    0   1 0.011 -0.931 0.355
    229    1    1    1   1 0.036  0.103 0.348
    230    0    1    0   1 0.006 -0.737 0.303
    231    1    1    1   1 0.026 -0.465 0.345
    232    1    0    0   1 0.001 -0.655 0.297
    233    0    1    1   1 0.001 -0.056 0.336
    234    1    1    1   1 0.005  0.040 0.338
    235    0    1    1   1 0.006 -0.107 0.342
    236    1    1    0   1 0.001 -0.011 0.335
    237    0    1    1   1 0.017  0.244 0.379
    238    0    1    1   1 0.012  0.389 0.403
    239    0    1    0   1 0.020 -0.811 0.324
    240    0    1    1   1 0.028 -0.453 0.348
    241    1    0    1   1 0.003 -0.485 0.340
    242    1    1    1   1 0.011 -0.045 0.336
    243    0    1    1   1 0.005 -0.321 0.364
    244    1    0    0   1 0.004 -0.551 0.321
    245    1    1    1   1 0.007 -0.248 0.361
    246    0    1    1   1 0.003 -0.683 0.296
    247    1    1    1   1 0.003 -0.226 0.359
    248    1    1    1   1 0.010  0.068 0.342
    249    1    1    1   1 0.042  0.634 0.422
    250    1    1    1   1 0.000 -0.281 0.363
    251    0    1    1   1 0.001 -0.069 0.338
    252    0    0    1   1 0.000  0.006 0.335
    253    1    1    1   1 0.014  0.675 0.426
    254    1    1    1   1 0.042  0.474 0.411
    255    0    0    1   1 0.001 -0.048 0.336
    256    0    1    0   1 0.005  0.493 0.412
    257    1    1    1   1 0.000 -0.014 0.335
    258    1    1    1   1 0.078  1.259 0.514
    259    0    1    0   1 0.009 -0.849 0.335
    260    1    1    1   1 0.006 -0.985 0.363
    261    1    1    1   1 0.017 -0.548 0.322
    262    1    1    1   1 0.023 -0.454 0.347
    263    1    0    1   1 0.000 -0.794 0.319
    264    0    1    0   1 0.007 -1.211 0.353
    265    1    1    0   1 0.015 -0.802 0.321
    266    1    1    1   1 0.005  0.253 0.381
    267    1    1    1   1 0.004 -0.323 0.364
    268    0    1    1   1 0.036 -0.917 0.352
    269    1    1    1   1 0.019 -0.421 0.354
    270    1    0    1   1 0.005 -0.721 0.300
    271    0    1    1   1 0.067 -0.804 0.322
    272    0    0    0   1 0.013 -0.938 0.357
    273    1    1    0   1 0.001 -0.879 0.344
    274    1    1    1   1 0.007 -0.126 0.345
    275    0    1    1   1 0.009 -0.257 0.362
    276    1    1    1   1 0.002 -0.027 0.335
    277    1    1    1   1 0.012 -0.554 0.320
    278    1    1    1   1 0.048 -0.343 0.363
    279    0    0    1   1 0.008 -0.486 0.340
    280    0    1    1   1 0.002 -0.651 0.297
    281    0    1    0   1 0.000 -0.094 0.340
    282    1    1    1   1 0.020  0.121 0.352
    283    1    1    1   1 0.001 -0.209 0.357
    284    1    1    1   1 0.004 -0.518 0.331
    285    0    1    0   1 0.007 -0.305 0.364
    286    1    1    0   1 0.004 -0.102 0.342
    287    0    1    1   1 0.023  0.262 0.383
    288    1    1    1   1 0.004 -0.713 0.299
    289    1    1    0   1 0.005 -0.507 0.334
    290    0    1    1   1 0.003 -0.328 0.363
    291    1    1    1   1 0.044 -0.353 0.362
    292    0    1    1   1 0.024 -0.582 0.312
    293    1    1    0   1 0.016 -0.628 0.301
    294    0    0    1   1 0.002 -0.505 0.334
    295    1    1    0   1 0.018  0.126 0.353
    296    0    1    0   1 0.003 -0.496 0.337
    297    1    0    0   1 0.001 -0.565 0.317
    298    0    1    0   1 0.003 -0.433 0.352
    299    0    0    1   1 0.007 -0.483 0.340
    300    1    1    1   1 0.045  0.120 0.352
    301    1    1    1   1 0.061  0.451 0.409
    302    0    1    1   1 0.023  0.311 0.392
    303    1    1    1   1 0.037  0.372 0.401
    304    1    1    1   1 0.129  0.151 0.359
    305    1    1    1   1 0.029  0.339 0.396
    306    1    1    1   1 0.000  0.087 0.345
    307    1    1    1   1 0.006  0.149 0.358
    308    1    1    0   1 0.001 -0.040 0.335
    309    1    1    1   1 0.025  0.437 0.408
    310    0    1    1   1 0.003 -0.530 0.327
    311    1    1    1   1 0.016  0.364 0.400
    312    0    0    1   1 0.003 -0.616 0.303
    313    0    1    0   1 0.003 -0.519 0.330
    314    1    1    1   1 0.044  0.620 0.421
    315    1    1    1   1 0.002  0.149 0.358
    316    1    1    1   1 0.001 -0.280 0.363
    317    0    1    1   1 0.001  0.138 0.355
    318    1    1    1   1 0.036  0.737 0.433
    319    0    0    1   1 0.000  0.124 0.352
    320    0    1    1   1 0.007 -0.233 0.360
    321    0    1    0   1 0.010  0.077 0.343
    322    1    1    1   1 0.177  0.706 0.429
    323    1    1    1   1 0.199  0.889 0.454
    324    0    1    1   1 0.012 -0.922 0.353
    325    1    1    1   1 0.014 -0.637 0.299
    326    0    1    0   1 0.002 -0.542 0.323
    327    1    1    1   1 0.016 -0.348 0.363
    328    1    1    1   1 0.005 -0.229 0.359
    329    0    1    1   1 0.007  0.042 0.338
    330    1    1    1   1 0.012  0.132 0.354
    331    0    1    0   1 0.014 -0.265 0.362
    332    0    1    0   1 0.017 -0.116 0.344
    333    1    1    0   1 0.000 -0.266 0.362
    334    1    1    1   1 0.005  0.322 0.394
    335    0    1    0   1 0.001 -0.409 0.356
    336    1    1    0   1 0.003 -0.071 0.338
    337    0    1    0   1 0.001 -0.621 0.302
    338    1    1    0   1 0.002  0.159 0.360
    339    0    0    0   1 0.001 -0.607 0.305
    340    1    1    0   1 0.004 -0.018 0.335
    341    0    0    1   1 0.001 -0.349 0.363
    342    0    0    1   1 0.002 -0.246 0.361
    343    0    0    0   1 0.001 -0.744 0.305
    344    1    1    1   1 0.028  0.120 0.351
    345    0    0    0   1 0.000 -0.360 0.362
    346    1    1    1   1 0.081  0.095 0.346
    347    1    1    0   1 0.009 -0.192 0.355
    348    0    1    1   1 0.005 -0.249 0.361
    349    1    1    1   1 0.146  0.252 0.381
    350    0    0    1   1 0.008  0.239 0.378
    351    0    1    1   1 0.110  0.671 0.425
    352    0    1    1   1 0.009  0.539 0.415
    353    1    1    1   1 0.000 -0.109 0.343
    354    0    1    1   1 0.010  0.595 0.419
    355    0    1    0   1 0.001  0.169 0.363
    356    1    1    1   1 0.011  0.166 0.362
    357    1    1    1   1 0.050  0.252 0.381
    358    1    1    1   1 0.001  0.290 0.388
    359    0    1    1   1 0.003  0.124 0.352
    360    0    1    1   1 0.005  0.484 0.411
    361    1    1    1   1 0.002 -0.110 0.343
    362    1    1    1   1 0.005 -0.027 0.335
    363    0    1    1   1 0.006 -0.141 0.347
    364    1    1    0   1 0.004  0.320 0.393
    365    1    1    1   1 0.017  0.042 0.338
    366    0    0    1   1 0.003 -0.033 0.335
    367    1    1    1   1 0.087  0.624 0.421
    368    1    0    1   1 0.001 -0.064 0.337
    369    1    1    1   1 0.010  0.524 0.414
    370    0    1    1   1 0.010  0.227 0.376
    371    1    1    1   1 0.306  0.847 0.448
    372    1    1    0   1 0.040  0.805 0.441
    373    1    1    1   1 0.397  1.051 0.481
    374    0    1    1   1 0.025  0.618 0.421
    375    0    1    1   1 0.080  1.313 0.522
    376    1    1    1   1 0.332  1.504 0.550
    377    0    1    0   1 0.078 -1.732 0.420
    378    0    1    1   1 0.001 -1.223 0.352
    379    1    1    0   1 0.012 -1.137 0.362
    380    0    1    1   1 0.000 -0.471 0.343
    381    1    0    0   1 0.069 -1.736 0.421
    382    1    0    0   1 0.007 -1.254 0.348
    383    0    1    0   1 0.012 -1.154 0.361
    384    1    1    0   1 0.003 -0.596 0.308
    385    1    1    1   1 0.002 -0.382 0.360
    386    1    1    1   1 0.011 -0.686 0.296
    387    0    0    0   1 0.140 -1.901 0.446
    388    1    1    0   1 0.003 -0.787 0.317
    389    1    0    1   1 0.000 -1.015 0.366
    390    1    1    1   1 0.008 -0.681 0.296
    391    1    0    0   1 0.002 -1.059 0.367
    392    1    0    0   1 0.012 -0.950 0.359
    393    0    0    0   1 0.023 -1.210 0.353
    394    0    1    1   1 0.009 -0.770 0.312
    395    0    0    0   1 0.021 -1.665 0.407
    396    0    1    1   1 0.002 -0.966 0.361
    397    1    1    0   1 0.004 -0.204 0.356
    398    0    0    0   1 0.000 -1.046 0.367
    399    1    1    0   1 0.000 -0.618 0.303
    400    0    1    0   1 0.001 -1.203 0.354
    401    1    1    1   1 0.005 -0.779 0.314
    402    0    1    1   1 0.001 -0.635 0.300
    403    1    1    1   1 0.011 -0.124 0.345
    404    0    1    1   1 0.001 -0.613 0.304
    405    0    1    1   1 0.018 -0.272 0.363
    406    1    0    1   1 0.000 -0.925 0.354
    407    1    1    1   1 0.017 -0.658 0.297
    408    0    1    1   1 0.003 -0.509 0.333
    409    1    1    1   1 0.002 -0.834 0.331
    410    1    1    1   1 0.000 -0.937 0.356
    411    1    1    1   1 0.002 -0.553 0.320
    412    0    1    1   1 0.007 -0.608 0.305
    413    0    1    0   1 0.002 -0.947 0.358
    414    1    1    1   1 0.011 -0.035 0.335
    415    1    1    1   1 0.000 -0.437 0.351
    416    0    0    0   1 0.050 -1.909 0.447
    417    1    1    0   1 0.003 -0.618 0.303
    418    0    1    1   1 0.002 -0.764 0.310
    419    0    1    0   1 0.015 -0.896 0.348
    420    1    1    0   1 0.014 -0.820 0.327
    421    1    1    1   1 0.017 -0.451 0.348
    422    0    1    1   1 0.000 -0.285 0.363
    423    1    0    1   1 0.001 -0.542 0.324
    424    1    0    0   1 0.005 -1.126 0.363
    425    0    1    1   1 0.003 -0.031 0.335
    426    0    1    0   1 0.000 -1.066 0.367
    427    1    1    1   1 0.016 -0.460 0.346
    428    0    1    1   1 0.015 -0.177 0.353
    429    0    1    1   1 0.006  0.203 0.371
    430    1    1    1   1 0.011  0.021 0.336
    431    0    1    1   1 0.001 -0.594 0.309
    432    1    1    1   1 0.000 -0.465 0.345
    433    1    0    1   1 0.000  0.034 0.337
    434    1    1    0   1 0.001 -0.622 0.302
    435    1    1    1   1 0.002  0.166 0.362
    436    1    1    0   1 0.005  0.361 0.399
    437    1    1    1   1 0.009  0.307 0.391
    438    1    1    1   1 0.000 -0.279 0.363
    439    0    1    0   1 0.024 -1.590 0.390
    440    0    1    0   1 0.029 -1.137 0.362
    441    1    0    0   1 0.003 -1.172 0.358
    442    0    0    1   1 0.005 -1.371 0.344
    443    1    1    1   1 0.001 -0.824 0.328
    444    1    1    1   1 0.039 -0.810 0.324
    445    1    1    1   1 0.009  0.146 0.357
    446    1    1    0   1 0.004 -0.497 0.337
    447    1    1    1   1 0.002  0.219 0.374
    448    1    1    0   1 0.001 -0.534 0.326
    449    1    1    0   1 0.000 -0.574 0.314
    450    1    1    1   1 0.004 -0.107 0.342
    451    1    0    0   1 0.002 -0.979 0.362
    452    1    1    1   1 0.014  0.035 0.337
    453    1    0    1   1 0.000 -1.131 0.363
    454    0    1    1   1 0.001 -0.131 0.346
    455    1    0    0   1 0.000 -0.495 0.337
    456    0    1    1   1 0.002  0.224 0.375
    457    0    1    0   1 0.000 -0.140 0.347
    458    1    1    1   1 0.027  0.709 0.429
    459    1    0    1   1 0.005 -1.150 0.361
    460    0    1    0   1 0.003 -0.459 0.346
    461    1    0    1   1 0.001 -0.923 0.354
    462    1    1    1   1 0.002 -0.296 0.364
    463    1    1    0   1 0.004 -0.833 0.331
    464    1    1    1   1 0.001 -0.356 0.362
    465    1    1    1   1 0.003 -0.174 0.352
    466    1    1    1   1 0.004  0.296 0.389
    467    1    1    1   1 0.015  0.366 0.400
    468    0    1    1   1 0.002  0.207 0.371
    469    1    1    0   1 0.006 -0.178 0.353
    470    1    0    1   1 0.004  0.231 0.377
    471    0    1    1   1 0.001  0.080 0.343
    472    0    1    1   1 0.000 -0.373 0.361
    473    0    1    1   1 0.003  0.858 0.449
    474    1    1    1   1 0.002 -0.017 0.335
    475    1    1    1   1 0.041  1.297 0.519
    476    0    0    0   1 0.002 -0.831 0.330
    477    0    1    1   1 0.023 -0.704 0.298
    478    1    1    1   1 0.015  0.225 0.376
    479    1    1    1   1 0.010 -0.276 0.363
    480    0    1    1   1 0.001 -0.980 0.363
    481    1    1    1   1 0.007 -0.573 0.314
    482    0    1    1   1 0.015 -0.588 0.310
    483    0    1    1   1 0.024  0.182 0.366
    484    0    1    1   1 0.063 -0.741 0.304
    485    0    1    0   1 0.011 -0.782 0.315
    486    0    1    1   1 0.017 -0.676 0.296
    487    0    0    0   1 0.012 -0.772 0.312
    488    1    0    1   1 0.006 -0.304 0.364
    489    0    1    1   1 0.029 -0.022 0.335
    490    0    1    0   1 0.009 -0.550 0.321
    491    0    0    1   1 0.001 -1.008 0.365
    492    0    1    1   1 0.011  0.031 0.337
    493    1    1    0   1 0.010  0.159 0.360
    494    1    1    1   1 0.040 -0.212 0.357
    495    1    1    1   1 0.104  0.687 0.427
    496    1    1    1   1 0.001 -0.957 0.360
    497    0    1    1   1 0.007  0.373 0.401
    498    0    1    0   1 0.001 -0.455 0.347
    499    1    1    0   1 0.009 -0.640 0.299
    500    1    1    1   1 0.001 -0.619 0.303
    501    1    1    1   1 0.023  0.379 0.402
    502    0    1    0   1 0.001 -0.039 0.335
    503    1    1    1   1 0.001 -0.044 0.336
    504    1    1    1   1 0.062  0.596 0.419
    505    1    0    1   1 0.006 -0.148 0.348
    506    1    1    1   1 0.045  0.044 0.338
    507    1    1    1   1 0.050  0.346 0.397
    508    0    0    1   1 0.009 -0.676 0.296
    509    0    0    1   1 0.003 -0.294 0.364
    510    0    1    0   1 0.008 -0.069 0.338
    511    1    1    1   1 0.032  0.272 0.385
    512    0    1    1   1 0.021  0.201 0.370
    513    1    1    0   1 0.013  0.472 0.410
    514    1    1    1   1 0.044  0.108 0.349
    515    0    1    1   1 0.021  0.482 0.411
    516    1    1    1   1 0.036 -0.209 0.357
    517    0    0    1   1 0.003 -0.118 0.344
    518    1    1    1   1 0.015  0.085 0.344
    519    1    1    1   1 0.029 -0.271 0.363
    520    0    1    0   1 0.003  0.090 0.345
    521    1    1    1   1 0.052  0.141 0.356
    522    1    1    1   2 0.310  0.737 0.432
    523    0    1    0   1 0.016  0.579 0.418
    524    1    1    1   1 0.359  0.924 0.460
    525    0    1    1   1 0.005  0.019 0.336
    526    1    1    1   1 0.002  0.022 0.336
    527    0    1    1   1 0.001 -0.261 0.362
    528    0    1    1   1 0.017  0.109 0.349
    529    0    0    0   1 0.001 -0.428 0.353
    530    1    1    0   1 0.001 -0.200 0.356
    531    0    1    0   1 0.007  0.125 0.353
    532    1    1    1   1 0.214  0.975 0.468
    533    1    0    1   1 0.005  0.610 0.420
    534    1    1    1   2 0.209  1.146 0.496
    535    1    1    1   1 0.021  0.596 0.419
    536    1    1    0   1 0.001 -0.685 0.296
    537    1    1    1   1 0.010  0.465 0.410
    538    0    0    1   1 0.001  0.164 0.361
    539    0    1    1   1 0.015  0.412 0.405
    540    0    1    1   1 0.010  0.060 0.340
    541    1    1    1   1 0.144  0.603 0.420
    542    1    1    0   1 0.000 -0.646 0.298
    543    1    1    1   1 0.090  0.723 0.431
    544    0    1    1   1 0.017 -0.044 0.336
    545    1    1    1   1 0.039  0.353 0.398
    546    0    1    0   1 0.000  0.124 0.352
    547    0    1    1   1 0.027  0.573 0.418
    548    1    0    1   1 0.016  0.690 0.427
    549    0    1    1   1 0.009  0.322 0.394
    550    1    1    1   1 0.392  0.982 0.469
    551    1    1    1   1 0.583  1.206 0.506
    552    1    1    1   1 0.059  1.128 0.493
    553    0    1    1   1 0.104  1.101 0.489
    554    1    1    1   1 0.457  1.475 0.546
    555    1    1    1   1 0.032 -0.070 0.338
    556    1    1    0   1 0.002  0.088 0.345
    557    1    1    1   1 0.017  0.048 0.339
    558    1    1    1   1 0.027  0.210 0.372
    559    1    1    1   1 0.012  0.277 0.386
    560    1    0    1   1 0.004 -0.457 0.347
    561    1    1    1   1 0.009  0.097 0.347
    562    1    0    0   1 0.003 -0.857 0.338
    563    0    0    0   1 0.002 -0.800 0.321
    564    0    1    1   1 0.030  0.061 0.340
    565    1    1    1   1 0.059  0.443 0.408
    566    0    1    0   1 0.002  0.035 0.337
    567    0    1    1   1 0.022  0.294 0.389
    568    1    1    0   1 0.009  0.222 0.375
    569    1    1    1   1 0.002  0.547 0.416
    570    1    1    1   1 0.030  0.466 0.410
    571    1    1    1   1 0.002  0.193 0.368
    572    1    1    0   1 0.002  0.037 0.337
    573    1    1    1   1 0.021  0.591 0.419
    574    1    1    1   1 0.073  0.832 0.445
    575    0    0    0   1 0.004 -0.920 0.353
    576    1    1    1   1 0.001 -0.394 0.358
    577    1    1    1   1 0.049  0.379 0.402
    578    1    1    1   1 0.025  0.199 0.370
    579    1    1    1   1 0.074  0.233 0.377
    580    1    1    1   1 0.136  0.769 0.436
    581    0    1    1   1 0.013 -0.171 0.352
    582    1    1    1   1 0.011  0.111 0.350
    583    1    1    1   1 0.207  0.508 0.413
    584    1    0    1   1 0.002  0.117 0.351
    585    1    1    1   1 0.081  0.560 0.417
    586    0    1    0   1 0.001  0.136 0.355
    587    0    1    1   1 0.023  0.573 0.418
    588    0    1    0   1 0.004 -0.069 0.338
    589    1    1    1   1 0.040  0.403 0.404
    590    0    1    1   1 0.047  0.214 0.373
    591    0    1    1   1 0.062  0.537 0.415
    592    1    1    1   1 0.124  0.443 0.408
    593    0    1    1   1 0.138  0.249 0.381
    594    0    0    1   1 0.014  0.336 0.396
    595    0    1    1   1 0.271  0.791 0.439
    596    1    1    1   1 0.781  0.924 0.460
    597    1    1    1   1 0.081  0.569 0.417
    598    1    1    1   1 0.080  0.739 0.433
    599    1    1    1   1 0.548  0.881 0.453
    600    1    1    1   1 0.736  1.089 0.487
    601    1    1    1   1 0.001  0.287 0.388
    602    1    1    1   1 0.017  0.540 0.415
    603    0    1    1   1 0.023  0.829 0.445
    604    1    1    1   1 0.018  0.770 0.437
    605    0    1    1   1 0.002  0.304 0.391
    606    1    1    1   1 0.077  1.016 0.475
    607    1    1    0   1 0.000 -0.115 0.343
    608    1    0    1   1 0.001  0.208 0.372
    609    1    1    1   1 0.083  0.696 0.428
    610    1    1    1   1 0.510  1.338 0.526
    611    1    1    0   1 0.001  0.465 0.410
    612    1    1    1   1 0.003 -0.410 0.356
    613    0    1    0   1 0.006 -0.192 0.355
    614    0    1    1   1 0.016  0.201 0.370
    615    1    1    1   1 0.002  0.296 0.389
    616    1    1    1   1 0.188  0.880 0.453
    617    0    1    1   1 0.001  0.779 0.438
    618    1    1    1   1 0.001  0.263 0.383
    619    0    1    1   1 0.004  0.936 0.462
    620    1    1    1   1 0.002  0.260 0.383
    621    1    1    1   1 0.110  0.930 0.461
    622    1    1    1   1 0.040  0.583 0.418
    623    1    1    1   1 0.007  0.067 0.341
    624    1    1    1   1 0.042  0.581 0.418
    625    1    0    1   1 0.005  0.683 0.427
    626    0    1    1   1 0.002  0.286 0.388
    627    0    1    1   1 0.084  0.795 0.440
    628    1    1    1   1 0.212  0.552 0.416
    629    0    1    1   1 0.285  0.843 0.447
    630    1    1    1   1 1.266  1.207 0.506
    631    0    0    1   1 0.002  0.380 0.402
    632    1    1    1   1 0.839  1.154 0.497
    633    0    1    1   2 0.382  1.226 0.508
    634    1    1    1   1 1.488  1.406 0.536
    635    0    1    1   1 0.000 -0.184 0.354
    636    0    1    0   1 0.003  0.840 0.447
    637    1    1    0   1 0.001  0.204 0.371
    638    0    1    0   1 0.002  0.354 0.398
    639    1    1    1   1 0.012  0.926 0.460
    640    1    1    1   1 0.084  0.726 0.431
    641    1    1    1   2 0.527  1.214 0.507
    642    1    1    1   1 0.993  1.475 0.546
    643    0    1    0   1 0.001  0.411 0.405
    644    1    1    0   1 0.004  0.755 0.435
    645    1    1    1   1 0.623  1.414 0.537
    646    1    1    1   1 1.440  1.709 0.581
    647    0    0    0   1 0.002 -1.361 0.343
    648    1    1    0   1 0.001 -0.537 0.325
    649    0    1    1   1 0.021  0.126 0.353
    650    0    0    0   1 0.000 -0.340 0.363
    651    0    1    1   1 0.038  0.077 0.343
    652    0    1    1   1 0.047 -0.245 0.361
    653    1    1    0   1 0.019  0.008 0.335
    654    0    1    0   1 0.002 -0.233 0.360
    655    1    0    0   1 0.002 -0.270 0.363
    656    1    1    0   1 0.019 -0.592 0.309
    657    1    1    1   1 0.037  0.259 0.383
    658    1    1    1   1 0.011  0.086 0.345
    659    1    1    1   2 0.207  0.190 0.368
    660    0    1    1   1 0.071  0.229 0.376
    661    1    1    1   1 0.041  0.073 0.342
    662    1    1    1   1 0.111  0.295 0.389
    663    0    1    0   1 0.003  0.175 0.364
    664    0    1    1   1 0.024 -0.129 0.346
    665    0    1    0   1 0.001 -0.046 0.336
    666    1    1    1   1 0.021  0.153 0.359
    667    1    1    1   1 0.003  0.182 0.366
    668    1    1    1   1 0.060  0.576 0.418
    669    1    0    0   1 0.003 -0.851 0.336
    670    1    1    1   1 0.019  0.005 0.335
    671    1    1    0   1 0.010 -0.184 0.354
    672    0    1    1   1 0.067 -0.040 0.335
    673    0    1    1   1 0.009 -0.125 0.345
    674    1    1    1   1 0.079 -0.019 0.335
    675    0    1    1   1 0.102  0.096 0.347
    676    1    1    1   1 0.291  0.530 0.415
    677    0    1    1   1 0.111  0.579 0.418
    678    1    1    0   1 0.014  0.253 0.381
    679    0    1    0   1 0.012  0.506 0.413
    680    1    1    0   1 0.007 -0.267 0.362
    681    1    1    1   1 0.004  0.070 0.342
    682    1    1    0   1 0.016  0.073 0.342
    683    1    1    1   1 0.115  0.582 0.418
    684    1    1    1   1 0.078 -0.008 0.335
    685    1    1    1   1 0.125  0.331 0.395
    686    1    1    1   1 0.099  0.299 0.390
    687    1    0    0   1 0.003 -0.517 0.331
    688    1    1    1   1 0.154  0.628 0.422
    689    1    1    1   2 0.974  0.761 0.435
    690    0    1    1   1 0.396  0.815 0.443
    691    0    1    1   1 0.001  0.481 0.411
    692    1    1    1   2 1.115  1.120 0.492
    693    1    1    0   1 0.000  0.420 0.406
    694    1    1    1   1 0.100  1.168 0.499
    695    1    1    1   1 0.011  0.834 0.446
    696    1    1    1   1 0.012 -0.044 0.336
    697    1    1    1   1 0.038  0.492 0.412
    698    1    1    1   1 0.026  0.794 0.440
    699    1    1    0   1 0.005 -0.040 0.335
    700    1    1    1   1 0.120  0.720 0.430
    701    1    1    1   1 0.137  0.905 0.457
    702    0    1    1   1 0.004  0.158 0.360
    703    1    1    1   1 0.479  0.957 0.465
    704    1    1    1   3 0.695  1.178 0.501
    705    1    1    0   1 0.000  0.086 0.345
    706    1    1    1   1 0.463  1.126 0.493
    707    1    1    1   3 0.798  1.374 0.531
    708    1    1    1   1 0.006  0.745 0.433
    709    1    1    0   1 0.003 -0.239 0.360
    710    0    1    1   1 0.000 -0.684 0.296
    711    0    1    0   1 0.002 -0.572 0.315
    712    0    1    0   1 0.001 -0.440 0.350
    713    0    0    0   1 0.001 -0.454 0.347
    714    0    1    1   1 0.012  0.327 0.395
    715    1    1    1   1 0.069  0.659 0.424
    716    1    1    1   1 0.032  0.898 0.456
    717    1    1    1   1 0.130  0.370 0.401
    718    1    1    1   1 0.334  0.760 0.435
    719    1    1    1   1 0.397  0.951 0.464
    720    0    1    0   1 0.013  0.734 0.432
    721    1    1    1   1 0.382  1.119 0.492
    722    1    1    1   1 0.006  0.694 0.428
    723    1    1    1   1 0.001  0.335 0.396
    724    1    1    1   1 0.006  0.282 0.387
    725    1    0    0   1 0.000  0.198 0.370
    726    0    1    1   1 0.006  0.364 0.400
    727    1    1    1   1 0.039  0.701 0.428
    728    1    1    1   1 0.016  0.758 0.435
    729    1    1    1   1 0.032  0.840 0.447
    730    1    1    1   1 0.004  0.234 0.377
    731    0    1    1   1 0.001  0.213 0.373
    732    1    1    1   1 0.043  0.737 0.433
    733    1    1    1   1 0.280  1.229 0.509
    734    1    1    1   1 0.024  0.700 0.428
    735    0    1    1   1 0.008 -0.193 0.355
    736    1    1    1   1 0.022  0.438 0.408
    737    1    1    1   1 0.019  0.218 0.374
    738    1    0    1   1 0.001 -0.348 0.363
    739    1    1    1   1 0.148  1.175 0.501
    740    1    1    1   1 0.006  0.101 0.348
    741    1    1    1   1 0.116  0.374 0.401
    742    1    1    1   2 0.359  0.956 0.465
    743    1    1    1   1 0.347  1.125 0.493
    744    1    0    1   1 0.002  0.373 0.401
    745    1    1    1   6 1.948  1.240 0.511
    746    0    1    1   1 0.086  0.759 0.435
    747    1    1    1   2 1.282  1.186 0.502
    748    0    1    1   1 0.589  1.259 0.514
    749    1    1    1   5 2.350  1.443 0.541
    750    1    1    1   1 0.225  1.502 0.550
    751    1    1    1   1 0.000 -0.172 0.352
    752    1    1    1   1 0.137  0.941 0.463
    753    1    1    1   1 0.016  0.943 0.463
    754    1    1    1   1 0.122  0.750 0.434
    755    1    1    1   2 1.582  1.514 0.552
    756    0    1    1   1 0.246  1.266 0.515
    757    1    1    1   1 2.364  1.752 0.588
    758    0    1    1   1 0.006  0.163 0.361
    759    1    1    1   1 0.033  0.477 0.411
    760    1    1    1   1 0.009 -0.078 0.339
    761    1    1    1   1 0.022  0.604 0.420
    762    0    1    1   1 0.025 -0.036 0.335
    763    0    1    1   1 0.038  0.100 0.347
    764    0    1    1   1 0.049  0.424 0.406
    765    1    1    1   1 0.004 -0.178 0.353
    766    1    1    1   1 0.030 -0.531 0.327
    767    1    1    1   1 0.004 -0.133 0.346
    768    1    1    1   1 0.030 -0.327 0.363
    769    1    1    1   1 0.077  0.260 0.383
    770    1    1    1   1 0.010 -0.018 0.335
    771    0    1    0   1 0.018  0.173 0.364
    772    1    0    1   1 0.007  0.299 0.390
    773    1    1    1   1 0.045  0.627 0.422
    774    1    1    0   1 0.013  0.060 0.340
    775    0    1    1   1 0.084  0.356 0.399
    776    1    1    1   1 0.119  0.591 0.419
    777    1    1    1   1 0.019  0.061 0.340
    778    0    1    1   1 0.053 -0.003 0.335
    779    1    1    1   1 0.241  0.296 0.389
    780    1    1    1   2 0.564  0.852 0.448
    781    1    0    1   1 0.005 -0.034 0.335
    782    1    0    1   1 0.004  0.242 0.379
    783    1    1    1   1 0.119  0.572 0.418
    784    0    1    1   1 0.002  0.132 0.354
    785    1    1    1   1 0.059  0.960 0.466
    786    1    1    1   1 0.006  0.210 0.372
    787    0    1    1   1 0.097  0.916 0.458
    788    1    1    1   1 0.305  1.062 0.483
    789    1    1    1   1 0.208  1.014 0.475
    790    1    1    1   1 0.030  0.481 0.411
    791    1    0    1   1 0.001  0.328 0.395
    792    0    1    1   1 0.017 -0.099 0.341
    793    1    1    1   2 0.088  0.690 0.427
    794    1    1    1   1 0.023  0.428 0.407
    795    0    0    1   1 0.000 -0.760 0.309
    796    0    1    1   1 0.001 -0.046 0.336
    797    1    1    1   1 0.351  1.110 0.490
    798    1    1    1   1 0.024  0.006 0.335
    799    1    1    1   1 0.040  0.788 0.439
    800    1    1    0   1 0.018 -0.210 0.357
    801    1    1    1   1 0.053 -0.071 0.338
    802    1    1    1   1 0.113  0.122 0.352
    803    1    1    1   1 0.154  0.453 0.409
    804    1    1    1   1 0.057  0.219 0.374
    805    1    0    1   1 0.004  0.454 0.409
    806    0    1    1   1 0.075  0.328 0.395
    807    0    1    1   1 0.174  0.711 0.429
    808    0    1    0   1 0.013  0.135 0.355
    809    1    1    0   1 0.005  0.594 0.419
    810    1    0    1   1 0.005  0.716 0.430
    811    1    1    0   1 0.019  0.814 0.443
    812    1    1    1   1 0.014  0.045 0.338
    813    1    0    1   1 0.019  0.075 0.343
    814    1    1    1   1 0.162  0.451 0.409
    815    1    1    1   1 0.455  1.014 0.475
    816    0    1    0   1 0.033  0.348 0.398
    817    1    1    1   2 0.462  0.657 0.424
    818    0    1    0   1 0.083  0.565 0.417
    819    1    1    0   1 0.204  0.683 0.427
    820    1    1    1   1 1.757  0.908 0.457
    821    1    1    0   1 0.224  0.864 0.450
    822    1    1    1   1 2.424  1.120 0.492
    823    0    1    1   1 0.518  0.923 0.460
    824    1    1    1   2 1.635  1.071 0.484
    825    1    1    1   1 2.663  1.309 0.521
    826    1    1    1   1 0.011  0.650 0.424
    827    1    1    1   1 0.013  0.484 0.411
    828    0    1    1   1 0.015  0.594 0.419
    829    1    1    1   1 0.146  1.117 0.491
    830    1    1    0   1 0.002  0.015 0.336
    831    0    1    1   1 0.081  0.900 0.456
    832    1    1    0   1 0.025  0.801 0.441
    833    1    1    1   1 0.261  1.225 0.508
    834    1    1    1   1 0.116  1.116 0.491
    835    1    1    1   1 0.032  0.553 0.416
    836    0    1    1   1 0.088  0.921 0.459
    837    0    1    1   1 0.075  0.714 0.430
    838    1    1    1   1 1.736  1.374 0.531
    839    1    1    1   1 1.111  1.317 0.522
    840    0    1    1   1 0.530  1.395 0.534
    841    1    1    1   1 2.325  1.595 0.564
    842    1    1    1   1 0.009  0.706 0.429
    843    0    1    1   1 0.004  0.175 0.364
    844    1    1    1   1 0.106  0.753 0.434
    845    0    1    1   1 0.005  0.470 0.410
    846    1    0    1   1 0.001 -0.157 0.350
    847    0    1    1   1 0.025  0.344 0.397
    848    1    1    0   1 0.017  0.126 0.353
    849    1    1    1   1 0.151  0.659 0.424
    850    0    1    1   1 0.020  0.295 0.389
    851    1    1    1   1 0.050  0.855 0.449
    852    0    1    0   1 0.006  0.174 0.364
    853    1    1    0   1 0.024  0.474 0.411
    854    0    1    1   1 0.008  0.561 0.417
    855    1    1    1   1 0.156  1.013 0.475
    856    1    1    1   1 0.024  0.610 0.420
    857    1    1    1   1 0.086  0.763 0.436
    858    1    1    1   1 0.602  0.907 0.457
    859    0    1    1   1 0.025  0.604 0.420
    860    0    1    1   1 0.050  0.505 0.413
    861    1    1    1   1 0.002  0.559 0.417
    862    1    1    1   1 0.560  1.070 0.484
    863    1    1    1   1 0.911  1.308 0.521
    864    1    1    0   1 0.001  0.720 0.431
    865    1    1    1   2 0.594  1.373 0.531
    866    1    1    1   1 0.795  1.594 0.564
    867    1    1    0   1 0.013  0.533 0.415
    868    1    1    1   1 0.047  0.992 0.471
    869    0    1    0   1 0.001  0.234 0.377
    870    1    1    1   2 0.610  1.229 0.509
    871    0    1    0   1 0.015  0.971 0.468
    872    1    1    1   1 0.012  0.618 0.421
    873    1    1    1   1 0.054  1.043 0.479
    874    1    1    1   1 0.073  0.578 0.418
    875    1    1    1   1 0.150  0.480 0.411
    876    1    1    1   1 0.135  0.644 0.423
    877    1    1    0   1 0.014  0.570 0.417
    878    1    1    1   1 0.463  1.053 0.481
    879    0    1    0   1 0.004  0.402 0.404
    880    1    1    1   1 0.222  0.956 0.465
    881    0    1    1   1 0.005  0.131 0.354
    882    1    1    0   1 0.063  0.687 0.427
    883    0    1    1   1 0.231  0.973 0.468
    884    0    1    1   1 0.022  0.608 0.420
    885    0    1    1   1 0.044  0.509 0.413
    886    1    1    1   1 0.509  1.076 0.485
    887    0    1    1   1 0.226  1.144 0.496
    888    0    1    1   1 0.098  0.690 0.427
    889    0    1    1   1 0.108  0.871 0.451
    890    1    1    1   1 0.468  0.706 0.429
    891    0    1    1   1 0.822  1.028 0.477
    892    1    1    1   5 2.788  1.186 0.502
    893    0    1    1   4 1.281  1.259 0.514
    894    1    1    1   2 5.108  1.444 0.541
    895    1    1    0   1 0.046  0.630 0.422
    896    0    1    1   1 0.840  1.205 0.505
    897    1    1    1   2 3.225  1.384 0.532
    898    0    1    1   4 1.568  1.465 0.545
    899    1    1    1   8 7.225  1.673 0.576
    900    1    1    0   1 0.005  0.705 0.429
    901    1    1    1   1 0.054  1.003 0.473
    902    0    1    1   1 0.012  0.633 0.422
    903    1    1    1   1 0.284  1.109 0.490
    904    1    1    1   1 0.633  1.571 0.560
    905    0    1    1   1 0.002  0.506 0.413
    906    1    1    1   1 0.004  0.413 0.405
    907    1    1    1   1 0.154  1.379 0.532
    908    1    1    1   2 0.344  1.668 0.575
    909    0    1    1   1 0.096  0.979 0.469
    910    1    1    0   1 0.029  0.874 0.452
    911    1    1    1   1 0.544  1.380 0.532
    912    1    1    1   1 0.348  1.323 0.523
    913    0    1    1   1 0.014  0.467 0.410
    914    1    1    1   1 0.136  1.019 0.476
    915    1    1    1   1 0.211  1.250 0.512
    916    0    1    1   1 0.534  1.267 0.515
    917    1    1    1   4 2.140  1.452 0.542
    918    0    1    1   3 1.062  1.537 0.555
    919    1    1    1   4 5.140  1.753 0.588
    920    1    1    1   1 0.023  0.713 0.430
    921    1    1    1   5 3.046  1.682 0.577
    922    0    1    1   1 1.615  1.779 0.592
    923    1    1    0   1 0.331  1.613 0.567
    924    1    1    1  10 9.271  2.025 0.628

The ltm package doesn’t allow for storing the thetas so we will use a
work around using the mirt package

``` r
twopl$theta <- fscores(mirt(twopl, 1,itemtype='2PL'))
```


    Iteration: 1, Log-Lik: -11111.818, Max-Change: 0.83540
    Iteration: 2, Log-Lik: -10901.119, Max-Change: 0.38992
    Iteration: 3, Log-Lik: -10870.437, Max-Change: 0.18239
    Iteration: 4, Log-Lik: -10860.796, Max-Change: 0.11287
    Iteration: 5, Log-Lik: -10857.172, Max-Change: 0.06837
    Iteration: 6, Log-Lik: -10855.450, Max-Change: 0.05071
    Iteration: 7, Log-Lik: -10853.983, Max-Change: 0.02214
    Iteration: 8, Log-Lik: -10853.850, Max-Change: 0.01295
    Iteration: 9, Log-Lik: -10853.771, Max-Change: 0.00785
    Iteration: 10, Log-Lik: -10853.676, Max-Change: 0.00301
    Iteration: 11, Log-Lik: -10853.659, Max-Change: 0.00307
    Iteration: 12, Log-Lik: -10853.648, Max-Change: 0.00245
    Iteration: 13, Log-Lik: -10853.621, Max-Change: 0.00278
    Iteration: 14, Log-Lik: -10853.619, Max-Change: 0.00124
    Iteration: 15, Log-Lik: -10853.617, Max-Change: 0.00121
    Iteration: 16, Log-Lik: -10853.615, Max-Change: 0.00054
    Iteration: 17, Log-Lik: -10853.615, Max-Change: 0.00058
    Iteration: 18, Log-Lik: -10853.614, Max-Change: 0.00042
    Iteration: 19, Log-Lik: -10853.614, Max-Change: 0.00038
    Iteration: 20, Log-Lik: -10853.614, Max-Change: 0.00032
    Iteration: 21, Log-Lik: -10853.614, Max-Change: 0.00031
    Iteration: 22, Log-Lik: -10853.614, Max-Change: 0.00027
    Iteration: 23, Log-Lik: -10853.614, Max-Change: 0.00026
    Iteration: 24, Log-Lik: -10853.614, Max-Change: 0.00023
    Iteration: 25, Log-Lik: -10853.614, Max-Change: 0.00022
    Iteration: 26, Log-Lik: -10853.614, Max-Change: 0.00020
    Iteration: 27, Log-Lik: -10853.614, Max-Change: 0.00019
    Iteration: 28, Log-Lik: -10853.614, Max-Change: 0.00017
    Iteration: 29, Log-Lik: -10853.614, Max-Change: 0.00017
    Iteration: 30, Log-Lik: -10853.614, Max-Change: 0.00015
    Iteration: 31, Log-Lik: -10853.614, Max-Change: 0.00015
    Iteration: 32, Log-Lik: -10853.614, Max-Change: 0.00013
    Iteration: 33, Log-Lik: -10853.614, Max-Change: 0.00013
    Iteration: 34, Log-Lik: -10853.614, Max-Change: 0.00012
    Iteration: 35, Log-Lik: -10853.614, Max-Change: 0.00011
    Iteration: 36, Log-Lik: -10853.614, Max-Change: 0.00053
    Iteration: 37, Log-Lik: -10853.614, Max-Change: 0.00049
    Iteration: 38, Log-Lik: -10853.614, Max-Change: 0.00040
    Iteration: 39, Log-Lik: -10853.614, Max-Change: 0.00032
    Iteration: 40, Log-Lik: -10853.614, Max-Change: 0.00007

Person fit statistics for the 2-PL so that we can assess for any
aberrant response patterns ( p \< .01)

``` r
person.fit(two_pl)
```


    Person-Fit Statistics and P-values

    Call:
    ltm(formula = twopl ~ z1, IRT.param = TRUE)

    Alternative: Inconsistent response pattern under the estimated model

        IT1 IT2 IT3 IT4 IT5 IT6 IT7 IT8 IT9 IT10 IT11 IT12 IT13 IT14 IT15 IT16 IT17
    1     0   0   0   0   0   0   0   0   0    0    0    0    0    0    0    0    0
    2     0   0   0   0   0   0   0   0   0    0    0    0    0    0    0    0    0
    3     0   0   0   0   0   0   0   0   0    0    0    0    0    0    0    0    0
    4     0   0   0   0   0   0   0   0   0    0    0    0    0    0    0    0    1
    5     0   0   0   0   0   0   0   0   0    0    0    0    0    0    0    1    0
    6     0   0   0   0   0   0   0   0   0    0    0    0    0    0    1    0    0
    7     0   0   0   0   0   0   0   0   0    0    0    0    1    0    0    0    0
    8     0   0   0   0   0   0   0   0   0    0    0    0    1    0    0    1    0
    9     0   0   0   0   0   0   0   0   0    0    0    0    1    0    0    1    0
    10    0   0   0   0   0   0   0   0   0    0    0    1    0    0    0    0    0
    11    0   0   0   0   0   0   0   0   0    0    0    1    0    0    0    0    1
    12    0   0   0   0   0   0   0   0   0    0    1    0    0    0    0    0    0
    13    0   0   0   0   0   0   0   0   0    0    1    0    0    0    0    1    0
    14    0   0   0   0   0   0   0   0   0    0    1    0    0    1    0    0    1
    15    0   0   0   0   0   0   0   0   0    0    1    1    0    0    0    0    0
    16    0   0   0   0   0   0   0   0   0    1    0    0    0    0    0    0    0
    17    0   0   0   0   0   0   0   0   0    1    0    0    0    0    0    0    1
    18    0   0   0   0   0   0   0   0   0    1    0    0    0    0    0    0    1
    19    0   0   0   0   0   0   0   0   0    1    0    0    0    1    0    0    0
    20    0   0   0   0   0   0   0   0   0    1    0    0    1    1    0    0    1
    21    0   0   0   0   0   0   0   0   0    1    1    0    0    0    0    0    0
    22    0   0   0   0   0   0   0   0   0    1    1    0    0    0    0    0    1
    23    0   0   0   0   0   0   0   0   0    1    1    0    0    0    1    0    0
    24    0   0   0   0   0   0   0   0   1    0    0    0    0    0    0    0    0
    25    0   0   0   0   0   0   0   0   1    0    0    0    0    0    0    0    0
    26    0   0   0   0   0   0   0   0   1    1    0    0    0    1    0    0    0
    27    0   0   0   0   0   0   0   0   1    1    0    0    1    0    0    0    0
    28    0   0   0   0   0   0   0   1   0    0    0    0    0    0    0    0    0
    29    0   0   0   0   0   0   0   1   0    0    0    0    0    0    0    0    0
    30    0   0   0   0   0   0   0   1   0    0    0    0    0    0    0    0    0
    31    0   0   0   0   0   0   0   1   0    0    0    0    0    0    0    0    1
    32    0   0   0   0   0   0   0   1   0    0    0    0    0    0    0    1    1
    33    0   0   0   0   0   0   0   1   0    0    0    0    0    0    0    1    1
    34    0   0   0   0   0   0   0   1   0    0    0    0    1    0    0    0    0
    35    0   0   0   0   0   0   0   1   0    0    0    1    0    1    0    0    1
    36    0   0   0   0   0   0   0   1   0    1    0    0    0    0    0    0    0
    37    0   0   0   0   0   0   0   1   0    1    0    0    0    0    0    0    1
    38    0   0   0   0   0   0   0   1   0    1    0    0    0    0    0    0    1
    39    0   0   0   0   0   0   0   1   0    1    0    0    0    0    0    0    1
    40    0   0   0   0   0   0   0   1   0    1    0    0    0    0    0    1    0
    41    0   0   0   0   0   0   0   1   0    1    1    0    0    0    0    0    1
    42    0   0   0   0   0   0   0   1   0    1    1    0    0    0    0    0    1
    43    0   0   0   0   0   0   0   1   0    1    1    0    0    0    0    0    1
    44    0   0   0   0   0   0   0   1   0    1    1    0    0    1    0    0    0
    45    0   0   0   0   0   0   0   1   1    0    0    0    0    0    0    0    1
    46    0   0   0   0   0   0   0   1   1    0    0    0    0    1    0    0    0
    47    0   0   0   0   0   0   0   1   1    0    0    0    1    0    0    0    1
    48    0   0   0   0   0   0   0   1   1    0    1    0    0    1    0    0    0
    49    0   0   0   0   0   0   0   1   1    1    0    0    0    0    0    0    1
    50    0   0   0   0   0   0   0   1   1    1    0    0    0    0    0    0    1
    51    0   0   0   0   0   0   0   1   1    1    0    0    0    1    0    0    0
    52    0   0   0   0   0   0   0   1   1    1    1    0    0    0    0    1    1
    53    0   0   0   0   0   0   1   0   1    0    0    0    0    0    0    0    0
    54    0   0   0   0   0   0   1   1   0    1    1    0    0    0    0    0    0
    55    0   0   0   0   0   0   1   1   1    1    0    0    1    0    0    0    0
    56    0   0   0   0   0   1   0   0   0    0    0    0    0    0    0    0    0
    57    0   0   0   0   0   1   0   0   0    0    0    0    1    0    0    0    1
    58    0   0   0   0   0   1   0   0   0    1    0    0    0    1    0    0    0
    59    0   0   0   0   0   1   0   0   0    1    0    0    0    1    0    0    1
    60    0   0   0   0   0   1   0   0   0    1    1    0    0    0    0    1    1
    61    0   0   0   0   0   1   0   0   1    1    1    1    0    1    1    1    1
    62    0   0   0   0   0   1   0   1   0    0    0    0    0    0    0    0    0
    63    0   0   0   0   0   1   0   1   0    0    0    0    0    0    0    0    1
    64    0   0   0   0   0   1   0   1   0    0    0    1    0    0    0    0    0
    65    0   0   0   0   0   1   0   1   0    0    1    1    0    0    1    0    1
    66    0   0   0   0   0   1   0   1   0    1    0    0    0    0    0    0    1
    67    0   0   0   0   0   1   0   1   0    1    0    1    0    1    1    1    1
    68    0   0   0   0   0   1   0   1   0    1    1    0    1    0    0    0    1
    69    0   0   0   0   0   1   0   1   1    0    0    1    0    1    0    1    1
    70    0   0   0   0   0   1   0   1   1    1    0    0    0    0    0    0    0
    71    0   0   0   0   0   1   0   1   1    1    1    1    0    1    0    0    1
    72    0   0   0   0   0   1   0   1   1    1    1    1    1    1    0    0    1
    73    0   0   0   0   1   0   0   0   0    0    0    0    0    0    0    0    0
    74    0   0   0   0   1   0   0   0   0    1    0    0    0    0    1    0    0
    75    0   0   0   0   1   0   0   0   0    1    0    0    0    0    1    0    1
    76    0   0   0   0   1   0   0   0   0    1    1    0    0    1    0    0    1
    77    0   0   0   0   1   0   0   1   0    0    0    1    0    0    0    1    1
    78    0   0   0   0   1   0   0   1   0    1    0    0    0    0    0    0    0
    79    0   0   0   0   1   0   0   1   1    0    0    0    1    0    0    0    0
    80    0   0   0   0   1   1   0   0   0    0    0    0    0    0    0    1    0
    81    0   0   0   0   1   1   0   0   1    0    1    0    0    0    0    0    0
    82    0   0   0   0   1   1   0   1   0    1    0    0    0    0    0    0    0
    83    0   0   0   1   0   0   0   0   0    0    0    0    0    0    0    0    1
    84    0   0   0   1   0   0   0   0   0    0    0    0    0    1    0    0    1
    85    0   0   0   1   0   0   0   0   0    1    0    0    0    0    0    0    0
    86    0   0   0   1   0   0   0   0   0    1    0    1    0    0    0    0    0
    87    0   0   0   1   0   0   0   0   0    1    0    1    0    0    1    1    1
    88    0   0   0   1   0   0   0   0   0    1    0    1    0    1    0    0    1
    89    0   0   0   1   0   0   0   0   0    1    1    0    0    0    0    0    1
    90    0   0   0   1   0   0   0   0   1    1    0    0    0    1    0    0    1
    91    0   0   0   1   0   0   0   0   1    1    0    0    1    0    0    0    0
    92    0   0   0   1   0   0   0   0   1    1    0    1    1    1    0    0    1
    93    0   0   0   1   0   0   0   0   1    1    1    0    0    0    0    0    1
    94    0   0   0   1   0   0   0   1   0    0    0    0    0    0    0    0    1
    95    0   0   0   1   0   0   0   1   0    0    0    0    1    1    0    1    1
    96    0   0   0   1   0   0   0   1   0    0    1    1    0    0    0    1    0
    97    0   0   0   1   0   0   0   1   0    1    0    0    0    0    0    0    0
    98    0   0   0   1   0   0   0   1   0    1    0    0    0    1    0    0    0
    99    0   0   0   1   0   0   0   1   0    1    0    0    1    0    0    0    1
    100   0   0   0   1   0   0   0   1   0    1    1    1    0    0    0    0    1
    101   0   0   0   1   0   0   0   1   1    1    0    0    0    1    0    0    1
    102   0   0   0   1   0   0   0   1   1    1    0    1    0    1    0    0    1
    103   0   0   0   1   0   0   0   1   1    1    1    1    1    1    0    1    1
    104   0   0   0   1   0   1   0   0   1    0    1    1    0    0    1    0    0
    105   0   0   0   1   0   1   0   1   0    0    0    0    0    1    0    0    1
    106   0   0   0   1   0   1   0   1   0    1    1    0    0    1    0    0    0
    107   0   0   0   1   1   0   0   0   0    1    1    0    0    0    0    0    1
    108   0   0   0   1   1   0   0   1   0    0    1    0    0    0    0    0    0
    109   0   0   0   1   1   1   0   1   0    0    0    0    0    0    1    0    1
    110   0   0   1   0   0   0   0   0   0    0    0    0    0    0    0    0    0
    111   0   0   1   0   0   0   0   0   0    0    0    0    1    1    0    1    0
    112   0   0   1   0   0   0   0   0   0    0    0    1    0    1    0    0    1
    113   0   0   1   0   0   0   0   0   0    0    1    0    0    0    0    0    0
    114   0   0   1   0   0   0   0   0   0    1    0    0    0    0    0    0    0
    115   0   0   1   0   0   0   0   0   0    1    0    0    0    0    0    1    1
    116   0   0   1   0   0   0   0   0   0    1    0    0    0    0    0    1    1
    117   0   0   1   0   0   0   0   0   0    1    0    0    0    1    0    0    0
    118   0   0   1   0   0   0   0   0   0    1    0    1    0    1    0    0    1
    119   0   0   1   0   0   0   0   0   1    0    0    1    0    1    0    0    1
    120   0   0   1   0   0   0   0   0   1    1    0    0    0    0    0    0    0
    121   0   0   1   0   0   0   0   1   0    0    0    0    0    0    0    0    0
    122   0   0   1   0   0   0   0   1   0    0    0    0    0    0    0    0    1
    123   0   0   1   0   0   0   0   1   0    0    0    0    0    1    0    0    0
    124   0   0   1   0   0   0   0   1   0    0    0    0    0    1    0    0    1
    125   0   0   1   0   0   0   0   1   0    0    0    1    0    0    0    1    1
    126   0   0   1   0   0   0   0   1   0    0    0    1    0    0    0    1    1
    127   0   0   1   0   0   0   0   1   0    1    0    1    1    1    0    0    1
    128   0   0   1   0   0   0   1   0   1    1    1    1    0    0    0    1    0
    129   0   0   1   0   0   0   1   1   0    1    0    0    0    0    0    0    1
    130   0   0   1   0   0   1   0   0   0    1    1    1    0    1    0    0    1
    131   0   0   1   0   0   1   0   0   1    1    0    0    0    0    0    0    1
    132   0   0   1   0   0   1   0   1   0    1    0    1    0    1    1    0    1
    133   0   0   1   0   0   1   0   1   0    1    0    1    1    0    0    0    1
    134   0   0   1   0   0   1   0   1   0    1    1    1    1    0    0    0    1
    135   0   0   1   0   0   1   0   1   1    1    0    1    1    1    1    0    1
    136   0   0   1   0   0   1   1   0   0    0    0    0    1    0    0    1    0
    137   0   0   1   0   0   1   1   0   0    0    0    1    0    0    0    0    1
    138   0   0   1   0   0   1   1   1   1    1    0    0    1    0    0    0    0
    139   0   0   1   0   1   1   0   0   1    1    0    1    0    1    0    0    0
    140   0   0   1   0   1   1   1   1   0    1    1    0    0    0    1    0    0
    141   0   0   1   1   0   0   0   0   0    0    0    0    0    0    0    0    0
    142   0   0   1   1   0   0   0   0   0    0    0    0    0    0    0    0    0
    143   0   0   1   1   0   0   0   0   0    0    0    0    0    0    0    1    0
    144   0   0   1   1   0   0   0   0   0    1    1    0    1    0    0    0    1
    145   0   0   1   1   0   0   0   1   0    0    0    0    0    0    0    1    1
    146   0   0   1   1   0   0   0   1   0    0    0    0    0    1    0    0    1
    147   0   0   1   1   0   0   0   1   0    0    0    0    1    0    0    0    1
    148   0   0   1   1   0   0   0   1   0    0    0    1    0    0    0    1    1
    149   0   0   1   1   0   0   0   1   0    1    0    0    0    0    0    0    1
    150   0   0   1   1   0   0   0   1   1    0    0    1    0    0    0    0    1
    151   0   0   1   1   0   0   0   1   1    1    0    0    0    0    0    0    1
    152   0   0   1   1   0   0   0   1   1    1    1    1    0    0    0    1    1
    153   0   0   1   1   0   0   0   1   1    1    1    1    1    1    1    0    0
    154   0   0   1   1   0   1   0   0   0    1    1    0    0    0    0    0    1
    155   0   0   1   1   0   1   0   0   0    1    1    1    0    0    0    0    1
    156   0   0   1   1   0   1   0   0   1    1    0    1    0    1    1    0    1
    157   0   0   1   1   0   1   0   1   0    0    0    0    0    0    0    0    0
    158   0   0   1   1   0   1   0   1   0    1    0    0    1    1    0    0    1
    159   0   0   1   1   0   1   0   1   0    1    1    1    0    1    0    0    1
    160   0   0   1   1   1   0   0   0   1    0    0    0    0    0    1    0    1
    161   0   0   1   1   1   0   0   0   1    1    0    0    0    0    1    0    1
    162   0   0   1   1   1   0   0   1   1    0    0    1    0    0    0    1    1
    163   0   0   1   1   1   0   0   1   1    1    1    1    0    0    0    1    1
    164   0   1   0   0   0   0   0   0   0    0    0    0    0    0    0    0    0
    165   0   1   0   0   0   0   0   0   0    0    0    1    0    1    0    0    0
    166   0   1   0   0   0   0   0   0   0    1    0    0    0    1    0    0    1
    167   0   1   0   0   0   0   0   0   1    0    0    0    0    0    0    0    1
    168   0   1   0   0   0   0   0   0   1    0    0    1    0    0    0    0    1
    169   0   1   0   0   0   0   0   0   1    0    1    0    0    0    0    0    1
    170   0   1   0   0   0   0   0   0   1    1    1    1    0    0    0    0    1
    171   0   1   0   0   0   0   0   0   1    1    1    1    0    1    0    0    1
    172   0   1   0   0   0   0   0   1   0    0    0    0    0    0    0    0    0
    173   0   1   0   0   0   0   0   1   0    0    0    0    0    1    0    0    0
    174   0   1   0   0   0   0   0   1   0    0    0    1    1    0    0    0    1
    175   0   1   0   0   0   0   0   1   0    1    0    0    0    0    0    0    1
    176   0   1   0   0   0   0   0   1   0    1    0    1    0    0    0    0    1
    177   0   1   0   0   0   0   0   1   0    1    0    1    0    0    0    0    1
    178   0   1   0   0   0   0   0   1   0    1    1    0    0    0    0    0    0
    179   0   1   0   0   0   0   0   1   0    1    1    0    0    0    0    0    1
    180   0   1   0   0   0   0   0   1   0    1    1    0    0    0    1    0    1
    181   0   1   0   0   0   0   0   1   1    0    0    0    0    1    0    1    1
    182   0   1   0   0   0   0   0   1   1    1    0    0    0    0    0    0    1
    183   0   1   0   0   0   0   0   1   1    1    0    0    0    1    0    0    0
    184   0   1   0   0   0   0   0   1   1    1    0    0    0    1    1    0    1
    185   0   1   0   0   0   0   0   1   1    1    0    1    0    1    1    0    1
    186   0   1   0   0   0   0   0   1   1    1    0    1    0    1    1    1    1
    187   0   1   0   0   0   0   0   1   1    1    0    1    1    1    0    0    1
    188   0   1   0   0   0   0   0   1   1    1    1    0    0    0    1    0    1
    189   0   1   0   0   0   0   0   1   1    1    1    1    0    1    1    1    1
    190   0   1   0   0   0   0   1   1   1    1    1    0    0    0    0    0    1
    191   0   1   0   0   0   1   0   0   0    0    1    1    0    0    0    0    0
    192   0   1   0   0   0   1   0   0   0    1    0    0    0    0    0    0    1
    193   0   1   0   0   0   1   0   0   0    1    0    1    1    0    0    0    1
    194   0   1   0   0   0   1   0   0   1    1    0    1    1    1    1    1    1
    195   0   1   0   0   0   1   0   0   1    1    1    1    1    0    0    1    1
    196   0   1   0   0   0   1   0   1   0    0    1    0    0    1    0    1    1
    197   0   1   0   0   0   1   0   1   0    1    0    1    0    1    1    0    1
    198   0   1   0   0   0   1   0   1   0    1    0    1    1    1    1    0    1
    199   0   1   0   0   0   1   0   1   0    1    1    1    0    0    0    0    1
    200   0   1   0   0   0   1   0   1   0    1    1    1    0    0    0    0    1
    201   0   1   0   0   0   1   0   1   0    1    1    1    0    1    0    0    1
    202   0   1   0   0   0   1   0   1   1    0    0    1    0    1    0    0    1
    203   0   1   0   0   0   1   1   0   0    1    0    1    0    1    0    0    1
    204   0   1   0   0   0   1   1   1   1    1    0    1    0    1    0    0    1
    205   0   1   0   0   0   1   1   1   1    1    1    1    1    1    1    1    1
    206   0   1   0   0   1   0   0   0   0    1    0    1    0    1    0    0    1
    207   0   1   0   0   1   0   0   0   1    0    0    1    0    1    0    0    1
    208   0   1   0   0   1   0   0   1   0    0    0    1    0    0    0    0    1
    209   0   1   0   0   1   0   0   1   0    0    1    1    0    1    0    0    1
    210   0   1   0   0   1   0   0   1   0    1    0    1    0    1    0    0    0
    211   0   1   0   0   1   0   0   1   1    1    1    0    0    1    1    1    1
    212   0   1   0   0   1   0   1   1   0    1    0    0    1    1    0    0    1
    213   0   1   0   0   1   0   1   1   1    1    1    1    1    1    0    0    1
    214   0   1   0   0   1   1   0   1   1    1    1    1    0    0    1    0    1
    215   0   1   0   0   1   1   0   1   1    1    1    1    0    0    1    0    1
    216   0   1   0   0   1   1   0   1   1    1    1    1    0    1    1    1    1
    217   0   1   0   0   1   1   1   1   1    1    0    1    0    1    0    0    1
    218   0   1   0   1   0   0   0   0   0    0    0    0    0    0    0    1    1
    219   0   1   0   1   0   0   0   0   0    0    1    0    0    0    1    0    0
    220   0   1   0   1   0   0   0   0   0    1    0    0    0    0    0    0    0
    221   0   1   0   1   0   0   0   0   0    1    0    0    1    0    0    1    1
    222   0   1   0   1   0   0   0   0   1    1    0    1    1    1    0    1    1
    223   0   1   0   1   0   0   0   0   1    1    0    1    1    1    1    0    1
    224   0   1   0   1   0   0   0   0   1    1    1    0    0    1    0    0    1
    225   0   1   0   1   0   0   0   1   0    0    0    0    0    0    0    0    1
    226   0   1   0   1   0   0   0   1   0    0    1    0    0    0    0    0    0
    227   0   1   0   1   0   0   0   1   0    1    0    0    0    0    0    0    1
    228   0   1   0   1   0   0   0   1   0    1    1    0    0    0    0    0    1
    229   0   1   0   1   0   0   0   1   0    1    1    1    0    1    1    0    1
    230   0   1   0   1   0   0   0   1   1    0    0    0    0    1    0    0    1
    231   0   1   0   1   0   0   0   1   1    1    0    0    0    1    0    0    1
    232   0   1   0   1   0   0   0   1   1    1    0    0    0    1    0    1    1
    233   0   1   0   1   0   0   0   1   1    1    0    0    1    1    1    1    1
    234   0   1   0   1   0   0   1   1   0    1    1    1    1    1    0    0    1
    235   0   1   0   1   0   0   1   1   1    1    0    1    0    1    0    0    1
    236   0   1   0   1   0   1   0   0   1    1    1    0    0    1    1    1    1
    237   0   1   0   1   0   1   0   0   1    1    1    1    0    1    1    0    1
    238   0   1   0   1   0   1   0   0   1    1    1    1    0    1    1    1    1
    239   0   1   0   1   0   1   0   1   0    1    0    0    0    0    0    0    1
    240   0   1   0   1   0   1   0   1   0    1    0    1    0    0    0    0    1
    241   0   1   0   1   0   1   0   1   0    1    0    1    1    0    0    0    1
    242   0   1   0   1   0   1   0   1   0    1    1    0    0    1    1    0    1
    243   0   1   0   1   0   1   0   1   1    1    0    0    0    0    1    0    1
    244   0   1   0   1   0   1   0   1   1    1    0    1    0    0    0    0    1
    245   0   1   0   1   0   1   0   1   1    1    1    0    0    0    0    1    1
    246   0   1   0   1   1   0   0   1   0    0    1    0    0    0    0    0    1
    247   0   1   0   1   1   0   0   1   0    1    1    0    1    1    0    0    1
    248   0   1   0   1   1   0   0   1   0    1    1    1    1    1    0    0    1
    249   0   1   0   1   1   0   0   1   1    1    1    1    0    1    1    1    1
    250   0   1   0   1   1   0   1   1   0    0    0    0    0    0    1    1    1
    251   0   1   0   1   1   1   0   0   0    0    1    1    1    1    0    0    1
    252   0   1   0   1   1   1   0   1   0    1    1    1    1    0    1    0    1
    253   0   1   0   1   1   1   0   1   1    0    1    1    0    1    1    1    1
    254   0   1   0   1   1   1   0   1   1    1    0    1    0    1    1    0    1
    255   0   1   0   1   1   1   0   1   1    1    0    1    1    1    0    0    1
    256   0   1   0   1   1   1   0   1   1    1    1    1    1    1    1    0    1
    257   0   1   0   1   1   1   1   1   0    1    0    0    0    0    1    1    1
    258   0   1   0   1   1   1   1   1   1    1    1    1    1    1    1    1    1
    259   0   1   1   0   0   0   0   0   0    0    0    1    0    0    0    0    1
    260   0   1   1   0   0   0   0   0   0    1    1    0    0    0    0    0    0
    261   0   1   1   0   0   0   0   0   0    1    1    0    0    1    0    0    1
    262   0   1   1   0   0   0   0   0   0    1    1    1    0    0    0    0    1
    263   0   1   1   0   0   0   0   0   1    0    0    1    0    1    0    0    0
    264   0   1   1   0   0   0   0   0   1    1    0    0    0    0    0    0    0
    265   0   1   1   0   0   0   0   0   1    1    0    0    0    0    0    0    1
    266   0   1   1   0   0   0   0   0   1    1    0    1    1    1    1    1    1
    267   0   1   1   0   0   0   0   0   1    1    1    0    0    0    1    0    1
    268   0   1   1   0   0   0   0   1   0    0    0    0    0    0    0    0    1
    269   0   1   1   0   0   0   0   1   0    0    0    1    0    1    0    0    1
    270   0   1   1   0   0   0   0   1   0    0    1    0    0    1    0    0    1
    271   0   1   1   0   0   0   0   1   0    1    0    0    0    0    0    0    1
    272   0   1   1   0   0   0   0   1   0    1    0    0    0    1    0    0    1
    273   0   1   1   0   0   0   0   1   0    1    0    0    1    1    0    0    0
    274   0   1   1   0   0   0   0   1   0    1    0    1    1    0    1    0    1
    275   0   1   1   0   0   0   0   1   0    1    1    0    0    1    1    0    1
    276   0   1   1   0   0   0   0   1   1    0    1    0    0    1    1    1    1
    277   0   1   1   0   0   0   0   1   1    1    0    0    1    0    0    0    1
    278   0   1   1   0   0   0   0   1   1    1    0    1    0    0    0    0    1
    279   0   1   1   0   0   0   0   1   1    1    1    1    0    0    0    0    1
    280   0   1   1   0   0   0   1   1   0    0    0    0    0    1    0    0    1
    281   0   1   1   0   0   0   1   1   1    0    1    1    0    1    0    1    1
    282   0   1   1   0   0   0   1   1   1    1    1    1    0    1    0    0    1
    283   0   1   1   0   0   1   0   0   0    1    1    0    0    0    1    1    1
    284   0   1   1   0   0   1   0   0   0    1    1    0    1    0    0    0    1
    285   0   1   1   0   0   1   0   0   1    1    0    1    0    1    0    0    1
    286   0   1   1   0   0   1   0   0   1    1    0    1    0    1    0    1    1
    287   0   1   1   0   0   1   0   0   1    1    1    1    0    1    1    0    1
    288   0   1   1   0   0   1   0   1   0    1    0    0    0    1    0    0    0
    289   0   1   1   0   0   1   0   1   0    1    0    0    1    1    0    0    1
    290   0   1   1   0   0   1   0   1   0    1    0    0    1    1    0    1    1
    291   0   1   1   0   0   1   0   1   0    1    0    1    0    0    0    0    1
    292   0   1   1   0   0   1   0   1   0    1    1    0    0    0    0    0    1
    293   0   1   1   0   0   1   0   1   0    1    1    0    0    0    0    0    1
    294   0   1   1   0   0   1   0   1   0    1    1    0    1    1    0    0    1
    295   0   1   1   0   0   1   0   1   0    1    1    1    0    1    1    0    1
    296   0   1   1   0   0   1   0   1   1    0    1    0    0    1    0    0    1
    297   0   1   1   0   0   1   0   1   1    1    0    0    0    0    1    0    1
    298   0   1   1   0   0   1   0   1   1    1    0    0    1    1    0    0    1
    299   0   1   1   0   0   1   0   1   1    1    0    1    0    0    0    0    1
    300   0   1   1   0   0   1   0   1   1    1    0    1    0    1    0    1    1
    301   0   1   1   0   0   1   0   1   1    1    0    1    0    1    1    1    1
    302   0   1   1   0   0   1   0   1   1    1    0    1    1    1    1    0    1
    303   0   1   1   0   0   1   0   1   1    1    1    1    0    0    1    1    1
    304   0   1   1   0   0   1   0   1   1    1    1    1    0    1    0    0    1
    305   0   1   1   0   0   1   0   1   1    1    1    1    1    0    1    0    1
    306   0   1   1   0   0   1   1   0   1    1    0    0    1    1    1    0    1
    307   0   1   1   0   0   1   1   1   1    0    1    1    0    1    0    0    1
    308   0   1   1   0   0   1   1   1   1    1    0    0    0    1    1    0    1
    309   0   1   1   0   0   1   1   1   1    1    1    1    0    1    0    1    1
    310   0   1   1   0   1   0   0   0   1    1    1    0    0    0    0    0    1
    311   0   1   1   0   1   0   0   0   1    1    1    1    0    1    1    0    1
    312   0   1   1   0   1   0   0   1   0    1    0    1    0    0    0    0    1
    313   0   1   1   0   1   0   0   1   1    1    0    0    0    1    0    0    1
    314   0   1   1   0   1   0   0   1   1    1    1    1    1    1    1    0    1
    315   0   1   1   0   1   0   1   1   1    0    1    1    0    1    0    0    1
    316   0   1   1   0   1   1   0   0   0    1    0    0    0    1    0    1    1
    317   0   1   1   0   1   1   0   0   1    1    0    1    1    0    1    0    1
    318   0   1   1   0   1   1   0   0   1    1    1    1    0    1    1    1    1
    319   0   1   1   0   1   1   0   0   1    1    1    1    1    1    0    1    1
    320   0   1   1   0   1   1   0   1   0    1    1    0    0    1    0    0    1
    321   0   1   1   0   1   1   0   1   1    1    1    1    0    1    0    0    1
    322   0   1   1   0   1   1   0   1   1    1    1    1    0    1    1    0    1
    323   0   1   1   0   1   1   0   1   1    1    1    1    0    1    1    1    1
    324   0   1   1   1   0   0   0   0   0    0    0    0    0    0    0    0    1
    325   0   1   1   1   0   0   0   0   0    1    1    0    0    0    0    0    1
    326   0   1   1   1   0   0   0   0   1    0    1    1    0    0    0    0    1
    327   0   1   1   1   0   0   0   0   1    1    0    1    0    0    0    0    1
    328   0   1   1   1   0   0   0   1   0    0    0    1    1    1    0    0    1
    329   0   1   1   1   0   0   0   1   0    1    1    1    0    0    1    1    1
    330   0   1   1   1   0   0   0   1   1    1    0    1    0    0    1    1    1
    331   0   1   1   1   0   0   0   1   1    1    0    1    0    1    0    0    1
    332   0   1   1   1   0   0   0   1   1    1    1    1    0    1    0    0    1
    333   0   1   1   1   0   0   0   1   1    1    1    1    1    1    0    0    0
    334   0   1   1   1   0   0   1   1   0    1    0    1    0    1    1    1    1
    335   0   1   1   1   0   1   0   0   0    0    0    1    1    1    0    0    1
    336   0   1   1   1   0   1   0   0   0    1    1    1    0    0    1    0    1
    337   0   1   1   1   0   1   0   0   1    0    0    0    0    1    0    0    1
    338   0   1   1   1   0   1   0   0   1    0    1    1    0    1    1    0    1
    339   0   1   1   1   0   1   0   0   1    1    0    1    0    0    0    0    1
    340   0   1   1   1   0   1   0   0   1    1    0    1    0    1    0    1    1
    341   0   1   1   1   0   1   0   1   0    0    0    1    0    1    0    1    1
    342   0   1   1   1   0   1   0   1   0    1    0    1    1    1    0    0    1
    343   0   1   1   1   0   1   0   1   1    0    1    0    0    0    0    0    1
    344   0   1   1   1   0   1   0   1   1    0    1    1    0    1    0    0    1
    345   0   1   1   1   0   1   0   1   1    1    0    0    0    1    1    0    1
    346   0   1   1   1   0   1   0   1   1    1    0    1    0    1    0    0    1
    347   0   1   1   1   0   1   0   1   1    1    1    0    0    1    0    0    1
    348   0   1   1   1   0   1   0   1   1    1    1    0    1    0    0    0    1
    349   0   1   1   1   0   1   0   1   1    1    1    1    0    1    0    0    1
    350   0   1   1   1   0   1   0   1   1    1    1    1    0    1    1    0    1
    351   0   1   1   1   0   1   0   1   1    1    1    1    0    1    1    1    1
    352   0   1   1   1   0   1   1   0   1    1    1    1    0    1    1    0    1
    353   0   1   1   1   0   1   1   1   1    0    0    0    1    1    0    0    1
    354   0   1   1   1   0   1   1   1   1    1    0    1    1    1    1    0    1
    355   0   1   1   1   1   0   0   0   1    1    0    1    0    1    1    1    1
    356   0   1   1   1   1   0   0   1   1    1    0    1    0    0    1    0    1
    357   0   1   1   1   1   0   0   1   1    1    1    1    0    1    0    0    1
    358   0   1   1   1   1   0   1   1   1    1    0    0    1    1    1    0    1
    359   0   1   1   1   1   1   0   0   1    1    1    1    0    0    0    1    1
    360   0   1   1   1   1   1   0   0   1    1    1    1    1    1    0    1    1
    361   0   1   1   1   1   1   0   1   0    0    0    1    0    0    0    1    1
    362   0   1   1   1   1   1   0   1   0    0    0    1    0    1    0    0    1
    363   0   1   1   1   1   1   0   1   0    1    1    0    0    1    0    0    1
    364   0   1   1   1   1   1   0   1   0    1    1    1    1    1    0    1    1
    365   0   1   1   1   1   1   0   1   1    1    0    1    0    0    0    0    1
    366   0   1   1   1   1   1   0   1   1    1    0    1    0    1    0    0    1
    367   0   1   1   1   1   1   0   1   1    1    0    1    0    1    1    0    1
    368   0   1   1   1   1   1   0   1   1    1    0    1    1    0    0    0    1
    369   0   1   1   1   1   1   0   1   1    1    1    0    1    1    1    0    1
    370   0   1   1   1   1   1   0   1   1    1    1    1    0    0    0    1    1
    371   0   1   1   1   1   1   0   1   1    1    1    1    0    1    1    0    1
    372   0   1   1   1   1   1   0   1   1    1    1    1    0    1    1    1    1
    373   0   1   1   1   1   1   0   1   1    1    1    1    0    1    1    1    1
    374   0   1   1   1   1   1   0   1   1    1    1    1    1    1    0    1    1
    375   0   1   1   1   1   1   1   1   1    1    1    1    1    1    1    1    1
    376   0   1   1   1   1   1   1   1   1    1    1    1    1    1    1    1    1
    377   1   0   0   0   0   0   0   0   0    0    0    0    0    0    0    0    0
    378   1   0   0   0   0   0   0   0   0    0    0    0    0    1    0    1    0
    379   1   0   0   0   0   0   0   0   0    0    1    0    0    0    0    0    1
    380   1   0   0   0   0   0   0   0   0    0    1    0    1    1    1    1    1
    381   1   0   0   0   0   0   0   0   0    1    0    0    0    0    0    0    0
    382   1   0   0   0   0   0   0   0   0    1    0    0    0    0    0    1    1
    383   1   0   0   0   0   0   0   0   0    1    0    0    1    0    0    0    1
    384   1   0   0   0   0   0   0   0   0    1    0    1    0    1    0    1    1
    385   1   0   0   0   0   0   0   0   0    1    0    1    1    1    0    1    1
    386   1   0   0   0   0   0   0   0   1    1    0    0    0    1    0    0    1
    387   1   0   0   0   0   0   0   1   0    0    0    0    0    0    0    0    0
    388   1   0   0   0   0   0   0   1   0    0    0    1    1    0    0    0    1
    389   1   0   0   0   0   0   0   1   0    0    1    1    1    0    0    0    0
    390   1   0   0   0   0   0   0   1   0    1    0    0    0    0    1    0    1
    391   1   0   0   0   0   0   0   1   0    1    0    0    1    0    0    1    1
    392   1   0   0   0   0   0   0   1   0    1    0    1    0    0    0    0    1
    393   1   0   0   0   0   0   0   1   0    1    1    0    0    0    0    0    1
    394   1   0   0   0   0   0   0   1   0    1    1    0    1    0    0    0    1
    395   1   0   0   0   0   0   0   1   1    0    0    0    0    0    0    0    0
    396   1   0   0   0   0   0   0   1   1    0    0    1    0    0    0    0    0
    397   1   0   0   0   0   0   0   1   1    1    1    1    0    1    0    1    1
    398   1   0   0   0   0   0   1   0   0    1    0    0    0    0    1    0    1
    399   1   0   0   0   0   0   1   0   0    1    0    0    0    1    1    0    1
    400   1   0   0   0   0   0   1   1   0    1    1    0    0    0    0    0    0
    401   1   0   0   0   0   1   0   0   0    1    0    0    1    0    0    0    1
    402   1   0   0   0   0   1   0   0   0    1    0    0    1    0    1    0    1
    403   1   0   0   0   0   1   0   0   1    1    1    1    0    1    0    0    1
    404   1   0   0   0   0   1   0   1   0    0    1    0    0    0    1    0    1
    405   1   0   0   0   0   1   0   1   0    1    1    1    0    1    0    0    1
    406   1   0   0   0   0   1   0   1   1    0    0    0    0    1    0    1    0
    407   1   0   0   0   0   1   0   1   1    1    0    0    0    0    0    0    1
    408   1   0   0   0   0   1   1   1   0    1    0    1    0    0    0    0    1
    409   1   0   0   0   1   0   0   0   0    0    1    0    0    0    0    0    1
    410   1   0   0   0   1   0   0   0   0    1    1    0    1    0    0    0    0
    411   1   0   0   0   1   0   0   1   0    1    0    0    1    1    0    0    1
    412   1   0   0   0   1   0   0   1   0    1    0    1    0    0    0    0    1
    413   1   0   0   0   1   0   0   1   1    0    0    0    0    0    0    0    1
    414   1   0   0   0   1   0   0   1   1    1    1    1    0    1    0    0    1
    415   1   0   0   0   1   0   1   0   0    1    0    1    1    0    0    0    1
    416   1   0   0   1   0   0   0   0   0    0    0    0    0    0    0    0    0
    417   1   0   0   1   0   0   0   0   1    1    1    0    0    1    0    0    1
    418   1   0   0   1   0   0   0   1   0    0    0    0    0    0    1    0    1
    419   1   0   0   1   0   0   0   1   0    1    1    0    0    0    0    0    1
    420   1   0   0   1   0   0   0   1   0    1    1    0    0    0    0    0    1
    421   1   0   0   1   0   0   0   1   1    1    0    1    0    0    0    0    1
    422   1   0   0   1   0   0   0   1   1    1    1    1    1    1    0    1    0
    423   1   0   0   1   0   0   1   1   0    1    1    1    0    0    0    0    1
    424   1   0   0   1   0   1   0   0   0    1    0    0    0    0    0    0    1
    425   1   0   0   1   0   1   0   0   1    1    1    1    1    1    0    0    1
    426   1   0   0   1   0   1   0   1   0    0    0    0    0    0    1    0    0
    427   1   0   0   1   0   1   0   1   0    1    0    1    0    0    0    0    1
    428   1   0   0   1   0   1   0   1   0    1    1    1    0    1    0    0    1
    429   1   0   0   1   0   1   0   1   0    1    1    1    0    1    1    1    1
    430   1   0   0   1   0   1   0   1   1    1    0    1    0    1    0    1    1
    431   1   0   0   1   0   1   1   1   0    1    0    0    0    0    0    1    1
    432   1   0   0   1   0   1   1   1   1    0    1    0    0    0    0    0    1
    433   1   0   0   1   0   1   1   1   1    1    1    1    0    0    1    0    1
    434   1   0   0   1   1   1   0   0   0    1    0    0    0    1    0    0    1
    435   1   0   0   1   1   1   0   1   1    0    1    1    1    1    0    0    1
    436   1   0   0   1   1   1   0   1   1    1    1    1    0    1    1    0    1
    437   1   0   0   1   1   1   0   1   1    1    1    1    1    1    0    0    1
    438   1   0   0   1   1   1   1   0   0    1    0    1    0    0    0    0    1
    439   1   0   1   0   0   0   0   0   0    0    0    0    0    0    0    0    0
    440   1   0   1   0   0   0   0   0   0    1    0    0    0    0    0    0    1
    441   1   0   1   0   0   0   0   0   0    1    0    0    1    0    0    0    1
    442   1   0   1   0   0   0   0   0   0    1    1    0    0    0    0    0    0
    443   1   0   1   0   0   0   0   1   0    0    0    1    0    0    0    1    0
    444   1   0   1   0   0   0   0   1   0    1    0    0    0    0    0    0    1
    445   1   0   1   0   0   0   0   1   0    1    1    1    0    1    1    1    1
    446   1   0   1   0   0   0   0   1   0    1    1    1    1    0    0    0    1
    447   1   0   1   0   0   0   1   0   1    1    1    1    0    1    1    0    1
    448   1   0   1   0   0   1   0   0   0    0    1    1    0    0    0    1    1
    449   1   0   1   0   0   1   0   0   0    1    0    1    1    1    0    1    0
    450   1   0   1   0   0   1   0   0   1    1    1    1    0    0    0    1    1
    451   1   0   1   0   0   1   0   1   0    0    1    0    0    0    0    0    1
    452   1   0   1   0   0   1   0   1   1    1    0    1    0    1    0    1    1
    453   1   0   1   0   0   1   1   0   1    0    0    0    0    0    0    0    0
    454   1   0   1   0   1   0   0   0   1    1    1    0    0    1    1    0    1
    455   1   0   1   0   1   0   0   1   0    1    0    1    1    1    0    0    1
    456   1   0   1   0   1   1   0   1   0    1    0    1    0    1    1    1    1
    457   1   0   1   0   1   1   0   1   0    1    0    1    1    0    1    0    1
    458   1   0   1   0   1   1   0   1   1    1    1    1    1    1    1    0    1
    459   1   0   1   1   0   0   0   0   0    0    0    0    0    0    0    0    1
    460   1   0   1   1   0   0   0   0   1    1    0    1    0    1    0    0    1
    461   1   0   1   1   0   0   0   1   0    0    0    0    0    0    0    1    1
    462   1   0   1   1   0   0   0   1   0    0    0    1    0    1    0    1    1
    463   1   0   1   1   0   0   0   1   0    0    1    0    0    0    0    0    1
    464   1   0   1   1   0   0   1   1   0    0    1    1    0    0    0    0    1
    465   1   0   1   1   0   1   0   0   0    1    1    1    0    0    0    1    1
    466   1   0   1   1   0   1   0   0   1    1    0    1    1    1    1    0    1
    467   1   0   1   1   0   1   0   0   1    1    1    1    0    1    1    0    1
    468   1   0   1   1   0   1   0   1   1    1    0    1    1    0    1    1    1
    469   1   0   1   1   0   1   0   1   1    1    1    1    0    0    0    0    1
    470   1   0   1   1   0   1   0   1   1    1    1    1    0    1    1    0    1
    471   1   0   1   1   1   0   0   1   1    0    0    1    0    1    1    0    1
    472   1   0   1   1   1   0   1   1   1    1    0    0    1    0    0    0    1
    473   1   0   1   1   1   0   1   1   1    1    1    1    1    1    1    1    1
    474   1   0   1   1   1   1   0   1   1    0    1    1    0    0    0    0    1
    475   1   0   1   1   1   1   1   1   1    1    1    1    1    1    1    1    1
    476   1   1   0   0   0   0   0   0   0    0    1    1    0    0    0    0    1
    477   1   1   0   0   0   0   0   0   0    1    1    0    0    0    0    0    1
    478   1   1   0   0   0   0   0   0   0    1    1    1    0    1    1    1    1
    479   1   1   0   0   0   0   0   0   0    1    1    1    1    0    0    0    1
    480   1   1   0   0   0   0   0   0   1    0    0    0    0    0    0    1    0
    481   1   1   0   0   0   0   0   0   1    0    0    0    0    1    0    0    1
    482   1   1   0   0   0   0   0   0   1    1    1    0    0    0    0    0    1
    483   1   1   0   0   0   0   0   0   1    1    1    1    0    1    1    0    1
    484   1   1   0   0   0   0   0   1   0    1    0    0    0    0    0    0    1
    485   1   1   0   0   0   0   0   1   0    1    0    0    1    0    0    0    1
    486   1   1   0   0   0   0   0   1   0    1    0    0    1    0    0    0    1
    487   1   1   0   0   0   0   0   1   0    1    0    1    0    0    0    0    1
    488   1   1   0   0   0   0   0   1   0    1    0    1    0    1    0    1    1
    489   1   1   0   0   0   0   0   1   0    1    0    1    0    1    1    0    1
    490   1   1   0   0   0   0   0   1   0    1    0    1    1    0    0    0    1
    491   1   1   0   0   0   0   0   1   0    1    1    0    1    0    0    0    0
    492   1   1   0   0   0   0   0   1   0    1    1    1    0    0    1    1    1
    493   1   1   0   0   0   0   0   1   1    1    0    1    1    1    1    0    1
    494   1   1   0   0   0   0   0   1   1    1    1    0    0    1    0    0    1
    495   1   1   0   0   0   0   0   1   1    1    1    1    1    1    1    1    1
    496   1   1   0   0   0   0   1   1   0    0    0    0    0    0    0    0    0
    497   1   1   0   0   0   0   1   1   1    1    0    1    1    1    1    0    1
    498   1   1   0   0   0   1   0   0   0    0    1    1    0    0    0    1    1
    499   1   1   0   0   0   1   0   0   0    1    1    0    0    0    0    0    1
    500   1   1   0   0   0   1   0   0   0    1    1    0    0    1    0    0    0
    501   1   1   0   0   0   1   0   0   0    1    1    1    1    1    1    0    1
    502   1   1   0   0   0   1   0   0   1    1    0    1    0    0    1    1    1
    503   1   1   0   0   0   1   0   0   1    1    0    1    0    1    1    0    0
    504   1   1   0   0   0   1   0   0   1    1    1    1    1    1    1    0    1
    505   1   1   0   0   0   1   0   1   0    1    0    1    0    1    0    1    1
    506   1   1   0   0   0   1   0   1   0    1    0    1    0    1    0    1    1
    507   1   1   0   0   0   1   0   1   0    1    0    1    0    1    1    1    1
    508   1   1   0   0   0   1   0   1   0    1    1    0    0    0    0    0    1
    509   1   1   0   0   0   1   0   1   0    1    1    1    0    0    0    1    1
    510   1   1   0   0   0   1   0   1   0    1    1    1    0    0    1    0    1
    511   1   1   0   0   0   1   0   1   0    1    1    1    0    0    1    1    1
    512   1   1   0   0   0   1   0   1   0    1    1    1    1    1    0    1    1
    513   1   1   0   0   0   1   0   1   0    1    1    1    1    1    1    1    1
    514   1   1   0   0   0   1   0   1   1    0    1    1    0    1    0    0    1
    515   1   1   0   0   0   1   0   1   1    0    1    1    0    1    1    1    1
    516   1   1   0   0   0   1   0   1   1    1    0    0    0    1    0    0    1
    517   1   1   0   0   0   1   0   1   1    1    0    1    0    0    1    0    1
    518   1   1   0   0   0   1   0   1   1    1    0    1    1    0    0    1    1
    519   1   1   0   0   0   1   0   1   1    1    1    0    0    0    0    0    1
    520   1   1   0   0   0   1   0   1   1    1    1    0    0    1    1    1    1
    521   1   1   0   0   0   1   0   1   1    1    1    1    0    0    0    1    1
    522   1   1   0   0   0   1   0   1   1    1    1    1    1    1    1    0    1
    523   1   1   0   0   0   1   0   1   1    1    1    1    1    1    1    1    1
    524   1   1   0   0   0   1   0   1   1    1    1    1    1    1    1    1    1
    525   1   1   0   0   0   1   1   0   0    1    1    1    0    1    0    0    1
    526   1   1   0   0   0   1   1   0   1    1    0    1    0    0    0    1    1
    527   1   1   0   0   0   1   1   1   0    1    0    0    0    0    1    0    1
    528   1   1   0   0   0   1   1   1   0    1    1    1    0    1    0    0    1
    529   1   1   0   0   0   1   1   1   1    1    0    1    0    0    0    0    1
    530   1   1   0   0   0   1   1   1   1    1    1    0    1    0    0    0    1
    531   1   1   0   0   0   1   1   1   1    1    1    1    0    1    0    0    1
    532   1   1   0   0   0   1   1   1   1    1    1    1    0    1    1    1    1
    533   1   1   0   0   0   1   1   1   1    1    1    1    1    1    1    0    1
    534   1   1   0   0   0   1   1   1   1    1    1    1    1    1    1    1    1
    535   1   1   0   0   1   0   0   0   1    1    1    1    1    1    1    0    1
    536   1   1   0   0   1   0   0   1   0    1    0    1    0    0    0    0    0
    537   1   1   0   0   1   0   0   1   0    1    0    1    1    1    1    1    1
    538   1   1   0   0   1   0   0   1   1    1    0    1    1    1    1    0    1
    539   1   1   0   0   1   0   0   1   1    1    0    1    1    1    1    0    1
    540   1   1   0   0   1   0   0   1   1    1    1    1    0    0    0    1    1
    541   1   1   0   0   1   0   0   1   1    1    1    1    0    1    1    0    1
    542   1   1   0   0   1   0   1   1   1    0    0    0    0    1    0    0    0
    543   1   1   0   0   1   1   0   1   0    1    1    1    1    1    1    0    1
    544   1   1   0   0   1   1   0   1   1    1    0    1    0    0    0    0    1
    545   1   1   0   0   1   1   0   1   1    1    0    1    1    1    0    0    1
    546   1   1   0   0   1   1   0   1   1    1    1    0    1    0    1    1    1
    547   1   1   0   0   1   1   0   1   1    1    1    1    0    0    1    1    1
    548   1   1   0   0   1   1   0   1   1    1    1    1    0    1    1    1    1
    549   1   1   0   0   1   1   0   1   1    1    1    1    1    0    0    1    1
    550   1   1   0   0   1   1   0   1   1    1    1    1    1    1    1    0    1
    551   1   1   0   0   1   1   0   1   1    1    1    1    1    1    1    1    1
    552   1   1   0   0   1   1   1   1   0    1    1    1    1    1    1    1    1
    553   1   1   0   0   1   1   1   1   1    1    1    1    0    1    1    1    1
    554   1   1   0   0   1   1   1   1   1    1    1    1    1    1    1    1    1
    555   1   1   0   1   0   0   0   0   0    1    1    1    0    1    0    0    1
    556   1   1   0   1   0   0   0   0   1    0    1    1    0    1    1    0    1
    557   1   1   0   1   0   0   0   0   1    1    0    1    0    1    0    1    1
    558   1   1   0   1   0   0   0   0   1    1    0    1    0    1    1    0    1
    559   1   1   0   1   0   0   0   0   1    1    1    1    0    0    1    1    1
    560   1   1   0   1   0   0   0   1   0    0    0    1    0    1    0    0    1
    561   1   1   0   1   0   0   0   1   0    1    1    0    0    1    1    1    1
    562   1   1   0   1   0   0   0   1   1    0    0    0    0    0    0    0    1
    563   1   1   0   1   0   0   0   1   1    0    1    0    0    0    0    0    1
    564   1   1   0   1   0   0   0   1   1    1    0    1    0    1    0    1    1
    565   1   1   0   1   0   0   0   1   1    1    0    1    1    1    1    0    1
    566   1   1   0   1   0   0   0   1   1    1    1    0    0    1    1    1    1
    567   1   1   0   1   0   0   0   1   1    1    1    1    0    0    1    1    1
    568   1   1   0   1   0   0   0   1   1    1    1    1    0    0    1    1    1
    569   1   1   0   1   0   0   1   0   1    0    1    1    0    1    1    1    1
    570   1   1   0   1   0   0   1   1   0    1    1    1    0    1    1    0    1
    571   1   1   0   1   0   0   1   1   1    1    1    0    1    1    0    1    1
    572   1   1   0   1   0   0   1   1   1    1    1    1    1    0    0    0    1
    573   1   1   0   1   0   0   1   1   1    1    1    1    1    1    0    1    1
    574   1   1   0   1   0   0   1   1   1    1    1    1    1    1    1    0    1
    575   1   1   0   1   0   1   0   0   0    1    0    0    0    0    0    0    1
    576   1   1   0   1   0   1   0   0   0    1    0    1    0    1    0    0    0
    577   1   1   0   1   0   1   0   0   0    1    1    1    0    1    1    0    1
    578   1   1   0   1   0   1   0   0   1    1    0    1    0    1    0    1    1
    579   1   1   0   1   0   1   0   0   1    1    1    1    0    1    0    0    1
    580   1   1   0   1   0   1   0   0   1    1    1    1    0    1    1    1    1
    581   1   1   0   1   0   1   0   1   0    0    0    1    0    1    0    0    1
    582   1   1   0   1   0   1   0   1   0    0    1    1    0    0    1    0    1
    583   1   1   0   1   0   1   0   1   0    1    1    1    0    1    1    0    1
    584   1   1   0   1   0   1   0   1   0    1    1    1    1    0    1    0    1
    585   1   1   0   1   0   1   0   1   1    0    1    1    0    1    1    0    1
    586   1   1   0   1   0   1   0   1   1    0    1    1    1    0    1    0    1
    587   1   1   0   1   0   1   0   1   1    0    1    1    1    1    1    0    1
    588   1   1   0   1   0   1   0   1   1    1    0    0    0    1    1    0    1
    589   1   1   0   1   0   1   0   1   1    1    0    1    0    0    1    1    1
    590   1   1   0   1   0   1   0   1   1    1    0    1    0    1    0    1    1
    591   1   1   0   1   0   1   0   1   1    1    0    1    1    1    1    0    1
    592   1   1   0   1   0   1   0   1   1    1    1    1    0    0    1    0    1
    593   1   1   0   1   0   1   0   1   1    1    1    1    0    1    0    0    1
    594   1   1   0   1   0   1   0   1   1    1    1    1    0    1    1    0    1
    595   1   1   0   1   0   1   0   1   1    1    1    1    0    1    1    1    1
    596   1   1   0   1   0   1   0   1   1    1    1    1    0    1    1    1    1
    597   1   1   0   1   0   1   0   1   1    1    1    1    1    0    1    0    1
    598   1   1   0   1   0   1   0   1   1    1    1    1    1    0    1    1    1
    599   1   1   0   1   0   1   0   1   1    1    1    1    1    1    1    0    1
    600   1   1   0   1   0   1   0   1   1    1    1    1    1    1    1    1    1
    601   1   1   0   1   0   1   1   0   0    1    1    0    0    1    1    1    1
    602   1   1   0   1   0   1   1   0   1    1    1    1    0    1    0    1    1
    603   1   1   0   1   0   1   1   0   1    1    1    1    0    1    1    1    1
    604   1   1   0   1   0   1   1   1   0    1    0    1    1    1    1    1    1
    605   1   1   0   1   0   1   1   1   0    1    1    0    0    1    1    1    1
    606   1   1   0   1   0   1   1   1   0    1    1    1    1    1    1    1    1
    607   1   1   0   1   0   1   1   1   1    0    1    0    1    0    0    1    1
    608   1   1   0   1   0   1   1   1   1    0    1    1    1    1    0    0    1
    609   1   1   0   1   0   1   1   1   1    1    0    1    0    1    1    0    1
    610   1   1   0   1   0   1   1   1   1    1    1    1    1    1    1    1    1
    611   1   1   0   1   1   0   0   0   0    1    1    1    1    1    1    1    1
    612   1   1   0   1   1   0   0   1   0    0    0    0    0    1    0    0    1
    613   1   1   0   1   1   0   0   1   0    1    0    1    0    1    0    0    1
    614   1   1   0   1   1   0   0   1   0    1    1    1    0    1    0    1    1
    615   1   1   0   1   1   0   0   1   1    1    1    1    0    1    1    0    0
    616   1   1   0   1   1   0   0   1   1    1    1    1    1    1    1    0    1
    617   1   1   0   1   1   0   1   0   1    0    1    1    1    1    1    1    1
    618   1   1   0   1   1   0   1   1   0    0    1    1    1    1    0    0    1
    619   1   1   0   1   1   0   1   1   1    0    1    1    1    1    1    1    1
    620   1   1   0   1   1   0   1   1   1    1    1    0    0    1    0    1    1
    621   1   1   0   1   1   0   1   1   1    1    1    1    0    1    1    0    1
    622   1   1   0   1   1   1   0   0   0    1    1    1    0    1    1    0    1
    623   1   1   0   1   1   1   0   0   1    1    1    0    0    1    0    0    1
    624   1   1   0   1   1   1   0   0   1    1    1    1    0    1    0    1    1
    625   1   1   0   1   1   1   0   0   1    1    1    1    0    1    1    1    1
    626   1   1   0   1   1   1   0   1   0    0    1    1    1    0    1    0    1
    627   1   1   0   1   1   1   0   1   1    1    0    1    0    1    1    1    1
    628   1   1   0   1   1   1   0   1   1    1    1    1    0    1    0    0    1
    629   1   1   0   1   1   1   0   1   1    1    1    1    0    1    1    0    1
    630   1   1   0   1   1   1   0   1   1    1    1    1    0    1    1    1    1
    631   1   1   0   1   1   1   0   1   1    1    1    1    1    0    1    0    1
    632   1   1   0   1   1   1   0   1   1    1    1    1    1    1    1    0    1
    633   1   1   0   1   1   1   0   1   1    1    1    1    1    1    1    1    1
    634   1   1   0   1   1   1   0   1   1    1    1    1    1    1    1    1    1
    635   1   1   0   1   1   1   1   0   0    1    0    0    0    1    0    0    1
    636   1   1   0   1   1   1   1   0   1    1    1    1    0    1    1    1    1
    637   1   1   0   1   1   1   1   1   0    1    1    1    0    0    0    1    1
    638   1   1   0   1   1   1   1   1   0    1    1    1    0    1    0    1    1
    639   1   1   0   1   1   1   1   1   1    0    0    1    0    1    1    1    1
    640   1   1   0   1   1   1   1   1   1    1    1    1    0    1    0    0    1
    641   1   1   0   1   1   1   1   1   1    1    1    1    0    1    1    0    1
    642   1   1   0   1   1   1   1   1   1    1    1    1    0    1    1    1    1
    643   1   1   0   1   1   1   1   1   1    1    1    1    1    0    0    1    1
    644   1   1   0   1   1   1   1   1   1    1    1    1    1    0    1    0    1
    645   1   1   0   1   1   1   1   1   1    1    1    1    1    1    1    0    1
    646   1   1   0   1   1   1   1   1   1    1    1    1    1    1    1    1    1
    647   1   1   1   0   0   0   0   0   0    0    1    0    0    0    0    0    0
    648   1   1   1   0   0   0   0   0   0    0    1    0    1    1    0    0    1
    649   1   1   1   0   0   0   0   0   1    1    1    1    0    1    0    1    1
    650   1   1   1   0   0   0   0   1   0    0    1    0    1    1    1    1    1
    651   1   1   1   0   0   0   0   1   0    1    0    1    0    1    1    0    1
    652   1   1   1   0   0   0   0   1   0    1    1    1    0    0    0    0    1
    653   1   1   1   0   0   0   0   1   0    1    1    1    0    1    0    1    1
    654   1   1   1   0   0   0   0   1   1    0    1    1    0    0    0    1    1
    655   1   1   1   0   0   0   0   1   1    0    1    1    0    1    0    0    1
    656   1   1   1   0   0   0   0   1   1    1    0    0    0    0    0    0    1
    657   1   1   1   0   0   0   0   1   1    1    0    1    1    1    0    1    1
    658   1   1   1   0   0   0   0   1   1    1    1    0    1    1    0    1    1
    659   1   1   1   0   0   0   0   1   1    1    1    1    0    1    0    0    1
    660   1   1   1   0   0   0   0   1   1    1    1    1    0    1    0    1    1
    661   1   1   1   0   0   0   0   1   1    1    1    1    1    0    0    0    1
    662   1   1   1   0   0   0   0   1   1    1    1    1    1    1    0    0    1
    663   1   1   1   0   0   0   1   1   1    1    1    1    1    1    0    0    1
    664   1   1   1   0   0   1   0   0   0    1    0    1    0    1    0    0    1
    665   1   1   1   0   0   1   0   0   0    1    1    0    0    1    1    1    1
    666   1   1   1   0   0   1   0   0   0    1    1    1    0    0    1    0    1
    667   1   1   1   0   0   1   0   0   1    0    0    1    1    1    0    1    1
    668   1   1   1   0   0   1   0   0   1    1    0    1    0    1    1    1    1
    669   1   1   1   0   0   1   0   1   0    0    0    0    0    0    0    0    1
    670   1   1   1   0   0   1   0   1   0    1    0    0    0    1    1    0    1
    671   1   1   1   0   0   1   0   1   0    1    0    1    0    0    0    1    1
    672   1   1   1   0   0   1   0   1   0    1    0    1    0    1    0    0    1
    673   1   1   1   0   0   1   0   1   0    1    1    0    0    0    1    0    1
    674   1   1   1   0   0   1   0   1   0    1    1    1    0    0    0    0    1
    675   1   1   1   0   0   1   0   1   0    1    1    1    0    1    0    0    1
    676   1   1   1   0   0   1   0   1   0    1    1    1    0    1    1    0    1
    677   1   1   1   0   0   1   0   1   0    1    1    1    0    1    1    1    1
    678   1   1   1   0   0   1   0   1   0    1    1    1    1    1    0    1    1
    679   1   1   1   0   0   1   0   1   0    1    1    1    1    1    1    1    1
    680   1   1   1   0   0   1   0   1   1    0    0    1    0    0    0    0    1
    681   1   1   1   0   0   1   0   1   1    0    0    1    1    0    0    1    1
    682   1   1   1   0   0   1   0   1   1    0    1    1    0    1    0    0    1
    683   1   1   1   0   0   1   0   1   1    0    1    1    0    1    1    0    1
    684   1   1   1   0   0   1   0   1   1    1    0    1    0    0    0    0    1
    685   1   1   1   0   0   1   0   1   1    1    0    1    0    1    0    1    1
    686   1   1   1   0   0   1   0   1   1    1    0    1    1    1    0    0    1
    687   1   1   1   0   0   1   0   1   1    1    1    0    0    0    0    0    1
    688   1   1   1   0   0   1   0   1   1    1    1    1    0    0    1    1    1
    689   1   1   1   0   0   1   0   1   1    1    1    1    0    1    1    0    1
    690   1   1   1   0   0   1   0   1   1    1    1    1    0    1    1    1    1
    691   1   1   1   0   0   1   0   1   1    1    1    1    1    1    1    1    0
    692   1   1   1   0   0   1   0   1   1    1    1    1    1    1    1    1    1
    693   1   1   1   0   0   1   1   0   1    0    1    1    1    0    1    1    1
    694   1   1   1   0   0   1   1   0   1    1    1    1    1    1    1    1    1
    695   1   1   1   0   0   1   1   1   0    0    1    1    1    1    1    1    1
    696   1   1   1   0   0   1   1   1   0    1    0    1    0    0    0    0    1
    697   1   1   1   0   0   1   1   1   0    1    0    1    0    1    1    0    1
    698   1   1   1   0   0   1   1   1   0    1    0    1    1    1    1    1    1
    699   1   1   1   0   0   1   1   1   0    1    1    1    0    0    0    0    1
    700   1   1   1   0   0   1   1   1   1    1    0    1    0    1    1    0    1
    701   1   1   1   0   0   1   1   1   1    1    0    1    0    1    1    1    1
    702   1   1   1   0   0   1   1   1   1    1    1    0    1    1    0    0    1
    703   1   1   1   0   0   1   1   1   1    1    1    1    0    1    1    0    1
    704   1   1   1   0   0   1   1   1   1    1    1    1    0    1    1    1    1
    705   1   1   1   0   0   1   1   1   1    1    1    1    1    1    0    0    0
    706   1   1   1   0   0   1   1   1   1    1    1    1    1    1    1    0    1
    707   1   1   1   0   0   1   1   1   1    1    1    1    1    1    1    1    1
    708   1   1   1   0   1   0   0   0   1    0    1    1    1    1    1    1    1
    709   1   1   1   0   1   0   0   0   1    1    0    1    0    0    0    0    1
    710   1   1   1   0   1   0   0   1   0    0    1    0    0    0    0    1    0
    711   1   1   1   0   1   0   0   1   0    1    0    0    0    0    0    1    1
    712   1   1   1   0   1   0   0   1   0    1    1    0    0    0    0    1    1
    713   1   1   1   0   1   0   0   1   0    1    1    1    0    0    0    0    1
    714   1   1   1   0   1   0   0   1   0    1    1    1    1    1    0    1    1
    715   1   1   1   0   1   0   0   1   0    1    1    1    1    1    1    0    1
    716   1   1   1   0   1   0   0   1   1    0    1    1    1    1    1    1    1
    717   1   1   1   0   1   0   0   1   1    1    1    1    0    1    0    0    1
    718   1   1   1   0   1   0   0   1   1    1    1    1    0    1    1    0    1
    719   1   1   1   0   1   0   0   1   1    1    1    1    0    1    1    1    1
    720   1   1   1   0   1   0   0   1   1    1    1    1    1    1    1    1    1
    721   1   1   1   0   1   0   0   1   1    1    1    1    1    1    1    1    1
    722   1   1   1   0   1   0   1   0   0    1    1    1    1    1    1    0    1
    723   1   1   1   0   1   0   1   0   1    0    1    1    0    0    1    0    1
    724   1   1   1   0   1   0   1   1   0    1    0    1    0    1    0    1    1
    725   1   1   1   0   1   0   1   1   0    1    0    1    0    1    1    1    1
    726   1   1   1   0   1   0   1   1   0    1    1    1    0    1    0    1    1
    727   1   1   1   0   1   0   1   1   0    1    1    1    0    1    1    0    1
    728   1   1   1   0   1   0   1   1   1    0    1    1    0    1    1    0    1
    729   1   1   1   0   1   0   1   1   1    1    1    1    1    1    0    1    1
    730   1   1   1   0   1   1   0   0   0    0    1    1    0    1    0    1    1
    731   1   1   1   0   1   1   0   0   1    1    1    0    1    1    0    1    1
    732   1   1   1   0   1   1   0   0   1    1    1    1    1    1    0    1    1
    733   1   1   1   0   1   1   0   0   1    1    1    1    1    1    1    1    1
    734   1   1   1   0   1   1   0   1   0    0    1    1    1    1    1    0    1
    735   1   1   1   0   1   1   0   1   0    1    0    0    0    1    0    0    1
    736   1   1   1   0   1   1   0   1   0    1    0    1    1    1    0    1    1
    737   1   1   1   0   1   1   0   1   0    1    1    1    1    0    0    0    1
    738   1   1   1   0   1   1   0   1   1    0    1    0    0    0    0    0    1
    739   1   1   1   0   1   1   0   1   1    0    1    1    1    1    1    1    1
    740   1   1   1   0   1   1   0   1   1    1    0    0    0    0    1    0    1
    741   1   1   1   0   1   1   0   1   1    1    0    1    0    1    0    0    1
    742   1   1   1   0   1   1   0   1   1    1    0    1    0    1    1    1    1
    743   1   1   1   0   1   1   0   1   1    1    0    1    1    1    1    1    1
    744   1   1   1   0   1   1   0   1   1    1    1    0    1    1    1    0    1
    745   1   1   1   0   1   1   0   1   1    1    1    1    0    1    1    1    1
    746   1   1   1   0   1   1   0   1   1    1    1    1    1    1    0    1    1
    747   1   1   1   0   1   1   0   1   1    1    1    1    1    1    1    0    1
    748   1   1   1   0   1   1   0   1   1    1    1    1    1    1    1    1    1
    749   1   1   1   0   1   1   0   1   1    1    1    1    1    1    1    1    1
    750   1   1   1   0   1   1   1   0   1    1    1    1    1    1    1    1    1
    751   1   1   1   0   1   1   1   1   0    0    1    0    0    0    0    0    1
    752   1   1   1   0   1   1   1   1   0    1    1    1    0    1    1    0    1
    753   1   1   1   0   1   1   1   1   0    1    1    1    1    0    1    1    1
    754   1   1   1   0   1   1   1   1   1    1    1    1    0    1    0    0    1
    755   1   1   1   0   1   1   1   1   1    1    1    1    0    1    1    1    1
    756   1   1   1   0   1   1   1   1   1    1    1    1    1    1    1    0    1
    757   1   1   1   0   1   1   1   1   1    1    1    1    1    1    1    1    1
    758   1   1   1   1   0   0   0   0   0    1    0    1    1    1    1    0    1
    759   1   1   1   1   0   0   0   0   0    1    1    1    0    1    1    1    1
    760   1   1   1   1   0   0   0   0   0    1    1    1    1    0    0    0    1
    761   1   1   1   1   0   0   0   0   0    1    1    1    1    1    1    1    1
    762   1   1   1   1   0   0   0   0   1    1    0    1    0    1    0    0    1
    763   1   1   1   1   0   0   0   0   1    1    1    1    0    1    0    0    1
    764   1   1   1   1   0   0   0   0   1    1    1    1    0    1    1    0    1
    765   1   1   1   1   0   0   0   1   0    0    1    0    0    1    0    1    1
    766   1   1   1   1   0   0   0   1   0    1    0    0    0    0    0    0    1
    767   1   1   1   1   0   0   0   1   0    1    0    0    0    0    1    1    1
    768   1   1   1   1   0   0   0   1   0    1    0    0    0    1    0    0    1
    769   1   1   1   1   0   0   0   1   0    1    0    1    0    1    1    0    1
    770   1   1   1   1   0   0   0   1   0    1    0    1    1    0    0    1    1
    771   1   1   1   1   0   0   0   1   0    1    1    1    0    1    1    0    1
    772   1   1   1   1   0   0   0   1   0    1    1    1    1    1    1    0    1
    773   1   1   1   1   0   0   0   1   1    0    1    1    1    1    1    0    1
    774   1   1   1   1   0   0   0   1   1    1    0    1    0    0    1    0    1
    775   1   1   1   1   0   0   0   1   1    1    0    1    0    1    1    0    1
    776   1   1   1   1   0   0   0   1   1    1    0    1    1    1    1    0    1
    777   1   1   1   1   0   0   0   1   1    1    1    0    1    1    0    0    1
    778   1   1   1   1   0   0   0   1   1    1    1    1    0    0    0    0    1
    779   1   1   1   1   0   0   0   1   1    1    1    1    0    1    0    0    1
    780   1   1   1   1   0   0   0   1   1    1    1    1    0    1    1    1    1
    781   1   1   1   1   0   0   0   1   1    1    1    1    1    0    0    0    1
    782   1   1   1   1   0   0   0   1   1    1    1    1    1    0    1    0    1
    783   1   1   1   1   0   0   0   1   1    1    1    1    1    1    0    1    1
    784   1   1   1   1   0   0   1   0   0    1    1    1    0    0    1    0    1
    785   1   1   1   1   0   0   1   1   1    1    0    1    1    1    1    1    1
    786   1   1   1   1   0   0   1   1   1    1    1    0    0    1    0    1    1
    787   1   1   1   1   0   0   1   1   1    1    1    1    0    1    1    1    1
    788   1   1   1   1   0   0   1   1   1    1    1    1    0    1    1    1    1
    789   1   1   1   1   0   0   1   1   1    1    1    1    1    1    1    0    1
    790   1   1   1   1   0   1   0   0   0    1    0    1    0    1    1    1    1
    791   1   1   1   1   0   1   0   0   0    1    0    1    1    1    1    1    1
    792   1   1   1   1   0   1   0   0   0    1    1    1    0    0    0    0    1
    793   1   1   1   1   0   1   0   0   0    1    1    1    0    1    1    1    1
    794   1   1   1   1   0   1   0   0   0    1    1    1    1    1    0    1    1
    795   1   1   1   1   0   1   0   0   1    1    0    0    1    0    0    0    0
    796   1   1   1   1   0   1   0   0   1    1    1    0    1    0    0    1    1
    797   1   1   1   1   0   1   0   0   1    1    1    1    1    1    1    1    1
    798   1   1   1   1   0   1   0   1   0    0    0    1    0    1    0    0    1
    799   1   1   1   1   0   1   0   1   0    0    1    1    1    1    1    1    1
    800   1   1   1   1   0   1   0   1   0    1    0    1    0    0    0    0    1
    801   1   1   1   1   0   1   0   1   0    1    0    1    0    0    0    0    1
    802   1   1   1   1   0   1   0   1   0    1    0    1    0    1    0    0    1
    803   1   1   1   1   0   1   0   1   0    1    0    1    0    1    1    0    1
    804   1   1   1   1   0   1   0   1   0    1    0    1    1    1    0    0    1
    805   1   1   1   1   0   1   0   1   0    1    0    1    1    1    1    1    1
    806   1   1   1   1   0   1   0   1   0    1    1    1    0    1    0    1    1
    807   1   1   1   1   0   1   0   1   0    1    1    1    0    1    1    1    1
    808   1   1   1   1   0   1   0   1   0    1    1    1    1    1    0    0    1
    809   1   1   1   1   0   1   0   1   1    0    0    1    1    1    1    1    1
    810   1   1   1   1   0   1   0   1   1    0    1    1    1    1    1    1    1
    811   1   1   1   1   0   1   0   1   1    0    1    1    1    1    1    1    1
    812   1   1   1   1   0   1   0   1   1    1    0    0    0    0    1    0    1
    813   1   1   1   1   0   1   0   1   1    1    0    1    0    1    0    0    1
    814   1   1   1   1   0   1   0   1   1    1    0    1    0    1    0    1    1
    815   1   1   1   1   0   1   0   1   1    1    0    1    1    1    1    1    1
    816   1   1   1   1   0   1   0   1   1    1    1    1    0    1    0    1    1
    817   1   1   1   1   0   1   0   1   1    1    1    1    0    1    0    1    1
    818   1   1   1   1   0   1   0   1   1    1    1    1    0    1    1    0    1
    819   1   1   1   1   0   1   0   1   1    1    1    1    0    1    1    0    1
    820   1   1   1   1   0   1   0   1   1    1    1    1    0    1    1    0    1
    821   1   1   1   1   0   1   0   1   1    1    1    1    0    1    1    1    1
    822   1   1   1   1   0   1   0   1   1    1    1    1    0    1    1    1    1
    823   1   1   1   1   0   1   0   1   1    1    1    1    1    1    1    0    1
    824   1   1   1   1   0   1   0   1   1    1    1    1    1    1    1    0    1
    825   1   1   1   1   0   1   0   1   1    1    1    1    1    1    1    1    1
    826   1   1   1   1   0   1   1   0   0    1    0    1    0    1    1    1    1
    827   1   1   1   1   0   1   1   0   1    1    0    1    0    1    0    1    1
    828   1   1   1   1   0   1   1   0   1    1    0    1    0    1    1    0    1
    829   1   1   1   1   0   1   1   0   1    1    1    1    1    1    1    0    1
    830   1   1   1   1   0   1   1   1   0    1    0    1    0    0    0    1    1
    831   1   1   1   1   0   1   1   1   0    1    1    1    0    1    1    1    1
    832   1   1   1   1   0   1   1   1   0    1    1    1    0    1    1    1    1
    833   1   1   1   1   0   1   1   1   0    1    1    1    1    1    1    1    1
    834   1   1   1   1   0   1   1   1   1    0    1    1    0    1    1    1    1
    835   1   1   1   1   0   1   1   1   1    1    0    1    0    0    1    0    1
    836   1   1   1   1   0   1   1   1   1    1    0    1    0    1    1    1    1
    837   1   1   1   1   0   1   1   1   1    1    1    1    0    1    0    1    1
    838   1   1   1   1   0   1   1   1   1    1    1    1    0    1    1    1    1
    839   1   1   1   1   0   1   1   1   1    1    1    1    1    1    1    0    1
    840   1   1   1   1   0   1   1   1   1    1    1    1    1    1    1    1    1
    841   1   1   1   1   0   1   1   1   1    1    1    1    1    1    1    1    1
    842   1   1   1   1   1   0   0   0   1    0    1    1    1    1    1    0    1
    843   1   1   1   1   1   0   0   0   1    1    0    1    0    0    1    0    1
    844   1   1   1   1   1   0   0   0   1    1    1    1    0    1    1    0    1
    845   1   1   1   1   1   0   0   0   1    1    1    1    1    0    1    0    1
    846   1   1   1   1   1   0   0   1   0    1    0    1    0    0    0    1    1
    847   1   1   1   1   1   0   0   1   0    1    0    1    0    1    1    0    1
    848   1   1   1   1   1   0   0   1   0    1    1    1    0    1    0    0    1
    849   1   1   1   1   1   0   0   1   0    1    1    1    0    1    1    0    1
    850   1   1   1   1   1   0   0   1   0    1    1    1    1    1    0    0    1
    851   1   1   1   1   1   0   0   1   1    0    1    1    1    1    1    0    1
    852   1   1   1   1   1   0   0   1   1    1    0    1    0    1    0    1    1
    853   1   1   1   1   1   0   0   1   1    1    0    1    0    1    1    0    1
    854   1   1   1   1   1   0   0   1   1    1    0    1    1    0    1    1    1
    855   1   1   1   1   1   0   0   1   1    1    0    1    1    1    1    1    1
    856   1   1   1   1   1   0   0   1   1    1    1    0    0    1    1    1    1
    857   1   1   1   1   1   0   0   1   1    1    1    1    0    0    1    1    1
    858   1   1   1   1   1   0   0   1   1    1    1    1    0    1    1    0    1
    859   1   1   1   1   1   0   0   1   1    1    1    1    1    0    1    0    1
    860   1   1   1   1   1   0   0   1   1    1    1    1    1    1    0    0    1
    861   1   1   1   1   1   0   0   1   1    1    1    1    1    1    1    0    0
    862   1   1   1   1   1   0   0   1   1    1    1    1    1    1    1    0    1
    863   1   1   1   1   1   0   0   1   1    1    1    1    1    1    1    1    1
    864   1   1   1   1   1   0   1   0   1    1    1    1    1    0    1    1    1
    865   1   1   1   1   1   0   1   1   1    1    1    1    0    1    1    1    1
    866   1   1   1   1   1   0   1   1   1    1    1    1    1    1    1    1    1
    867   1   1   1   1   1   1   0   0   0    1    1    1    0    1    1    0    1
    868   1   1   1   1   1   1   0   0   1    0    1    1    0    1    1    1    1
    869   1   1   1   1   1   1   0   0   1    0    1    1    1    1    0    0    1
    870   1   1   1   1   1   1   0   0   1    1    1    1    0    1    1    1    1
    871   1   1   1   1   1   1   0   0   1    1    1    1    1    1    1    1    1
    872   1   1   1   1   1   1   0   1   0    0    0    1    1    1    1    0    1
    873   1   1   1   1   1   1   0   1   0    0    1    1    1    1    1    1    1
    874   1   1   1   1   1   1   0   1   0    1    1    1    0    0    1    0    1
    875   1   1   1   1   1   1   0   1   0    1    1    1    0    1    0    0    1
    876   1   1   1   1   1   1   0   1   0    1    1    1    0    1    0    1    1
    877   1   1   1   1   1   1   0   1   0    1    1    1    1    1    0    1    1
    878   1   1   1   1   1   1   0   1   0    1    1    1    1    1    1    0    1
    879   1   1   1   1   1   1   0   1   1    0    0    1    0    1    1    0    1
    880   1   1   1   1   1   1   0   1   1    0    1    1    0    1    1    0    1
    881   1   1   1   1   1   1   0   1   1    1    0    0    1    1    0    0    1
    882   1   1   1   1   1   1   0   1   1    1    0    1    0    1    1    0    1
    883   1   1   1   1   1   1   0   1   1    1    0    1    0    1    1    1    1
    884   1   1   1   1   1   1   0   1   1    1    0    1    1    0    1    0    1
    885   1   1   1   1   1   1   0   1   1    1    0    1    1    1    0    0    1
    886   1   1   1   1   1   1   0   1   1    1    0    1    1    1    1    0    1
    887   1   1   1   1   1   1   0   1   1    1    0    1    1    1    1    1    1
    888   1   1   1   1   1   1   0   1   1    1    1    1    0    0    1    0    1
    889   1   1   1   1   1   1   0   1   1    1    1    1    0    0    1    1    1
    890   1   1   1   1   1   1   0   1   1    1    1    1    0    1    0    0    1
    891   1   1   1   1   1   1   0   1   1    1    1    1    0    1    1    0    1
    892   1   1   1   1   1   1   0   1   1    1    1    1    0    1    1    0    1
    893   1   1   1   1   1   1   0   1   1    1    1    1    0    1    1    1    1
    894   1   1   1   1   1   1   0   1   1    1    1    1    0    1    1    1    1
    895   1   1   1   1   1   1   0   1   1    1    1    1    1    1    0    0    1
    896   1   1   1   1   1   1   0   1   1    1    1    1    1    1    1    0    1
    897   1   1   1   1   1   1   0   1   1    1    1    1    1    1    1    0    1
    898   1   1   1   1   1   1   0   1   1    1    1    1    1    1    1    1    1
    899   1   1   1   1   1   1   0   1   1    1    1    1    1    1    1    1    1
    900   1   1   1   1   1   1   1   0   0    1    1    1    0    1    1    0    1
    901   1   1   1   1   1   1   1   1   0    1    0    1    1    1    1    0    1
    902   1   1   1   1   1   1   1   1   0    1    1    1    0    0    1    0    1
    903   1   1   1   1   1   1   1   1   0    1    1    1    0    1    1    0    1
    904   1   1   1   1   1   1   1   1   0    1    1    1    1    1    1    1    1
    905   1   1   1   1   1   1   1   1   1    0    0    1    1    1    0    0    1
    906   1   1   1   1   1   1   1   1   1    0    1    1    0    0    0    0    1
    907   1   1   1   1   1   1   1   1   1    0    1    1    1    1    1    0    1
    908   1   1   1   1   1   1   1   1   1    0    1    1    1    1    1    1    1
    909   1   1   1   1   1   1   1   1   1    1    0    1    0    1    1    0    1
    910   1   1   1   1   1   1   1   1   1    1    0    1    0    1    1    0    1
    911   1   1   1   1   1   1   1   1   1    1    0    1    0    1    1    1    1
    912   1   1   1   1   1   1   1   1   1    1    0    1    1    1    1    0    1
    913   1   1   1   1   1   1   1   1   1    1    1    1    0    0    0    0    1
    914   1   1   1   1   1   1   1   1   1    1    1    1    0    0    1    0    1
    915   1   1   1   1   1   1   1   1   1    1    1    1    0    0    1    1    1
    916   1   1   1   1   1   1   1   1   1    1    1    1    0    1    1    0    1
    917   1   1   1   1   1   1   1   1   1    1    1    1    0    1    1    0    1
    918   1   1   1   1   1   1   1   1   1    1    1    1    0    1    1    1    1
    919   1   1   1   1   1   1   1   1   1    1    1    1    0    1    1    1    1
    920   1   1   1   1   1   1   1   1   1    1    1    1    1    0    0    0    1
    921   1   1   1   1   1   1   1   1   1    1    1    1    1    1    1    0    1
    922   1   1   1   1   1   1   1   1   1    1    1    1    1    1    1    1    1
    923   1   1   1   1   1   1   1   1   1    1    1    1    1    1    1    1    1
    924   1   1   1   1   1   1   1   1   1    1    1    1    1    1    1    1    1
        IT18 IT19 IT20       L0      Lz Pr(<Lz)
    1      0    0    0  -2.3921  1.3715  0.9149
    2      1    0    0  -3.7714  1.1085  0.8662
    3      1    1    0  -5.6272  1.0110   0.844
    4      1    1    1  -7.3786  1.4328   0.924
    5      0    1    0  -7.3426  0.4100  0.6591
    6      1    0    0  -9.2430 -0.2818   0.389
    7      0    0    0  -5.2581  0.5617  0.7128
    8      1    0    0  -9.2061 -0.4344   0.332
    9      1    1    1 -11.0233 -0.4808  0.3153
    10     1    0    0  -8.0206  0.3036  0.6193
    11     1    0    0  -8.8420  0.6598  0.7453
    12     0    1    1  -7.7661  0.6691  0.7483
    13     0    0    1  -9.1870 -0.1474  0.4414
    14     0    1    1  -9.6092  0.8387  0.7992
    15     0    1    0  -9.8077  0.1182   0.547
    16     0    0    0  -3.6336  1.2916  0.9017
    17     0    0    0  -5.6592  1.4315  0.9239
    18     0    1    1  -6.8529  1.7734  0.9619
    19     0    1    0  -7.7502  0.7672  0.7785
    20     0    0    1 -10.0601  0.3870  0.6506
    21     0    0    0  -6.0440  0.7873  0.7845
    22     0    1    1  -8.0271  1.5029  0.9336
    23     0    1    1 -11.4834 -0.2992  0.3824
    24     0    0    0  -5.4803  0.6865  0.7538
    25     0    1    0  -7.0257  0.6525   0.743
    26     0    0    0  -8.8264  0.1896  0.5752
    27     0    0    0  -8.5788  0.0246  0.5098
    28     0    0    0  -3.4736  1.2696  0.8979
    29     0    1    1  -6.3136  1.1624  0.8775
    30     1    0    0  -4.6428  1.0724  0.8582
    31     1    0    0  -6.3730  1.2751  0.8989
    32     0    0    0  -7.9885  0.6576  0.7446
    33     1    1    1  -9.1945  0.9568  0.8307
    34     0    1    0  -7.5546  0.5043   0.693
    35     1    1    0  -9.9012  0.9500   0.829
    36     1    0    0  -5.3828  1.1427  0.8734
    37     0    0    1  -6.5844  1.6404  0.9495
    38     0    1    1  -6.8030  2.0107  0.9778
    39     1    1    1  -7.1370  2.0145   0.978
    40     0    0    1  -8.0724  0.4911  0.6883
    41     0    1    0  -7.6958  1.5764  0.9425
    42     1    0    0  -7.9689  1.1861  0.8822
    43     1    1    1  -8.0902  1.7875  0.9631
    44     0    0    0  -8.8983  0.3428  0.6341
    45     1    1    0  -8.3952  1.1751    0.88
    46     1    0    1 -10.1333  0.0116  0.5046
    47     1    0    0 -10.0331  0.1704  0.5676
    48     0    1    0 -10.8490 -0.1069  0.4574
    49     0    0    0  -7.6414  1.1887  0.8827
    50     1    1    1  -8.2011  1.7476  0.9597
    51     1    0    1 -10.0842  0.2511  0.5991
    52     1    0    0 -11.0595  0.1111  0.5442
    53     0    1    0 -10.3993 -0.4924  0.3112
    54     1    0    1 -11.1853 -0.4360  0.3314
    55     0    1    0 -12.5815 -0.9717  0.1656
    56     0    0    0  -5.4166  0.6841  0.7531
    57     1    1    1 -10.4611  0.3536  0.6382
    58     0    0    1  -9.5355  0.1612   0.564
    59     1    1    1  -9.5137  1.1756  0.8801
    60     1    0    0 -11.2621 -0.1306   0.448
    61     0    1    1 -12.5435 -0.6142  0.2695
    62     0    0    1  -7.3102  0.5950  0.7241
    63     0    0    1  -8.1391  0.9834  0.8373
    64     0    0    0  -9.5611  0.0155  0.5062
    65     1    0    1 -13.1471 -0.7073  0.2397
    66     0    0    1  -8.0636  1.2731  0.8985
    67     1    1    1 -11.6108  0.0280  0.5112
    68     1    0    1 -11.0418  0.2489  0.5983
    69     1    1    1 -11.8105  0.0646  0.5258
    70     0    1    0  -9.2719  0.5029  0.6925
    71     0    1    1 -10.0233  1.0762  0.8591
    72     1    1    1 -10.7053  0.5370  0.7044
    73     1    0    0  -7.5503  0.0793  0.5316
    74     0    1    1 -12.7030 -0.9025  0.1834
    75     0    1    1 -12.0001 -0.2080  0.4176
    76     0    0    0 -11.5559 -0.2690   0.394
    77     0    1    1 -12.1746 -0.2161  0.4144
    78     0    0    0  -7.8018  0.3361  0.6316
    79     1    0    0 -12.2871 -1.1524  0.1246
    80     1    0    1 -12.9207 -1.3407    0.09
    81     1    1    0 -13.5454 -1.3258  0.0925
    82     1    1    0 -10.7970 -0.0958  0.4618
    83     1    1    0  -8.0232  1.0169  0.8454
    84     0    1    0  -9.2464  0.6851  0.7534
    85     0    0    1  -6.6861  0.7670  0.7785
    86     0    0    1  -9.6628  0.1890   0.575
    87     0    1    0 -13.2058 -0.7619  0.2231
    88     1    1    0 -10.3899  0.7947  0.7866
    89     1    1    0  -9.0942  1.0157  0.8451
    90     0    1    0 -10.1385  0.6779  0.7511
    91     1    0    0 -10.6777 -0.4976  0.3094
    92     1    1    0 -12.2779 -0.1869  0.4259
    93     0    0    1 -10.2272  0.4586  0.6767
    94     0    0    0  -7.0193  1.0186  0.8458
    95     0    1    0 -12.5275 -0.5742  0.2829
    96     0    1    0 -12.7418 -0.8456  0.1989
    97     0    0    1  -7.0861  0.8504  0.8024
    98     1    0    0  -9.1647  0.2785  0.6097
    99     1    1    0  -9.5720  0.8352  0.7982
    100    1    1    1  -9.4822  1.4386  0.9249
    101    0    1    1  -9.6449  1.2242  0.8896
    102    1    1    0 -10.4128  0.9135  0.8195
    103    1    1    0 -12.7115 -0.5385  0.2951
    104    1    1    1 -15.5521 -2.1170  0.0171
    105    0    1    1 -10.2840  0.7314  0.7677
    106    0    1    0 -11.5871 -0.1924  0.4237
    107    1    0    0 -11.4595 -0.2651  0.3955
    108    1    1    1 -12.0980 -0.5802  0.2809
    109    1    1    0 -13.9906 -1.2373   0.108
    110    0    0    0  -4.5434  0.8893  0.8131
    111    0    0    0 -12.3387 -1.2688  0.1023
    112    0    0    1 -11.0118  0.1891   0.575
    113    0    1    1  -8.9812  0.3741  0.6458
    114    1    0    0  -6.4074  0.7572  0.7755
    115    0    0    1  -9.5522  0.5086  0.6945
    116    1    0    1  -9.9425  0.4583  0.6766
    117    0    1    1  -9.4011  0.5377  0.7046
    118    1    1    0 -10.2210  0.9040   0.817
    119    1    1    1 -11.1633  0.4586  0.6767
    120    1    1    1  -9.6250  0.4925  0.6888
    121    0    0    1  -6.6286  0.7628  0.7772
    122    0    1    0  -7.5059  1.3239  0.9072
    123    0    1    1  -9.4881  0.4479  0.6729
    124    1    0    1  -9.6328  0.7175  0.7635
    125    0    1    1 -10.8216  0.5172  0.6975
    126    1    1    1 -10.8878  0.5434  0.7066
    127    0    1    1 -10.6107  0.7980  0.7876
    128    1    1    1 -15.6539 -2.1776  0.0147
    129    1    1    0 -10.4534  0.4419  0.6707
    130    1    1    1 -10.5455  0.8065    0.79
    131    0    0    1 -10.2542  0.4723  0.6817
    132    1    1    1 -10.7114  0.5468  0.7077
    133    0    1    1 -10.7686  0.6936   0.756
    134    0    0    0 -12.5292 -0.4171  0.3383
    135    1    1    1 -11.2541 -0.0501    0.48
    136    1    0    0 -15.7627 -2.6105  0.0045
    137    0    1    1 -12.9532 -0.6514  0.2574
    138    1    1    1 -14.7719 -1.7127  0.0434
    139    1    1    0 -14.8213 -1.6920  0.0453
    140    0    0    0 -18.1052 -3.5676  0.0002
    141    0    0    1  -7.6637  0.3243  0.6272
    142    0    1    0  -7.7902  0.4334  0.6676
    143    0    0    0  -9.0978 -0.3286  0.3712
    144    0    0    0 -11.3438 -0.2546  0.3995
    145    0    0    0 -10.3049  0.0116  0.5046
    146    0    1    0  -9.9527  0.6660  0.7473
    147    0    0    1 -10.5586  0.0861  0.5343
    148    0    1    0 -11.7042 -0.0208  0.4917
    149    0    0    0  -8.2583  0.9841  0.8375
    150    0    1    1 -10.7148  0.6763  0.7506
    151    1    0    1 -10.0283  0.7559  0.7751
    152    0    1    1 -11.2748  0.3675  0.6434
    153    1    1    1 -14.6434 -1.6923  0.0453
    154    0    1    1 -10.5159  0.6785  0.7513
    155    1    1    1 -10.8372  0.6651   0.747
    156    1    1    1 -11.5867 -0.1078  0.4571
    157    0    1    0  -9.8827  0.0601   0.524
    158    1    1    1 -11.3706  0.3508  0.6371
    159    0    1    0 -11.0741  0.5093  0.6947
    160    0    0    1 -15.2141 -1.9690  0.0245
    161    0    1    1 -13.6654 -1.0056  0.1573
    162    1    1    1 -13.3280 -0.8252  0.2046
    163    0    1    1 -12.4650 -0.4102  0.3408
    164    0    0    1  -7.3559  0.6506  0.7423
    165    1    0    0 -11.7786 -0.4767  0.3168
    166    0    0    0  -9.1606  0.9052  0.8173
    167    1    0    1  -9.7845  0.6805  0.7519
    168    0    0    0 -10.8719  0.2361  0.5933
    169    0    1    0 -10.1706  0.6648  0.7469
    170    1    1    1  -9.5250  1.4190  0.9221
    171    1    1    1  -9.3924  1.3476  0.9111
    172    0    0    1  -7.6777  0.7494  0.7732
    173    0    1    0  -9.5062  0.4865  0.6867
    174    0    0    1 -11.1338  0.2846   0.612
    175    0    0    1  -7.8629  1.6195  0.9473
    176    0    0    0  -9.1968  1.2049  0.8859
    177    1    0    0  -9.3740  1.2208  0.8889
    178    1    0    0  -9.2641  0.4841  0.6858
    179    1    0    0  -9.0372  1.1663  0.8783
    180    1    1    1 -10.1719  1.0591  0.8552
    181    0    0    1 -11.9042 -0.0791  0.4685
    182    0    1    1  -8.4120  1.9007  0.9713
    183    1    0    1 -11.0553  0.1635  0.5649
    184    1    1    1 -10.2337  0.9346   0.825
    185    1    0    1 -11.1639  0.3165  0.6242
    186    1    1    1  -9.8044  0.6484  0.7416
    187    0    0    0 -11.9767 -0.0104  0.4959
    188    0    1    1 -10.5931  0.7957  0.7869
    189    0    1    0 -11.1956  0.0265  0.5106
    190    1    1    1 -11.0354  0.5486  0.7084
    191    0    1    1 -12.2034 -0.3097  0.3784
    192    0    0    0  -9.0200  0.8752  0.8093
    193    1    1    0 -11.1536  0.4702  0.6809
    194    0    0    0 -15.2094 -2.0075  0.0223
    195    0    1    1 -11.8722 -0.1298  0.4484
    196    1    0    1 -12.4904 -0.3197  0.3746
    197    0    0    1 -11.5704  0.1505  0.5598
    198    0    1    1 -10.6317  0.3827   0.649
    199    0    1    0  -9.7096  1.3325  0.9086
    200    1    1    1  -8.8313  1.7777  0.9623
    201    0    1    1  -8.9158  1.6150  0.9468
    202    1    0    1 -11.3697  0.3391  0.6327
    203    0    0    1 -13.0048 -0.6141  0.2696
    204    1    1    1 -10.1970  0.6286  0.7352
    205    1    1    1  -9.5271 -0.2550  0.3994
    206    0    1    1 -10.7609  0.7006  0.7582
    207    0    1    1 -11.9944 -0.0372  0.4852
    208    1    1    0 -11.1508  0.4276  0.6655
    209    1    0    0 -12.9907 -0.6073  0.2718
    210    1    1    1 -12.1429 -0.1070  0.4574
    211    1    1    1 -11.6565 -0.3062  0.3797
    212    0    1    0 -14.0396 -1.2255  0.1102
    213    1    1    1 -11.4357 -0.3779  0.3528
    214    1    1    0 -11.2822 -0.1000  0.4602
    215    1    1    1  -9.8212  0.4209  0.6631
    216    1    1    1  -8.4008  0.4023  0.6563
    217    0    1    1 -11.4756 -0.1525  0.4394
    218    0    1    1 -10.7316  0.4184  0.6622
    219    0    1    1 -13.4993 -1.0864  0.1387
    220    0    0    0  -8.1852  0.4683  0.6802
    221    0    0    0 -12.2212 -0.5244     0.3
    222    0    0    0 -14.2349 -1.3443  0.0894
    223    0    1    1 -11.6320 -0.1650  0.4345
    224    0    1    0 -10.9344  0.5913  0.7228
    225    0    0    1  -9.0551  0.9609  0.8317
    226    0    1    0 -10.2048  0.2063  0.5817
    227    0    1    1  -8.3260  1.8645  0.9689
    228    1    0    0  -9.8585  0.8649  0.8064
    229    1    1    1  -9.2185  1.0208  0.8463
    230    0    1    0 -10.6645  0.6526   0.743
    231    1    1    1  -9.3250  1.5555  0.9401
    232    1    0    0 -12.1719 -0.1524  0.4394
    233    0    1    1 -12.5592 -0.5721  0.2836
    234    1    1    1 -11.2768  0.0166  0.5066
    235    0    1    1 -10.8753  0.4243  0.6643
    236    1    1    0 -12.9752 -0.8472  0.1984
    237    0    1    1  -9.9795  0.3952  0.6537
    238    0    1    1 -10.3100  0.0158  0.5063
    239    0    1    0  -9.3520  1.3024  0.9036
    240    0    1    1  -9.2584  1.5923  0.9443
    241    1    0    1 -11.5571  0.2395  0.5946
    242    1    1    1 -10.3723  0.6286  0.7352
    243    0    1    1 -11.0564  0.4842  0.6859
    244    1    0    0 -11.2785  0.4044   0.657
    245    1    1    1 -10.6893  0.6547  0.7437
    246    0    1    1 -11.4035  0.2761  0.6088
    247    1    1    1 -11.6470  0.0860  0.5342
    248    1    1    1 -10.5306  0.3781  0.6473
    249    1    1    1  -9.0362  0.2509  0.5991
    250    1    1    1 -15.7721 -2.2813  0.0113
    251    0    1    1 -13.2728 -0.9574  0.1692
    252    0    0    1 -13.9009 -1.3701  0.0853
    253    1    1    1 -10.0872 -0.2980  0.3829
    254    1    1    1  -9.0589  0.4940  0.6893
    255    0    0    1 -12.8643 -0.7495  0.2268
    256    0    1    0 -11.1426 -0.5323  0.2973
    257    1    1    1 -14.4660 -1.6628  0.0482
    258    1    1    1  -8.0785 -0.2627  0.3964
    259    0    1    0 -10.1575  0.8066  0.7901
    260    1    1    1 -10.4560  0.4794  0.6842
    261    1    1    1  -9.7237  1.3224   0.907
    262    1    1    1  -9.4551  1.4764  0.9301
    263    1    0    1 -13.5317 -1.0161  0.1548
    264    0    1    0 -10.1349  0.3296  0.6291
    265    1    1    0  -9.6412  1.1529  0.8755
    266    1    1    1 -11.2851 -0.2821  0.3889
    267    1    1    1 -11.2545  0.3691   0.644
    268    0    1    1  -8.6916  1.5135  0.9349
    269    1    1    1  -9.6508  1.3518  0.9118
    270    1    0    1 -10.9250  0.5195  0.6983
    271    0    1    1  -8.1672  1.9739  0.9758
    272    0    0    0  -9.6977  0.9420  0.8269
    273    1    1    0 -11.9500 -0.2093  0.4171
    274    1    1    1 -10.7337  0.5245     0.7
    275    0    1    1 -10.4966  0.7726  0.7801
    276    1    1    1 -12.1051 -0.3513  0.3627
    277    1    1    1 -10.1126  1.0920  0.8626
    278    1    1    1  -8.7596  1.8407  0.9672
    279    0    0    1 -10.4661  0.8843  0.8117
    280    0    1    1 -11.8191  0.0552   0.522
    281    0    1    0 -13.5767 -1.1077   0.134
    282    1    1    1  -9.7844  0.6917  0.7554
    283    1    1    1 -12.2797 -0.2898   0.386
    284    1    1    1 -11.1251  0.4967  0.6903
    285    0    1    0 -10.7133  0.6759  0.7505
    286    1    1    0 -11.2700  0.1965  0.5779
    287    0    1    1  -9.6720  0.5230  0.6995
    288    1    1    1 -11.1028  0.4254  0.6647
    289    1    1    0 -11.0090  0.5651   0.714
    290    0    1    1 -11.5579  0.1941  0.5769
    291    1    1    1  -8.8448  1.7962  0.9638
    292    0    1    1  -9.3572  1.5301   0.937
    293    1    1    0  -9.7803  1.2591   0.896
    294    0    0    1 -12.0080 -0.0254  0.4899
    295    1    1    0  -9.8970  0.6238  0.7336
    296    0    1    0 -11.4578  0.2992  0.6176
    297    1    0    0 -12.6581 -0.4109  0.3406
    298    0    1    0 -11.4377  0.3009  0.6183
    299    0    0    1 -10.6019  0.8038  0.7892
    300    1    1    1  -8.9978  1.1073  0.8659
    301    1    1    1  -8.6815  0.7140  0.7624
    302    0    1    1  -9.6763  0.4460  0.6722
    303    1    1    1  -9.2026  0.5857   0.721
    304    1    1    1  -7.9459  1.6027  0.9455
    305    1    1    1  -9.4443  0.5178  0.6977
    306    1    1    1 -13.5543 -1.2627  0.1034
    307    1    1    1 -10.9666  0.0262  0.5105
    308    1    1    0 -12.8901 -0.7711  0.2203
    309    1    1    1  -9.5641  0.3079  0.6209
    310    0    1    1 -11.4411  0.3098  0.6217
    311    1    1    1 -10.0414  0.1849  0.5734
    312    0    0    1 -11.2815  0.3860  0.6503
    313    0    1    0 -11.3263  0.3779  0.6472
    314    1    1    1  -8.9806  0.2996  0.6178
    315    1    1    1 -12.0356 -0.5325  0.2972
    316    1    1    1 -12.2906 -0.2560   0.399
    317    0    1    1 -12.5459 -0.7859   0.216
    318    1    1    1  -9.1701  0.0258  0.5103
    319    0    0    1 -13.9160 -1.4916  0.0679
    320    0    1    1 -10.7809  0.5910  0.7227
    321    0    1    0 -10.5349  0.3623  0.6414
    322    1    1    1  -7.5776  0.8027  0.7889
    323    1    1    1  -7.3899  0.5813  0.7195
    324    0    1    1  -9.7657  0.9275  0.8232
    325    1    1    1  -9.8549  1.2098  0.8868
    326    0    1    0 -11.8829  0.0484  0.5193
    327    1    1    1  -9.8583  1.1994  0.8848
    328    1    1    1 -11.0250  0.4470  0.6726
    329    0    1    1 -10.8216  0.2608  0.6029
    330    1    1    1 -10.2920  0.4060  0.6576
    331    0    1    0 -10.0132  1.0588  0.8552
    332    0    1    0  -9.9181  0.9738  0.8349
    333    1    1    0 -13.5179 -0.9760  0.1645
    334    1    1    1 -11.2078 -0.3360  0.3684
    335    0    1    0 -13.0686 -0.6665  0.2525
    336    1    1    0 -11.7045 -0.0800  0.4681
    337    0    1    0 -12.2165 -0.1638   0.435
    338    1    1    0 -12.0731 -0.5651   0.286
    339    0    0    0 -12.2482 -0.1780  0.4293
    340    1    1    0 -11.3743  0.0410  0.5163
    341    0    0    1 -12.9009 -0.5849  0.2793
    342    0    0    1 -11.8627 -0.0262  0.4895
    343    0    0    0 -12.5344 -0.4182  0.3379
    344    1    1    1  -9.4762  0.8555  0.8039
    345    0    0    0 -13.3501 -0.8449  0.1991
    346    1    1    1  -8.4072  1.4659  0.9287
    347    1    1    0 -10.4561  0.7457  0.7721
    348    0    1    1 -11.0922  0.4218  0.6634
    349    1    1    1  -7.8248  1.4772  0.9302
    350    0    0    1 -10.7736 -0.0029  0.4988
    351    0    1    1  -8.0637  0.6397  0.7388
    352    0    1    1 -10.5408 -0.3097  0.3784
    353    1    1    1 -13.5674 -1.0914  0.1376
    354    0    1    1 -10.4472 -0.3464  0.3645
    355    0    1    0 -13.1654 -1.1458  0.1259
    356    1    1    1 -10.4246  0.2837  0.6117
    357    1    1    1  -8.8940  0.9345   0.825
    358    1    1    1 -13.2884 -1.3397  0.0902
    359    0    1    1 -11.7190 -0.3340  0.3692
    360    0    1    1 -11.0954 -0.4973  0.3095
    361    1    1    1 -12.3208 -0.3882  0.3489
    362    1    1    1 -11.0668  0.2209  0.5874
    363    0    1    1 -10.9847  0.3965  0.6541
    364    1    1    0 -11.5470 -0.5020  0.3078
    365    1    1    1  -9.9364  0.7392  0.7701
    366    0    0    1 -11.8250 -0.1894  0.4249
    367    1    1    1  -8.3112  0.6034  0.7269
    368    1    0    1 -12.8148 -0.7066  0.2399
    369    1    1    1 -10.4943 -0.2660  0.3951
    370    0    1    1 -10.4991  0.1546  0.5614
    371    1    1    1  -6.9794  0.8329  0.7975
    372    1    1    0  -9.0426 -0.0226   0.491
    373    1    1    1  -6.6107  0.6702  0.7486
    374    0    1    1  -9.5581  0.0331  0.5132
    375    0    1    1  -8.0093 -0.3069  0.3795
    376    1    1    1  -6.3982  0.1271  0.5506
    377    0    1    0  -7.1455  0.6734  0.7497
    378    0    1    1 -11.8878 -0.5495  0.2913
    379    1    1    0  -9.5929  0.7179  0.7636
    380    0    1    1 -15.1350 -1.8765  0.0303
    381    1    0    0  -7.2634  0.6168  0.7313
    382    1    0    0 -10.0638  0.2927  0.6151
    383    0    1    0  -9.6343  0.6707  0.7488
    384    1    1    0 -11.5841  0.2154  0.5853
    385    1    1    1 -12.1594 -0.1385  0.4449
    386    1    1    1 -10.0555  1.0534  0.8539
    387    0    0    0  -6.3565  0.6981  0.7575
    388    1    1    0 -11.2447  0.2730  0.6076
    389    1    0    1 -13.4172 -1.1104  0.1334
    390    1    1    1 -10.4062  0.8553  0.8038
    391    1    0    0 -11.6690 -0.2415  0.4046
    392    1    0    0  -9.7198  0.9147  0.8198
    393    0    0    0  -8.9212  0.9305   0.824
    394    0    1    1 -10.2065  0.8759  0.8095
    395    0    0    0  -8.5479  0.2024  0.5802
    396    0    1    1 -11.3938  0.0032  0.5013
    397    1    1    0 -11.2086  0.3230  0.6266
    398    0    0    0 -13.7784 -1.3229  0.0929
    399    1    1    0 -14.0751 -1.2520  0.1053
    400    0    1    0 -11.9891 -0.5740   0.283
    401    1    1    1 -10.8519  0.5022  0.6922
    402    0    1    1 -12.8913 -0.5636  0.2865
    403    1    1    1 -10.3066  0.7632  0.7773
    404    0    1    1 -12.2549 -0.1837  0.4271
    405    0    1    1  -9.7881  1.1940  0.8838
    406    1    0    1 -14.4516 -1.5997  0.0548
    407    1    1    1  -9.6447  1.3168   0.906
    408    0    1    1 -11.5997  0.2160  0.5855
    409    1    1    1 -11.7227 -0.0412  0.4836
    410    1    1    1 -13.7711 -1.2415  0.1072
    411    1    1    1 -11.9203  0.0254  0.5102
    412    0    1    1 -10.5372  0.8260  0.7956
    413    0    1    0 -11.4282  0.0046  0.5019
    414    1    1    1 -10.3861  0.6077  0.7283
    415    1    1    1 -14.3848 -1.4377  0.0753
    416    0    0    0  -7.3868  0.2691  0.6061
    417    1    1    0 -11.4803  0.2689   0.606
    418    0    1    1 -11.6613  0.0607  0.5242
    419    0    1    0  -9.5896  1.0571  0.8548
    420    1    1    0  -9.7372  1.0760   0.859
    421    1    1    1  -9.7366  1.3095  0.9048
    422    0    1    1 -14.6815 -1.6456  0.0499
    423    1    0    1 -12.9709 -0.5940  0.2763
    424    1    0    0 -10.4863  0.2807  0.6105
    425    0    1    1 -11.7234 -0.1365  0.4457
    426    0    1    0 -13.6644 -1.2794  0.1004
    427    1    1    1  -9.8146  1.2653  0.8971
    428    0    1    1 -10.0187  0.9820  0.8369
    429    0    1    1 -11.0413 -0.0889  0.4646
    430    1    1    1 -10.3864  0.5276  0.7011
    431    0    1    1 -13.1935 -0.7303  0.2326
    432    1    1    1 -13.4710 -0.8941  0.1856
    433    1    0    1 -14.0516 -1.4779  0.0697
    434    1    1    0 -12.7515 -0.4776  0.3165
    435    1    1    1 -12.3574 -0.7218  0.2352
    436    1    1    0 -11.2789 -0.4231  0.3361
    437    1    1    1 -10.6511 -0.0361  0.4856
    438    1    1    1 -13.8356 -1.1554   0.124
    439    0    1    0  -8.4944  0.3606  0.6408
    440    0    1    0  -8.7426  1.1471  0.8743
    441    1    0    0 -10.8386  0.0394  0.5157
    442    0    0    1 -10.2248  0.0034  0.5013
    443    1    1    1 -12.8463 -0.6550  0.2562
    444    1    1    1  -8.6987  1.6678  0.9523
    445    1    1    1 -10.6134  0.2153  0.5852
    446    1    1    0 -11.2892  0.3989   0.655
    447    1    1    1 -12.1390 -0.6739  0.2502
    448    1    1    0 -13.2351 -0.7498  0.2267
    449    1    1    0 -15.5933 -2.1414  0.0161
    450    1    1    1 -11.4189  0.1180   0.547
    451    1    0    0 -11.4818 -0.0568  0.4774
    452    1    1    1 -10.1119  0.6545  0.7436
    453    1    0    1 -15.1554 -2.0883  0.0184
    454    0    1    1 -13.3369 -0.9455  0.1722
    455    1    0    0 -13.9128 -1.1520  0.1247
    456    0    1    1 -11.9261 -0.5712  0.2839
    457    0    1    0 -14.0524 -1.3454  0.0892
    458    1    1    1  -9.4410 -0.0532  0.4788
    459    1    0    1 -10.5156  0.2326   0.592
    460    0    1    0 -11.5665  0.2301   0.591
    461    1    0    1 -12.0109 -0.2837  0.3883
    462    1    1    1 -11.7716  0.0541  0.5216
    463    1    1    0 -10.8478  0.4440  0.6715
    464    1    1    1 -12.8301 -0.5409  0.2943
    465    1    1    1 -11.7133  0.0104  0.5041
    466    1    1    1 -11.4062 -0.4000  0.3446
    467    1    1    1 -10.0653  0.1688   0.567
    468    0    1    1 -12.2993 -0.7417  0.2291
    469    1    1    0 -10.8396  0.5138  0.6963
    470    1    0    1 -11.3733 -0.2973  0.3831
    471    0    1    1 -12.8340 -0.8699  0.1922
    472    0    1    1 -14.8268 -1.7093  0.0437
    473    0    1    1 -11.7353 -1.3025  0.0964
    474    1    1    1 -12.0866 -0.3525  0.3622
    475    1    1    1  -8.6871 -0.5746  0.2828
    476    0    0    0 -11.6116  0.0233  0.5093
    477    0    1    1  -9.3234  1.4571  0.9275
    478    1    1    1 -10.0854  0.3690  0.6439
    479    1    1    1 -10.3843  0.8506  0.8025
    480    0    1    1 -12.6639 -0.6844  0.2469
    481    1    1    1 -10.6219  0.7878  0.7846
    482    0    1    1  -9.8572  1.2336  0.8913
    483    0    1    1  -9.6262  0.6745    0.75
    484    0    1    1  -8.2872  2.0017  0.9773
    485    0    1    0  -9.9645  0.9981  0.8409
    486    0    1    1  -9.6670  1.2882  0.9012
    487    0    0    0  -9.9505  1.0175  0.8455
    488    1    0    1 -10.8667  0.5860  0.7211
    489    0    1    1  -9.3941  1.1355  0.8719
    490    0    1    0 -10.3793  0.9352  0.8252
    491    0    0    1 -12.1484 -0.4388  0.3304
    492    0    1    1 -10.3609  0.5263  0.7006
    493    1    1    0 -10.4997  0.2561  0.6011
    494    1    1    1  -8.9830  1.6102  0.9463
    495    1    1    1  -8.1142  0.5898  0.7223
    496    1    1    1 -12.5635 -0.6107  0.2707
    497    0    1    1 -10.9084 -0.2564  0.3988
    498    0    1    0 -12.5865 -0.3730  0.3546
    499    1    1    0 -10.3403  0.9242  0.8223
    500    1    1    1 -12.1426 -0.1197  0.4523
    501    1    1    1  -9.6787  0.3413  0.6336
    502    0    1    0 -12.4048 -0.5036  0.3073
    503    1    1    1 -12.9181 -0.7826  0.2169
    504    1    1    1  -8.6491  0.4917  0.6885
    505    1    0    1 -10.9425  0.4275  0.6655
    506    1    1    1  -8.9780  1.2533   0.895
    507    1    1    1  -8.8876  0.7838  0.7834
    508    0    0    1 -10.3402  0.8979  0.8154
    509    0    0    1 -11.4349  0.2494  0.5985
    510    0    1    0 -10.6453  0.5086  0.6945
    511    1    1    1  -9.3561  0.6674  0.7477
    512    0    1    1  -9.7471  0.5812  0.7195
    513    1    1    0 -10.2125 -0.0564  0.4775
    514    1    1    1  -9.0244  1.1146  0.8675
    515    0    1    1  -9.7327  0.1593  0.5633
    516    1    1    1  -9.1124  1.5326  0.9373
    517    0    0    1 -11.4834  0.0917  0.5365
    518    1    1    1 -10.0908  0.5869  0.7214
    519    1    1    1  -9.2888  1.4837  0.9311
    520    0    1    0 -11.6551 -0.2547  0.3995
    521    1    1    1  -8.8597  1.1440  0.8737
    522    1    1    1  -7.0080  1.0091  0.8435
    523    0    1    0  -9.9802 -0.1047  0.4583
    524    1    1    1  -6.7818  0.7942  0.7865
    525    0    1    1 -11.0788  0.1537  0.5611
    526    1    1    1 -11.9383 -0.3187   0.375
    527    0    1    1 -12.4216 -0.3416  0.3663
    528    0    1    1  -9.9597  0.6175  0.7316
    529    0    0    0 -13.1710 -0.7229  0.2349
    530    1    1    0 -13.0289 -0.7260  0.2339
    531    0    1    0 -10.8046  0.1463  0.5582
    532    1    1    1  -7.2750  0.4970  0.6904
    533    1    0    1 -11.1816 -0.7107  0.2386
    534    1    1    1  -7.1847  0.2800  0.6102
    535    1    1    1  -9.7194 -0.0079  0.4968
    536    1    1    0 -12.4975 -0.3576  0.3603
    537    1    1    1 -10.4498 -0.1616  0.4358
    538    0    0    1 -12.6774 -0.8854   0.188
    539    0    1    1 -10.0713  0.0984  0.5392
    540    0    1    1 -10.4716  0.4216  0.6633
    541    1    1    1  -7.8050  0.8751  0.8093
    542    1    1    0 -16.1782 -2.4847  0.0065
    543    1    1    1  -8.2494  0.4681  0.6801
    544    0    1    1  -9.9346  0.8701  0.8079
    545    1    1    1  -9.1507  0.6408  0.7392
    546    0    1    0 -13.9388 -1.5040  0.0663
    547    0    1    1  -9.4755  0.1422  0.5565
    548    1    0    1  -9.9655 -0.2651  0.3955
    549    0    1    1 -10.6598 -0.0628   0.475
    550    1    1    1  -6.6647  0.7530  0.7743
    551    1    1    1  -6.1156  0.6526   0.743
    552    1    1    1  -8.4656 -0.2464  0.4027
    553    0    1    1  -7.9184  0.0282  0.5112
    554    1    1    1  -6.1100  0.2868  0.6128
    555    1    1    1  -9.2812  1.2711  0.8982
    556    1    1    0 -11.9635 -0.4157  0.3388
    557    1    1    1  -9.9776  0.7073  0.7603
    558    1    1    1  -9.5089  0.6889  0.7546
    559    1    1    1 -10.3470  0.1592  0.5632
    560    1    0    1 -11.2253  0.4314  0.6669
    561    1    1    1 -10.5928  0.3014  0.6184
    562    1    0    0 -11.2715  0.1846  0.5732
    563    0    0    0 -11.8654 -0.0877  0.4651
    564    0    1    1  -9.3743  1.0104  0.8439
    565    1    1    1  -8.7202  0.7072  0.7603
    566    0    1    0 -11.9619 -0.3469  0.3644
    567    0    1    1  -9.7137  0.4525  0.6746
    568    1    1    0 -10.5896  0.1162  0.5462
    569    1    1    1 -11.9624 -0.9930  0.1604
    570    1    1    1  -9.3898  0.3473  0.6358
    571    1    1    1 -12.1473 -0.6458  0.2592
    572    1    1    0 -12.1533 -0.4526  0.3254
    573    1    1    1  -9.7340 -0.0076   0.497
    574    1    1    1  -8.4191  0.2154  0.5853
    575    0    0    0 -10.9689  0.2814  0.6108
    576    1    1    1 -12.2667 -0.1981  0.4215
    577    1    1    1  -8.9030  0.7226   0.765
    578    1    1    1  -9.5843  0.6687  0.7481
    579    1    1    1  -8.5102  1.1622  0.8774
    580    1    1    1  -7.8180  0.5877  0.7216
    581    0    1    1 -10.1308  0.9119  0.8191
    582    1    1    1 -10.4409  0.3603  0.6407
    583    1    1    1  -7.4581  1.2044  0.8858
    584    1    0    1 -11.8872 -0.4133  0.3397
    585    1    1    1  -8.3869  0.6753  0.7503
    586    0    1    0 -12.5656 -0.7947  0.2134
    587    0    1    1  -9.6318  0.0688  0.5274
    588    0    1    0 -11.3974  0.0888  0.5354
    589    1    1    1  -9.1026  0.5865  0.7212
    590    0    1    1  -8.9608  0.9634  0.8323
    591    0    1    1  -8.6627  0.5819  0.7197
    592    1    1    1  -7.9737  1.0688  0.8574
    593    0    1    1  -7.8823  1.4532  0.9269
    594    0    0    1 -10.1499  0.1721  0.5683
    595    0    1    1  -7.1209  0.8652  0.8065
    596    1    1    1  -6.0055  1.1363  0.8721
    597    1    1    1  -8.3856  0.6606  0.7456
    598    1    1    1  -8.3625  0.3901  0.6518
    599    1    1    1  -6.3803  1.0430  0.8515
    600    1    1    1  -5.9673  0.8901  0.8133
    601    1    1    1 -12.7035 -1.0419  0.1487
    602    1    1    1  -9.9352 -0.0243  0.4903
    603    0    1    1  -9.5806 -0.2995  0.3823
    604    1    1    1  -9.8331 -0.3243  0.3728
    605    0    1    1 -12.0690 -0.7436  0.2286
    606    1    1    1  -8.2687 -0.0001     0.5
    607    1    1    0 -15.3105 -2.0709  0.0192
    608    1    0    1 -13.4041 -1.3119  0.0948
    609    1    1    1  -8.3320  0.4738  0.6822
    610    1    1    1  -6.1356  0.4587  0.6768
    611    1    1    0 -12.4152 -1.1072  0.1341
    612    1    1    1 -11.5064  0.2544  0.6004
    613    0    1    0 -10.9197  0.4795  0.6842
    614    0    1    1 -10.0406  0.4303  0.6665
    615    1    1    1 -12.2685 -0.8333  0.2023
    616    1    1    1  -7.4516  0.5686  0.7152
    617    0    1    1 -13.1468 -1.8332  0.0334
    618    1    1    1 -12.6344 -0.9793  0.1637
    619    0    1    1 -11.3231 -1.2254  0.1102
    620    1    1    1 -12.0441 -0.6765  0.2494
    621    1    1    1  -7.9583  0.2669  0.6052
    622    1    1    1  -9.1022  0.3006  0.6182
    623    1    1    1 -10.9137  0.1736  0.5689
    624    1    1    1  -9.0538  0.3263  0.6279
    625    1    0    1 -11.1080 -0.7790   0.218
    626    0    1    1 -12.2882 -0.8315  0.2028
    627    0    1    1  -8.2932  0.3307  0.6296
    628    1    1    1  -7.4305  1.1393  0.8727
    629    0    1    1  -7.0505  0.8078  0.7904
    630    1    1    1  -5.3390  0.9857  0.8379
    631    0    0    1 -12.3565 -0.9777  0.1641
    632    1    1    1  -5.7903  0.8687  0.8075
    633    0    1    1  -6.5223  0.4504  0.6738
    634    1    1    1  -4.9997  0.8501  0.8024
    635    0    1    1 -13.4910 -1.0000  0.1587
    636    0    1    0 -11.5696 -1.2042  0.1143
    637    1    1    0 -12.7678 -0.9791  0.1638
    638    0    1    0 -12.2691 -0.9030  0.1833
    639    1    1    1 -10.2084 -0.7196  0.2359
    640    1    1    1  -8.3151  0.4328  0.6674
    641    1    1    1  -6.2100  0.6016  0.7263
    642    1    1    1  -5.3331  0.6165  0.7312
    643    0    1    0 -13.2568 -1.4536   0.073
    644    1    1    0 -11.2453 -0.9412  0.1733
    645    1    1    1  -5.8629  0.4722  0.6816
    646    1    1    1  -4.6815  0.5978   0.725
    647    0    0    0 -11.3001 -0.4804  0.3155
    648    1    1    0 -12.6056 -0.3781  0.3527
    649    0    1    1  -9.7615  0.6954  0.7566
    650    0    0    0 -15.8619 -2.3227  0.0101
    651    0    1    1  -9.1481  1.1038  0.8652
    652    0    1    1  -8.8099  1.7400  0.9591
    653    1    1    0  -9.8389  0.8455  0.8011
    654    0    1    0 -11.8031  0.0005  0.5002
    655    1    0    0 -12.0145 -0.1006  0.4599
    656    1    1    0  -9.6237  1.3694  0.9146
    657    1    1    1  -9.1875  0.7742  0.7806
    658    1    1    1 -10.4352  0.4020  0.6561
    659    1    1    1  -7.4756  1.7735  0.9619
    660    0    1    1  -8.5403  1.1541  0.8758
    661    1    1    1  -9.0895  1.1427  0.8734
    662    1    1    1  -8.0970  1.2627  0.8967
    663    0    1    0 -11.5644 -0.3209  0.3741
    664    0    1    1  -9.5402  1.2035  0.8856
    665    0    1    0 -12.8576 -0.7470  0.2275
    666    1    1    1  -9.7400  0.6615  0.7459
    667    1    1    1 -11.6070 -0.3524  0.3623
    668    1    1    1  -8.6931  0.5050  0.6932
    669    1    0    0 -11.1629  0.2510  0.5991
    670    1    1    1  -9.8247  0.8573  0.8044
    671    1    1    0 -10.4423  0.7460  0.7722
    672    0    1    1  -8.5562  1.6276  0.9482
    673    0    1    1 -10.5093  0.6500  0.7422
    674    1    1    1  -8.3981  1.6802  0.9535
    675    0    1    1  -8.1747  1.5873  0.9438
    676    1    1    1  -7.1142  1.3291  0.9081
    677    0    1    1  -8.0734  0.7910  0.7855
    678    1    1    0 -10.1602  0.2891  0.6138
    679    0    1    0 -10.3482 -0.1712   0.432
    680    1    1    0 -10.7245  0.6467  0.7411
    681    1    1    1 -11.3220 -0.0496  0.4802
    682    1    1    0 -10.0311  0.6375  0.7381
    683    1    1    1  -8.0356  0.8032  0.7891
    684    1    1    1  -8.4126  1.6532  0.9509
    685    1    1    1  -7.9795  1.2589   0.896
    686    1    1    1  -8.2123  1.1983  0.8846
    687    1    0    0 -11.5366  0.2535  0.6001
    688    1    1    1  -7.7330  0.8656  0.8066
    689    1    1    1  -5.8536  1.4894  0.9318
    690    0    1    1  -6.7341  0.9966  0.8405
    691    0    1    1 -12.3906 -1.1154  0.1323
    692    1    1    1  -5.5307  1.0326  0.8491
    693    1    1    0 -14.5395 -2.0883  0.0184
    694    1    1    1  -7.9051 -0.0617  0.4754
    695    1    1    1 -10.3553 -0.6537  0.2566
    696    1    1    1 -10.3126  0.6610  0.7457
    697    1    1    1  -9.1619  0.4165  0.6615
    698    1    1    1  -9.4496 -0.1885  0.4253
    699    1    1    0 -11.1647  0.1841   0.573
    700    1    1    1  -7.9600  0.6047  0.7273
    701    1    1    1  -7.7551  0.3948  0.6535
    702    0    1    1 -11.5075 -0.2681  0.3943
    703    1    1    1  -6.4772  0.8745  0.8091
    704    1    1    1  -5.9617  0.7603  0.7765
    705    1    1    0 -14.4757 -1.7522  0.0399
    706    1    1    1  -6.4050  0.6450  0.7405
    707    1    1    1  -5.6543  0.6153  0.7308
    708    1    1    1 -10.9898 -0.8117  0.2085
    709    1    1    0 -11.4715  0.1962  0.5778
    710    0    1    1 -14.6477 -1.6008  0.0547
    711    0    1    0 -12.0589 -0.0587  0.4766
    712    0    1    0 -12.3226 -0.2199   0.413
    713    0    0    0 -12.3370 -0.2258  0.4107
    714    0    1    1 -10.3365  0.0914  0.5364
    715    1    1    1  -8.5232  0.4481   0.673
    716    1    1    1  -9.2019 -0.2343  0.4074
    717    1    1    1  -7.9392  1.2119  0.8872
    718    1    1    1  -6.9245  1.0058  0.8427
    719    1    1    1  -6.6684  0.8010  0.7884
    720    0    1    0 -10.1957 -0.4359  0.3314
    721    1    1    1  -6.6028  0.5704  0.7158
    722    1    1    1 -11.0149 -0.7516  0.2262
    723    1    1    1 -13.1420 -1.3151  0.0942
    724    1    1    1 -11.0697 -0.2131  0.4156
    725    1    0    0 -14.8843 -2.0648  0.0195
    726    0    1    1 -10.9969 -0.2881  0.3866
    727    1    1    1  -9.0959  0.1161  0.5462
    728    1    1    1  -9.9556 -0.3614  0.3589
    729    1    1    1  -9.2332 -0.1611   0.436
    730    1    1    1 -11.5240 -0.3793  0.3522
    731    0    1    1 -12.4175 -0.8096  0.2091
    732    1    1    1  -8.9721  0.1159  0.5461
    733    1    1    1  -6.8285  0.3142  0.6233
    734    1    1    1  -9.5675 -0.0978  0.4611
    735    0    1    1 -10.5661  0.6827  0.7526
    736    1    1    1  -9.7002  0.2406  0.5951
    737    1    1    1  -9.8567  0.4975  0.6906
    738    1    0    1 -13.2350 -0.7810  0.2174
    739    1    1    1  -7.5098  0.0977  0.5389
    740    1    1    1 -10.9345  0.1130   0.545
    741    1    1    1  -8.0525  1.1491  0.8747
    742    1    1    1  -6.7646  0.7501  0.7734
    743    1    1    1  -6.6935  0.5223  0.6993
    744    1    0    1 -12.3274 -0.9551  0.1698
    745    1    1    1  -4.8817  1.1339  0.8716
    746    0    1    1  -8.2794  0.3955  0.6538
    747    1    1    1  -5.3423  1.0143  0.8448
    748    0    1    1  -6.0616  0.6006   0.726
    749    1    1    1  -4.5057  1.0106  0.8439
    750    1    1    1  -6.7884 -0.0360  0.4856
    751    1    1    1 -13.4533 -0.9855  0.1622
    752    1    1    1  -7.7394  0.3451   0.635
    753    1    1    1  -9.8674 -0.5946  0.2761
    754    1    1    1  -7.9385  0.5645  0.7138
    755    1    1    1  -4.8258  0.7818  0.7828
    756    0    1    1  -6.9306  0.2186  0.5865
    757    1    1    1  -4.1273  0.7808  0.7825
    758    0    1    1 -10.9890 -0.0052  0.4979
    759    1    1    1  -9.2891  0.3797  0.6479
    760    1    1    1 -10.5270  0.5857   0.721
    761    1    1    1  -9.6728  0.0016  0.5006
    762    0    1    1  -9.5592  1.0668   0.857
    763    0    1    1  -9.1722  1.0500  0.8531
    764    0    1    1  -8.9144  0.6449  0.7405
    765    1    1    1 -11.3124  0.2427  0.5959
    766    1    1    1  -9.1561  1.6595  0.9515
    767    1    1    1 -11.3189  0.1992  0.5789
    768    1    1    1  -9.2404  1.5496  0.9394
    769    1    1    1  -8.4641  1.1383  0.8725
    770    1    1    1 -10.5090  0.5167  0.6973
    771    0    1    0  -9.9370  0.5275  0.7011
    772    1    0    1 -10.9307 -0.1652  0.4344
    773    1    1    1  -8.9597  0.2968  0.6167
    774    1    1    0 -10.2531  0.5386  0.7049
    775    0    1    1  -8.3787  1.0180  0.8457
    776    1    1    1  -8.0034  0.8021  0.7887
    777    1    1    1  -9.8362  0.7611  0.7767
    778    0    1    1  -8.8057  1.4279  0.9233
    779    1    1    1  -7.3214  1.6521  0.9507
    780    1    1    1  -6.3655  1.0989  0.8641
    781    1    0    1 -11.2265  0.1416  0.5563
    782    1    0    1 -11.3621 -0.3067  0.3795
    783    1    1    1  -8.0054  0.8333  0.7977
    784    0    1    1 -12.0841 -0.5361   0.296
    785    1    1    1  -8.5749 -0.0506  0.4798
    786    1    1    1 -11.0715 -0.1147  0.4544
    787    0    1    1  -8.0948  0.2282  0.5903
    788    1    1    1  -6.8676  0.5410  0.7057
    789    1    1    1  -7.2791  0.4342  0.6679
    790    1    1    1  -9.3996  0.3201  0.6256
    791    1    0    1 -12.7152 -1.0949  0.1368
    792    0    1    1  -9.9260  0.9488  0.8286
    793    1    1    1  -8.2789  0.5091  0.6947
    794    1    1    1  -9.6500  0.2802  0.6104
    795    0    0    1 -14.8502 -1.7415  0.0408
    796    0    1    1 -12.4620 -0.5282  0.2987
    797    1    1    1  -6.6945  0.5437  0.7067
    798    1    1    1  -9.5903  0.9835  0.8373
    799    1    1    1  -9.0445  0.0030  0.5012
    800    1    1    0  -9.7803  1.1495  0.8748
    801    1    1    1  -8.7687  1.5579  0.9404
    802    1    1    1  -8.0781  1.5898  0.9441
    803    1    1    1  -7.7576  1.1571  0.8764
    804    1    1    1  -8.7586  1.0598  0.8554
    805    1    0    1 -11.3138 -0.5628  0.2868
    806    0    1    1  -8.4914  1.0098  0.8437
    807    0    1    1  -7.5928  0.7877  0.7846
    808    0    1    0 -10.2037  0.4474  0.6727
    809    1    1    0 -11.0741 -0.6390  0.2614
    810    1    0    1 -11.1057 -0.8235  0.2051
    811    1    1    0  -9.7878 -0.3698  0.3557
    812    1    1    1 -10.1788  0.6021  0.7265
    813    1    0    1  -9.8571  0.7271  0.7664
    814    1    1    1  -7.7077  1.1844  0.8819
    815    1    1    1  -6.4951  0.7772  0.7815
    816    0    1    0  -9.3042  0.5725  0.7165
    817    1    1    1  -6.6293  1.3243  0.9073
    818    0    1    0  -8.3678  0.6748  0.7501
    819    1    1    0  -7.4410  0.9050  0.8173
    820    1    1    1  -5.2025  1.5194  0.9357
    821    1    1    0  -7.2817  0.6710  0.7489
    822    1    1    1  -4.7542  1.3677  0.9143
    823    0    1    1  -6.4157  0.9573  0.8308
    824    1    1    1  -5.1815  1.2600  0.8962
    825    1    1    1  -4.5100  1.1933  0.8836
    826    1    1    1 -10.3666 -0.3900  0.3483
    827    1    1    1 -10.2460 -0.0901  0.4641
    828    0    1    1 -10.0533 -0.1606  0.4362
    829    1    1    1  -7.5691  0.1559  0.5619
    830    1    1    0 -12.2366 -0.4718  0.3185
    831    0    1    1  -8.2817  0.1691  0.5671
    832    1    1    0  -9.4930 -0.2182  0.4136
    833    1    1    1  -6.9039  0.2876  0.6132
    834    1    1    1  -7.7925  0.0604  0.5241
    835    1    1    1  -9.3197  0.2459  0.5971
    836    0    1    1  -8.1922  0.1772  0.5703
    837    0    1    1  -8.4348  0.3982  0.6548
    838    1    1    1  -4.8775  0.9459  0.8279
    839    1    1    1  -5.3770  0.8126  0.7918
    840    0    1    1  -6.0430  0.4209  0.6631
    841    1    1    1  -4.3467  0.8812  0.8109
    842    1    1    1 -10.5424 -0.5528  0.2902
    843    0    1    1 -11.3048 -0.1869  0.4259
    844    1    1    1  -8.0703  0.4995  0.6913
    845    0    1    1 -11.0945 -0.4784  0.3162
    846    1    0    1 -12.4752 -0.4374  0.3309
    847    0    1    1  -9.5879  0.4382  0.6694
    848    1    1    0  -9.9923  0.5740   0.717
    849    1    1    1  -7.7472  0.8059  0.7898
    850    0    1    1  -9.7928  0.4107  0.6594
    851    1    1    1  -8.7935  0.0113  0.5045
    852    0    1    0 -11.0715 -0.0641  0.4744
    853    1    1    0  -9.6228  0.2232  0.5883
    854    0    1    1 -10.7418 -0.4361  0.3314
    855    1    1    1  -7.5669  0.3104  0.6219
    856    1    1    1  -9.6087  0.0214  0.5085
    857    1    1    1  -8.2787  0.3896  0.6516
    858    1    1    1  -6.2739  1.0467  0.8524
    859    0    1    1  -9.5694  0.0498  0.5199
    860    0    1    1  -8.8862  0.5286  0.7014
    861    1    1    1 -12.1153 -1.0806  0.1399
    862    1    1    1  -6.2535  0.7958  0.7869
    863    1    1    1  -5.5829  0.7362  0.7692
    864    1    1    0 -13.1284 -1.7520  0.0399
    865    1    1    1  -5.9506  0.4898  0.6879
    866    1    1    1  -5.4207  0.4265  0.6651
    867    1    1    0 -10.2591 -0.1675  0.4335
    868    1    1    1  -8.7851 -0.1908  0.4243
    869    0    1    0 -13.0622 -1.1645  0.1221
    870    1    1    1  -6.0519  0.6469  0.7411
    871    0    1    0  -9.9413 -0.6669  0.2524
    872    1    1    1 -10.2979 -0.3116  0.3777
    873    1    1    1  -8.6146 -0.1903  0.4245
    874    1    1    1  -8.4855  0.5983  0.7252
    875    1    1    1  -7.7859  1.0967  0.8636
    876    1    1    1  -7.8636  0.7777  0.7816
    877    1    1    0 -10.1693 -0.1795  0.4288
    878    1    1    1  -6.4548  0.7342  0.7686
    879    0    1    0 -11.5186 -0.5943  0.2761
    880    1    1    1  -7.2462  0.5397  0.7053
    881    0    1    1 -11.2081 -0.0737  0.4706
    882    1    1    0  -8.6162  0.3589  0.6401
    883    0    1    1  -7.1967  0.5347  0.7036
    884    0    1    1  -9.6763 -0.0068  0.4973
    885    0    1    1  -8.9959  0.4694  0.6806
    886    1    1    1  -6.3458  0.7469  0.7724
    887    0    1    1  -7.1086  0.3163  0.6241
    888    0    1    1  -8.1754  0.5565  0.7111
    889    0    1    1  -8.0078  0.3361  0.6316
    890    1    1    1  -6.6035  1.2477  0.8939
    891    0    1    1  -5.8967  1.0165  0.8453
    892    1    1    1  -4.5658  1.3479  0.9112
    893    0    1    1  -5.2849  0.9328  0.8245
    894    1    1    1  -3.7288  1.3406    0.91
    895    1    1    0  -8.9396  0.3017  0.6185
    896    0    1    1  -5.7511  0.8107  0.7912
    897    1    1    1  -4.2490  1.2007  0.8851
    898    0    1    1  -4.8866  0.8192  0.7936
    899    1    1    1  -3.1155  1.3063  0.9043
    900    1    1    0 -11.1666 -0.8364  0.2015
    901    1    1    1  -8.6416 -0.1438  0.4428
    902    0    1    1 -10.3202 -0.3430  0.3658
    903    1    1    1  -6.9074  0.4544  0.6752
    904    1    1    1  -5.6761  0.3471  0.6357
    905    0    1    1 -12.0226 -0.9703   0.166
    906    1    1    1 -11.3151 -0.5095  0.3052
    907    1    1    1  -7.2928 -0.0898  0.4642
    908    1    1    1  -6.1660  0.0177  0.5071
    909    0    1    1  -8.0763  0.1400  0.5557
    910    1    1    0  -9.3353 -0.2576  0.3984
    911    1    1    1  -6.0324  0.4454   0.672
    912    1    1    1  -6.5338  0.3105  0.6219
    913    0    1    1 -10.1814 -0.0352  0.4859
    914    1    1    1  -7.6988  0.2437  0.5963
    915    1    1    1  -7.0985  0.1698  0.5674
    916    0    1    1  -6.1539  0.5506   0.709
    917    1    1    1  -4.5904  0.9635  0.8323
    918    0    1    1  -5.1987  0.5937  0.7236
    919    1    1    1  -3.3499  1.1106  0.8666
    920    1    1    1  -9.6233 -0.1432  0.4431
    921    1    1    1  -3.9674  0.9335  0.8247
    922    0    1    1  -4.4717  0.6034  0.7269
    923    1    1    0  -6.2754  0.0401   0.516
    924    1    1    1  -2.3457  1.2317   0.891

# 3-PL Model

The 3-PL model adds a third parameter which is a pseudo-guessing
parameter.

Formula for a 3-PL model:

$$P_{ij} (\theta_j, b_i, a_i, g_i) = g_i + (1-g_i)*(exp[a_i(\theta_j - b_i)] / 1 + exp[a_i(\theta_j - b_i)]) $$

$\theta$ = ability

$b_i$ = difficulty parameter

$g_i$ = guessing parameter

Load the data

``` r
threepl <- read.csv("3pl_example.csv")
glimpse(threepl)
```

    Rows: 1,500
    Columns: 30
    $ MC1  <int> 0, 1, 1, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,…
    $ MC2  <int> 0, 1, 1, 0, 1, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 1, 1,…
    $ MC3  <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1,…
    $ MC4  <int> 1, 0, 1, 1, 1, 0, 0, 1, 1, 1, 1, 0, 1, 0, 0, 1, 1, 0, 1, 0, 0, 0,…
    $ MC5  <int> 0, 1, 1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 0, 0, 1, 1, 1, 0, 1,…
    $ MC6  <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,…
    $ MC7  <int> 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,…
    $ MC8  <int> 1, 0, 0, 0, 1, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 0, 1, 0, 0, 1, 0,…
    $ MC9  <int> 0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 0, 0, 1, 1,…
    $ MC10 <int> 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 0, 1, 0, 1, 1, 0, 0,…
    $ MC11 <int> 1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1,…
    $ MC12 <int> 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1,…
    $ MC13 <int> 1, 1, 1, 0, 1, 0, 1, 1, 0, 1, 1, 1, 0, 1, 1, 0, 1, 1, 1, 0, 1, 1,…
    $ MC14 <int> 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 0,…
    $ MC15 <int> 0, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 0,…
    $ MC16 <int> 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1,…
    $ MC17 <int> 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 0, 1, 0, 0, 1, 1, 0, 1,…
    $ MC18 <int> 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1,…
    $ MC19 <int> 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 0, 0, 1, 0, 1,…
    $ MC20 <int> 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1,…
    $ MC21 <int> 1, 1, 1, 0, 1, 1, 0, 1, 0, 1, 0, 0, 0, 1, 1, 1, 0, 0, 1, 1, 0, 0,…
    $ MC22 <int> 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 0, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1,…
    $ MC23 <int> 0, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 0, 1, 1, 1, 0, 1, 0, 0, 1,…
    $ MC24 <int> 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1,…
    $ MC25 <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1,…
    $ MC26 <int> 1, 1, 1, 0, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 0, 1, 1, 0, 1,…
    $ MC27 <int> 1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0, 0, 0,…
    $ MC28 <int> 1, 1, 0, 1, 1, 0, 1, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1, 0, 0, 1, 1, 1,…
    $ MC29 <int> 1, 1, 0, 0, 1, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0,…
    $ MC30 <int> 0, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 0, 1, 1, 1, 0, 0, 1, 1, 1,…

This data are 1,500 responses for test-takers who either had a 0 for the
question (incorrect or no) or a 1 (correct or yes). There were 30
dichtomous questions in the dataset.

When running a 3-PL model, it is common to set a prior distribution for
the pseudo-guessing parameter to help aid in the convergence of the
model (Paek & Cole, 2020, p. 76). The standard prior used in mirt is the
normal distribution of the logit transformation of the pseudo-guessing
parameter.

``` r
spec <- 'F = 1-30
PRIOR = (1-30, g, norm, -1.1, 2)'
```

Here we are noting that the single factor consists of items 1 to 30. The
priors are the distribution of pseudo-guessing (g) that are normally
distributed around a mean of -1.1 with a standard deviation of 2 (Paek &
Cole, 2020, p. 76).

We can then calibrate the model

``` r
three_pl <- mirt(threepl,model=spec, 
                 itemtype = "3PL", 
                 SE=TRUE)
```


    Iteration: 1, Log-Lik: -24613.962, Max-Change: 3.40029
    Iteration: 2, Log-Lik: -23729.075, Max-Change: 1.36533
    Iteration: 3, Log-Lik: -23604.225, Max-Change: 0.80745
    Iteration: 4, Log-Lik: -23568.457, Max-Change: 0.76936
    Iteration: 5, Log-Lik: -23533.517, Max-Change: 0.25231
    Iteration: 6, Log-Lik: -23517.540, Max-Change: 0.20213
    Iteration: 7, Log-Lik: -23507.830, Max-Change: 0.13605
    Iteration: 8, Log-Lik: -23501.499, Max-Change: 0.13264
    Iteration: 9, Log-Lik: -23496.742, Max-Change: 0.09349
    Iteration: 10, Log-Lik: -23492.336, Max-Change: 0.09332
    Iteration: 11, Log-Lik: -23489.786, Max-Change: 0.07348
    Iteration: 12, Log-Lik: -23487.332, Max-Change: 0.07441
    Iteration: 13, Log-Lik: -23484.536, Max-Change: 0.05486
    Iteration: 14, Log-Lik: -23483.544, Max-Change: 0.03560
    Iteration: 15, Log-Lik: -23482.915, Max-Change: 0.03579
    Iteration: 16, Log-Lik: -23481.938, Max-Change: 0.03060
    Iteration: 17, Log-Lik: -23481.557, Max-Change: 0.02822
    Iteration: 18, Log-Lik: -23481.302, Max-Change: 0.02429
    Iteration: 19, Log-Lik: -23480.814, Max-Change: 0.01683
    Iteration: 20, Log-Lik: -23480.713, Max-Change: 0.01493
    Iteration: 21, Log-Lik: -23480.632, Max-Change: 0.01309
    Iteration: 22, Log-Lik: -23480.372, Max-Change: 0.00285
    Iteration: 23, Log-Lik: -23480.364, Max-Change: 0.00299
    Iteration: 24, Log-Lik: -23480.358, Max-Change: 0.00271
    Iteration: 25, Log-Lik: -23480.345, Max-Change: 0.00497
    Iteration: 26, Log-Lik: -23480.341, Max-Change: 0.00210
    Iteration: 27, Log-Lik: -23480.339, Max-Change: 0.00232
    Iteration: 28, Log-Lik: -23480.338, Max-Change: 0.00205
    Iteration: 29, Log-Lik: -23480.337, Max-Change: 0.00168
    Iteration: 30, Log-Lik: -23480.336, Max-Change: 0.00086
    Iteration: 31, Log-Lik: -23480.336, Max-Change: 0.00070
    Iteration: 32, Log-Lik: -23480.335, Max-Change: 0.00072
    Iteration: 33, Log-Lik: -23480.335, Max-Change: 0.00067
    Iteration: 34, Log-Lik: -23480.334, Max-Change: 0.00134
    Iteration: 35, Log-Lik: -23480.334, Max-Change: 0.00178
    Iteration: 36, Log-Lik: -23480.334, Max-Change: 0.00051
    Iteration: 37, Log-Lik: -23480.334, Max-Change: 0.00046
    Iteration: 38, Log-Lik: -23480.334, Max-Change: 0.00121
    Iteration: 39, Log-Lik: -23480.334, Max-Change: 0.00160
    Iteration: 40, Log-Lik: -23480.334, Max-Change: 0.00093
    Iteration: 41, Log-Lik: -23480.334, Max-Change: 0.00123
    Iteration: 42, Log-Lik: -23480.334, Max-Change: 0.00035
    Iteration: 43, Log-Lik: -23480.334, Max-Change: 0.00031
    Iteration: 44, Log-Lik: -23480.334, Max-Change: 0.00083
    Iteration: 45, Log-Lik: -23480.334, Max-Change: 0.00110
    Iteration: 46, Log-Lik: -23480.334, Max-Change: 0.00074
    Iteration: 47, Log-Lik: -23480.334, Max-Change: 0.00098
    Iteration: 48, Log-Lik: -23480.333, Max-Change: 0.00028
    Iteration: 49, Log-Lik: -23480.333, Max-Change: 0.00125
    Iteration: 50, Log-Lik: -23480.333, Max-Change: 0.00034
    Iteration: 51, Log-Lik: -23480.333, Max-Change: 0.00088
    Iteration: 52, Log-Lik: -23480.333, Max-Change: 0.00045
    Iteration: 53, Log-Lik: -23480.333, Max-Change: 0.00118
    Iteration: 54, Log-Lik: -23480.333, Max-Change: 0.00032
    Iteration: 55, Log-Lik: -23480.333, Max-Change: 0.00029
    Iteration: 56, Log-Lik: -23480.333, Max-Change: 0.00075
    Iteration: 57, Log-Lik: -23480.333, Max-Change: 0.00106
    Iteration: 58, Log-Lik: -23480.333, Max-Change: 0.00057
    Iteration: 59, Log-Lik: -23480.333, Max-Change: 0.00082
    Iteration: 60, Log-Lik: -23480.333, Max-Change: 0.00022
    Iteration: 61, Log-Lik: -23480.333, Max-Change: 0.00099
    Iteration: 62, Log-Lik: -23480.333, Max-Change: 0.00028
    Iteration: 63, Log-Lik: -23480.333, Max-Change: 0.00073
    Iteration: 64, Log-Lik: -23480.333, Max-Change: 0.00036
    Iteration: 65, Log-Lik: -23480.333, Max-Change: 0.00094
    Iteration: 66, Log-Lik: -23480.333, Max-Change: 0.00026
    Iteration: 67, Log-Lik: -23480.333, Max-Change: 0.00024
    Iteration: 68, Log-Lik: -23480.333, Max-Change: 0.00062
    Iteration: 69, Log-Lik: -23480.333, Max-Change: 0.00084
    Iteration: 70, Log-Lik: -23480.333, Max-Change: 0.00048
    Iteration: 71, Log-Lik: -23480.333, Max-Change: 0.00064
    Iteration: 72, Log-Lik: -23480.333, Max-Change: 0.00018
    Iteration: 73, Log-Lik: -23480.333, Max-Change: 0.00081
    Iteration: 74, Log-Lik: -23480.333, Max-Change: 0.00022
    Iteration: 75, Log-Lik: -23480.333, Max-Change: 0.00057
    Iteration: 76, Log-Lik: -23480.333, Max-Change: 0.00029
    Iteration: 77, Log-Lik: -23480.333, Max-Change: 0.00077
    Iteration: 78, Log-Lik: -23480.333, Max-Change: 0.00021
    Iteration: 79, Log-Lik: -23480.333, Max-Change: 0.00019
    Iteration: 80, Log-Lik: -23480.333, Max-Change: 0.00049
    Iteration: 81, Log-Lik: -23480.333, Max-Change: 0.00069
    Iteration: 82, Log-Lik: -23480.333, Max-Change: 0.00038
    Iteration: 83, Log-Lik: -23480.333, Max-Change: 0.00053
    Iteration: 84, Log-Lik: -23480.333, Max-Change: 0.00072
    Iteration: 85, Log-Lik: -23480.333, Max-Change: 0.00039
    Iteration: 86, Log-Lik: -23480.333, Max-Change: 0.00052
    Iteration: 87, Log-Lik: -23480.333, Max-Change: 0.00015
    Iteration: 88, Log-Lik: -23480.333, Max-Change: 0.00066
    Iteration: 89, Log-Lik: -23480.333, Max-Change: 0.00018
    Iteration: 90, Log-Lik: -23480.333, Max-Change: 0.00047
    Iteration: 91, Log-Lik: -23480.333, Max-Change: 0.00024
    Iteration: 92, Log-Lik: -23480.333, Max-Change: 0.00063
    Iteration: 93, Log-Lik: -23480.333, Max-Change: 0.00017
    Iteration: 94, Log-Lik: -23480.333, Max-Change: 0.00015
    Iteration: 95, Log-Lik: -23480.333, Max-Change: 0.00040
    Iteration: 96, Log-Lik: -23480.333, Max-Change: 0.00056
    Iteration: 97, Log-Lik: -23480.333, Max-Change: 0.00030
    Iteration: 98, Log-Lik: -23480.333, Max-Change: 0.00043
    Iteration: 99, Log-Lik: -23480.333, Max-Change: 0.00058
    Iteration: 100, Log-Lik: -23480.333, Max-Change: 0.00031
    Iteration: 101, Log-Lik: -23480.333, Max-Change: 0.00042
    Iteration: 102, Log-Lik: -23480.333, Max-Change: 0.00012
    Iteration: 103, Log-Lik: -23480.333, Max-Change: 0.00052
    Iteration: 104, Log-Lik: -23480.333, Max-Change: 0.00014
    Iteration: 105, Log-Lik: -23480.333, Max-Change: 0.00037
    Iteration: 106, Log-Lik: -23480.333, Max-Change: 0.00019
    Iteration: 107, Log-Lik: -23480.333, Max-Change: 0.00050
    Iteration: 108, Log-Lik: -23480.333, Max-Change: 0.00014
    Iteration: 109, Log-Lik: -23480.333, Max-Change: 0.00012
    Iteration: 110, Log-Lik: -23480.333, Max-Change: 0.00032
    Iteration: 111, Log-Lik: -23480.333, Max-Change: 0.00045
    Iteration: 112, Log-Lik: -23480.333, Max-Change: 0.00024
    Iteration: 113, Log-Lik: -23480.333, Max-Change: 0.00034
    Iteration: 114, Log-Lik: -23480.333, Max-Change: 0.00047
    Iteration: 115, Log-Lik: -23480.333, Max-Change: 0.00025
    Iteration: 116, Log-Lik: -23480.332, Max-Change: 0.00034
    Iteration: 117, Log-Lik: -23480.332, Max-Change: 0.00009

    Calculating information matrix...

We can check for convergence and other fit indices.

``` r
print(three_pl)
```


    Call:
    mirt(data = threepl, model = spec, itemtype = "3PL", SE = TRUE)

    Full-information item factor analysis with 1 factor(s).
    Converged within 1e-04 tolerance after 117 EM iterations.
    mirt version: 1.36.1 
    M-step optimizer: BFGS 
    EM acceleration: Ramsay 
    Number of rectangular quadrature: 61
    Latent density type: Gaussian 

    Information matrix estimated with method: Oakes
    Second-order test: model is a possible local maximum
    Condition number of information matrix =  1635.746

    Log-posterior = -23480.33
    Estimated parameters: 90 
    AIC = 47040.59
    BIC = 47518.78; SABIC = 47232.88
    G2 (1073741733) = 25145.69, p = 1
    RMSEA = 0, CFI = NaN, TLI = NaN

We can then look at the parameters in the output.

- a = item difficulty
- b = discimination
- g = guessing parameters
- u = is the upper asymptote parameter which is fixed in 3PL

``` r
coef(three_pl, IRTpars=TRUE)
```

    $MC1
                a      b     g  u
    par     1.527  0.062 0.325  1
    CI_2.5  1.027 -0.294 0.198 NA
    CI_97.5 2.026  0.418 0.452 NA

    $MC2
                a      b     g  u
    par     1.968 -0.135 0.082  1
    CI_2.5  1.586 -0.295 0.004 NA
    CI_97.5 2.349  0.025 0.161 NA

    $MC3
                a      b      g  u
    par     1.412 -1.542  0.150  1
    CI_2.5  1.075 -2.146 -0.192 NA
    CI_97.5 1.749 -0.938  0.491 NA

    $MC4
                a    b     g  u
    par     1.856 1.10 0.353  1
    CI_2.5  0.916 0.88 0.284 NA
    CI_97.5 2.796 1.32 0.421 NA

    $MC5
                a      b      g  u
    par     0.585 -0.583  0.088  1
    CI_2.5  0.420 -1.393 -0.112 NA
    CI_97.5 0.750  0.226  0.288 NA

    $MC6
                a      b      g  u
    par     2.388 -2.196  0.324  1
    CI_2.5  1.284 -3.344 -0.615 NA
    CI_97.5 3.491 -1.049  1.262 NA

    $MC7
                a      b     g  u
    par     2.286 -0.780 0.319  1
    CI_2.5  1.618 -1.121 0.140 NA
    CI_97.5 2.954 -0.439 0.498 NA

    $MC8
                 a     b      g  u
    par      0.572 2.628  0.195  1
    CI_2.5  -0.043 1.705 -0.008 NA
    CI_97.5  1.187 3.552  0.399 NA

    $MC9
                a     b     g  u
    par     1.503 0.553 0.375  1
    CI_2.5  0.895 0.230 0.274 NA
    CI_97.5 2.110 0.876 0.476 NA

    $MC10
                a      b     g  u
    par     1.234 -0.106 0.358  1
    CI_2.5  0.798 -0.638 0.194 NA
    CI_97.5 1.669  0.426 0.522 NA

    $MC11
                a     b     g  u
    par     2.671 0.660 0.163  1
    CI_2.5  1.919 0.539 0.116 NA
    CI_97.5 3.424 0.780 0.210 NA

    $MC12
                a      b     g  u
    par     1.748 -1.122 0.615  1
    CI_2.5  0.941 -2.051 0.347 NA
    CI_97.5 2.555 -0.193 0.882 NA

    $MC13
                a      b     g  u
    par     1.282  0.008 0.389  1
    CI_2.5  0.835 -0.463 0.248 NA
    CI_97.5 1.730  0.479 0.529 NA

    $MC14
                a     b     g  u
    par     2.333 0.798 0.125  1
    CI_2.5  1.736 0.676 0.083 NA
    CI_97.5 2.931 0.920 0.167 NA

    $MC15
                a     b     g  u
    par     1.133 1.079 0.283  1
    CI_2.5  0.626 0.744 0.183 NA
    CI_97.5 1.639 1.414 0.383 NA

    $MC16
                a      b      g  u
    par     1.201 -1.605  0.212  1
    CI_2.5  0.850 -2.571 -0.247 NA
    CI_97.5 1.552 -0.639  0.670 NA

    $MC17
                a      b      g  u
    par     1.421 -0.148  0.086  1
    CI_2.5  1.098 -0.420 -0.036 NA
    CI_97.5 1.744  0.123  0.208 NA

    $MC18
                a      b      g  u
    par     2.074 -1.194  0.237  1
    CI_2.5  1.497 -1.635 -0.034 NA
    CI_97.5 2.651 -0.753  0.508 NA

    $MC19
                a      b     g  u
    par     2.658  0.106 0.196  1
    CI_2.5  2.004 -0.037 0.128 NA
    CI_97.5 3.312  0.248 0.265 NA

    $MC20
                a      b      g  u
    par     1.613 -1.326  0.210  1
    CI_2.5  1.159 -1.956 -0.144 NA
    CI_97.5 2.067 -0.697  0.564 NA

    $MC21
                a      b     g  u
    par     1.802 -0.065 0.100  1
    CI_2.5  1.398 -0.265 0.004 NA
    CI_97.5 2.205  0.136 0.197 NA

    $MC22
                a      b      g  u
    par     1.080 -0.410  0.233  1
    CI_2.5  0.657 -1.229 -0.049 NA
    CI_97.5 1.504  0.409  0.516 NA

    $MC23
                a     b     g  u
    par     1.793 0.384 0.123  1
    CI_2.5  1.369 0.218 0.053 NA
    CI_97.5 2.217 0.549 0.193 NA

    $MC24
                a      b     g  u
    par     1.388  0.233 0.331  1
    CI_2.5  0.936 -0.116 0.216 NA
    CI_97.5 1.839  0.582 0.445 NA

    $MC25
                a      b     g  u
    par     1.626 -1.043 0.347  1
    CI_2.5  1.087 -1.704 0.053 NA
    CI_97.5 2.165 -0.382 0.641 NA

    $MC26
                a      b     g  u
    par     4.943  0.057 0.276  1
    CI_2.5  3.165 -0.051 0.221 NA
    CI_97.5 6.721  0.165 0.331 NA

    $MC27
                a     b     g  u
    par     2.823 0.201 0.211  1
    CI_2.5  2.126 0.073 0.152 NA
    CI_97.5 3.521 0.330 0.271 NA

    $MC28
                a     b     g  u
    par     1.962 1.523 0.259  1
    CI_2.5  1.127 1.303 0.218 NA
    CI_97.5 2.796 1.744 0.301 NA

    $MC29
                a     b     g  u
    par     2.076 1.862 0.175  1
    CI_2.5  1.104 1.590 0.143 NA
    CI_97.5 3.047 2.134 0.207 NA

    $MC30
                a     b     g  u
    par     1.923 0.288 0.313  1
    CI_2.5  1.286 0.044 0.219 NA
    CI_97.5 2.560 0.533 0.407 NA

    $GroupPars
            MEAN_1 COV_11
    par          0      1
    CI_2.5      NA     NA
    CI_97.5     NA     NA

ICCs for item 4, we can see the lower asymptote is not 0 since we
estimated the guessing parameter

``` r
itemplot(three_pl, 4)
```

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-55-1.png)

We can look at the test information curve

``` r
plot(three_pl, type="info")
```

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-56-1.png)

We can also look at the expected total scores by ability

``` r
plot(three_pl)
```

![](Introduction-to-Item-Response-Theory_files/figure-gfm/unnamed-chunk-57-1.png)

Model-data fit We can assess if we reject the 3-PL for this dataset

``` r
M2(three_pl)
```

               M2  df         p RMSEA RMSEA_5    RMSEA_95      SRMSR      TLI CFI
    stats 352.222 375 0.7952366     0       0 0.006506372 0.02118802 1.001596   1

We can then extract the latent trait

``` r
pl3_fs <- fscores(three_pl, method="EAP", full.scores = TRUE,
                  full.scores.SE = TRUE)
head(pl3_fs)
```

                  F      SE_F
    [1,]  0.3893076 0.3208395
    [2,]  0.9479370 0.3533298
    [3,]  0.4160670 0.3051453
    [4,] -0.4518911 0.3665721
    [5,]  2.2485636 0.5540024
    [6,]  0.5521798 0.2895993

Then we can merge with the data

``` r
threepl$theta <- pl3_fs
```

# Citations:

- De Ayala, R. J. (2013). The theory and practice of item response
  theory. Guilford Publications.
- Embretson, S. E., & Reise, S. P. (2013). Item response theory.
  Psychology Press.
- Mair, P. (2018). Modern psychometrics with R. Cham: Springer
  International Publishing.
- Paek, I., & Cole, K. (2019). Using R for item response theory model
  applications. Routledge.

# Resources

- https://www.publichealth.columbia.edu/research/population-health-methods/item-response-theory
- https://bookdown.org/bean_jerry/using_r\_for_social_work_research/item-response-theory.html
- https://statmath.wu.ac.at/people/trusch/IMPS2017/RIRT-Workshop-IMPS-2017Pt2.pdf
- https://m-clark.github.io/sem/item-response-theory.html
- https://blog.dominodatalab.com/item-response-theory-r-survey-analysis
- http://www.thetaminusb.com/intro-measurement-r/irt.html
- https://www.tandfonline.com/doi/abs/10.1080/15366367.2019.1586404?journalCode=hmes20
- https://aidenloe.github.io/irtplots.html
- https://hansjoerg.me/2018/04/23/rasch-in-r-tutorial/
- https://rstudio-pubs-static.s3.amazonaws.com/316159_1ce8a47f848043d4afc65eda4a2653c3.html
- https://rdrr.io/cran/ltm/man/factor.scores.html
