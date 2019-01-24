Test
================
Alexey Bessudnov
24/01/2019

Enter data
----------

Let's create a data frame with 5 observations abd 2 variables.

``` r
Data <- data.frame(x = c(1:4, -5), y = 5:1)
```

Calculate correlation
---------------------

``` r
cor(Data$x, Data$y)
```

    ## [1] 0.4472136
