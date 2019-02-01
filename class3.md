Data Analysis 3: Class 3
================
Alexey Bessudnov
31 January 2019

Plan for today:

-   Test assignment.
-   The **tidyverse** framework (<https://www.tidyverse.org/>).
-   Reading in data with **readr**.
-   Transforming data with **dplyr**.
-   Statistical assignment 1: questions.
-   Homework for next week.

Importing data: read ch.11 from R for Data Science (Data import): <https://r4ds.had.co.nz/data-import.html> and ch.2 from my website (Read data): <http://abessudnov.net/dataanalysis3/readdata.html>.

``` r
library(tidyverse)
Data <- read_tsv("data/UKDA-6614-tab/tab/ukhls_wx/xwavedat.tab")
```

This is a cross-wave data file with stable characteristics of individuals. See the codebook at <https://www.understandingsociety.ac.uk/documentation/mainstage/dataset-documentation/wave/xwave/datafile/xwavedat>.

See the dplyr cheetsheet: <https://github.com/rstudio/cheatsheets/blob/master/data-transformation.pdf>

Exercises.

1.  Select the variables for: sex (derived), date of birth (derived), ethnic group (racel\_dv). Also keep the cross-wave identifier (pidp) and the sample origin variable (memorig).

    ``` r
    # With base R:
    subset(Data, select = c("pidp", "memorig", "sex_dv", "doby_dv", "racel_dv"))
    ```

        ## # A tibble: 145,779 x 5
        ##     pidp memorig sex_dv doby_dv racel_dv
        ##    <dbl>   <dbl>  <dbl>   <dbl>    <dbl>
        ##  1   687       3    -20     -20        1
        ##  2  1367       3    -20     -20       15
        ##  3  2051       3    -20     -20       15
        ##  4  2727       3    -20     -20       97
        ##  5  3407       3    -20     -20        1
        ##  6  4091       3    -20     -20        1
        ##  7  4767       3    -20     -20        1
        ##  8  5451       3    -20     -20        1
        ##  9  6135       3    -20     -20        1
        ## 10  6807       3    -20     -20        1
        ## # ... with 145,769 more rows

    ``` r
    Data[,c(1, 5, 15, 19)]
    ```

        ## # A tibble: 145,779 x 4
        ##     pidp memorig sex_dv dcsedw_dv
        ##    <dbl>   <dbl>  <dbl>     <dbl>
        ##  1   687       3    -20       -20
        ##  2  1367       3    -20       -20
        ##  3  2051       3    -20       -20
        ##  4  2727       3    -20       -20
        ##  5  3407       3    -20       -20
        ##  6  4091       3    -20       -20
        ##  7  4767       3    -20       -20
        ##  8  5451       3    -20       -20
        ##  9  6135       3    -20       -20
        ## 10  6807       3    -20       -20
        ## # ... with 145,769 more rows

    ``` r
    # with dplyr
    select(Data, pidp, memorig, sex_dv, doby_dv, racel_dv)
    ```

        ## # A tibble: 145,779 x 5
        ##     pidp memorig sex_dv doby_dv racel_dv
        ##    <dbl>   <dbl>  <dbl>   <dbl>    <dbl>
        ##  1   687       3    -20     -20        1
        ##  2  1367       3    -20     -20       15
        ##  3  2051       3    -20     -20       15
        ##  4  2727       3    -20     -20       97
        ##  5  3407       3    -20     -20        1
        ##  6  4091       3    -20     -20        1
        ##  7  4767       3    -20     -20        1
        ##  8  5451       3    -20     -20        1
        ##  9  6135       3    -20     -20        1
        ## 10  6807       3    -20     -20        1
        ## # ... with 145,769 more rows

    ``` r
    # using a pipe (%>%)

    Data <- Data %>%
      select(pidp, memorig, sex_dv, doby_dv, racel_dv)
    ```

2.  Filter the data to keep (in new data frames): a) men only. b) people born before 1950 and after 1975. c) men of Pakistani origin born in 1958 or 1982.

    ``` r
    # a)
    Data %>%
      filter(sex_dv == 1)
    ```

        ## # A tibble: 58,304 x 5
        ##      pidp memorig sex_dv doby_dv racel_dv
        ##     <dbl>   <dbl>  <dbl>   <dbl>    <dbl>
        ##  1 184965       3      1    1982        1
        ##  2 223725       3      1    1975        1
        ##  3 261125       3      1    1985        1
        ##  4 299885       3      1    1980        1
        ##  5 496405       3      1    1975        1
        ##  6 537205       3      1    1975        1
        ##  7 541285       3      1    1985        1
        ##  8 665045       3      1    1981        1
        ##  9 732365       3      1    1985        1
        ## 10 813285       3      1    1970        1
        ## # ... with 58,294 more rows

    ``` r
    # b)
    Data %>%
      filter(doby_dv > 0 & (doby_dv < 1950 |  doby_dv > 1975))
    ```

        ## # A tibble: 81,765 x 5
        ##      pidp memorig sex_dv doby_dv racel_dv
        ##     <dbl>   <dbl>  <dbl>   <dbl>    <dbl>
        ##  1  22445       3      2    1984        1
        ##  2  29925       3      2    1977        1
        ##  3  76165       3      2    1982        1
        ##  4 184965       3      1    1982        1
        ##  5 261125       3      1    1985        1
        ##  6 280165       3      2    1979        1
        ##  7 299885       3      1    1980        1
        ##  8 333205       3      2    1990        1
        ##  9 387605       3      2    1988        1
        ## 10 469205       3      2    1990       -9
        ## # ... with 81,755 more rows

    ``` r
    # c)
    Data %>%
      filter(sex_dv == 1 & racel_dv == 10 & (doby_dv == 1958 | doby_dv == 1982))
    ```

        ## # A tibble: 38 x 5
        ##         pidp memorig sex_dv doby_dv racel_dv
        ##        <dbl>   <dbl>  <dbl>   <dbl>    <dbl>
        ##  1  69584407       7      1    1982       10
        ##  2 205542927       7      1    1958       10
        ##  3 205845531       7      1    1982       10
        ##  4 347949284       7      1    1982       10
        ##  5 411567965       3      1    1958       10
        ##  6 620581844       7      1    1982       10
        ##  7 749455207       7      1    1958       10
        ##  8 750075367       7      1    1958       10
        ##  9 817945503       7      1    1982       10
        ## 10 953419319       7      1    1982       10
        ## # ... with 28 more rows

3.  Recode birth year into cohorts (a new variable): the G.I. Generation (born before 1924), Silent Generation (1925-42), Baby Boomers (1943-65), Generation X (1966-1980), Millenials (1981-99), Generation Z (2000-). (The years are approximate.)

    ``` r
    # with case_when() (good with complex conditions)
    Data <- Data %>%
      mutate(generation = case_when(
    between(doby_dv, 0, 1924) ~ "GI Generation",
    between(doby_dv, 1925, 1942) ~ "Silent Generation",
    between(doby_dv, 1943, 1965) ~ "Baby Boomers",
    between(doby_dv, 1966, 1980) ~ "Generation X",
    between(doby_dv, 1981, 1999) ~ "Millenials",
    doby_dv >= 2000 ~ "Generation Z"
      ))
    # case_when is particularly useful when there are multiple variables in logical statements, for example:

    # men, 18 to 25 years old, recoded to "young men"
    # (the variable names do not refer to our data; this is just an example.)
    # case_when(
    #         between(age, 18, 25) & sex == "male" ~ "young men"
    # )

    # You can also use ifelse for recoding, but it works best with simple cases.
    # recode sex into "male" or "female"", dropping other cases.

    Data %>%
        mutate(sexBinary = ifelse(sex_dv == 1, "male",
                                   ifelse(sex_dv == 2, "female", NA)))
    ```

        ## # A tibble: 145,779 x 7
        ##     pidp memorig sex_dv doby_dv racel_dv generation sexBinary
        ##    <dbl>   <dbl>  <dbl>   <dbl>    <dbl> <chr>      <chr>    
        ##  1   687       3    -20     -20        1 <NA>       <NA>     
        ##  2  1367       3    -20     -20       15 <NA>       <NA>     
        ##  3  2051       3    -20     -20       15 <NA>       <NA>     
        ##  4  2727       3    -20     -20       97 <NA>       <NA>     
        ##  5  3407       3    -20     -20        1 <NA>       <NA>     
        ##  6  4091       3    -20     -20        1 <NA>       <NA>     
        ##  7  4767       3    -20     -20        1 <NA>       <NA>     
        ##  8  5451       3    -20     -20        1 <NA>       <NA>     
        ##  9  6135       3    -20     -20        1 <NA>       <NA>     
        ## 10  6807       3    -20     -20        1 <NA>       <NA>     
        ## # ... with 145,769 more rows

4.  Recode ethnicity into the following groups: white British, Other White, Indian, Pakistani, other. (This classification doesn't make much sense, but we're doing this as an exercise).

    ``` r
    table(Data$racel_dv)
    ```

        ## 
        ##    -9     1     2     3     4     5     6     7     8     9    10    11 
        ## 47559 75100  2069    27  3302   614   260   383   353  3574  3125  1957 
        ##    12    13    14    15    16    17    97 
        ##   501  1051  1919  2726   197   507   555

    ``` r
    # using recode

    Data <- Data %>%
        mutate(ethnRecoded = recode(racel_dv,
                `1` = "white British",
                `2` = "other White",
                `3` = "other White",
                `4` = "other White",
                `9` = "Indian",
                `10` = "Pakistani",
                `-9` = NA_character_,
                # .default is for all other values
                .default = "other"
        )) %>%
        # let's make this new variable a factor
        mutate(ethnRecoded = factor(ethnRecoded))

    # checking that the recoding was correct
    Data %>%
        count(racel_dv, ethnRecoded)
    ```

        ## # A tibble: 19 x 3
        ##    racel_dv ethnRecoded       n
        ##       <dbl> <fct>         <int>
        ##  1       -9 <NA>          47559
        ##  2        1 white British 75100
        ##  3        2 other White    2069
        ##  4        3 other White      27
        ##  5        4 other White    3302
        ##  6        5 other           614
        ##  7        6 other           260
        ##  8        7 other           383
        ##  9        8 other           353
        ## 10        9 Indian         3574
        ## 11       10 Pakistani      3125
        ## 12       11 other          1957
        ## 13       12 other           501
        ## 14       13 other          1051
        ## 15       14 other          1919
        ## 16       15 other          2726
        ## 17       16 other           197
        ## 18       17 other           507
        ## 19       97 other           555

    ``` r
    # we could do the same recoding with case_when

    Data %>%
        mutate(ethnRecoded = case_when(
                racel_dv == 1 ~ "white British",
                between(racel_dv, 2, 4) ~ "other White",
                racel_dv == 9 ~ "Indian",
                racel_dv == 10 ~ "Pakistani",
                (racel_dv >= 5 & racel_dv <= 8) | racel_dv > 10 ~ "other",
                racel_dv == -9 ~ NA_character_
        )) %>%
        count(racel_dv, ethnRecoded)
    ```

        ## # A tibble: 19 x 3
        ##    racel_dv ethnRecoded       n
        ##       <dbl> <chr>         <int>
        ##  1       -9 <NA>          47559
        ##  2        1 white British 75100
        ##  3        2 other White    2069
        ##  4        3 other White      27
        ##  5        4 other White    3302
        ##  6        5 other           614
        ##  7        6 other           260
        ##  8        7 other           383
        ##  9        8 other           353
        ## 10        9 Indian         3574
        ## 11       10 Pakistani      3125
        ## 12       11 other          1957
        ## 13       12 other           501
        ## 14       13 other          1051
        ## 15       14 other          1919
        ## 16       15 other          2726
        ## 17       16 other           197
        ## 18       17 other           507
        ## 19       97 other           555

5.  Count the number of people belonging to different ethnic groups (and produce percentages).

    ``` r
    Data %>%
      count(ethnRecoded) %>%
      mutate(perc = n / sum(n) * 100)
    ```

        ## # A tibble: 6 x 3
        ##   ethnRecoded       n  perc
        ##   <fct>         <int> <dbl>
        ## 1 Indian         3574  2.45
        ## 2 other         11023  7.56
        ## 3 other White    5398  3.70
        ## 4 Pakistani      3125  2.14
        ## 5 white British 75100 51.5 
        ## 6 <NA>          47559 32.6

6.  Summarise the proportion of white British by generation.

    ``` r
    Data %>%
      filter(racel_dv != -9) %>%
      mutate(whiteBritish = if_else(racel_dv == 1, 1, 0)) %>%
      group_by(generation) %>%
      summarise(
    propWhiteBritish = mean(whiteBritish, na.rm = TRUE) * 100
      )
    ```

        ## # A tibble: 7 x 2
        ##   generation        propWhiteBritish
        ##   <chr>                        <dbl>
        ## 1 Baby Boomers                  80.7
        ## 2 Generation X                  64.7
        ## 3 Generation Z                  59.9
        ## 4 GI Generation                 92.5
        ## 5 Millenials                    64.2
        ## 6 Silent Generation             87.0
        ## 7 <NA>                          93.0

7.  Summarise the percentage of women by birth year.

    ``` r
    Data %>%
        filter(doby_dv > 0) %>%
        mutate(female = ifelse(sex_dv == 2, TRUE,
                                   ifelse(sex_dv == 1, FALSE, NA))) %>%
        group_by(doby_dv) %>%
        summarise(
                propFemale = mean(female, na.rm = TRUE) * 100
                )
    ```

        ## # A tibble: 110 x 2
        ##    doby_dv propFemale
        ##      <dbl>      <dbl>
        ##  1    1908      100  
        ##  2    1910      100  
        ##  3    1911       25  
        ##  4    1912       76.9
        ##  5    1913       72.2
        ##  6    1914       80  
        ##  7    1915       40.9
        ##  8    1916       73.8
        ##  9    1917       60  
        ## 10    1918       66.7
        ## # ... with 100 more rows
