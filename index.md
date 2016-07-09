---
title       : Session7
subtitle    : 
author      : 
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
---

<style>
strong {
  font-weight: bold;
  color: red;
  font-size: 115%
}
</style>



--- .segue bg:grey

# Exkurs

Graphics (line)



--- .segue bg:grey

# Recap


---

## Recap


You should know
  - what factor means in R
  - how to create factors


---

## Recap




```r
library(readxl)
babies <- read_excel("../session7dta/babies.xlsx",1)
babies$sex.mf <- factor(babies$sex, 
                        levels = c(0,1),
                        labels = c("male","female"))
```


--- 

## Recap factors 

- use `factor()` to change the `marital` and `inc` variable from numeric to factor
  - 1 = married, 2 = legally separated, 3 = divorced, 4 = widowed, 5 = never married
  - 0 = <2500, 1 = 2500-4999, 2 = 5000-7499, 3 = 7500-9999, ..., 9 = 15000+, 98 = unknown, 99 = not asked
- table the two variables using `table()` and `prop.table()`; what is the percentage of <2500 amongst married and what amongst never married?


---


## Solution


```r
babies$marital <- factor(babies$marital, 
                         levels = 1:5, 
                         labels = c("married","legally separated",
                                    "divorced","widowed","never married"))
babies$inc <- factor(babies$inc,
                     levels = c(0:9,98,99),
                     labels = c("<2500",
                                paste(seq(2500,20000,by = 2500),
                                       seq(4999,22500,by = 2500),sep = "-"),
                                "25000+","unknown","not asked"))
```



---


## Solution

```r
addmargins(prop.table(table(babies$inc,babies$marital),2))
```

```
##              
##                  married legally separated   divorced widowed
##   <2500       0.02400662        0.06666667 0.00000000        
##   2500-4999   0.15397351        0.26666667 0.40000000        
##   5000-7499   0.14486755        0.06666667 0.20000000        
##   7500-9999   0.14900662        0.00000000 0.00000000        
##   10000-12499 0.11341060        0.06666667 0.00000000        
##   12500-14999 0.10264901        0.13333333 0.00000000        
##   15000-17499 0.05877483        0.06666667 0.00000000        
##   17500-19999 0.11672185        0.06666667 0.20000000        
##   20000-22499 0.02069536        0.00000000 0.00000000        
##   25000+      0.01738411        0.06666667 0.20000000        
##   unknown     0.09850993        0.20000000 0.00000000        
##   not asked   0.00000000        0.00000000 0.00000000        
##   Sum         1.00000000        1.00000000 1.00000000        
##              
##               never married Sum
##   <2500          0.16666667    
##   2500-4999      0.50000000    
##   5000-7499      0.00000000    
##   7500-9999      0.00000000    
##   10000-12499    0.00000000    
##   12500-14999    0.00000000    
##   15000-17499    0.00000000    
##   17500-19999    0.00000000    
##   20000-22499    0.00000000    
##   25000+         0.00000000    
##   unknown        0.33333333    
##   not asked      0.00000000    
##   Sum            1.00000000
```



---


## change labels





```r
table(babies$sex.mf)
```

```
## 
##   male female 
##    626    610
```

```r
babies$sex.mj <- factor(babies$sex.mf, 
                        levels = c("female","male"),
                          labels = c("Maedchen","Junge"))
table(babies$sex.mj,babies$sex.mf)
```

```
##           
##            male female
##   Maedchen    0    610
##   Junge     626      0
```



---


## Exercise

Recode the sex variable again. Use `boy` and `girl` as label.


---


## recoding

>  - another kind of issue is a problem like the following:
>  - the `drace` variable contains 11 races plus one unknown coding
>  - coding 0-5 means all white, 6 mex, 7 black, 8 asian, 9 and 10 mixed, 99 unknown



---



```r
library(car)
babies$drace <- recode(babies$drace,
                       '0:5="white";6="mex";7="black";
                       8="asian";c(9,10)="mixed";99=NA')
```



---


## Exercise

- recode `drace` in the same way
- use the variables `race` and `drace` to get the percentage of mixed paires (man and woman from different races)


---


## Solution


```r
prop.table(table(babies$race == babies$drace))
```

```
## 
##      FALSE       TRUE 
## 0.05660377 0.94339623
```


---


## recoding missings

- remember how we used numbers to extract parts of vectors and data frames, e.g.



```r
babies$id[1] ## the first element of the id-row
```

```
## [1] 15
```




```r
babies[2,1] ## the second element of the first column
```

```
## Source: local data frame [1 x 1]
## 
##      id
##   (dbl)
## 1    20
```


---



```r
babies[1:2,] ## the first two rows
```

```
## Source: local data frame [2 x 25]
## 
##      id pluralty outcome  date gestation   sex    wt parity  race   age
##   (dbl)    (dbl)   (dbl) (dbl)     (dbl) (dbl) (dbl)  (dbl) (chr) (dbl)
## 1    15        5       1  1411       284     0   120      1 asian    27
## 2    20        5       1  1499       282     1   113      2 white    33
## Variables not shown: ed (dbl), ht (dbl), wt1 (dbl), drace (chr), dage
##   (dbl), ded (dbl), dht (dbl), dwt (dbl), marital (fctr), inc (fctr),
##   smoke (chr), time (dbl), number (chr), bwt (dbl), sex.mf (fctr)
```


---



```r
babies[1:2,c(1,3)] ## the first and third column for the first two rows
```

```
## Source: local data frame [2 x 2]
## 
##      id outcome
##   (dbl)   (dbl)
## 1    15       1
## 2    20       1
```


---


## Logical Indexing

>  - Problem: get all ids of babies with a birth weight > 4
>  - Solution: instead of numbers as indices we use conditions 



---


## Logical Indexing

- Problem: get all ids of babies with a birth weight > 4
- Solution: instead of numbers as indices we use conditions 


```r
babies$id[babies$bwt > 4]
```

```
##   [1]  148  171  239  357  767  773  813  842  911  985 1153 1168 1204 1251
##  [15] 1617 1912 2293 2347 2420 2638 2985 3164 3247 3265 3468 3816 3906 3917
##  [29] 3945 4048 4145 4311 4829 4889 4897 5091 5258 5461 5476 5914 5946 5982
##  [43] 6021 6030 6073 6101 6122 6213 6214 6384 6441 6442 6476 6532 6534 6554
##  [57] 6560 6638 6660 6675 6692 6705 6719 6729 6760 6762 6778 6784 6787 6825
##  [71] 6835 6876 6910 6955 6997 7010 7080 7094 7109 7136 7143 7159 7160 7195
##  [85] 7241 7278 7290 7341 7355 7363 7386 7392 7420 7449 7499 7541 7552 7557
##  [99] 7581 7603 7616 7646 7681 7712 7737 7744 7797 7828 7837 7839 7840 7844
## [113] 7845 7883 7888 7965 7978 8005 8036 8054 8098 8112 8118 8122 8202 8218
## [127] 8323 8397 8434 8664 8725 8801 8960
```


---


## Logical Indexing

- Problem: get all rows of babies with a birth weight > 5


```r
babies[babies$bwt > 5,]
```

```
## Source: local data frame [0 x 25]
## 
## Variables not shown: id (dbl), pluralty (dbl), outcome (dbl), date (dbl),
##   gestation (dbl), sex (dbl), wt (dbl), parity (dbl), race (chr), age
##   (dbl), ed (dbl), ht (dbl), wt1 (dbl), drace (chr), dage (dbl), ded
##   (dbl), dht (dbl), dwt (dbl), marital (fctr), inc (fctr), smoke (chr),
##   time (dbl), number (chr), bwt (dbl), sex.mf (fctr)
```


---


## Logical Indexing

- Problem: get all rows of babie girls with a birth weight > 4.5


```r
babies[babies$bwt > 4.5 & babies$sex.mf == "female",]
```

```
## Source: local data frame [9 x 25]
## 
##      id pluralty outcome  date gestation   sex    wt parity  race   age
##   (dbl)    (dbl)   (dbl) (dbl)     (dbl) (dbl) (dbl)  (dbl) (chr) (dbl)
## 1  2420        5       1  1687       300     1   160      7 white    29
## 2  3906        5       1  1515       293     1   173      6 white    30
## 3  7290        5       1  1599       282     1   165      1 white    29
## 4  7449        5       1  1574       286     1   164      0 white    32
## 5  7581        5       1  1510       289     1   163      0 white    25
## 6  7737        5       1  1657       296     1   159      0 white    27
## 7  8098        5       1  1712       297     1   160      1 mixed    20
## 8  8122        5       1  1677       298     1   163      3 white    37
## 9  8323        5       1  1594       291     1   160      3 white    34
## Variables not shown: ed (dbl), ht (dbl), wt1 (dbl), drace (chr), dage
##   (dbl), ded (dbl), dht (dbl), dwt (dbl), marital (fctr), inc (fctr),
##   smoke (chr), time (dbl), number (chr), bwt (dbl), sex.mf (fctr)
```



---


## Recode Missings

Problem: the `age` and the `dage` column contain 99 to indicate missing. In R a missing is indicated by `NA`.  



```r
summary(babies$age)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   15.00   23.00   26.00   27.37   31.00   99.00
```


--- 


## Recode Missings

Problem: the `age` and the `dage` column contain 99 to indicate missing. In R a missing is indicated by `NA`.  



```r
babies$age[babies$age == 99]
```

```
## [1] 99 99
```


--- 


## Recode Missings

Problem: the `age` and the `dage` column contain 99 to indicate missing. In R a missing is indicated by `NA`.  



```r
babies$age[babies$age == 99] <- NA
```




```r
summary(babies$age)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##   15.00   23.00   26.00   27.26   31.00   45.00       2
```



---

## Exercise


Recode the 99 by `NA`s in the `dage` column of the data set.



---

## Solution


Recode the 99 by `NA`s in the `dage` column of the data set.



```r
babies$dage[babies$dage == 99] <- NA
```



```r
summary(babies$dage)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##   18.00   25.00   29.00   30.35   34.00   62.00       7
```


---

## cut a numeric vector


Another way of recoding often used is the transformation of a numeric vector into a factor by cutting the numeric vector at specific cut points (breaks).

For example we want to transform the fathers age variable into five-years intervals:


```r
summary(babies$age)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##   15.00   23.00   26.00   27.26   31.00   45.00       2
```



```r
babies$dage.cat <- cut(babies$dage,
                      breaks = seq(15,45,by = 5), 
                      include.lowest = T)
```



---

## Exercise

Cut also the mother's age into five-years intervals. Make a boxplot with mother's age (use the age groups) on the x-axis and 
birth weight on the y-axis.




