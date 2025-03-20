---
title: Searching for files with the `findR` package
date: '2023-03-09 18:00:00 -0800'
categories: [R, R_Getting Started]
tags: [R, windows] # tags always lowercase
author: Stefano Mezzini
math: false # to enable math (not on by default)
output:
  html_document:
    keep_md: TRUE
---

<!-- https://chirpy.cotes.page/posts/write-a-new-post/ -->



It is good practice to have descriptive but concise file names so one can easily find the code they are looking for. But as projects grow, the number of scripts can become quite large (unless you are one of those chaotic people that put everything in a single script...). With UNIX computers, one can easily search for code in the a file explorer window, but this does not work with Windows. It is possible to search for files by name, but it is not possible to search for specific content in `R` scripts.

The `findR` package allows Windows users to search for specific strings of code within directories using a simple syntax:


``` r
library('findR')
findRscript(pattern = 'a string of code',
            path = 'my-folder',
            case.sensitive = TRUE)
```



The `pattern` argument can be any code string, but note that single and double quotes need to be escaped by placing a `\` before them (i.e., `\'` and `\"`), while some special characters need to be preceeded by `\\`, such as `+`, `(`. Failing to escape and double-escape special characters will cause the function to miss the files or fail:


``` r
setwd('~/Github/bc-mammals-temperature') # change working directory
findRscript('library(\'ctmm\')', path = 'analysis')
```

```
## Number of R scripts scanned: 33
```

```
## Number of R scripts with matching content: 0
```

```
## Total number of matches: 0
```

``` r
findRscript('library\\(\'ctmm\'\\)', path = 'analysis')
```

```
## Number of R scripts scanned: 33
```

```
## Number of R scripts with matching content: 10
```

```
## Total number of matches: 10
```

```
##                                          path_to_file line
## 1                       analysis/00-download-bc-dem.R    7
## 2                 analysis/01-compile-tracking-data.R    4
## 3        analysis/01a-compile-goat-calibration-data.R    4
## 4                       analysis/02-remove-outliers.R    4
## 5              analysis/03-movement-models-parallel.R    5
## 6         analysis/04-dowload-era5-temperature-data.R    7
## 7            analysis/05-add-speeds-and-temperature.R    9
## 8                     analysis/06-temperature-hrsfs.R    5
## 9  analysis/10-local-weather-predictions-by-species.R   11
## 10                analysis/figures/telemetry-ud-map.R    5
```

The package can also search for `R` Markdown files, PDFs, and text (`.txt`) files via the `findRmd()`, `findPDF()`, and `findtxt()` functions, respectively. Each of the functions can also copy the files that matched the `pattern` argument to a new folder (if `copy = TRUE`, but it is `FALSE` by default). You can decide which folder the files get copied to using the `folder` argument. Note that by default `overwrite` is set to `TRUE`, so any files present with the same name will be overwritten.
