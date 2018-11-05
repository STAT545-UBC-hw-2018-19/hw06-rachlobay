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

Answer:

I suppose the easy (and cheap) solution would be to look up the str\_c() function and reiterate what it says the arguments are in my own words... But, I will use my example then interpret appraoch to try to see the differences between the two arguments.

We shall look at the two arguments in str\_c() using an example where we will concatenate strings that pertain to a few of Van Gogh's famous paintings.

If we just use the sep argument, let's see what we find.

``` r
str_c(c("The Starry Night", "Sunflowers", "The Potato Eaters"), "Van Gogh", sep = " ")
```

    ## [1] "The Starry Night Van Gogh"  "Sunflowers Van Gogh"       
    ## [3] "The Potato Eaters Van Gogh"

So we see that we get three strings "The Starry Night Van Gogh", "Sunflowers Van Gogh", and "The Potato Eaters Van Gogh". However, let's say we want to use `sep = " by "` so that we get, for example, "The Starry Night by Van Gogh" as one of our outputs. Does that work?

``` r
str_c(c("The Starry Night", "Sunflowers", "The Potato Eaters"), "Van Gogh", sep = " by ")
```

    ## [1] "The Starry Night by Van Gogh"  "Sunflowers by Van Gogh"       
    ## [3] "The Potato Eaters by Van Gogh"

Yes. It does. What we can say from this is that the sep argument puts a string of the sep argument in between the string arguments.

Now, let's look at the collapse argument. We shall start by looking at its default.

``` r
str_c(c("The Starry Night", "Sunflowers", "The Potato Eaters"), " Van Gogh", collapse="")
```

    ## [1] "The Starry Night Van GoghSunflowers Van GoghThe Potato Eaters Van Gogh"

By default we get no space between the strings. Furthermore, the output is a vector of length one with all the individual strings pasted together (as was discussed in class).

We shall try to have a comma in between each of the strings when we use the collapse argument to collapse those strings down into a vector of length one.

``` r
str_c(c("The Starry Night", "Sunflowers", "The Potato Eaters"), " Van Gogh", collapse=", ")
```

    ## [1] "The Starry Night Van Gogh, Sunflowers Van Gogh, The Potato Eaters Van Gogh"

We got what we wanted. So, it did work to use a comma as the argument of collapse.

Now, what if we try to put a string "or" as the argument of collapse?

``` r
str_c(c("The Starry Night", "Sunflowers", "The Potato Eaters"), " Van Gogh", collapse="or")
```

    ## [1] "The Starry Night Van GoghorSunflowers Van GoghorThe Potato Eaters Van Gogh"

No dice. the result doesn't produce or between each of the string arguments. Rather, we get the default collapse argument, which was just all the strings pasted together in one vector of length one with no space between the strings.

What if we now used the sep and collapse arguments together.

``` r
str_c(c("The Starry Night", "Sunflowers", "The Potato Eaters"), " Van Gogh", sep = " by", collapse=", ")
```

    ## [1] "The Starry Night by Van Gogh, Sunflowers by Van Gogh, The Potato Eaters by Van Gogh"

The output is one vector of length one with a comma between each of the original string arguments and a string of the sep argument " by" in between each of the original string arguments.

Let's look at the big picture. What is the difference between the sep and collapse arguments of str\_c())? Simply put, the sep argument puts a string of the sep argument in between the string arguments, while the collapse argument separates the elements of the vector of length one.

14.2.5 Exercise 3 - Write a function that turns a vector into a string
----------------------------------------------------------------------

Question:

Write a function that turns (e.g.) a vector c("a", "b", "c") into the string a, b, and c. Think carefully about what it should do if given a vector of length 0, 1, or 2.

Answer:

We shall create a function and test it using the vector c("U", "B", "C").

The prompt wants us to handle vectors of length 0, 1, or 2. Therefore, when we design our function, we want to keep the deal with the cases of different vector lengths.

For example: - A vector of length 0 would be the empty string "". - A vector of length 1 would be something like "U". - A vector of length 2 would be "U and B" (ie. we want to return a string of the two vector elements separated by and). - Finally, for a vector of length 3, we would have a string "U, B, and C" returned.

An easy solution would be to use, cumbersome but effective, if-else statements to handle the case where the vector is less than length 2, exactly equal to length 2, and the case where the vector is greater than length 2.

<!-- The last part under the else statement is a bit tricky to understand, so I will explain the steps. To minimize the code, I only used two lines. In the first line, I created a vector of length one of all the elements but the last element of the original vector, where each element is separated by a comma. The second line is where I simply add ", and " between the vector of length one and last element.-->
``` r
vector_to_string_fun <- function(x){
  # handles the cases where the vector is less than length 2
  if(length(x) < 2){
    x
  }
  # handles the cases where the vector is exactly length 2
  else if(length(x) == 2){
    x %>% 
    str_c(x[1], " and ", x[2]) # separates the two elements of the vector by " and "
  }
  else {
     # handles the cases where the vector is > than length 2
    # create a vector of length one of all the elements but the last element of the vector, where each element is separated by a comma
    str_before_last <- str_c(x[seq_len(length(x)-1)], collapse = ", ")
    # add ", and " between str_before_last and last element
    str_c(str_before_last, ", and ", x[length(x)])
  }
}
```

Now we shall test it using our ubc example.

``` r
# Define ubc vector
ubc <- c("U", "B", "C")

vector_to_string_fun(ubc)
```

    ## [1] "U, B, and C"

We got what we wanted - a vector of length 1 that is a string "U, B, and C".

14.3.3.1 Exercise 1 - Create regular expressions to find all words
------------------------------------------------------------------

Create regular expressions to find all words that:

1.  Start with a vowel.
2.  That only contain consonants. (Hint: thinking about matching “not”-vowels.)
3.  End with `ed`, but not with `eed`.
4.  End with `ing` or `ise`.

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
