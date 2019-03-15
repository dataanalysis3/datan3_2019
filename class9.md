Data Analysis 3: Week 9
================
Alexey Bessudnov
14 March 2019

Plan for today:

1.  Final statistical report.
2.  Assignment 4: solutions.
3.  Exercises on conditional statements and iteration.

Exercises.

1.  Check if a number is even. If it is print "Even". If it isn't print "Odd". If it's 0 print "Zero".

    ``` r
    x <- 3

    if (x == 0) {
      print("Zero")
    } else {
    if (x %% 2 == 0) {
      print("Even")
    } else {
        print("Odd")
    }
      }
    ```

        ## [1] "Odd"

2.  Write a for loop finding the largest element of a numeric vector and print it on the screen.

    ``` r
    x <- c(3, 5, 8, -2)
    max(x)
    ```

        ## [1] 8

    ``` r
    for (i in 1:3) {
      print(i)
    }
    ```

        ## [1] 1
        ## [1] 2
        ## [1] 3

    ``` r
    for(i in 1:length(x)) {
      print(x[i])
    }
    ```

        ## [1] 3
        ## [1] 5
        ## [1] 8
        ## [1] -2

    ``` r
    max_x <- x[1]
    for (i in 2:length(x)) {
      if (x[i] > max_x) {max_x <- x[i]}
    }
    max_x
    ```

        ## [1] 8

3.  Write a for loop finding the largest element of a numeric matrix and print it on the screen.

    ``` r
    x <- matrix(c(1, -3, 7, 4, 10, 7, -5, 0), nrow = 4)
    x
    ```

        ##      [,1] [,2]
        ## [1,]    1   10
        ## [2,]   -3    7
        ## [3,]    7   -5
        ## [4,]    4    0

    ``` r
    max_x <- x[1, 1]
    for (i in 1:dim(x)[1]) {
      for (j in 1:dim(x)[2]) {
      if (x[i,j] > max_x) {max_x <- x[i,j]}
      }
    }
    max_x
    ```

        ## [1] 10

4.  Write a while loop finding the largest element of a numeric vector and print it on the screen.

    ``` r
    i <- 1 
    while (i < 10) {
      print(i)
      i <- i + 1
    }
    ```

        ## [1] 1
        ## [1] 2
        ## [1] 3
        ## [1] 4
        ## [1] 5
        ## [1] 6
        ## [1] 7
        ## [1] 8
        ## [1] 9

    ``` r
    x <- c(3, 5, 8, -2)

    max_x <- x[1]
    i <- 2
    while (i <= length(x)) {
    if (x[i] > max_x) {max_x <- x[i]}
      i <- i + 1
    }
    max_x
    ```

        ## [1] 8

5.  x is a vector with whole numbers (zero and positive integers). If the largest even element of x is smaller than the largest odd element of x, all even elements of x are replaced by 0s. Otherwise all odd elements of x are replaced by 0s. For example, if x = {7; 1; 3; 2; 14; 5; 9; 6} the output should be \[0; 0; 0; 2; 14; 0; 0; 6\].

    ``` r
    x <- c(7, 1, 3, 2, 14, 5, 9, 6)
    max <- 0
    for (i in seq_along(x)) {
                if (x[i] > max) {max <- x[i]}
    }
    for (i in seq_along(x)) {
        if (max %% 2 == 0) {
                if (x[i] %% 2 != 0) {x[i] <- 0}
        }
        else {
                if (x[i] %% 2 == 0) {x[i] <- 0}
        }
    }
    x
    ```

        ## [1]  0  0  0  2 14  0  0  6

6.  x is a vector with whole numbers. Write a programme that count the number of pairs of the elements of x where the sum can be divided by 12 without a remainder.

For example, for x = {8; 10; 14; 7; 13; 5; 30; 9; 6} then the answer is 3 ((10, 14); (7, 5); (30, 6)).

    ```r
    x <- c(8, 10, 14, 7, 13, 5, 30, 9, 6)
    k <- 0
    for (i in 1:(length(x)-1)) {
        for (j in (i+1):length(x)) {
                if ((x[i] + x[j]) %% 12 == 0) {
                         k <- k + 1
                         print(c(x[i], x[j]))
                }
        }
    }
    ```

    ```
    ## [1] 10 14
    ## [1] 7 5
    ## [1] 30  6
    ```

    ```r
    k
    ```

    ```
    ## [1] 3
    ```
