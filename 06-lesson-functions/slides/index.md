---
title       : Functions in R
subtitle    : 
author      : Andrew Do
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Motivation
From _R for Data Science_
One of the best ways to improve your reach as a data scientist is to write functions. Functions allow you to automate common tasks. Writing a function has three big advantages over using copy-and-paste:

* You drastically reduce the chances of making incidental mistakes when you copy and paste
* As requirements change, you only need to update code in one place, instead of many
* You can give a function an evocative name that makes your code easier to understand as a human

---

## Functions
Use `function()` to write your own function.  Basic usage looks something like this:
```
function_name <- function(arg1, arg2) {
  expression1
  expression2
  return_statement
}
```

---

## Parts of a function
Here is a simple squaring function

```r
SqPlusOne <- function(x) {
  y <- x^2
  y + 1
}

SqPlusOne(2)
```

```
## [1] 5
```
* `Square` is the __name__ of the function
* `x` is an __argument__ of the function
* `y <- x^2` is an expression
* The two lines between the curly braces form the __body__ of the function.

---

## Arguments of a function


```r
CylinderVol <- function(r = 2, h = 1) {
  pi * r^2 * h
}
```
You can call on a function using the argument names

```r
CylinderVol(r = 1, h = 2)
```

```
## [1] 6.283185
```
Or not.  `R` will then assume that the arguments are ordered.

```r
CylinderVol(1, 2)
```

```
## [1] 6.283185
```

---

## Moar Arguments


```r
CylinderVol <- function(r = 2, h = 1) {
  pi * r^2 * h
}
```
Using argument names lets you declare them out of order:

```r
CylinderVol(h = 2, r = 1)
```

```
## [1] 6.283185
```
`2` and `1` are __default values__ of `r` and `h`, respectively.  If not explicitly stated in a function call, `R` will assume those values.

```r
CylinderVol()
```

```
## [1] 12.56637
```

---

## Where does R look for variables?

`R` looks for variable values in the following places in the order specified:

1.  The function.
2.  The function call.
3.  The default values.
4.  The "global environment" (saved from previous code --- dangerous!)


```r
x <- 0 # This saves x to the global environment

example <- function(x = 1) { # 1 is a default value
  x <- 2 # x is equal to 2 inside the function
  x
}
# This call will return 2 since x is defined as 2 inside the function
example(x = 3) # 3 is the value of x in the function call
```

```
## [1] 2
```


---

## If-then-else Statements

If you want your script to do certain operations under certain conditions, then you use if-then-else statements.  The basic construct looks like:

```r
# One way of writing conditional statements
if (condition) expression1 else expression2

# Better way of writing conditional statements
if (condition) {
  expression1
} else {
  expression2
}
```
The second formating is preferred for complex expressions

---

## Example


```r
x <- 4

if (x > 5) {
  x + 2
} else {
  x - 3
}
```

```
## [1] 1
```

---

## Example 2
You can have just an if statement if you want nothing to happen otherwise.


```r
party <- TRUE

if (party) {
  print("Hooray!")
}
```

```
## [1] "Hooray!"
```

---

## How it works

* `if()` takes a __logical__ condition
* The condition __must be of length one__
* It executes the next expression if the condition is true
* If the statement is false, it looks for an `else` clause to perform.  Otherwise it does nothing.

---

## Chaining If-else

If you want to check multiple conditions, you can chain together if-else statements

```r
library(stringr)
x <- -2

if (x > 0) {
  str_c(x, "is positive", sep = " ")
} else if (x < 0) {
  str_c(x, "is negative", sep = " ")
} else {
  str_c(x, "is zero", sep = " ")
}
```

```
## [1] "-2 is negative"
```

---

## Returning Values

Functions return values in an explicit return statement

```r
Adder <- function(x, y) {
  return(x + y)
}
```
If no explicit return statement is given, the last evaluated expression is returned.

```r
Subtracter <- function(x, y) {
  x + y # Evaluated, but not returned
  x - y
}
Subtracter(4, 2)
```

```
## [1] 2
```

---

## Returning values

Functions can only return one thing in R.  To get around this, we use lists.

```r
BasicOperations <- function(x, y) {
  results <- list(add = x + y, subtract = x - y)
  return(results)
}
BasicOperations(2, 1)
```

```
## $add
## [1] 3
## 
## $subtract
## [1] 1
```

---

## Return halts execution

Using return stops function execution.

```r
Parity <- function(x) {
  if (x > 0) {
    return(str_c(x, "is positive", sep = " "))
  } else if (x < 0) {
    return(str_c(x, "is negative", sep = " "))
  } else {
    return(str_c(x, "is zero", sep = " "))
  }
  print("Who cares about parity? I want a party")
}
Parity(1)
```

```
## [1] "1 is positive"
```

---

## Stop
`stop` halts function execution and returns an error message.  This is helpful when you want to make sure the user has given meaningful inputs.


```r
IsEven <- function(x) {
  if (x != as.integer(x)) {
    stop("x is not a whole number")
  } else if (x <= 0) {
    stop("x must be positive")
  } else {
    return(x %% 2 == 0)
  }
}
IsEven(-1)
```

```
## Error in IsEven(-1): x must be positive
```

```r
IsEven(1)
```

```
## [1] FALSE
```

---

## Strategy

* Start simple: try to get the code to work without writing the function
* Divide and conquer - break the problem into small steps
* Check each step as you go
* "Wrap it up" as a function once everything works
* Test all the use cases you can think of
* Optimize if you'd like

---

## Lets write a function together

A leap year is defined as follows:

Every year that can be evenly divided by 4 unless the year can be divided by 100 but not 400.  Write a function that, given a year, returns whether or not it's a leap year.


```r
LeapYear <- function(year) {
  condition1 <- year %% 4 == 0
  condition2 <- year %% 100 != 0
  condition3 <- year %% 400 == 0

  condition1 & (condition2 | condition3)
}

LeapYear(2000)
```

```
## [1] TRUE
```

```r
LeapYear(2004)
```

```
## [1] TRUE
```

```r
LeapYear(2015)
```

```
## [1] FALSE
```

