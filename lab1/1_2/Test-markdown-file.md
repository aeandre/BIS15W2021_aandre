---
title: "Test Markdown"
output: 
  html_document: 
    keep_md: yes
---




```r
getwd()
```

```
## [1] "/Users/allyandre/Desktop/Winter 2021/BIS 15L/GitHub/BIS15W2021_aandre/lab1"
```

# This is my first markdown file
###Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents.
## The hashtags mean that it is written text
### More hashtags make the text smaller
### Go to insert, R. This allows you to run code through r

```r
4*2
```

```
## [1] 8
```

```r
3-5
```

```
## [1] -2
```

```r
4+68
```

```
## [1] 72
```

```r
9*42
```

```
## [1] 378
```


```r
#install.packages("tidyverse")
library("tidyverse")
```


```r
ggplot(mtcars, aes(x = factor(cyl))) +
    geom_bar()
```

![](Test-markdown-file_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

##This is my [email](mail to: aeandre@ucdavis.edu)
##This is [Google](www.google.com)

#Then knit to html to see the final product
