Activity 2
================

### A typical modeling process

The process that we will use for today’s activity is:

1.  Identify our research question(s),
2.  Explore (graphically and with numerical summaries) the variables of
    interest - both individually and in relationship to one another,
3.  Fit a simple linear regression model to obtain and describe model
    estimates,
4.  Assess how “good” our model is, and
5.  Predict new values.

We will continue to update/tweak/adapt this process and you are
encouraged to build your own process. Before we begin, we set up our R
session and introduce this activity’s data.

## Day 1

### The setup

We will be using two packages from Posit (formerly
[RStudio](https://posit.co/)): `{tidyverse}` and `{tidymodels}`. If you
would like to try the *ISLR* labs using these two packages instead of
base R, [Emil Hvitfeldt](https://www.emilhvitfeldt.com/) (of Posit) has
put together a [complementary online
text](https://emilhvitfeldt.github.io/ISLR-tidymodels-labs/index.html).

- In the **Packages** pane of RStudio (same area as **Files**), check to
  see if `{tidyverse}` and `{tidymodels}` are installed. Be sure to
  check both your **User Library** and **System Library**.

- If either of these are not currently listed, type the following in
  your **Console** pane, replacing `package_name` with the appropriate
  name, and press Enter/Return afterwards.

  ``` r
  # Note: the "eval = FALSE" in the above line tells R not to evaluate this code
  install.packages("package_name")
  ```

- Once you have verified that both `{tidyverse}` and `{tidymodels}` are
  installed, load these packages in the R chunk below titled `setup`.
  That is, type the following:

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.2     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ## ✔ ggplot2   3.4.4     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(tidymodels)
```

    ## ── Attaching packages ────────────────────────────────────── tidymodels 1.1.1 ──
    ## ✔ broom        1.0.5     ✔ rsample      1.2.0
    ## ✔ dials        1.2.0     ✔ tune         1.1.2
    ## ✔ infer        1.0.4     ✔ workflows    1.1.3
    ## ✔ modeldata    1.3.0     ✔ workflowsets 1.0.1
    ## ✔ parsnip      1.1.1     ✔ yardstick    1.2.0
    ## ✔ recipes      1.0.9     
    ## ── Conflicts ───────────────────────────────────────── tidymodels_conflicts() ──
    ## ✖ scales::discard() masks purrr::discard()
    ## ✖ dplyr::filter()   masks stats::filter()
    ## ✖ recipes::fixed()  masks stringr::fixed()
    ## ✖ dplyr::lag()      masks stats::lag()
    ## ✖ yardstick::spec() masks readr::spec()
    ## ✖ recipes::step()   masks stats::step()
    ## • Learn how to get started at https://www.tidymodels.org/start/

``` r
library(dplyr)
library(ggplot2)
```

- Run the `setup` code chunk and/or **knit**
  <img src="../README-img/knit-icon.png" alt="knit" width = "20"/> icon
  your Rmd document to verify that no errors occur.

![check-in](../README-img/noun-magnifying-glass.png) **Check in**

Test your GitHub skills by staging, committing, and pushing your changes
to GitHub and verify that your changes have been added to your GitHub
repository.

### The data

The data we’re working with is from the OpenIntro site:
`https://www.openintro.org/data/csv/hfi.csv`. Here is the “about” page:
<https://www.openintro.org/data/index.php?data=hfi>.

In the R code chunk below titled `load-data`, you will type the code
that reads in the above linked CSV file by doing the following:

- Rather than downloading this file, uploading to RStudio, then reading
  it in, explore how to load this file directly from the provided URL
  with `readr::read_csv` (`{readr}` is part of `{tidyverse}`).
- Assign this data set into a data frame named `hfi` (short for “Human
  Freedom Index”).

After doing this and viewing the loaded data, answer the following
questions:

``` r
#loading data
hfi<- readr::read_csv("hfi.csv")
```

    ## Rows: 1458 Columns: 123
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr   (3): ISO_code, countries, region
    ## dbl (120): year, pf_rol_procedural, pf_rol_civil, pf_rol_criminal, pf_rol, p...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
#viewing data
#view(hfi)
```

1.  What are the dimensions of the dataset? What does each row
    represent?

**123 columns/ variables and 1458 entries/rows. Row represent the
entries.**

The dataset spans a lot of years. We are only interested in data from
year 2016. In the R code chunk below titled `hfi-2016`, type the code
that does the following:

- Filter the data `hfi` data frame for year 2016, and
- Assigns the result to a data frame named `hfi_2016`.

``` r
# Filter the data for the year 2016
hfi_2016 <- hfi %>% 
  filter(year == 2016)


# Display the first few rows of the filtered data
head(hfi_2016)
```

    ## # A tibble: 6 × 123
    ##    year ISO_code countries region pf_rol_procedural pf_rol_civil pf_rol_criminal
    ##   <dbl> <chr>    <chr>     <chr>              <dbl>        <dbl>           <dbl>
    ## 1  2016 ALB      Albania   Easte…              6.66         4.55            4.67
    ## 2  2016 DZA      Algeria   Middl…             NA           NA              NA   
    ## 3  2016 AGO      Angola    Sub-S…             NA           NA              NA   
    ## 4  2016 ARG      Argentina Latin…              7.10         5.79            4.34
    ## 5  2016 ARM      Armenia   Cauca…             NA           NA              NA   
    ## 6  2016 AUS      Australia Ocean…              8.44         7.53            7.36
    ## # ℹ 116 more variables: pf_rol <dbl>, pf_ss_homicide <dbl>,
    ## #   pf_ss_disappearances_disap <dbl>, pf_ss_disappearances_violent <dbl>,
    ## #   pf_ss_disappearances_organized <dbl>,
    ## #   pf_ss_disappearances_fatalities <dbl>, pf_ss_disappearances_injuries <dbl>,
    ## #   pf_ss_disappearances <dbl>, pf_ss_women_fgm <dbl>,
    ## #   pf_ss_women_missing <dbl>, pf_ss_women_inheritance_widows <dbl>,
    ## #   pf_ss_women_inheritance_daughters <dbl>, pf_ss_women_inheritance <dbl>, …

### 1. Identify our research question(s)

The research question is often defined by you (or your company, boss,
etc.). Today’s research question/goal is to predict a country’s personal
freedom score in 2016.

For this activity we want to explore the relationship between the
personal freedom score, `pf_score`, and the political pressures and
controls on media content index,`pf_expression_control`. Specifically,
we are going to use the political pressures and controls on media
content index to predict a country’s personal freedom score in 2016.

### 2. Explore the variables of interest

Answer the following questions (use your markdown skills) and complete
the following tasks.

2.  What type of plot would you use to display the distribution of the
    personal freedom scores, `pf_score`? Would this be the same type of
    plot to display the distribution of the political pressures and
    controls on media content index, `pf_expression_control`?

- In the R code chunk below titled `univariable-plots`, type the R code
  that displays this plot for `pf_score`.
- In the R code chunk below titled `univariable-plots`, type the R code
  that displays this plot for `pf_expression_control`.

``` r
#`univariable-plots`
# Plotting pf_score
ggplot(hfi_2016, aes(x = pf_score)) +
  geom_histogram(binwidth = 1, fill = "skyblue", color = "black") +
  labs(title = "Distribution of pf_score",
       x = "pf_score",
       y = "Frequency")
```

![](activity02_files/figure-gfm/distribution-plots-1.png)<!-- -->

``` r
#`univariable-plots`
# Plotting pf_expression_control
ggplot(hfi_2016, aes(x = pf_expression_control)) +
  geom_density(fill = "skyblue") +
  labs(title = "Distribution of pf_expression_control",
       x = "pf_expression_control",
       y = "percent")
```

![](activity02_files/figure-gfm/distribution-plots-2.png)<!-- -->

4.  Comment on each of these two distributions. Be sure to describe
    their centers, spread, shape, and any potential outliers.

**Distribution of pf_score**– The distribution of scores in the image
appears to be left-skewed. The center of the distribution is near the
upper end of the range, at around 6. The spread of the distribution is
fairly wide, with scores ranging from 0 to 7.5. There are no clear
outliers in the distribution.

**Distribution of pf_expression_control**– The distribution in the image
appears to be a left-skewed normal distribution. The center of the
distribution is at around 2.5, with a long tail stretching out to the
right. The spread of the distribution is fairly wide, with values
ranging from 0 to 7.5. There are no clear outliers in the distribution.

3.  What type of plot would you use to display the relationship between
    the personal freedom score, `pf_score`, and the political pressures
    and controls on media content index,`pf_expression_control`?

**Scaterplot**

- In the R code chunk below titled `relationship-plot`, plot this
  relationship using the variable `pf_expression_control` as the
  predictor/explanatory variable.

``` r
# Scatter plot of pf_score vs. pf_expression_control
ggplot(hfi_2016, aes(x = pf_expression_control, y = pf_score, color = pf_expression_control)) +
  geom_point(position = position_jitter(width = 0.3, height = 0), size = 3) +
  labs(title = "Relationship between pf_score and pf_expression_control",
       x = "pf_expression_control",
       y = "pf_score",
       color = "pf_expression_control")
```

![](activity02_files/figure-gfm/relationship-plot-1.png)<!-- -->

4.  Does the relationship look linear? If you knew a country’s
    `pf_expression_control`, or its score out of 10, with 0 being the
    most, of political pressures and controls on media content, would
    you be comfortable using a linear model to predict the personal
    freedom score?

–The relationship between pf_expression_control and pf_scores in the
image does not appear to be perfectly linear, but there is a moderate
positive correlation. This means that as the score for political
pressures and controls on media content increases, the personal freedom
score tends to decrease, but not at a constant rate.

Therefore, using a linear model to predict the personal freedom score
from a country’s pf_expression_control would not be ideal, as it would
not capture the full complexity of the relationship. However, a linear
model could still be a useful starting point, especially if you are not
able to use a more complex model.

#### Challenge

For each plot and using your `{dplyr}` skills, obtain the appropriate
numerical summary statistics and provide more detailed descriptions of
these plots. For example, in (4) you were asked to comment on the
center, spread, shape, and potential outliers. What measures
could/should be used to describe these? You might not know of one for
each of those terms.

What numerical summary would you use to describe the relationship
between two numerical variables? (hint: explore the `cor` function from
Base R)

### 3. Fit a simple linear regression model

Regardless of your response to (4), we will continue fitting a simple
linear regression (SLR) model to these data. The code that we will be
using to fit statistical models in this course use `{tidymodels}` - an
opinionated way to fit models in R - and this is likely new to most of
you. I will provide you with example code when I do not think you should
know what to do - i.e., anything `{tidymodels}` related.

To begin, we will create a `{parsnip}` specification for a linear model.

- In the code chunk below titled `parsnip-spec`, replace “verbatim” with
  “r” just before the code chunk title.

``` r
lm_spec <- linear_reg() %>%
  set_mode("regression") %>%
  set_engine("lm")

lm_spec
```

    ## Linear Regression Model Specification (regression)
    ## 
    ## Computational engine: lm

Note that the `set_mode("regression")` is really unnecessary/redundant
as linear models (`"lm"`) can only be regression models. It is better to
be explicit as we get comfortable with this new process. Remember that
you can type `?function_name` in the R **Console** to explore a
function’s help documentation.

The above code also outputs the `lm_spec` output. This code does not do
any calculations by itself, but rather specifies what we plan to do.

Using this specification, we can now fit our model:
$\texttt{pf\score} = \beta_0 + \beta_1 \times \texttt{pf\_expression\_control} + \varepsilon$.
Note, the “\$” portion in the previous sentence is LaTeX snytex which is
a math scripting (and other scripting) language. I do not expect you to
know this, but you will become more comfortable with this. Look at your
knitted document to see how this syntax appears.

- In the code chunk below titled `fit-lm`, replace “verbatim” with “r”
  just before the code chunk title.

``` r
slr_mod <- lm_spec %>% 
  fit(pf_score ~ pf_expression_control, data = hfi_2016)

tidy(slr_mod)
```

    ## # A tibble: 2 × 5
    ##   term                  estimate std.error statistic  p.value
    ##   <chr>                    <dbl>     <dbl>     <dbl>    <dbl>
    ## 1 (Intercept)              4.28     0.149       28.8 4.23e-65
    ## 2 pf_expression_control    0.542    0.0271      20.0 2.31e-45

The above code fits our SLR model, then provides a `tidy` parameter
estimates table.

5.  Using the `tidy` output, update the below formula with the estimated
    parameters. That is, replace “intercept” and “slope” with the
    appropriate values

$\hat{\texttt{pf\score}} = 4.2838153 + 0.5418452 \times \texttt{pf\_expression\_control}$

6.  Interpret each of the estimated parameters from (5) in the context
    of this research question. That is, what do these values represent?

## Day 2

Hopefully, you were able to interpret the SLR model parameter estimates
(i.e., the *y*-intercept and slope) as follows:

> For countries with a `pf_expression_control` of 0 (those with the
> largest amount of political pressure on media content), we expect
> their mean personal freedom score to be 4.28.

> For every 1 unit increase in `pf_expression_control` (political
> pressure on media content index), we expect a country’s mean personal
> freedom score to increase 0.542 units.

### 4. Assessing

#### 4.A: Assess with your Day 1 model

To assess our model fit, we can use $R^2$ (the coefficient of
determination), the proportion of variability in the response variable
that is explained by the explanatory variable. We use `glance` from
`{broom}` (which is automatically loaded with `{tidymodels}` - `{broom}`
is also where `tidy` is from) to access this information.

- In the code chunk below titled `glance-lm`, replace “verbatim” with
  “r” just before the code chunk title.

``` r
glance(slr_mod)
```

    ## # A tibble: 1 × 12
    ##   r.squared adj.r.squared sigma statistic  p.value    df logLik   AIC   BIC
    ##       <dbl>         <dbl> <dbl>     <dbl>    <dbl> <dbl>  <dbl> <dbl> <dbl>
    ## 1     0.714         0.712 0.799      400. 2.31e-45     1  -193.  391.  400.
    ## # ℹ 3 more variables: deviance <dbl>, df.residual <int>, nobs <int>

After doing this and running the code, answer the following questions:

7.  What is the value of $R^2$ for this model?

0.7141342

8.  What does this value mean in the context of this model? Think about
    what would a “good” value of $R^2$ would be? Can/should this value
    be “perfect”?

**The value of ranges from 0 to 1, and higher values generally indicate
a better fit of the model to the data.** The value is 0.7141342 which
shows a strong relation

#### 4.B: Assess with test/train

You previously fit a model and evaluated it using the exact same data.
This is a bit of circular reasoning and does not provide much
information about the model’s performance. Now we will work through the
test/train process of fitting and assessing a simple linear regression
model.

Using the `diamonds` example provided to you in the Day 2 `README`, do
the following

- Create a new R code chunk and provide it with a descriptive tile
  (e.g., `train-test`).
- Set a seed.
- Create an initial 80-20 split of the `hfi_2016` dataset
- Using your initial split R object, assign the two splits into a
  training R object and a testing R object.

``` r
# set seed before random split
set.seed(1)

# put 80% of the data into the training set
hfi_2016_split <- initial_split(hfi_2016, prop = 0.80)

# assign the two splits to data frames - with descriptive names
hfi_2016_train <- training(hfi_2016_split)
hfi_2016_test <- testing(hfi_2016_split)

# splits
hfi_2016_split
```

    ## <Training/Testing/Total>
    ## <129/33/162>

``` r
hfi_2016_train
```

    ## # A tibble: 129 × 123
    ##     year ISO_code countries      region           pf_rol_procedural pf_rol_civil
    ##    <dbl> <chr>    <chr>          <chr>                        <dbl>        <dbl>
    ##  1  2016 IDN      Indonesia      South Asia                    4.18         4.51
    ##  2  2016 SYC      Seychelles     Sub-Saharan Afr…             NA           NA   
    ##  3  2016 DOM      Dominican Rep. Latin America &…              5.24         4.54
    ##  4  2016 BEL      Belgium        Western Europe                8.67         7.34
    ##  5  2016 FRA      France         Western Europe                6.82         7.02
    ##  6  2016 LSO      Lesotho        Sub-Saharan Afr…             NA           NA   
    ##  7  2016 BRA      Brazil         Latin America &…              4.97         5.29
    ##  8  2016 NPL      Nepal          South Asia                    3.93         4.66
    ##  9  2016 JAM      Jamaica        Latin America &…              5.79         5.44
    ## 10  2016 AUT      Austria        Western Europe                8.97         7.87
    ## # ℹ 119 more rows
    ## # ℹ 117 more variables: pf_rol_criminal <dbl>, pf_rol <dbl>,
    ## #   pf_ss_homicide <dbl>, pf_ss_disappearances_disap <dbl>,
    ## #   pf_ss_disappearances_violent <dbl>, pf_ss_disappearances_organized <dbl>,
    ## #   pf_ss_disappearances_fatalities <dbl>, pf_ss_disappearances_injuries <dbl>,
    ## #   pf_ss_disappearances <dbl>, pf_ss_women_fgm <dbl>,
    ## #   pf_ss_women_missing <dbl>, pf_ss_women_inheritance_widows <dbl>, …

Now, you will use your training dataset to fit a SLR model.

- In the code chunk below titled `train-fit-lm`, replace “verbatim” with
  “r” just before the code chunk title and update the data set to your
  training R object you just created.

``` r
slr_train <- lm_spec %>% 
  fit(pf_score ~ pf_expression_control, data = hfi_2016_train)

tidy(slr_train)
```

    ## # A tibble: 2 × 5
    ##   term                  estimate std.error statistic  p.value
    ##   <chr>                    <dbl>     <dbl>     <dbl>    <dbl>
    ## 1 (Intercept)              4.17     0.166       25.2 3.14e-51
    ## 2 pf_expression_control    0.568    0.0305      18.6 4.32e-38

Notice that you can reuse the `lm_spec` specification because we are
still doing a linear model.

9.  Using the `tidy` output, update the below formula with the estimated
    parameters. That is, replace “intercept” and “slope” with the
    appropriate values

$\hat{\texttt{pf\score}} = 4.1748130 + 0.5679447 \times \texttt{pf\_expression\_control}$

10. Interpret each of the estimated parameters from (10) in the context
    of this research question. That is, what do these values represent?

Now we will assess using the testing data set.

- In the code chunk below titled `train-fit-lm`, replace “verbatim” with
  “r” just before the code chunk title and update `data_test` to
  whatever R object you assigned your testing data to above.

``` r
test_aug <- augment(slr_train, new_data = hfi_2016_test)
test_aug
```

    ## # A tibble: 33 × 125
    ##    .pred  .resid  year ISO_code countries  region pf_rol_procedural pf_rol_civil
    ##    <dbl>   <dbl> <dbl> <chr>    <chr>      <chr>              <dbl>        <dbl>
    ##  1  7.30  0.801   2016 ARG      Argentina  Latin…              7.10         5.79
    ##  2  6.59  0.324   2016 ARM      Armenia    Cauca…             NA           NA   
    ##  3  4.32  1.36    2016 AZE      Azerbaijan Cauca…             NA           NA   
    ##  4  8.29 -0.838   2016 BHS      Bahamas    Latin…              6.93         6.01
    ##  5  6.02 -0.718   2016 BGD      Bangladesh South…              2.33         3.71
    ##  6  8.43 -0.728   2016 BRD      Barbados   Latin…              7.67         6.52
    ##  7  5.88 -0.413   2016 CAF      Central A… Sub-S…             NA           NA   
    ##  8  5.31  1.47    2016 COG      Congo, Re… Sub-S…             NA           NA   
    ##  9  7.16 -0.0940  2016 CIV      Cote d'Iv… Sub-S…              2.99         5.19
    ## 10  9.29 -0.273   2016 EST      Estonia    Easte…              8.77         7.85
    ## # ℹ 23 more rows
    ## # ℹ 117 more variables: pf_rol_criminal <dbl>, pf_rol <dbl>,
    ## #   pf_ss_homicide <dbl>, pf_ss_disappearances_disap <dbl>,
    ## #   pf_ss_disappearances_violent <dbl>, pf_ss_disappearances_organized <dbl>,
    ## #   pf_ss_disappearances_fatalities <dbl>, pf_ss_disappearances_injuries <dbl>,
    ## #   pf_ss_disappearances <dbl>, pf_ss_women_fgm <dbl>,
    ## #   pf_ss_women_missing <dbl>, pf_ss_women_inheritance_widows <dbl>, …

This takes your SLR model and applies it to your testing data.

![check-in](../README-img/noun-magnifying-glass.png) **Check in**

Look at the various information produced by this code. Can you identify
what each column represents?

The `.fitted` column in this output can also be obtained by using
`predict` (i.e., `predict(slr_fit, new_data = data_test)`)

11. Now, using your responses to (7) and (8) as an example, assess how
    well your model fits your testing data. Compare your results here to
    your results from your Day 1 of this activity. Did this model
    perform any differently?

different

### Model diagnostics

To assess whether the linear model is reliable, we should check for (1)
linearity, (2) nearly normal residuals, and (3) constant variability.
Note that the normal residuals is not really necessary for all models
(sometimes we simply want to describe a relationship for the data that
we have or population-level data, where statistical inference is not
appropriate/necessary).

In order to do these checks we need access to the fitted (predicted)
values and the residuals. We can use `broom::augment` to calculate
these.

- In the code chunk below titled `augment`, replace “verbatim” with “r”
  just before the code chunk title and update `data_test` to whatever R
  object you assigned your testing data to above.

``` r
train_aug <- augment(slr_train, new_data =hfi_2016_train)

train_aug
```

    ## # A tibble: 129 × 125
    ##    .pred  .resid  year ISO_code countries  region pf_rol_procedural pf_rol_civil
    ##    <dbl>   <dbl> <dbl> <chr>    <chr>      <chr>              <dbl>        <dbl>
    ##  1  7.30 -0.917   2016 IDN      Indonesia  South…              4.18         4.51
    ##  2  7.58 -0.210   2016 SYC      Seychelles Sub-S…             NA           NA   
    ##  3  6.87  0.0700  2016 DOM      Dominican… Latin…              5.24         4.54
    ##  4  9.43 -0.441   2016 BEL      Belgium    Weste…              8.67         7.34
    ##  5  8.01  0.758   2016 FRA      France     Weste…              6.82         7.02
    ##  6  6.73 -0.0421  2016 LSO      Lesotho    Sub-S…             NA           NA   
    ##  7  6.73 -0.0646  2016 BRA      Brazil     Latin…              4.97         5.29
    ##  8  6.87  0.0391  2016 NPL      Nepal      South…              3.93         4.66
    ##  9  8.58 -1.31    2016 JAM      Jamaica    Latin…              5.79         5.44
    ## 10  8.72  0.529   2016 AUT      Austria    Weste…              8.97         7.87
    ## # ℹ 119 more rows
    ## # ℹ 117 more variables: pf_rol_criminal <dbl>, pf_rol <dbl>,
    ## #   pf_ss_homicide <dbl>, pf_ss_disappearances_disap <dbl>,
    ## #   pf_ss_disappearances_violent <dbl>, pf_ss_disappearances_organized <dbl>,
    ## #   pf_ss_disappearances_fatalities <dbl>, pf_ss_disappearances_injuries <dbl>,
    ## #   pf_ss_disappearances <dbl>, pf_ss_women_fgm <dbl>,
    ## #   pf_ss_women_missing <dbl>, pf_ss_women_inheritance_widows <dbl>, …

**Linearity**: You already checked if the relationship between
`pf_score` and `pf_expression_control` is linear using a scatterplot. We
should also verify this condition with a plot of the residuals
vs. fitted (predicted) values.

- In the code chunk below titled `fitted-residual`, replace “verbatim”
  with “r” just before the code chunk title.

``` r
ggplot(data = train_aug, aes(x = .pred, y = .resid)) +
  geom_point() +
  geom_hline(yintercept = 0, linetype = "dashed", color = "red") +
  xlab("Fitted values") +
  ylab("Residuals")
```

![](activity02_files/figure-gfm/fitted-residual-1.png)<!-- -->

Notice here that `train_aug` can also serve as a data set because stored
within it are the fitted values ($\hat{y}$) and the residuals. Also note
that we are getting fancy with the code here. After creating the
scatterplot on the first layer (first line of code), we overlay a red
horizontal dashed line at $y = 0$ (to help us check whether the
residuals are distributed around 0), and we also rename the axis labels
to be more informative.

Answer the following question:

11. Is there any apparent pattern in the residuals plot? What does this
    indicate about the linearity of the relationship between the two
    variables?

Dispersed points show relationship

**Nearly normal residuals**: To check this condition, we can look at a
histogram of the residuals.

- In the code chunk below titled `residual-histogram`, replace
  “verbatim” with “r” just before the code chunk title.

``` r
ggplot(data = train_aug, aes(x = .resid)) +
  geom_histogram(binwidth = 0.25) +
  xlab("Residuals")
```

![](activity02_files/figure-gfm/residual_histogram-1.png)<!-- -->

Answer the following question:

12. Based on the histogram, does the nearly normal residuals condition
    appear to be violated? Why or why not? No because residuals are
    normally distributed

**Constant variability**:

13. Based on the residuals vs. fitted plot, does the constant
    variability condition appear to be violated? Why or why not?

## Attribution

This document is based on labs from
[OpenIntro](https://www.openintro.org/).

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png"
style="border-width:0" alt="Creative Commons License" /></a><br />This
work is licensed under a
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative
Commons Attribution-ShareAlike 4.0 International License</a>.
