---
title: "Reading multiple csv files in R"
author: "Evgeni Ovcharov"
date:   2019-08-24
categories: r-programming
---


Comma-separated value (csv) files are one of the most common file formats used in data analysis today. Sometimes we need to read multiple csv files from disk and combine them into a single data frame or data table object in R. We shall explore five different approaches to that task and determine the most efficient one. First, let us make sure that we know how to answer the following question:

### How to list the files in a given directory?

The function `list.files()` lists all files in a given directory whose names contain a specific character pattern. An example of its use can be the following:

```r
list.files(path = "./csv/", pattern = "^f.*199", full.names = TRUE) 

[1] "./csv/football-results-1998.csv" "./csv/football-results-1999.csv"
```

The output is a character vector giving the names of the files matching the search criterion. In our example, we have called the function with the following options:

  * `path = "./csv/"` the directory path is set to the csv subdirectory of the working directory;
  
  * `pattern = "^f.*199"` a *regex* expression matching strings starting with the character *f*, followed by an unspecified number of arbitrary characters, followed by *199*;
  
  * `full.names = TRUE` the path is attached to the returned file names. 

### Examining five approaches

The stated problem is different from looking for the fastest way to read one large csv file into memory. In our use case we need to import multiple csv files (say 10 or more), each having many columns (say 10 or more), which we need to row bind together to produce a single large data frame or data table object. We next list five approaches to that task in increasing order of efficiency.

The variable `files` in the code below is a character vector containing a list of names of csv files, for example as produced by `list.files()`. 

#### 1. Sequentially reading and binding all data frames

```r
table = NULL
for (file in files)
  table = rbind(table, read.csv(file))
```
This is the least efficient solution in which we simply append each data frame sequentially, starting with an empty `data.frame` object. At each step R stores the resulting data frame at some new address in memory, which results in copying the data of the earlier data frames multiple times.


#### 2. Making a single call to `rbind()` <!-- and passing all data frames as a list of arguments-->
  

```r
table = lapply(files, read.csv) %>% do.call(rbind, .)
```

Here we use the fact that the function `rbind()` can simultaneously bind together multiple data frames, which is significantly faster than the previous approach. A special feature of R functions like `rbind()` is having a dynamic argument list. The R way of passing a variable number of arguments to a function is by invoking the method `do.call()`.


#### 3. Further optimization with `rbindlist()`
  

```r
table = lapply(files, read.csv) %>% rbindlist()
```

The row binding of multiple data frames and other matrix-like structures with the same column names can be optimized with the function `rbindlist()`, a member of the `data.table` package.

#### 4. A solution based on the newer `readr` package

```r
col_spec = spec_csv(files[1])
table = lapply(files, read_csv, col_types = col_spec) %>% rbindlist()
```

Another optimization may come from the replacement of `read.csv()` with a faster analogue of it. Such is the function `read_csv()` from the package `readr`. The function `read_csv()`, however, may be very slow and inefficient if it has to determine the column types of the data very often. To overcome this problem we have first invoked `readr::spec_csv()` to automatically set up the column types before running the rest of the code. 

#### 5. A solution based on the `data.table` package

```r
table = lapply(files, fread) %>% rbindlist()
```

The package `data.table` is a modern and much optimized analogue to the more familiar package `data.frame`. The function `fread()` from `data.table` is designed to read a csv file into a `data.table` object. For large csv files it can be an order of magnitude faster than `read.csv()`. 


### Comparison of running times

We have benchmarked the relative efficiency of each approach in the table below.

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
   <td style="text-align:right;"> 3.37 </td>
   <td style="text-align:right;"> 2.84 </td>
   <td style="text-align:right;"> 0.53 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> read_csv </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:right;"> 9.02 </td>
   <td style="text-align:right;"> 5.83 </td>
   <td style="text-align:right;"> 2.81 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> rbindlist </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:right;"> 16.51 </td>
   <td style="text-align:right;"> 16.09 </td>
   <td style="text-align:right;"> 0.42 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> do.call </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:right;"> 19.48 </td>
   <td style="text-align:right;"> 19.06 </td>
   <td style="text-align:right;"> 0.41 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sequential </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:right;"> 36.83 </td>
   <td style="text-align:right;"> 36.26 </td>
   <td style="text-align:right;"> 0.50 </td>
  </tr>
</tbody>
</table>

Apart from these results, we also note the following:

 * The test involves reading 22 csv files with 15 columns (a mixture of character and integer values) and around 1000 rows each with actual data on international football matches. The elapsed time in the table above is given in seconds for 100 repetitions.
 
  * The relative performance of the functions `fread()`, `read_csv()` and `read.csv()` under equal conditions may be gleaned from the first three results in the table above.
 
 * The method `rbindlist()` is faster than `rbind()` called with `do.call()` for multiple `data.frame` elements, but not for `data.table` ones. So, in fact replacing Solution 5 above with the code
```r
table = lapply(files, fread) %>% do.call(rbind, .)
```
 results in marginally faster running times.

 * Without specifying the column types in `read_csv()` the function runs very slowly and we get a 4-5 fold reduction in speed. Its default usage, such as in `lapply(files, read_csv)` is not recommended in a set-up like ours.
