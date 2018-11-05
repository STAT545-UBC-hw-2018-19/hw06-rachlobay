HW06-data-wrangling-wrap-up
================
Rachel Lobay
2018-11-04

Table of contents
=================

-   [Cheat sheet for tidyr functions](#cheat-sheet-for-tidyr-functions)
-   [Stop. How do we want to handle a data set in RStudio?](#stop-how-do-we-want-to-handle-a-data-set-in-rstudio)
-   [Quick refresher on dplyr](#quick-refresher-on-dplyr)
    -   [filter() and select() functions](#filter-and-select-functions)
        -   [select() function](#select-function)
        -   [filter() function](#filter-function)

Part 1: Character Data Overview
===============================

I will read and work on the exercises from the [Strings chapter of R for Data Science](https://r4ds.had.co.nz/strings.html).

First, I will load the necessary packages for this part of the assignment.

``` r
library(tidyverse)
library(stringr)
```

14.2.5 Exercise 1 - Difference between paste() and paste0()
-----------------------------------------------------------

Questions:

In code that doesn’t use stringr, you’ll often see paste() and paste0(). What’s the difference between the two functions? What stringr function are they equivalent to? How do the functions differ in their handling of NA?

Answer:

Since there are multiple questions in this exercise, we will go through each question one-by-one.

First, we will see what the difference is between paste() and paste0().

A good approach to discern the difference between paste() and paste0() is to first test them on a simple case. Hence, I will look at two strings that say "Hi" and "mom".

We will see what the output is for paste.

``` r
paste("hi","mom")
```

    ## [1] "hi mom"

Next, we will inspect paste0.

``` r
paste0("hi","mom")
```

    ## [1] "himom"

Clearly, we can see that when we use the paste function on the two strings, we get a space between the strings by default. However, when we use the paste0 function, we do not get a space between the string. Hence the paste and paste0 functions serve slightly difference purposes.

Now, we will move on to see what stringr function paste() and paste0() are equivalent to.

Out of the possible stringr functions, I think that str\_c() is the closest function because, as we discussed in class cm102, str\_c() is used to concatenate strings. Now, out of paste() and paste0(), which is str\_c() closest in function to? Let's try it out below and see!

``` r
str_c("hi", "mom")
```

    ## [1] "himom"

So, we get "himom" as our output without any space between the strings. Hence, str\_c() is more similar to paste0() than paste().

The final question to answer is how do the functions differ in their handling of NA?

Again, our approach to this problem will lead with an example. We shall see what happens when we use NA as an argument in each of paste(), paste0(), and str\_c().

First, we will look at the paste function with an NA as the third argument.

``` r
paste("hi","mom", NA)
```

    ## [1] "hi mom NA"

It looks like with the paste function, NA is made into a string and treated as such. So, we see that, like with the strings "hi" and "mom", there is a space between the 'strings' "mom" and "NA".

Now, we will look at the paste0 function with an NA as the third argument.

``` r
paste0("hi","mom", NA)
```

    ## [1] "himomNA"

Similar to the paste function, we see that the paste0 function appears to make NA into a string and treat it as such. So, there is no space between the mom and NA 'strings'.

Finally, we will look at the str\_c function with an NA as the third argument.

``` r
str_c("hi","mom", NA)
```

    ## [1] NA

When we have a NA as the third argument of str\_c, we only get NA returned. So, what is happening? Based on our example, it looks like str\_c puts importance on the NA. If we try to have the NA between the "hi" and "mom" strings, does the same thing happen?

``` r
str_c("hi", NA, "mom")
```

    ## [1] NA

Yes. There is just a NA returned.

Finally, what if NA comes before our "hi" "mom" strings.

``` r
str_c(NA, "hi", "mom")
```

    ## [1] NA

Again, there is just a NA returned.

Based on our examples, we can conclude that the str\_c() function will return a NA if any argument is NA.

14.2.5 Exercise 2 - Difference between sep and collapse arguments to str\_c()
-----------------------------------------------------------------------------

Question:

In your own words, describe the difference between the sep and collapse arguments to str\_c().

14.2.5 Exercise 3 - Write a function that turns a vector into a string
----------------------------------------------------------------------

Question:

Write a function that turns (e.g.) a vector c("a", "b", "c") into the string a, b, and c. Think carefully about what it should do if given a vector of length 0, 1, or 2.

14.3.4.1 Exercise 4 - Solve the beginner regexp crosswords
----------------------------------------------------------

I will solve the beginner regexp crosswords from [here](https://regexcrossword.com/challenges/beginner).

14.4.3.1 Exercise \# 2
----------------------

Question:

From the Harvard sentences data, extract:

1.  The first word from each sentence.
2.  All words ending in ing.
3.  All plurals.

14.7.1 Exercise \# 1 on Stringi package functions
-------------------------------------------------

Question:

Find the stringi functions that:

1.  Count the number of words.
2.  Find duplicated strings.
3.  Generate random text.

14.7.2 Exercise \# 2 on Stringi package functions
-------------------------------------------------

Question:

How do you control the language that stri\_sort() uses for sorting?