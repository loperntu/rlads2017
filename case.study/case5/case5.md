---
title: "案例學習五"
output: 
  html_notebook:
    toc: true
---



```r
library(jiebaR)
```

```
## Warning: package 'jiebaR' was built under R version 3.2.5
```

```
## Loading required package: jiebaRD
```

```r
library(tidytext)
library(gutenbergr)
```

```
## Warning: package 'gutenbergr' was built under R version 3.2.5
```

```r
library(rvest) 
```

```
## Warning: package 'rvest' was built under R version 3.2.5
```

```
## Loading required package: xml2
```

```
## Warning: package 'xml2' was built under R version 3.2.5
```

```r
library(wordcloud2)
```

```
## Warning: package 'wordcloud2' was built under R version 3.2.5
```


想撈取 Facebook 上的資料，得先取得通過 Facebook 的權限認證機制，


# Exploratory Textual Data Analysis





```r
plot(cars)
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-1.png)

