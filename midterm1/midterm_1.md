---
title: "Midterm 1"
author: "Christian Flynn Atallah"
date: "2022-01-26"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 15 total questions, each is worth 2 points.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

This exam is due by 12:00p on Thursday, January 27.  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

```r
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
```

```
## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
## ✓ tibble  3.1.6     ✓ dplyr   1.0.7
## ✓ tidyr   1.1.4     ✓ stringr 1.4.0
## ✓ readr   2.1.1     ✓ forcats 0.5.1
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(dplyr)
library(skimr)
```

## Questions  
Wikipedia's definition of [data science](https://en.wikipedia.org/wiki/Data_science): "Data science is an interdisciplinary field that uses scientific methods, processes, algorithms and systems to extract knowledge and insights from noisy, structured and unstructured data, and apply knowledge and actionable insights from data across a broad range of application domains."  

1. (2 points) Consider the definition of data science above. Although we are only part-way through the quarter, what specific elements of data science do you feel we have practiced? Provide at least one specific example.  
#I think so far an element we have spent the most time on, would be data manipulation, that is to say, making data more manageable to work with, ie. moving between data classes, handling NA values, sorting data values by max and min as well as grouping and filtering variables of interest. One example appeared on HW 6 where we wanted to look at which country produced the most of a specific fish species, over a certain amount of years, so I filtered the original data set for those years, and that type of fish, and then grouped by country so we could look at the sum (or average depending how you did it) of fish yeild for that country for those years.

2. (2 points) What is the most helpful or interesting thing you have learned so far in BIS 15L? What is something that you think needs more work or practice?  
#So far the most helpful thing I have have learned is the filter command, I actually work in a lab where I do some data analysis and I know I will be using this frequently, lets say your doing differential gene expression but only want to look at values for one specific gene out of ~28000 , its very helpful. I want to get better at piping ">%>" and just making less objects and writing more concise, with less processing, and less steps required.

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.

```r
setwd('~')
elephants = read.csv('~/Midtermbis15/ElephantsMF.csv')
glimpse(elephants)
```

```
## Rows: 288
## Columns: 3
## $ Age    <dbl> 1.40, 17.50, 12.75, 11.17, 12.67, 12.67, 12.25, 12.17, 28.17, 1…
## $ Height <dbl> 120.00, 227.00, 235.00, 210.00, 220.00, 189.00, 225.00, 204.00,…
## $ Sex    <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M"…
```

```r
skim(elephants)
```


Table: Data summary

|                         |          |
|:------------------------|:---------|
|Name                     |elephants |
|Number of rows           |288       |
|Number of columns        |3         |
|_______________________  |          |
|Column type frequency:   |          |
|character                |1         |
|numeric                  |2         |
|________________________ |          |
|Group variables          |None      |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|Sex           |         0|             1|   1|   1|     0|        2|          0|


**Variable type: numeric**

|skim_variable | n_missing| complete_rate|   mean|   sd|    p0|    p25|    p50|    p75|   p100|hist  |
|:-------------|---------:|-------------:|------:|----:|-----:|------:|------:|------:|------:|:-----|
|Age           |         0|             1|  10.97|  8.4|  0.01|   4.58|   9.46|  16.50|  32.17|▆▇▂▂▂ |
|Height        |         0|             1| 187.68| 50.6| 75.46| 160.75| 200.00| 221.09| 304.06|▃▃▇▇▁ |
4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.

```r
names(elephants)[names(elephants) == 'Age'] <- "age"
names(elephants)[names(elephants) == 'Height'] <- "height"
names(elephants)[names(elephants) == 'Sex'] <- "sex"
elephants$sex <- as.factor(elephants$sex)
```


5. (2 points) How many male and female elephants are represented in the data?

```r
table(elephants$sex)
```

```
## 
##   F   M 
## 150 138
```

```r
#150 females represented and 138 males represented
```
6. (2 points) What is the average age of all elephants in the data?

```r
mean(elephants$age)
```

```
## [1] 10.97132
```

```r
#average age is 10.97132
```
7. (2 points) How does the average age and height of elephants compare by sex?


```r
males <- filter(elephants, sex == 'M')
females <- filter(elephants, sex == 'F')
skim(males)
```


Table: Data summary

|                         |      |
|:------------------------|:-----|
|Name                     |males |
|Number of rows           |138   |
|Number of columns        |3     |
|_______________________  |      |
|Column type frequency:   |      |
|factor                   |1     |
|numeric                  |2     |
|________________________ |      |
|Group variables          |None  |


**Variable type: factor**

|skim_variable | n_missing| complete_rate|ordered | n_unique|top_counts   |
|:-------------|---------:|-------------:|:-------|--------:|:------------|
|sex           |         0|             1|FALSE   |        1|M: 138, F: 0 |


**Variable type: numeric**

|skim_variable | n_missing| complete_rate|   mean|    sd|    p0|    p25|    p50|    p75|   p100|hist  |
|:-------------|---------:|-------------:|------:|-----:|-----:|------:|------:|------:|------:|:-----|
|age           |         0|             1|   8.95|  7.21|  0.01|   3.14|   8.33|  12.00|  28.75|▇▇▃▁▂ |
|height        |         0|             1| 185.13| 55.62| 75.46| 150.49| 194.50| 222.71| 304.06|▅▃▇▅▂ |

```r
skim(females)
```


Table: Data summary

|                         |        |
|:------------------------|:-------|
|Name                     |females |
|Number of rows           |150     |
|Number of columns        |3       |
|_______________________  |        |
|Column type frequency:   |        |
|factor                   |1       |
|numeric                  |2       |
|________________________ |        |
|Group variables          |None    |


**Variable type: factor**

|skim_variable | n_missing| complete_rate|ordered | n_unique|top_counts   |
|:-------------|---------:|-------------:|:-------|--------:|:------------|
|sex           |         0|             1|FALSE   |        1|F: 150, M: 0 |


**Variable type: numeric**

|skim_variable | n_missing| complete_rate|   mean|    sd|    p0|    p25|    p50|    p75|   p100|hist  |
|:-------------|---------:|-------------:|------:|-----:|-----:|------:|------:|------:|------:|:-----|
|age           |         0|             1|  12.84|  8.98|  0.01|   6.62|  11.29|  19.84|  32.17|▆▇▃▃▃ |
|height        |         0|             1| 190.03| 45.57| 75.57| 166.57| 204.88| 220.88| 277.80|▂▂▃▇▁ |

```r
# females mean age = 12.8 and mean height = 190 whereas males mean age = 8.95 and mean height = 185, so males on average are smaller and live shorter lives than females
```

8. (2 points) How does the average height of elephants compare by sex for individuals over 20 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.  

```r
old_elephants <- filter(elephants, age >= 20)
old_males <- filter(old_elephants, sex == 'M')
old_females <- filter(old_elephants, sex == 'F')
skim(old_males)
```


Table: Data summary

|                         |          |
|:------------------------|:---------|
|Name                     |old_males |
|Number of rows           |13        |
|Number of columns        |3         |
|_______________________  |          |
|Column type frequency:   |          |
|factor                   |1         |
|numeric                  |2         |
|________________________ |          |
|Group variables          |None      |


**Variable type: factor**

|skim_variable | n_missing| complete_rate|ordered | n_unique|top_counts  |
|:-------------|---------:|-------------:|:-------|--------:|:-----------|
|sex           |         0|             1|FALSE   |        1|M: 13, F: 0 |


**Variable type: numeric**

|skim_variable | n_missing| complete_rate|   mean|    sd|     p0|    p25|    p50|    p75|   p100|hist  |
|:-------------|---------:|-------------:|------:|-----:|------:|------:|------:|------:|------:|:-----|
|age           |         0|             1|  25.19|  2.25|  21.25|  24.17|  25.42|  26.50|  28.75|▅▇▇▇▅ |
|height        |         0|             1| 269.59| 23.82| 228.69| 253.05| 268.56| 287.13| 304.06|▅▇▇▅▇ |

```r
skim(old_females)
```


Table: Data summary

|                         |            |
|:------------------------|:-----------|
|Name                     |old_females |
|Number of rows           |37          |
|Number of columns        |3           |
|_______________________  |            |
|Column type frequency:   |            |
|factor                   |1           |
|numeric                  |2           |
|________________________ |            |
|Group variables          |None        |


**Variable type: factor**

|skim_variable | n_missing| complete_rate|ordered | n_unique|top_counts  |
|:-------------|---------:|-------------:|:-------|--------:|:-----------|
|sex           |         0|             1|FALSE   |        1|F: 37, M: 0 |


**Variable type: numeric**

|skim_variable | n_missing| complete_rate|   mean|    sd|     p0|    p25|    p50|    p75|   p100|hist  |
|:-------------|---------:|-------------:|------:|-----:|------:|------:|------:|------:|------:|:-----|
|age           |         0|             1|  25.63|  2.97|  20.17|  23.33|  26.08|  27.42|  32.17|▅▂▇▃▂ |
|height        |         0|             1| 232.20| 18.27| 192.54| 221.61| 227.34| 241.94| 277.80|▂▇▇▃▂ |

```r
max(old_elephants$height)
```

```
## [1] 304.06
```

```r
min(old_elephants$height)
```

```
## [1] 192.54
```

```r
# average height of old males = 270, average height of females = 232
# number of individuals = 13 old males + 37 old females = 40 individuals in the analysis
# max height is 304.06 and min height is 192.54
```
For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall 

```r
setwd('~')
ivindo = read.csv('~/Midtermbis15/IvindoData_DryadVersion.csv')
skim(ivindo)
```


Table: Data summary

|                         |       |
|:------------------------|:------|
|Name                     |ivindo |
|Number of rows           |24     |
|Number of columns        |26     |
|_______________________  |       |
|Column type frequency:   |       |
|character                |2      |
|numeric                  |24     |
|________________________ |       |
|Group variables          |None   |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|HuntCat       |         0|             1|   4|   8|     0|        3|          0|
|LandUse       |         0|             1|   4|   7|     0|        3|          0|


**Variable type: numeric**

|skim_variable           | n_missing| complete_rate|  mean|    sd|    p0|   p25|   p50|   p75|  p100|hist  |
|:-----------------------|---------:|-------------:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|:-----|
|TransectID              |         0|             1| 13.50|  8.51|  1.00|  5.75| 14.50| 20.25| 27.00|▇▃▅▆▆ |
|Distance                |         0|             1| 11.88|  7.28|  2.70|  5.67|  9.72| 17.68| 26.76|▇▂▂▅▂ |
|NumHouseholds           |         0|             1| 37.88| 17.80| 13.00| 24.75| 29.00| 54.00| 73.00|▇▇▂▇▂ |
|Veg_Rich                |         0|             1| 14.83|  2.07| 10.88| 13.10| 14.94| 16.54| 18.75|▃▂▃▇▁ |
|Veg_Stems               |         0|             1| 32.80|  5.96| 23.44| 28.69| 32.44| 37.08| 47.56|▆▇▆▃▁ |
|Veg_liana               |         0|             1| 11.04|  3.29|  4.75|  9.03| 11.94| 13.25| 16.38|▃▂▃▇▃ |
|Veg_DBH                 |         0|             1| 46.09| 10.67| 28.45| 40.65| 43.90| 50.57| 76.48|▂▇▃▁▁ |
|Veg_Canopy              |         0|             1|  3.47|  0.35|  2.50|  3.25|  3.43|  3.75|  4.00|▁▁▇▅▇ |
|Veg_Understory          |         0|             1|  3.02|  0.34|  2.38|  2.88|  3.00|  3.17|  3.88|▂▆▇▂▁ |
|RA_Apes                 |         0|             1|  2.04|  3.03|  0.00|  0.00|  0.48|  3.82| 12.93|▇▂▁▁▁ |
|RA_Birds                |         0|             1| 58.64| 14.71| 31.56| 52.51| 57.89| 68.18| 85.03|▅▅▇▇▃ |
|RA_Elephant             |         0|             1|  0.54|  0.67|  0.00|  0.00|  0.36|  0.89|  2.30|▇▂▂▁▁ |
|RA_Monkeys              |         0|             1| 31.30| 12.38|  5.84| 22.70| 31.74| 39.88| 54.12|▂▅▃▇▂ |
|RA_Rodent               |         0|             1|  3.28|  1.47|  1.06|  2.05|  3.23|  4.09|  6.31|▇▅▇▃▃ |
|RA_Ungulate             |         0|             1|  4.17|  4.31|  0.00|  1.23|  2.54|  5.16| 13.86|▇▂▁▁▂ |
|Rich_AllSpecies         |         0|             1| 20.21|  2.06| 15.00| 19.00| 20.00| 22.00| 24.00|▁▁▇▅▁ |
|Evenness_AllSpecies     |         0|             1|  0.77|  0.05|  0.67|  0.75|  0.78|  0.81|  0.83|▃▁▅▇▇ |
|Diversity_AllSpecies    |         0|             1|  2.31|  0.15|  1.97|  2.25|  2.32|  2.43|  2.57|▂▃▇▆▅ |
|Rich_BirdSpecies        |         0|             1| 10.33|  1.24|  8.00| 10.00| 11.00| 11.00| 13.00|▃▅▇▁▁ |
|Evenness_BirdSpecies    |         0|             1|  0.71|  0.08|  0.56|  0.68|  0.72|  0.77|  0.82|▅▁▇▆▇ |
|Diversity_BirdSpecies   |         0|             1|  1.66|  0.20|  1.16|  1.60|  1.68|  1.78|  2.01|▂▂▅▇▃ |
|Rich_MammalSpecies      |         0|             1|  9.88|  1.68|  6.00|  9.00| 10.00| 11.00| 12.00|▂▂▃▅▇ |
|Evenness_MammalSpecies  |         0|             1|  0.75|  0.06|  0.62|  0.71|  0.74|  0.78|  0.86|▂▃▇▂▅ |
|Diversity_MammalSpecies |         0|             1|  1.70|  0.17|  1.38|  1.57|  1.70|  1.81|  2.06|▅▇▇▇▃ |

```r
glimpse(ivindo)
```

```
## Rows: 24
## Columns: 26
## $ TransectID              <int> 1, 2, 2, 3, 4, 5, 6, 7, 8, 9, 13, 14, 15, 16, …
## $ Distance                <dbl> 7.14, 17.31, 18.32, 20.85, 15.95, 17.47, 24.06…
## $ HuntCat                 <chr> "Moderate", "None", "None", "None", "None", "N…
## $ NumHouseholds           <int> 54, 54, 29, 29, 29, 29, 29, 54, 25, 73, 46, 56…
## $ LandUse                 <chr> "Park", "Park", "Park", "Logging", "Park", "Pa…
## $ Veg_Rich                <dbl> 16.67, 15.75, 16.88, 12.44, 17.13, 16.50, 14.7…
## $ Veg_Stems               <dbl> 31.20, 37.44, 32.33, 29.39, 36.00, 29.22, 31.2…
## $ Veg_liana               <dbl> 5.78, 13.25, 4.75, 9.78, 13.25, 12.88, 8.38, 8…
## $ Veg_DBH                 <dbl> 49.57, 34.59, 42.82, 36.62, 41.52, 44.07, 51.2…
## $ Veg_Canopy              <dbl> 3.78, 3.75, 3.43, 3.75, 3.88, 2.50, 4.00, 4.00…
## $ Veg_Understory          <dbl> 2.89, 3.88, 3.00, 2.75, 3.25, 3.00, 2.38, 2.71…
## $ RA_Apes                 <dbl> 1.87, 0.00, 4.49, 12.93, 0.00, 2.48, 3.78, 6.1…
## $ RA_Birds                <dbl> 52.66, 52.17, 37.44, 59.29, 52.62, 38.64, 42.6…
## $ RA_Elephant             <dbl> 0.00, 0.86, 1.33, 0.56, 1.00, 0.00, 1.11, 0.43…
## $ RA_Monkeys              <dbl> 38.59, 28.53, 41.82, 19.85, 41.34, 43.29, 46.2…
## $ RA_Rodent               <dbl> 4.22, 6.04, 1.06, 3.66, 2.52, 1.83, 3.10, 1.26…
## $ RA_Ungulate             <dbl> 2.66, 12.41, 13.86, 3.71, 2.53, 13.75, 3.10, 8…
## $ Rich_AllSpecies         <int> 22, 20, 22, 19, 20, 22, 23, 19, 19, 19, 21, 22…
## $ Evenness_AllSpecies     <dbl> 0.793, 0.773, 0.740, 0.681, 0.811, 0.786, 0.81…
## $ Diversity_AllSpecies    <dbl> 2.452, 2.314, 2.288, 2.006, 2.431, 2.429, 2.56…
## $ Rich_BirdSpecies        <int> 11, 10, 11, 8, 8, 10, 11, 11, 11, 9, 11, 11, 1…
## $ Evenness_BirdSpecies    <dbl> 0.732, 0.704, 0.688, 0.559, 0.799, 0.771, 0.80…
## $ Diversity_BirdSpecies   <dbl> 1.756, 1.620, 1.649, 1.162, 1.660, 1.775, 1.92…
## $ Rich_MammalSpecies      <int> 11, 10, 11, 11, 12, 12, 12, 8, 8, 10, 10, 11, …
## $ Evenness_MammalSpecies  <dbl> 0.736, 0.705, 0.650, 0.619, 0.736, 0.694, 0.77…
## $ Diversity_MammalSpecies <dbl> 1.764, 1.624, 1.558, 1.484, 1.829, 1.725, 1.92…
```
10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?

```r
high_hunt <- filter(ivindo, HuntCat == 'High')
moderate_hunt <- filter(ivindo, HuntCat == 'Moderate')
mean(high_hunt$Diversity_BirdSpecies)
```

```
## [1] 1.660857
```

```r
mean(high_hunt$Diversity_MammalSpecies)
```

```
## [1] 1.737
```

```r
mean(moderate_hunt$Diversity_BirdSpecies)
```

```
## [1] 1.62125
```

```r
mean(moderate_hunt$Diversity_MammalSpecies)
```

```
## [1] 1.68375
```

```r
# high hunt transects Bird Diversity mean = 1.6 , Mammal Diversity mean = 1.74
# moderate hunt transects Bird Diversity mean = 1.62 , Mammal Diversity mean = 1.68
# so high hunt transects receive higher scores in Mammal Diversity but low in Bird Diversity 
```

11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 3km from a village to sites that are greater than 25km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.  

```r
close_to_village <- filter(ivindo, Distance <= 3)
far_from_village <- filter(ivindo, Distance >= 25)
close_to_village$meanRA <- rowMeans(close_to_village[ , c(10,11,12,13,14,15)], na.rm=TRUE)
mean(close_to_village$meanRA)
```

```
## [1] 16.81833
```

```r
far_from_village$meanRA <- rowMeans(far_from_village[ , c(10,11,12,13,14,15)], na.rm=TRUE)
mean(far_from_village$meanRA)
```

```
## [1] 16.225
```

```r
# far _from RAmean = 16.25 and close_to RAmean = 16.8. okay so from this it looks like RA is higher, but just slightly, near the village, but also might not be signicant.
```
12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`

```r
#what is the mean RA of villages not too far and too close but just the right distance from village ie. less than 25km but more than 3km away

just_right <- filter(ivindo, Distance <= 25) 
actually_just_right <- filter(ivindo, Distance >= 3)
actually_just_right$meanRA <- rowMeans(actually_just_right[ , c(10,11,12,13,14,15)], na.rm=TRUE)
mean(actually_just_right$meanRA)
```

```
## [1] 16.47409
```

```r
# mean RA= 16.47, interesting, it is right between the far and close too villages
```
