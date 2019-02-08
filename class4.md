Data Analysis 3: Week 4
================
Alexey Bessudnov
7 February 2019

Plan for today:

-   Assignment 1
-   Relational data
-   Homework for next week

Exercises
---------

1.  Create a balanced panel of all individuals who took part in the Understanding Society from 1 to 4. (Balanced means that you only want to include the individuals who took part in ALL 4 waves.) Only keep the following variables: person's unique identifier, sex and age. Are there any inconsistencies in the data?

    ``` r
    library(tidyverse)
    # Let's read the data first (and select only the required variables so that we don't have large data frames in the memory).
    # Generally, I'd want to use iteration to read several data frames, but let's ignore this for now.
    W1 <- read_tsv("data/UKDA-6614-tab/tab/ukhls_w1/a_indresp.tab")
    W1 <- W1 %>%
      select(pidp, a_sex, a_dvage)
    W2 <- read_tsv("data/UKDA-6614-tab/tab/ukhls_w2/b_indresp.tab")
    W2 <- W2 %>%
      select(pidp, b_sex, b_dvage)
    W3 <- read_tsv("data/UKDA-6614-tab/tab/ukhls_w3/c_indresp.tab")
    W3 <- W3 %>%
      select(pidp, c_sex, c_dvage)
    W4 <- read_tsv("data/UKDA-6614-tab/tab/ukhls_w4/d_indresp.tab")
    W4 <- W4 %>%
      select(pidp, d_sex, d_dvage)

    # Joining: balanced panel
    Joined <- W1 %>%
      inner_join(W2, by = "pidp") %>%
      inner_join(W3, by = "pidp") %>%
      inner_join(W4, by = "pidp")

    # Joining: every individual who ever took part in any of the waves 1 to 4
    Joined2 <- W1 %>%
      full_join(W2, by = "pidp") %>%
      full_join(W3, by = "pidp") %>%
      full_join(W4, by = "pidp")

    # Checking consitency of coding sex across the waves
    Joined %>%
      count(a_sex, b_sex, c_sex, d_sex)
    ```

        ## # A tibble: 8 x 5
        ##   a_sex b_sex c_sex d_sex     n
        ##   <int> <int> <int> <int> <int>
        ## 1     1     1     1     1 11994
        ## 2     1     1     2     2     4
        ## 3     1     2     1     1     6
        ## 4     1     2     2     2     2
        ## 5     2     1     1     1     1
        ## 6     2     1     2     2     8
        ## 7     2     2     1     1     6
        ## 8     2     2     2     2 15168

    ``` r
    # Pulling out pidp of individuals with inconsistent coding
    Joined %>%
      mutate(sumSex = a_sex + b_sex + c_sex + d_sex) %>%
      filter(sumSex != 8 & sumSex != 4) %>%
      pull(pidp)
    ```

        ##  [1]  137865931  138826767  204696331  205945487  340924807  341221287
        ##  [7]  342528927  476172727  478333767  544308727  546148127  748746647
        ## [13]  884767055  953250531 1090552727 1157917607 1157917611 1157929847
        ## [19] 1157929851 1158542527 1225148595 1293088687 1360010895 1361179127
        ## [25] 1429544971 1565066939 1632361087

2.  Construct a table with the average number of calls per household by region in waves 1 and 8. You'll need to use the following data tables: household data from wave 1 and 8 (a\_gor\_dv and h\_gor\_dv identify region) and call records from waves 1 and 8 (callrec, a\_ivtnc and h\_ivtnc show the total number of calls).

    ``` r
    # Read data for wave 1
    # household data
    H1 <- read_tsv("data/UKDA-6614-tab/tab/ukhls_w1/a_hhresp.tab")
    # call record data
    CR1 <- read_tsv("data/UKDA-6614-tab/tab/ukhls_w1/a_callrec.tab")

    # select required variables
    H1 <- H1 %>% select(a_hidp, a_gor_dv)
    CR1ed <- CR1 %>%
      select(a_hidp, a_ivtnc) %>%
      # this aggregates data at the household level. The original data frame has calls (rather than households) as observations
      group_by(a_hidp) %>%
      summarise(
    ncalls = mean(a_ivtnc)
      )

    # Joining the data tables by household identifier. Note the use of left_join to drop households where an interview was not obtained
    Joined1 <- H1 %>%
      left_join(CR1ed, by = "a_hidp")

    # Produce an aggregated table with summary statistics by region
    Aggr1 <- Joined1 %>%
      group_by(a_gor_dv) %>%
      summarise(
    meanCall = mean(ncalls, na.rm = TRUE)
      )
    Aggr1
    ```

        ## # A tibble: 12 x 2
        ##    a_gor_dv meanCall
        ##       <int>    <dbl>
        ##  1        1 4.742534
        ##  2        2 4.437214
        ##  3        3 4.332133
        ##  4        4 4.536808
        ##  5        5 4.503695
        ##  6        6 4.537721
        ##  7        7 5.805382
        ##  8        8 4.614321
        ##  9        9 4.031657
        ## 10       10 4.136103
        ## 11       11 4.248895
        ## 12       12 0.000000

    ``` r
    # Now performing the same steps for wave 8.

    H8 <- read_tsv("data/UKDA-6614-tab/tab/ukhls_w8/h_hhresp.tab")
    CR8 <- read_tsv("data/UKDA-6614-tab/tab/ukhls_w8/h_callrec.tab")

    H8 <- H8 %>% select(h_hidp, h_gor_dv)
    CR8ed <- CR8 %>%
      select(h_hidp, h_ivtnc) %>%
      group_by(h_hidp) %>%
      summarise(
    ncalls = mean(h_ivtnc)
      )

    Joined8 <- H8 %>%
      left_join(CR8ed, by = "h_hidp")

    Aggr8 <- Joined8 %>%
      group_by(h_gor_dv) %>%
      summarise(
    meanCall = mean(ncalls, na.rm = TRUE)
      ) %>%
      # filter out observations with missing region
      filter(h_gor_dv != -9)
    Aggr8
    ```

        ## # A tibble: 12 x 2
        ##    h_gor_dv meanCall
        ##       <int>    <dbl>
        ##  1        1 5.910299
        ##  2        2 5.743811
        ##  3        3 5.874745
        ##  4        4 6.049180
        ##  5        5 6.505655
        ##  6        6 6.107971
        ##  7        7 7.435961
        ##  8        8 6.360983
        ##  9        9 5.948239
        ## 10       10 5.857847
        ## 11       11 5.935006
        ## 12       12 5.497588

    ``` r
    # at this stage you need to check in the codebook that the numerical codes for regions are the same in waves 1 and 8 (they are)

    # Joining the aggregated data tables for waves 1 and 8 using region as the key. 

    Aggr1 %>%
      full_join(Aggr8, by = c("a_gor_dv" = "h_gor_dv")) %>%
      rename(region = a_gor_dv) %>%
      rename(wave1 = meanCall.x) %>%
      rename(wave8 = meanCall.y)
    ```

        ## # A tibble: 12 x 3
        ##    region    wave1    wave8
        ##     <int>    <dbl>    <dbl>
        ##  1      1 4.742534 5.910299
        ##  2      2 4.437214 5.743811
        ##  3      3 4.332133 5.874745
        ##  4      4 4.536808 6.049180
        ##  5      5 4.503695 6.505655
        ##  6      6 4.537721 6.107971
        ##  7      7 5.805382 7.435961
        ##  8      8 4.614321 6.360983
        ##  9      9 4.031657 5.948239
        ## 10     10 4.136103 5.857847
        ## 11     11 4.248895 5.935006
        ## 12     12 0.000000 5.497588

    ``` r
    # The labels for numerical codes for regions are available here -- https://www.understandingsociety.ac.uk/documentation/mainstage/dataset-documentation/wave/1/datafile/a_hhresp/variable/a_gor_dv
    ```

3.  1.  Construct a table with the average age of women at childbirth in wave 8, the proportion of newborn children who were breastfed and the average newborn child's birthweight in kilograms (for children born between waves 7 and 8).
    2.  Split the table by ethnic group for mothers from the following groups: White British, Indian, Pakistani, Bangladeshi, and African.
    3.  Compare the results from a) with the results from wave 2.
    4.  Produce a data table that includes only twins born between waves 7 and 8. How many observations have we got? What do their fathers do for living? How many of them participated in wave 1 of the Understanding Society?

We did not have time to complete this exercise in class. I'll keep it here for future use.
