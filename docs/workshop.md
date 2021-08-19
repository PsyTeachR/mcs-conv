---
title: "Untitled"
author: "Nordmann"
date: "25/05/2021"
output: html_document
---


```r
library("car")
```

```
## Loading required package: carData
```

```
## 
## Attaching package: 'car'
```

```
## The following object is masked from 'package:dplyr':
## 
##     recode
```

```
## The following object is masked from 'package:purrr':
## 
##     some
```

```r
library("correlation")
library("report")
library("psych")
```

```
## 
## Attaching package: 'psych'
```

```
## The following object is masked from 'package:car':
## 
##     logit
```

```
## The following objects are masked from 'package:ggplot2':
## 
##     %+%, alpha
```

```r
library("tidyverse")
mh <- read_csv("MillerHadenData.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   Participant = col_double(),
##   Abil = col_double(),
##   IQ = col_double(),
##   Home = col_double(),
##   TV = col_double()
## )
```

```r
results <- correlation(data = mh, select = "IQ", select2 = "Abil",  method = "pearson", alternative = "two.sided")
results2 <- cor.test(~ IQ + Abil, data = mh,  method = "pearson", alternative = "two.sided")
```


```r
ggplot(data = mh, aes(x = Abil)) +
  geom_histogram()
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

<div class="figure" style="text-align: center">
<img src="workshop_files/figure-html/unnamed-chunk-2-1.png" alt="**CAPTION THIS FIGURE!!**" width="100%" />
<p class="caption">(\#fig:unnamed-chunk-2)**CAPTION THIS FIGURE!!**</p>
</div>


```r
qqPlot(x = mh$Abil)
```

<div class="figure" style="text-align: center">
<img src="workshop_files/figure-html/unnamed-chunk-3-1.png" alt="**CAPTION THIS FIGURE!!**" width="100%" />
<p class="caption">(\#fig:unnamed-chunk-3)**CAPTION THIS FIGURE!!**</p>
</div>

```
## [1] 15  4
```


```r
ggplot(data = mh, aes(x = Abil, y = IQ)) +
  geom_point()+
  geom_smooth(method = lm) # if you don't want the shaded CI, add se = FALSE to this
```

```
## `geom_smooth()` using formula 'y ~ x'
```

<div class="figure" style="text-align: center">
<img src="workshop_files/figure-html/unnamed-chunk-4-1.png" alt="**CAPTION THIS FIGURE!!**" width="100%" />
<p class="caption">(\#fig:unnamed-chunk-4)**CAPTION THIS FIGURE!!**</p>
</div>


```r
descriptives <- summarise(mh, 
                          Abil_mean = mean(Abil),
                          Abil_SD = sd(Abil),
                          IQ_mean = mean(IQ),
                          IQ_SD = sd(IQ))
descriptives
```

```
## # A tibble: 1 x 4
##   Abil_mean Abil_SD IQ_mean IQ_SD
##       <dbl>   <dbl>   <dbl> <dbl>
## 1      55.1    6.08    100.  9.04
```

The mean IQ score was 100.04 (9.04) and the mean reading ability score was 55.12 (6.08). A Pearson\`s correlation found a significant, medium positive correlation between the two variables (r (23) = 0.45, *p* = 0.024).


```r
report(results2)
```

```
## Effect sizes were labelled following Funder's (2019) recommendations.
## 
## The Pearson's product-moment correlation between IQ and Abil is positive, statistically significant, and very large (r = 0.45, 95% CI [0.07, 0.72], t(23) = 2.42, p = 0.024)
```

