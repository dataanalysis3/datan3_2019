Data Analysis 3: Week 5
================
Alexey Bessudnov
14 February 2019

Plan for today:

-   Assignment 1: marks and feedback
-   Assignment 2
-   Tidy data and reshaping
-   Homework for next week

Exercises
---------

1.  Join individual-level substantive data for the first three waves of the Understabding Society (*indresp* files). Keep the balanced panel only (i.e. individuals who participated in all three waves). Keep only the following variables: pidp, derived sex and age, and total net personal income (*fimnnet\_dv*).

    ``` r
    library(tidyverse)
    # Read data and select variables
    Ind1 <- read_tsv("data/UKDA-6614-tab/tab/ukhls_w1/a_indresp.tab")
    Ind1 <- Ind1 %>%
      select(pidp, a_sex_dv, a_age_dv, a_fimnnet_dv)
    Ind2 <- read_tsv("data/UKDA-6614-tab/tab/ukhls_w2/b_indresp.tab")
    Ind2 <- Ind2 %>%
      select(pidp, b_sex_dv, b_age_dv, b_fimnnet_dv)
    Ind3 <- read_tsv("data/UKDA-6614-tab/tab/ukhls_w3/c_indresp.tab")
    Ind3 <- Ind3 %>%
      select(pidp, c_sex_dv, c_age_dv, c_fimnnet_dv)

    # Join three data sets together
    # I use inner_join here; in most analyses you want full_join so that you don't lose observations.
    Wide <- Ind1 %>%
      inner_join(Ind2, by = "pidp") %>%
      inner_join(Ind3, by = "pidp")
    Wide
    ```

        ## # A tibble: 31,183 x 10
        ##      pidp a_sex_dv a_age_dv a_fimnnet_dv b_sex_dv b_age_dv b_fimnnet_dv
        ##     <dbl>    <dbl>    <dbl>        <dbl>    <dbl>    <dbl>        <dbl>
        ##  1 6.80e7        1       59         802.        1       60       1277. 
        ##  2 6.80e7        2       39        1180.        2       40       1116. 
        ##  3 6.80e7        2       72         673         2       73        966. 
        ##  4 6.80e7        2       57         400         2       58         16.7
        ##  5 6.80e7        2       51        1342         2       52       1662  
        ##  6 6.80e7        1       31        3075         1       32        119. 
        ##  7 6.80e7        2       45        1700         2       46       1800  
        ##  8 6.80e7        2       72         988         2       73       1008. 
        ##  9 6.80e7        2       73        1195.        2       74       1189. 
        ## 10 6.80e7        2       36         491.        2       38        187. 
        ## # ... with 31,173 more rows, and 3 more variables: c_sex_dv <dbl>,
        ## #   c_age_dv <dbl>, c_fimnnet_dv <dbl>

2.  Calculate the average difference in monthly income between waves 1 and 2, and waves 2 and 3.

    ``` r
    Wide %>%
      # create the variables for income differences between waves 2 and 1, and 3 and 2.
      mutate(diff2_1 = b_fimnnet_dv - a_fimnnet_dv) %>%
      mutate(diff3_2 = c_fimnnet_dv - b_fimnnet_dv) %>%
      summarise(
    # Calculate mean difference
    mean2_1 = mean(diff2_1, na.rm = TRUE),
    mean3_2 = mean(diff3_2, na.rm = TRUE),
    # calculate percentage change to previous wave
    perc2_1 = mean2_1 / mean(a_fimnnet_dv, na.rm = TRUE) * 100,
    perc3_2 = mean3_2 / mean(b_fimnnet_dv, na.rm = TRUE) * 100
      )
    ```

        ## # A tibble: 1 x 4
        ##   mean2_1 mean3_2 perc2_1 perc3_2
        ##     <dbl>   <dbl>   <dbl>   <dbl>
        ## 1    60.7    83.5    4.76    6.25

3.  Estimate and interpret a linear model showing how income is associated with age and sex, for wave 1. Can you do it for all three waves of UndSoc at the same time?

    ``` r
    Wide %>%
      # first we need to clean and recode a_sex_dv
      mutate(a_sex_dv = ifelse(a_sex_dv == 2, "female",
                           ifelse(a_sex_dv == 1, "male", NA))) %>%
      # here I use the tidy function from the broom package to convert the model object to the data frame
      do(broom::tidy(lm(a_fimnnet_dv ~ a_sex_dv + a_age_dv + I(a_age_dv^2), .)))
    ```

        ## # A tibble: 4 x 5
        ##   term          estimate std.error statistic   p.value
        ##   <chr>            <dbl>     <dbl>     <dbl>     <dbl>
        ## 1 (Intercept)   -857.      49.8        -17.2 4.69e- 66
        ## 2 a_sex_dvmale   481.      14.6         32.9 5.90e-234
        ## 3 a_age_dv        87.5      2.15        40.7 0.       
        ## 4 I(a_age_dv^2)   -0.873    0.0217     -40.3 0.

    ``` r
    # broom::tidy means that I want to use the function tidy() from the package broom without explicitly attaching it 
    # we wanted to use do() to perform calcalution with the data frame that would return more than one value (for example, fir a linear model). see https://dplyr.tidyverse.org/reference/do.html

    # Alternatively, you can do:

    WideNew <-  Wide %>%
      mutate(a_sex_dv = ifelse(a_sex_dv == 2, "female",
                           ifelse(a_sex_dv == 1, "male", NA)))

    summary(lm(a_fimnnet_dv ~ a_sex_dv + a_age_dv + I(a_age_dv^2), WideNew))
    ```

        ## 
        ## Call:
        ## lm(formula = a_fimnnet_dv ~ a_sex_dv + a_age_dv + I(a_age_dv^2), 
        ##     data = WideNew)
        ## 
        ## Residuals:
        ##    Min     1Q Median     3Q    Max 
        ##  -8027   -637   -185    375  44381 
        ## 
        ## Coefficients:
        ##                 Estimate Std. Error t value Pr(>|t|)    
        ## (Intercept)   -856.94748   49.80003  -17.21   <2e-16 ***
        ## a_sex_dvmale   481.12041   14.60555   32.94   <2e-16 ***
        ## a_age_dv        87.53227    2.15259   40.66   <2e-16 ***
        ## I(a_age_dv^2)   -0.87308    0.02168  -40.28   <2e-16 ***
        ## ---
        ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
        ## 
        ## Residual standard error: 1281 on 31178 degrees of freedom
        ##   (1 observation deleted due to missingness)
        ## Multiple R-squared:  0.08092,    Adjusted R-squared:  0.08084 
        ## F-statistic: 915.1 on 3 and 31178 DF,  p-value: < 2.2e-16

4.  Reshape the data from the wide to long format (check <http://abessudnov.net/dataanalysis3/tidy-data.html>).

    ``` r
    Long <- Wide %>%
      # convert data into the "very long" format.
      gather(a_sex_dv:c_fimnnet_dv, key = "variable", value = "value") %>%
      # split the column with variable names into two (one for wave and one for fgeneric variable name)
      separate(variable, into = c("wave", "variable"), sep = "_", extra = "merge") %>%
      # convert into the format we need
      spread(key = variable, value = value)
    Long
    ```

        ## # A tibble: 93,549 x 5
        ##        pidp wave  age_dv fimnnet_dv sex_dv
        ##       <dbl> <chr>  <dbl>      <dbl>  <dbl>
        ##  1 68004087 a         59       802.      1
        ##  2 68004087 b         60      1277.      1
        ##  3 68004087 c         61       914.      1
        ##  4 68006127 a         39      1180.      2
        ##  5 68006127 b         40      1116.      2
        ##  6 68006127 c         41      1176.      2
        ##  7 68006807 a         72       673       2
        ##  8 68006807 b         73       966.      2
        ##  9 68006807 c         74      1224.      2
        ## 10 68007487 a         57       400       2
        ## # ... with 93,539 more rows

5.  Estimate a model showing how income depends on age and sex for all three waves, adding dummy variables for wave. Remember that the association between income and age is often non-linear and account for this in your models.

    ``` r
    Long %>%
      # note that now sex needs to be cleaned only once for all waves
      mutate(sex_dv = ifelse(sex_dv == 2, "female",
                           ifelse(sex_dv == 1, "male", NA))) %>%
      do(broom::tidy(lm(fimnnet_dv ~ sex_dv + age_dv + I(age_dv^2) + wave, .)))
    ```

        ## # A tibble: 6 x 5
        ##   term        estimate std.error statistic   p.value
        ##   <chr>          <dbl>     <dbl>     <dbl>     <dbl>
        ## 1 (Intercept) -770.      30.5       -25.3  2.00e-140
        ## 2 sex_dvmale   489.       8.48       57.6  0.       
        ## 3 age_dv        81.3      1.27       64.0  0.       
        ## 4 I(age_dv^2)   -0.792    0.0125    -63.2  0.       
        ## 5 waveb         55.2     10.3         5.35 8.60e-  8
        ## 6 wavec        135.      10.3        13.1  5.31e- 39

6.  In the long data set, create two variables showing income in the previous and subsequent wave. Use the *lead* and *lag* window functions from **dplyr**: <https://cran.r-project.org/web/packages/dplyr/vignettes/window-functions.html> . Summarise the differences in incomes between waves 1 and 2, and 2 and 3.

    ``` r
    Long %>%
        group_by(pidp) %>%
        mutate(lagIncome = lag(fimnnet_dv)) %>%
        mutate(leadIncome = lead(fimnnet_dv))
    ```

        ## # A tibble: 93,549 x 7
        ## # Groups:   pidp [31,183]
        ##        pidp wave  age_dv fimnnet_dv sex_dv lagIncome leadIncome
        ##       <dbl> <chr>  <dbl>      <dbl>  <dbl>     <dbl>      <dbl>
        ##  1 68004087 a         59       802.      1       NA      1277. 
        ##  2 68004087 b         60      1277.      1      802.      914. 
        ##  3 68004087 c         61       914.      1     1277.       NA  
        ##  4 68006127 a         39      1180.      2       NA      1116. 
        ##  5 68006127 b         40      1116.      2     1180.     1176. 
        ##  6 68006127 c         41      1176.      2     1116.       NA  
        ##  7 68006807 a         72       673       2       NA       966. 
        ##  8 68006807 b         73       966.      2      673      1224. 
        ##  9 68006807 c         74      1224.      2      966.       NA  
        ## 10 68007487 a         57       400       2       NA        16.7
        ## # ... with 93,539 more rows

7.  Use the three original data tables for waves 1 to 3 and combine them in the long format. (check *bind\_rows*). Now convert into the wide format.

    ``` r
    # standardising column names across waves
    colnames(Ind3)<- colnames(Ind2) <- colnames(Ind1) <- c("pidp", "sex_dv", "age_dv", "fimnnet_dv")
    Ind1 %>%
        bind_rows(Ind2) %>%
        bind_rows(Ind3) %>%
        arrange(pidp)
    ```

        ## # A tibble: 155,247 x 4
        ##       pidp sex_dv age_dv fimnnet_dv
        ##      <dbl>  <dbl>  <dbl>      <dbl>
        ##  1  280165      2     31      1728.
        ##  2  280165      2     32      2303.
        ##  3  541285      1     25       780.
        ##  4  541965      2     23      1156.
        ##  5  665045      1     29       563.
        ##  6  956765      1     55      4518.
        ##  7  956765      1     56      1347.
        ##  8  987365      2     20       668.
        ##  9  987365      2     21       497.
        ## 10 1114525      1     36      2247.
        ## # ... with 155,237 more rows

    ``` r
    # Note that this data frame is unbalanced and includes all observations (i.e. people who took part in at least one of the three waves.)
    ```
