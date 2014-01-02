# Why test?

* Nobody writes buggy code... sure!

* How do you know your code is right unless you test?

* Can act as specification/examples of what prog should do.

# Inverting some data can have serious consequences ...
  http://www.sciencemag.org/content/314/5807/1856.full.pdf

2001 Science paper, and two more Science papers retracted after
inversion along x axis noted.

# Testing needs to be:

* reliable
* fast
* easy to run
* easy to summarise

# Forms of testing in R

stopifnot()

# Handling errors


```r
safelog <- function(x) {
    tryCatch(log(x), error = function(e) "an error", warning = function(e) "a warning")
}

safelog(3)
safelog(-5)
safelog("string")
```


# stop() vs warning()

* A warning is softer than an error; if a warning is generated
your program will still continue, whereas an error will stop the
program.


```r
log(c(2, 1, 0, -1, 2))
print("end")  # warning 
xor(c(TRUE, FALSE))
print("end")  # error
```


* If you try to isolate warnings, you can change warnings to
  errors: `options(warn=2)`

* Add warnings and errors to your code using `warning()`,
  `stop()`


```r
if (any(x < 0)) {
    warning("some elements of x are negative")
}
if (all(x < 0)) {
    stop("all elements of x are negative")
}
stopifnot(all.equal(pi, 3.1415927), 2 < 2, all(1:10 < 12), "a" < "b")
```




# Floating point issues to be aware of

R FAQ 7.31?

```r
1 + 2 == 3
0.1 + 0.2 == 0.3
```

http://cran.r-project.org/doc/FAQ/R-FAQ.html#Why-doesn_0027t-R-think-these-numbers-are-equal_003f

# Floating point exercise.  What does the following code do?

Ex: type this in and work out what is going on.

```r
eps <- 1
while (eps + 1 > 1) {
    eps <- eps * 0.5
}
eps
1 + eps
(1 + eps == 1)
1 + (2 * eps)
(1 + (2 * eps) == 1)
```


# Floating point: how to compare


```r
all.equal(0.1 + 0.2, 0.3)
```

```
## [1] TRUE
```

```r
all.equal(0.1 + 0.2, 3)
```

```
## [1] "Mean relative difference: 9"
```

```r
isTRUE(all.equal(0.1 + 0.2, 3))  #when you just want TRUE/FALSE
```

```
## [1] FALSE
```



# EX: write some unit testing for the leap year function,

or for isvaliddate("2002-09-30")

e.g. check you can extract all three dates.
e.g. check months of the year (feb 29)

solve the leap year problem.

## Example: simple table

I prefer xtable but this is simple and works nice enough:

```r
kable(head(iris[, 1:3]), format = "html")
```

<table>
 <thead>
  <tr>
   <th> Sepal.Length </th>
   <th> Sepal.Width </th>
   <th> Petal.Length </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td> 5.1 </td>
   <td> 3.5 </td>
   <td> 1.4 </td>
  </tr>
  <tr>
   <td> 4.9 </td>
   <td> 3.0 </td>
   <td> 1.4 </td>
  </tr>
  <tr>
   <td> 4.7 </td>
   <td> 3.2 </td>
   <td> 1.3 </td>
  </tr>
  <tr>
   <td> 4.6 </td>
   <td> 3.1 </td>
   <td> 1.5 </td>
  </tr>
  <tr>
   <td> 5.0 </td>
   <td> 3.6 </td>
   <td> 1.4 </td>
  </tr>
  <tr>
   <td> 5.4 </td>
   <td> 3.9 </td>
   <td> 1.7 </td>
  </tr>
</tbody>
</table>


# The "testthat" package


```r
install.package("testthat")  #CRAN
```


* Designed to be simple to get started with; "other packages are
  available" as well-as standalone coding,


```r
system.file("test-tools-1.R", package = "Matrix")
```

Good overview:

http://adv-r.had.co.nz/Testing.html

## Simple test file

See the file `testthat_examples.R`.



# Running test_that() on leapyear problem.

What is the rule?
[http://www.mathsisfun.com/leap-years.html](http://www.mathsisfun.com/leap-years.html)

    Leap Years are any year that can be evenly divided by 4 (such as 2012, 2016, etc)
 		except if it can can be evenly divided by 100, then it isn't (such as 2100, 2200, etc)
  	  		except if it can be evenly divided by 400, then it is (such as 2000, 2400)


Leap year bugs are [common](http://en.wikipedia.org/wiki/Leap_year_bug)
and sometimes deliberately [left broken](http://support.microsoft.com/kb/214326)



Write a function to check leap years.  See `leapyear.R`


Advanced: can you reproduce this graph
http://www.mathsisfun.com/images/leap-year-graph.gif


# Putting your tests in your R package

Follow the advice on
[http://adv-r.had.co.nz/Testing.html#r-cmd-check](http://adv-r.had.co.nz/Testing.html#r-cmd-check)

# Compile this document.

```r
require(knitr)
pandoc(knit("testing.Rmd"), format = "latex")
knit("testing.Rmd")
knit2html("testing.Rmd")
browseURL("testing.html")
```
