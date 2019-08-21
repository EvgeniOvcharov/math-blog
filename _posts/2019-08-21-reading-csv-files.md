---
layout: post
title: "Reading multiple csv files in R"
date:   2019-08-21
categories: r-programming
---

Suppose you need to read multiple csv files and combine them into a single data frame or data table in R. We shall explore different approaches and find the most efficient one.

### List the files in a given directory

The function `list.files()` is a very useful base R function when working with files. It lists all files in a given directory whose names fulfill a given criterion. Some of the arguments of the function with their default settings are:

  * `path = "."` the directory path is set to the current directory;
  
  * `pattern = NULL` an empty matching criterion, but otherwise accepts a valid *regex* expression;
  
  * `full.names = FALSE` the relative or absolute directory path is not attached to the file names. 

For example, consider the following function call and its output:


```r
list.files(path = "./csv/", pattern = "^f.*199", full.names = TRUE) 

## [1] "./csv/football_results-1998.csv" "./csv/football_results-1999.csv"
```

The output is stored as a character vector, giving the names of the two files matching our search criterion. In the `path` argument we have selected the subdirectory *csv* of the working directory. In the argument `pattern` we have used a regular expression to select all file names that start with the letter *f*, followed by an unspecified number of characters, followed by 199. More on regular expressions for strings one may learn for example [here](https://www.regular-expressions.info/index.html). 

### Examining four approaches

In the variable `files` we have stored the names of 22 csv files containing data on international football matches. Out goal is to read them all from disk and combine them into a single data frame or data table.

#### 1. The straightforward approach, sequentially binding all data frames

```r
table = NULL
for (file in files)
  table = rbind(table, read.csv(file, encoding = "UTF-8"))
```
This is the least effective solution, where we simply append each data frame sequentially to an empty `data.frame` object. At each step R stores the resulting data frame at some new address in the memory, which results in copying the earlier data frames multiple times.


####2. Making a single call to `rbind()` and passing all data frames as a list of arguments
  


```r
list_of_tables = lapply(files, read.csv, encoding = "UTF-8")
table = do.call(rbind, list_of_tables)
```

The function `rbind()` accepts multiple arguments if all of them are matrix-like data structures with the same column names. The way in R to pass a list of arguments to a function is to invoke the method `do.call()`. This approach is faster than the previous one as `rbind()` has some optimization in case of multiple arguments.

Notice that in the code above we are not interested in keeping the intermediate output `list_of_tables`, which is a list of data frames. R provides us with a way to pipe together multiple operations into a single expression. To that end, we need to use the pipe operator `%>%` from the `magrittr` package. When the output from the previous step is passed on as the first argument in the next function call, it is suppressed from writing. If it is passed on in any other position, a `.` is used to mark its position. The two line code above may be equivalently written as:


```r
table = lapply(files, read.csv, encoding = "UTF-8") %>% do.call(rbind, .)
```


####3. Optimizing the binding of multiple data frames with `rbindlist()`
  

```r
table = lapply(files, read.csv, encoding = "UTF-8") %>% rbindlist()
```

The function `rbindlist()` is a member of the `data.table` package and as the name suggests, it row binds multiple matrix-like data structures with the same column names. Notice that in the code above its argument is suppressed.

####4. A solution based entirely on the `data.table` package

```r
table = lapply(files, fread, encoding = "UTF-8") %>% rbindlist()
```

The class `data.table` is a modern and much optimized analogue to the older and more familiar class `data.frame`. The function `fread()` may be almost an order of magnitude faster than `read.csv()` for large csv files.


### Comparison of running times

We have benchmarked the relative efficiency of each approach and the results are given in the table below.

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> test </th>
   <th style="text-align:right;"> replications </th>
   <th style="text-align:right;"> elapsed </th>
   <th style="text-align:right;"> user </th>
   <th style="text-align:right;"> system </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> fread </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:right;"> 3.44 </td>
   <td style="text-align:right;"> 2.96 </td>
   <td style="text-align:right;"> 0.48 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> rbindlist </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:right;"> 16.91 </td>
   <td style="text-align:right;"> 16.49 </td>
   <td style="text-align:right;"> 0.41 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> do.call </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:right;"> 20.54 </td>
   <td style="text-align:right;"> 20.12 </td>
   <td style="text-align:right;"> 0.39 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sequential </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:right;"> 39.69 </td>
   <td style="text-align:right;"> 38.84 </td>
   <td style="text-align:right;"> 0.78 </td>
  </tr>
</tbody>
</table>

The approach based on `fread()` is by far superior in terms of speed than its alternatives. Interestingly, `rbindlist()` is faster than `do.call()` for `data.frame` elements but not for `data.table` ones. Another side remark would be that the function `read_csv()` from the package `readr` addresses many of the shortcomings of `read.csv()`. The function `fread()` is still faster than `read_csv()`, but not overwhelmingly so. The package [readr](https://github.com/tidyverse/readr) was recently developed by Hadley Wickham as part of the [R tidyverse](https://www.tidyverse.org/packages/).
