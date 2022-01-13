---
title: "Transforming data 1: Dplyr `select()`"
date: "2022-01-13"
output:
  html_document: 
    theme: spacelab
    toc: yes
    toc_float: yes
    keep_md: yes
  pdf_document:
    toc: yes
---

## Learning Goals
*At the end of this exercise, you will be able to:*    
1. Use summary functions to assess the structure of a data frame.  
2. Us the select function of `dplyr` to build data frames restricted to variable of interest.  
3. Use the `rename()` function to provide new, consistent names to variables in data frames.  

## Review
At this point, you should have familiarity in RStudio, GitHub, and basic operations in R. If you need extra help, please [email me](mailto: jmledford@ucdavis.edu).  

## Load the tidyverse
For the remainder of the quarter, we will work within the `tidyverse`. At the start of each lab, the library needs to be called as shown below.  

```r
library("tidyverse")
```

## Load the data
These data are from: Gaeta J., G. Sass, S. Carpenter. 2012. Biocomplexity at North Temperate Lakes LTER: Coordinated Field Studies: Large Mouth Bass Growth 2006. Environmental Data Initiative.  [link](https://portal.edirepository.org/nis/mapbrowse?scope=knb-lter-ntl&identifier=267)  

```r
fish <- readr::read_csv("data/Gaeta_etal_CLC_data.csv")
```

```
## Rows: 4033 Columns: 6
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (2): lakeid, annnumber
## dbl (4): fish_id, length, radii_length_mm, scalelength
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

## Data Structure
Once data have been uploaded, let's get an idea of its structure, contents, and dimensions.  

```r
glimpse(fish)
```

```
## Rows: 4,033
## Columns: 6
## $ lakeid          <chr> "AL", "AL", "AL", "AL", "AL", "AL", "AL", "AL", "AL", …
## $ fish_id         <dbl> 299, 299, 299, 300, 300, 300, 300, 301, 301, 301, 301,…
## $ annnumber       <chr> "EDGE", "2", "1", "EDGE", "3", "2", "1", "EDGE", "3", …
## $ length          <dbl> 167, 167, 167, 175, 175, 175, 175, 194, 194, 194, 194,…
## $ radii_length_mm <dbl> 2.697443, 2.037518, 1.311795, 3.015477, 2.670733, 2.13…
## $ scalelength     <dbl> 2.697443, 2.697443, 2.697443, 3.015477, 3.015477, 3.01…
```


```r
str(fish)
```

```
## spec_tbl_df [4,033 × 6] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ lakeid         : chr [1:4033] "AL" "AL" "AL" "AL" ...
##  $ fish_id        : num [1:4033] 299 299 299 300 300 300 300 301 301 301 ...
##  $ annnumber      : chr [1:4033] "EDGE" "2" "1" "EDGE" ...
##  $ length         : num [1:4033] 167 167 167 175 175 175 175 194 194 194 ...
##  $ radii_length_mm: num [1:4033] 2.7 2.04 1.31 3.02 2.67 ...
##  $ scalelength    : num [1:4033] 2.7 2.7 2.7 3.02 3.02 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   lakeid = col_character(),
##   ..   fish_id = col_double(),
##   ..   annnumber = col_character(),
##   ..   length = col_double(),
##   ..   radii_length_mm = col_double(),
##   ..   scalelength = col_double()
##   .. )
##  - attr(*, "problems")=<externalptr>
```


```r
summary(fish)
```

```
##     lakeid             fish_id       annnumber             length     
##  Length:4033        Min.   :  1.0   Length:4033        Min.   : 58.0  
##  Class :character   1st Qu.:156.0   Class :character   1st Qu.:253.0  
##  Mode  :character   Median :267.0   Mode  :character   Median :299.0  
##                     Mean   :258.3                      Mean   :293.3  
##                     3rd Qu.:376.0                      3rd Qu.:342.0  
##                     Max.   :478.0                      Max.   :420.0  
##  radii_length_mm    scalelength     
##  Min.   : 0.4569   Min.   : 0.6282  
##  1st Qu.: 2.3252   1st Qu.: 4.2596  
##  Median : 3.5380   Median : 5.4062  
##  Mean   : 3.6589   Mean   : 5.3821  
##  3rd Qu.: 4.8229   3rd Qu.: 6.4145  
##  Max.   :11.0258   Max.   :11.0258
```


```r
names(fish)
```

```
## [1] "lakeid"          "fish_id"         "annnumber"       "length"         
## [5] "radii_length_mm" "scalelength"
```

## Tidyverse
The [tidyverse](www.tidyverse.org) is an "opinionated" collection of packages that make workflow in R easier. The packages operate more intuitively than base R commands and share a common organizational philosophy.  
![*Data Science Workflow in the Tidyverse.*](tidy-1.png)  

## dplyr
The first package that we will use that is part of the tidyverse is `dplyr`. `dplyr` is used to transform data frames by extracting, rearranging, and summarizing data such that they are focused on a question of interest. This is very helpful,  especially when wrangling large data, and makes dplyr one of most frequently used packages in the tidyverse. The two functions we will use most are `select()` and `filter()`.  

## `select()`
Select allows you to pull out columns of interest from a dataframe. To do this, just add the names of the columns to the `select()` command. The order in which you add them, will determine the order in which they appear in the output.

```r
names(fish)
```

```
## [1] "lakeid"          "fish_id"         "annnumber"       "length"         
## [5] "radii_length_mm" "scalelength"
```

We are only interested in lakeid and scalelength.

```r
select(fish, "lakeid", "scalelength")
```

```
## # A tibble: 4,033 × 2
##    lakeid scalelength
##    <chr>        <dbl>
##  1 AL            2.70
##  2 AL            2.70
##  3 AL            2.70
##  4 AL            3.02
##  5 AL            3.02
##  6 AL            3.02
##  7 AL            3.02
##  8 AL            3.34
##  9 AL            3.34
## 10 AL            3.34
## # … with 4,023 more rows
```

To add a range of columns use `start_col:end_col`.

```r
fish
```

```
## # A tibble: 4,033 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         299 1            167            1.31        2.70
##  4 AL         300 EDGE         175            3.02        3.02
##  5 AL         300 3            175            2.67        3.02
##  6 AL         300 2            175            2.14        3.02
##  7 AL         300 1            175            1.23        3.02
##  8 AL         301 EDGE         194            3.34        3.34
##  9 AL         301 3            194            2.97        3.34
## 10 AL         301 2            194            2.29        3.34
## # … with 4,023 more rows
```


```r
select(fish, fish_id:length)
```

```
## # A tibble: 4,033 × 3
##    fish_id annnumber length
##      <dbl> <chr>      <dbl>
##  1     299 EDGE         167
##  2     299 2            167
##  3     299 1            167
##  4     300 EDGE         175
##  5     300 3            175
##  6     300 2            175
##  7     300 1            175
##  8     301 EDGE         194
##  9     301 3            194
## 10     301 2            194
## # … with 4,023 more rows
```

The - operator is useful in select. It allows us to select everything except the specified variables.

```r
select(fish, -fish_id, -annnumber, -length, -radii_length_mm)
```

```
## # A tibble: 4,033 × 2
##    lakeid scalelength
##    <chr>        <dbl>
##  1 AL            2.70
##  2 AL            2.70
##  3 AL            2.70
##  4 AL            3.02
##  5 AL            3.02
##  6 AL            3.02
##  7 AL            3.02
##  8 AL            3.34
##  9 AL            3.34
## 10 AL            3.34
## # … with 4,023 more rows
```

For very large data frames with lots of variables, `select()` utilizes lots of different operators to make things easier. Let's say we are only interested in the variables that deal with length.


```r
names(fish)
```

```
## [1] "lakeid"          "fish_id"         "annnumber"       "length"         
## [5] "radii_length_mm" "scalelength"
```


```r
select(fish, contains("length"))
```

```
## # A tibble: 4,033 × 3
##    length radii_length_mm scalelength
##     <dbl>           <dbl>       <dbl>
##  1    167            2.70        2.70
##  2    167            2.04        2.70
##  3    167            1.31        2.70
##  4    175            3.02        3.02
##  5    175            2.67        3.02
##  6    175            2.14        3.02
##  7    175            1.23        3.02
##  8    194            3.34        3.34
##  9    194            2.97        3.34
## 10    194            2.29        3.34
## # … with 4,023 more rows
```

When columns are sequentially named, `starts_with()` makes selecting columns easier.

```r
select(fish, starts_with("radii"))
```

```
## # A tibble: 4,033 × 1
##    radii_length_mm
##              <dbl>
##  1            2.70
##  2            2.04
##  3            1.31
##  4            3.02
##  5            2.67
##  6            2.14
##  7            1.23
##  8            3.34
##  9            2.97
## 10            2.29
## # … with 4,023 more rows
```

Options to select columns based on a specific criteria include:  
1. ends_with() = Select columns that end with a character string  
2. contains() = Select columns that contain a character string  
3. matches() = Select columns that match a regular expression  
4. one_of() = Select columns names that are from a group of names  


```r
names(fish)
```

```
## [1] "lakeid"          "fish_id"         "annnumber"       "length"         
## [5] "radii_length_mm" "scalelength"
```


```r
select(fish, ends_with("id"))
```

```
## # A tibble: 4,033 × 2
##    lakeid fish_id
##    <chr>    <dbl>
##  1 AL         299
##  2 AL         299
##  3 AL         299
##  4 AL         300
##  5 AL         300
##  6 AL         300
##  7 AL         300
##  8 AL         301
##  9 AL         301
## 10 AL         301
## # … with 4,023 more rows
```


```r
select(fish, contains("fish"))
```

```
## # A tibble: 4,033 × 1
##    fish_id
##      <dbl>
##  1     299
##  2     299
##  3     299
##  4     300
##  5     300
##  6     300
##  7     300
##  8     301
##  9     301
## 10     301
## # … with 4,023 more rows
```

We won't cover [regex](https://en.wikipedia.org/wiki/Regular_expression) in this class, but the following code is helpful when you know that a column contains a letter (in this case "a") followed by a subsequent string (in this case "er").  

```r
select(fish, matches("a.+er"))
```

```
## # A tibble: 4,033 × 1
##    annnumber
##    <chr>    
##  1 EDGE     
##  2 2        
##  3 1        
##  4 EDGE     
##  5 3        
##  6 2        
##  7 1        
##  8 EDGE     
##  9 3        
## 10 2        
## # … with 4,023 more rows
```

You can also select columns based on the class of data.  

```r
glimpse(fish)
```

```
## Rows: 4,033
## Columns: 6
## $ lakeid          <chr> "AL", "AL", "AL", "AL", "AL", "AL", "AL", "AL", "AL", …
## $ fish_id         <dbl> 299, 299, 299, 300, 300, 300, 300, 301, 301, 301, 301,…
## $ annnumber       <chr> "EDGE", "2", "1", "EDGE", "3", "2", "1", "EDGE", "3", …
## $ length          <dbl> 167, 167, 167, 175, 175, 175, 175, 194, 194, 194, 194,…
## $ radii_length_mm <dbl> 2.697443, 2.037518, 1.311795, 3.015477, 2.670733, 2.13…
## $ scalelength     <dbl> 2.697443, 2.697443, 2.697443, 3.015477, 3.015477, 3.01…
```


```r
select_if(fish, is.numeric)
```

```
## # A tibble: 4,033 × 4
##    fish_id length radii_length_mm scalelength
##      <dbl>  <dbl>           <dbl>       <dbl>
##  1     299    167            2.70        2.70
##  2     299    167            2.04        2.70
##  3     299    167            1.31        2.70
##  4     300    175            3.02        3.02
##  5     300    175            2.67        3.02
##  6     300    175            2.14        3.02
##  7     300    175            1.23        3.02
##  8     301    194            3.34        3.34
##  9     301    194            2.97        3.34
## 10     301    194            2.29        3.34
## # … with 4,023 more rows
```

To select all columns that are *not* a class of data, you need to add a `~`.

```r
select_if(fish, ~!is.numeric(.))
```

```
## # A tibble: 4,033 × 2
##    lakeid annnumber
##    <chr>  <chr>    
##  1 AL     EDGE     
##  2 AL     2        
##  3 AL     1        
##  4 AL     EDGE     
##  5 AL     3        
##  6 AL     2        
##  7 AL     1        
##  8 AL     EDGE     
##  9 AL     3        
## 10 AL     2        
## # … with 4,023 more rows
```

## Practice  
For this exercise we will use life history data for mammals. The [data](http://esapubs.org/archive/ecol/E084/093/) are from:  
**S. K. Morgan Ernest. 2003. Life history characteristics of placental non-volant mammals. Ecology 84:3402.**  

```r
mammals <- readr::read_csv("data/mammal_lifehistories_v2.csv")
```

```
## Rows: 1440 Columns: 13
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): order, family, Genus, species
## dbl (9): mass, gestation, newborn, weaning, wean mass, AFR, max. life, litte...
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. Use one or more of your favorite functions to assess the structure of the data.  

```r
glimpse(mammals)
```

```
## Rows: 1,440
## Columns: 13
## $ order          <chr> "Artiodactyla", "Artiodactyla", "Artiodactyla", "Artiod…
## $ family         <chr> "Antilocapridae", "Bovidae", "Bovidae", "Bovidae", "Bov…
## $ Genus          <chr> "Antilocapra", "Addax", "Aepyceros", "Alcelaphus", "Amm…
## $ species        <chr> "americana", "nasomaculatus", "melampus", "buselaphus",…
## $ mass           <dbl> 45375.0, 182375.0, 41480.0, 150000.0, 28500.0, 55500.0,…
## $ gestation      <dbl> 8.13, 9.39, 6.35, 7.90, 6.80, 5.08, 5.72, 5.50, 8.93, 9…
## $ newborn        <dbl> 3246.36, 5480.00, 5093.00, 10166.67, -999.00, 3810.00, …
## $ weaning        <dbl> 3.00, 6.50, 5.63, 6.50, -999.00, 4.00, 4.04, 2.13, 10.7…
## $ `wean mass`    <dbl> 8900, -999, 15900, -999, -999, -999, -999, -999, 157500…
## $ AFR            <dbl> 13.53, 27.27, 16.66, 23.02, -999.00, 14.89, 10.23, 20.1…
## $ `max. life`    <dbl> 142, 308, 213, 240, -999, 251, 228, 255, 300, 324, 300,…
## $ `litter size`  <dbl> 1.85, 1.00, 1.00, 1.00, 1.00, 1.37, 1.00, 1.00, 1.00, 1…
## $ `litters/year` <dbl> 1.00, 0.99, 0.95, -999.00, -999.00, 2.00, -999.00, 1.89…
```

2. Are there any NAs? Are you sure? Try taking an average of `max. life` as a test.  

```r
is.na.data.frame(mammals)
```

```
##         order family Genus species  mass gestation newborn weaning wean mass
##    [1,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##    [2,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##    [3,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##    [4,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##    [5,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##    [6,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##    [7,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##    [8,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##    [9,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [10,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [11,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [12,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [13,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [14,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [15,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [16,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [17,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [18,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [19,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [20,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [21,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [22,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [23,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [24,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [25,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [26,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [27,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [28,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [29,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [30,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [31,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [32,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [33,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [34,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [35,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [36,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [37,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [38,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [39,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [40,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [41,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [42,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [43,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [44,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [45,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [46,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [47,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [48,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [49,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [50,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [51,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [52,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [53,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [54,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [55,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [56,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [57,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [58,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [59,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [60,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [61,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [62,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [63,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [64,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [65,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [66,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [67,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [68,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [69,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [70,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [71,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [72,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [73,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [74,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [75,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [76,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [77,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [78,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [79,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [80,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [81,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [82,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [83,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [84,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [85,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [86,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [87,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [88,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [89,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [90,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [91,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [92,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [93,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [94,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [95,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [96,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [97,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [98,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##   [99,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [100,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [101,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [102,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [103,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [104,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [105,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [106,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [107,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [108,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [109,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [110,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [111,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [112,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [113,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [114,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [115,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [116,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [117,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [118,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [119,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [120,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [121,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [122,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [123,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [124,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [125,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [126,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [127,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [128,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [129,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [130,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [131,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [132,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [133,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [134,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [135,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [136,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [137,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [138,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [139,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [140,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [141,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [142,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [143,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [144,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [145,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [146,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [147,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [148,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [149,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [150,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [151,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [152,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [153,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [154,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [155,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [156,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [157,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [158,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [159,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [160,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [161,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [162,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [163,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [164,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [165,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [166,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [167,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [168,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [169,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [170,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [171,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [172,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [173,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [174,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [175,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [176,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [177,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [178,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [179,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [180,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [181,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [182,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [183,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [184,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [185,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [186,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [187,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [188,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [189,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [190,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [191,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [192,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [193,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [194,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [195,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [196,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [197,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [198,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [199,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [200,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [201,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [202,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [203,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [204,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [205,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [206,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [207,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [208,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [209,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [210,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [211,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [212,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [213,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [214,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [215,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [216,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [217,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [218,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [219,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [220,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [221,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [222,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [223,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [224,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [225,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [226,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [227,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [228,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [229,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [230,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [231,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [232,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [233,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [234,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [235,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [236,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [237,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [238,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [239,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [240,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [241,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [242,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [243,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [244,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [245,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [246,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [247,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [248,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [249,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [250,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [251,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [252,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [253,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [254,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [255,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [256,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [257,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [258,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [259,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [260,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [261,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [262,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [263,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [264,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [265,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [266,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [267,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [268,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [269,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [270,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [271,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [272,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [273,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [274,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [275,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [276,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [277,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [278,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [279,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [280,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [281,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [282,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [283,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [284,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [285,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [286,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [287,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [288,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [289,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [290,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [291,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [292,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [293,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [294,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [295,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [296,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [297,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [298,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [299,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [300,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [301,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [302,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [303,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [304,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [305,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [306,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [307,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [308,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [309,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [310,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [311,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [312,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [313,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [314,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [315,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [316,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [317,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [318,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [319,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [320,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [321,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [322,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [323,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [324,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [325,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [326,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [327,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [328,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [329,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [330,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [331,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [332,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [333,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [334,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [335,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [336,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [337,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [338,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [339,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [340,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [341,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [342,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [343,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [344,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [345,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [346,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [347,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [348,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [349,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [350,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [351,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [352,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [353,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [354,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [355,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [356,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [357,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [358,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [359,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [360,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [361,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [362,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [363,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [364,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [365,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [366,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [367,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [368,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [369,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [370,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [371,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [372,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [373,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [374,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [375,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [376,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [377,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [378,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [379,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [380,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [381,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [382,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [383,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [384,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [385,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [386,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [387,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [388,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [389,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [390,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [391,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [392,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [393,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [394,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [395,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [396,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [397,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [398,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [399,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [400,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [401,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [402,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [403,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [404,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [405,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [406,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [407,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [408,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [409,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [410,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [411,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [412,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [413,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [414,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [415,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [416,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [417,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [418,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [419,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [420,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [421,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [422,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [423,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [424,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [425,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [426,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [427,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [428,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [429,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [430,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [431,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [432,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [433,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [434,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [435,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [436,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [437,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [438,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [439,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [440,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [441,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [442,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [443,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [444,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [445,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [446,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [447,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [448,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [449,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [450,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [451,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [452,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [453,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [454,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [455,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [456,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [457,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [458,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [459,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [460,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [461,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [462,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [463,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [464,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [465,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [466,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [467,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [468,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [469,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [470,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [471,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [472,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [473,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [474,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [475,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [476,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [477,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [478,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [479,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [480,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [481,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [482,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [483,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [484,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [485,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [486,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [487,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [488,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [489,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [490,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [491,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [492,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [493,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [494,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [495,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [496,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [497,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [498,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [499,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [500,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [501,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [502,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [503,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [504,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [505,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [506,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [507,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [508,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [509,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [510,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [511,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [512,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [513,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [514,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [515,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [516,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [517,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [518,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [519,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [520,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [521,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [522,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [523,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [524,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [525,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [526,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [527,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [528,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [529,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [530,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [531,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [532,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [533,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [534,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [535,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [536,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [537,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [538,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [539,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [540,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [541,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [542,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [543,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [544,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [545,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [546,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [547,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [548,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [549,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [550,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [551,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [552,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [553,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [554,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [555,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [556,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [557,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [558,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [559,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [560,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [561,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [562,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [563,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [564,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [565,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [566,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [567,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [568,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [569,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [570,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [571,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [572,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [573,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [574,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [575,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [576,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [577,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [578,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [579,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [580,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [581,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [582,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [583,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [584,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [585,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [586,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [587,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [588,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [589,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [590,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [591,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [592,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [593,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [594,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [595,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [596,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [597,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [598,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [599,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [600,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [601,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [602,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [603,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [604,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [605,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [606,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [607,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [608,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [609,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [610,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [611,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [612,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [613,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [614,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [615,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [616,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [617,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [618,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [619,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [620,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [621,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [622,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [623,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [624,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [625,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [626,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [627,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [628,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [629,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [630,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [631,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [632,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [633,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [634,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [635,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [636,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [637,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [638,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [639,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [640,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [641,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [642,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [643,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [644,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [645,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [646,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [647,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [648,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [649,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [650,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [651,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [652,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [653,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [654,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [655,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [656,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [657,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [658,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [659,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [660,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [661,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [662,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [663,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [664,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [665,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [666,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [667,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [668,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [669,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [670,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [671,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [672,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [673,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [674,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [675,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [676,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [677,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [678,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [679,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [680,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [681,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [682,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [683,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [684,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [685,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [686,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [687,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [688,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [689,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [690,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [691,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [692,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [693,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [694,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [695,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [696,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [697,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [698,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [699,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [700,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [701,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [702,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [703,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [704,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [705,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [706,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [707,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [708,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [709,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [710,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [711,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [712,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [713,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [714,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [715,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [716,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [717,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [718,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [719,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [720,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [721,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [722,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [723,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [724,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [725,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [726,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [727,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [728,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [729,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [730,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [731,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [732,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [733,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [734,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [735,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [736,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [737,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [738,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [739,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [740,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [741,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [742,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [743,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [744,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [745,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [746,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [747,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [748,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [749,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [750,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [751,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [752,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [753,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [754,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [755,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [756,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [757,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [758,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [759,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [760,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [761,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [762,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [763,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [764,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [765,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [766,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [767,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [768,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [769,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [770,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [771,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [772,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [773,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [774,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [775,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [776,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [777,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [778,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [779,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [780,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [781,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [782,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [783,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [784,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [785,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [786,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [787,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [788,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [789,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [790,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [791,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [792,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [793,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [794,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [795,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [796,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [797,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [798,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [799,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [800,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [801,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [802,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [803,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [804,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [805,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [806,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [807,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [808,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [809,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [810,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [811,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [812,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [813,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [814,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [815,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [816,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [817,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [818,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [819,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [820,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [821,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [822,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [823,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [824,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [825,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [826,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [827,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [828,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [829,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [830,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [831,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [832,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [833,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [834,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [835,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [836,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [837,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [838,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [839,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [840,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [841,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [842,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [843,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [844,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [845,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [846,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [847,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [848,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [849,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [850,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [851,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [852,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [853,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [854,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [855,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [856,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [857,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [858,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [859,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [860,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [861,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [862,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [863,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [864,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [865,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [866,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [867,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [868,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [869,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [870,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [871,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [872,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [873,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [874,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [875,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [876,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [877,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [878,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [879,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [880,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [881,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [882,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [883,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [884,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [885,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [886,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [887,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [888,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [889,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [890,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [891,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [892,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [893,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [894,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [895,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [896,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [897,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [898,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [899,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [900,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [901,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [902,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [903,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [904,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [905,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [906,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [907,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [908,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [909,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [910,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [911,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [912,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [913,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [914,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [915,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [916,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [917,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [918,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [919,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [920,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [921,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [922,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [923,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [924,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [925,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [926,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [927,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [928,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [929,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [930,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [931,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [932,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [933,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [934,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [935,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [936,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [937,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [938,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [939,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [940,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [941,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [942,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [943,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [944,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [945,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [946,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [947,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [948,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [949,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [950,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [951,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [952,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [953,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [954,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [955,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [956,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [957,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [958,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [959,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [960,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [961,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [962,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [963,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [964,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [965,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [966,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [967,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [968,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [969,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [970,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [971,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [972,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [973,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [974,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [975,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [976,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [977,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [978,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [979,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [980,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [981,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [982,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [983,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [984,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [985,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [986,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [987,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [988,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [989,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [990,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [991,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [992,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [993,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [994,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [995,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [996,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [997,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [998,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##  [999,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1000,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1001,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1002,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1003,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1004,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1005,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1006,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1007,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1008,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1009,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1010,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1011,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1012,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1013,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1014,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1015,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1016,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1017,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1018,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1019,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1020,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1021,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1022,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1023,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1024,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1025,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1026,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1027,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1028,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1029,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1030,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1031,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1032,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1033,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1034,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1035,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1036,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1037,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1038,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1039,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1040,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1041,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1042,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1043,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1044,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1045,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1046,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1047,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1048,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1049,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1050,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1051,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1052,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1053,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1054,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1055,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1056,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1057,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1058,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1059,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1060,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1061,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1062,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1063,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1064,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1065,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1066,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1067,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1068,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1069,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1070,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1071,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1072,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1073,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1074,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1075,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1076,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1077,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1078,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1079,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1080,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1081,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1082,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1083,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1084,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1085,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1086,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1087,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1088,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1089,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1090,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1091,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1092,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1093,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1094,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1095,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1096,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1097,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1098,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1099,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1100,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1101,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1102,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1103,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1104,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1105,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1106,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1107,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1108,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1109,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1110,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1111,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1112,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1113,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1114,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1115,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1116,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1117,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1118,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1119,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1120,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1121,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1122,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1123,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1124,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1125,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1126,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1127,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1128,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1129,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1130,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1131,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1132,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1133,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1134,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1135,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1136,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1137,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1138,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1139,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1140,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1141,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1142,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1143,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1144,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1145,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1146,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1147,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1148,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1149,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1150,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1151,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1152,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1153,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1154,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1155,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1156,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1157,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1158,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1159,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1160,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1161,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1162,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1163,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1164,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1165,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1166,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1167,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1168,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1169,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1170,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1171,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1172,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1173,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1174,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1175,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1176,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1177,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1178,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1179,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1180,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1181,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1182,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1183,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1184,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1185,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1186,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1187,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1188,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1189,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1190,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1191,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1192,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1193,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1194,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1195,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1196,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1197,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1198,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1199,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1200,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1201,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1202,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1203,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1204,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1205,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1206,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1207,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1208,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1209,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1210,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1211,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1212,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1213,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1214,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1215,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1216,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1217,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1218,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1219,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1220,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1221,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1222,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1223,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1224,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1225,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1226,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1227,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1228,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1229,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1230,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1231,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1232,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1233,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1234,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1235,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1236,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1237,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1238,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1239,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1240,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1241,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1242,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1243,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1244,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1245,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1246,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1247,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1248,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1249,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1250,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1251,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1252,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1253,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1254,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1255,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1256,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1257,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1258,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1259,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1260,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1261,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1262,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1263,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1264,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1265,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1266,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1267,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1268,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1269,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1270,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1271,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1272,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1273,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1274,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1275,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1276,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1277,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1278,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1279,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1280,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1281,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1282,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1283,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1284,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1285,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1286,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1287,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1288,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1289,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1290,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1291,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1292,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1293,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1294,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1295,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1296,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1297,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1298,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1299,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1300,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1301,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1302,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1303,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1304,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1305,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1306,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1307,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1308,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1309,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1310,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1311,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1312,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1313,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1314,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1315,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1316,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1317,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1318,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1319,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1320,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1321,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1322,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1323,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1324,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1325,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1326,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1327,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1328,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1329,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1330,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1331,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1332,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1333,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1334,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1335,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1336,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1337,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1338,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1339,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1340,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1341,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1342,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1343,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1344,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1345,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1346,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1347,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1348,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1349,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1350,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1351,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1352,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1353,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1354,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1355,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1356,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1357,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1358,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1359,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1360,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1361,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1362,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1363,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1364,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1365,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1366,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1367,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1368,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1369,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1370,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1371,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1372,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1373,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1374,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1375,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1376,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1377,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1378,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1379,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1380,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1381,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1382,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1383,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1384,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1385,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1386,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1387,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1388,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1389,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1390,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1391,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1392,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1393,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1394,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1395,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1396,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1397,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1398,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1399,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1400,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1401,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1402,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1403,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1404,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1405,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1406,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1407,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1408,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1409,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1410,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1411,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1412,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1413,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1414,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1415,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1416,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1417,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1418,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1419,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1420,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1421,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1422,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1423,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1424,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1425,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1426,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1427,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1428,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1429,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1430,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1431,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1432,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1433,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1434,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1435,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1436,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1437,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1438,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1439,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
## [1440,] FALSE  FALSE FALSE   FALSE FALSE     FALSE   FALSE   FALSE     FALSE
##           AFR max. life litter size litters/year
##    [1,] FALSE     FALSE       FALSE        FALSE
##    [2,] FALSE     FALSE       FALSE        FALSE
##    [3,] FALSE     FALSE       FALSE        FALSE
##    [4,] FALSE     FALSE       FALSE        FALSE
##    [5,] FALSE     FALSE       FALSE        FALSE
##    [6,] FALSE     FALSE       FALSE        FALSE
##    [7,] FALSE     FALSE       FALSE        FALSE
##    [8,] FALSE     FALSE       FALSE        FALSE
##    [9,] FALSE     FALSE       FALSE        FALSE
##   [10,] FALSE     FALSE       FALSE        FALSE
##   [11,] FALSE     FALSE       FALSE        FALSE
##   [12,] FALSE     FALSE       FALSE        FALSE
##   [13,] FALSE     FALSE       FALSE        FALSE
##   [14,] FALSE     FALSE       FALSE        FALSE
##   [15,] FALSE     FALSE       FALSE        FALSE
##   [16,] FALSE     FALSE       FALSE        FALSE
##   [17,] FALSE     FALSE       FALSE        FALSE
##   [18,] FALSE     FALSE       FALSE        FALSE
##   [19,] FALSE     FALSE       FALSE        FALSE
##   [20,] FALSE     FALSE       FALSE        FALSE
##   [21,] FALSE     FALSE       FALSE        FALSE
##   [22,] FALSE     FALSE       FALSE        FALSE
##   [23,] FALSE     FALSE       FALSE        FALSE
##   [24,] FALSE     FALSE       FALSE        FALSE
##   [25,] FALSE     FALSE       FALSE        FALSE
##   [26,] FALSE     FALSE       FALSE        FALSE
##   [27,] FALSE     FALSE       FALSE        FALSE
##   [28,] FALSE     FALSE       FALSE        FALSE
##   [29,] FALSE     FALSE       FALSE        FALSE
##   [30,] FALSE     FALSE       FALSE        FALSE
##   [31,] FALSE     FALSE       FALSE        FALSE
##   [32,] FALSE     FALSE       FALSE        FALSE
##   [33,] FALSE     FALSE       FALSE        FALSE
##   [34,] FALSE     FALSE       FALSE        FALSE
##   [35,] FALSE     FALSE       FALSE        FALSE
##   [36,] FALSE     FALSE       FALSE        FALSE
##   [37,] FALSE     FALSE       FALSE        FALSE
##   [38,] FALSE     FALSE       FALSE        FALSE
##   [39,] FALSE     FALSE       FALSE        FALSE
##   [40,] FALSE     FALSE       FALSE        FALSE
##   [41,] FALSE     FALSE       FALSE        FALSE
##   [42,] FALSE     FALSE       FALSE        FALSE
##   [43,] FALSE     FALSE       FALSE        FALSE
##   [44,] FALSE     FALSE       FALSE        FALSE
##   [45,] FALSE     FALSE       FALSE        FALSE
##   [46,] FALSE     FALSE       FALSE        FALSE
##   [47,] FALSE     FALSE       FALSE        FALSE
##   [48,] FALSE     FALSE       FALSE        FALSE
##   [49,] FALSE     FALSE       FALSE        FALSE
##   [50,] FALSE     FALSE       FALSE        FALSE
##   [51,] FALSE     FALSE       FALSE        FALSE
##   [52,] FALSE     FALSE       FALSE        FALSE
##   [53,] FALSE     FALSE       FALSE        FALSE
##   [54,] FALSE     FALSE       FALSE        FALSE
##   [55,] FALSE     FALSE       FALSE        FALSE
##   [56,] FALSE     FALSE       FALSE        FALSE
##   [57,] FALSE     FALSE       FALSE        FALSE
##   [58,] FALSE     FALSE       FALSE        FALSE
##   [59,] FALSE     FALSE       FALSE        FALSE
##   [60,] FALSE     FALSE       FALSE        FALSE
##   [61,] FALSE     FALSE       FALSE        FALSE
##   [62,] FALSE     FALSE       FALSE        FALSE
##   [63,] FALSE     FALSE       FALSE        FALSE
##   [64,] FALSE     FALSE       FALSE        FALSE
##   [65,] FALSE     FALSE       FALSE        FALSE
##   [66,] FALSE     FALSE       FALSE        FALSE
##   [67,] FALSE     FALSE       FALSE        FALSE
##   [68,] FALSE     FALSE       FALSE        FALSE
##   [69,] FALSE     FALSE       FALSE        FALSE
##   [70,] FALSE     FALSE       FALSE        FALSE
##   [71,] FALSE     FALSE       FALSE        FALSE
##   [72,] FALSE     FALSE       FALSE        FALSE
##   [73,] FALSE     FALSE       FALSE        FALSE
##   [74,] FALSE     FALSE       FALSE        FALSE
##   [75,] FALSE     FALSE       FALSE        FALSE
##   [76,] FALSE     FALSE       FALSE        FALSE
##   [77,] FALSE     FALSE       FALSE        FALSE
##   [78,] FALSE     FALSE       FALSE        FALSE
##   [79,] FALSE     FALSE       FALSE        FALSE
##   [80,] FALSE     FALSE       FALSE        FALSE
##   [81,] FALSE     FALSE       FALSE        FALSE
##   [82,] FALSE     FALSE       FALSE        FALSE
##   [83,] FALSE     FALSE       FALSE        FALSE
##   [84,] FALSE     FALSE       FALSE        FALSE
##   [85,] FALSE     FALSE       FALSE        FALSE
##   [86,] FALSE     FALSE       FALSE        FALSE
##   [87,] FALSE     FALSE       FALSE        FALSE
##   [88,] FALSE     FALSE       FALSE        FALSE
##   [89,] FALSE     FALSE       FALSE        FALSE
##   [90,] FALSE     FALSE       FALSE        FALSE
##   [91,] FALSE     FALSE       FALSE        FALSE
##   [92,] FALSE     FALSE       FALSE        FALSE
##   [93,] FALSE     FALSE       FALSE        FALSE
##   [94,] FALSE     FALSE       FALSE        FALSE
##   [95,] FALSE     FALSE       FALSE        FALSE
##   [96,] FALSE     FALSE       FALSE        FALSE
##   [97,] FALSE     FALSE       FALSE        FALSE
##   [98,] FALSE     FALSE       FALSE        FALSE
##   [99,] FALSE     FALSE       FALSE        FALSE
##  [100,] FALSE     FALSE       FALSE        FALSE
##  [101,] FALSE     FALSE       FALSE        FALSE
##  [102,] FALSE     FALSE       FALSE        FALSE
##  [103,] FALSE     FALSE       FALSE        FALSE
##  [104,] FALSE     FALSE       FALSE        FALSE
##  [105,] FALSE     FALSE       FALSE        FALSE
##  [106,] FALSE     FALSE       FALSE        FALSE
##  [107,] FALSE     FALSE       FALSE        FALSE
##  [108,] FALSE     FALSE       FALSE        FALSE
##  [109,] FALSE     FALSE       FALSE        FALSE
##  [110,] FALSE     FALSE       FALSE        FALSE
##  [111,] FALSE     FALSE       FALSE        FALSE
##  [112,] FALSE     FALSE       FALSE        FALSE
##  [113,] FALSE     FALSE       FALSE        FALSE
##  [114,] FALSE     FALSE       FALSE        FALSE
##  [115,] FALSE     FALSE       FALSE        FALSE
##  [116,] FALSE     FALSE       FALSE        FALSE
##  [117,] FALSE     FALSE       FALSE        FALSE
##  [118,] FALSE     FALSE       FALSE        FALSE
##  [119,] FALSE     FALSE       FALSE        FALSE
##  [120,] FALSE     FALSE       FALSE        FALSE
##  [121,] FALSE     FALSE       FALSE        FALSE
##  [122,] FALSE     FALSE       FALSE        FALSE
##  [123,] FALSE     FALSE       FALSE        FALSE
##  [124,] FALSE     FALSE       FALSE        FALSE
##  [125,] FALSE     FALSE       FALSE        FALSE
##  [126,] FALSE     FALSE       FALSE        FALSE
##  [127,] FALSE     FALSE       FALSE        FALSE
##  [128,] FALSE     FALSE       FALSE        FALSE
##  [129,] FALSE     FALSE       FALSE        FALSE
##  [130,] FALSE     FALSE       FALSE        FALSE
##  [131,] FALSE     FALSE       FALSE        FALSE
##  [132,] FALSE     FALSE       FALSE        FALSE
##  [133,] FALSE     FALSE       FALSE        FALSE
##  [134,] FALSE     FALSE       FALSE        FALSE
##  [135,] FALSE     FALSE       FALSE        FALSE
##  [136,] FALSE     FALSE       FALSE        FALSE
##  [137,] FALSE     FALSE       FALSE        FALSE
##  [138,] FALSE     FALSE       FALSE        FALSE
##  [139,] FALSE     FALSE       FALSE        FALSE
##  [140,] FALSE     FALSE       FALSE        FALSE
##  [141,] FALSE     FALSE       FALSE        FALSE
##  [142,] FALSE     FALSE       FALSE        FALSE
##  [143,] FALSE     FALSE       FALSE        FALSE
##  [144,] FALSE     FALSE       FALSE        FALSE
##  [145,] FALSE     FALSE       FALSE        FALSE
##  [146,] FALSE     FALSE       FALSE        FALSE
##  [147,] FALSE     FALSE       FALSE        FALSE
##  [148,] FALSE     FALSE       FALSE        FALSE
##  [149,] FALSE     FALSE       FALSE        FALSE
##  [150,] FALSE     FALSE       FALSE        FALSE
##  [151,] FALSE     FALSE       FALSE        FALSE
##  [152,] FALSE     FALSE       FALSE        FALSE
##  [153,] FALSE     FALSE       FALSE        FALSE
##  [154,] FALSE     FALSE       FALSE        FALSE
##  [155,] FALSE     FALSE       FALSE        FALSE
##  [156,] FALSE     FALSE       FALSE        FALSE
##  [157,] FALSE     FALSE       FALSE        FALSE
##  [158,] FALSE     FALSE       FALSE        FALSE
##  [159,] FALSE     FALSE       FALSE        FALSE
##  [160,] FALSE     FALSE       FALSE        FALSE
##  [161,] FALSE     FALSE       FALSE        FALSE
##  [162,] FALSE     FALSE       FALSE        FALSE
##  [163,] FALSE     FALSE       FALSE        FALSE
##  [164,] FALSE     FALSE       FALSE        FALSE
##  [165,] FALSE     FALSE       FALSE        FALSE
##  [166,] FALSE     FALSE       FALSE        FALSE
##  [167,] FALSE     FALSE       FALSE        FALSE
##  [168,] FALSE     FALSE       FALSE        FALSE
##  [169,] FALSE     FALSE       FALSE        FALSE
##  [170,] FALSE     FALSE       FALSE        FALSE
##  [171,] FALSE     FALSE       FALSE        FALSE
##  [172,] FALSE     FALSE       FALSE        FALSE
##  [173,] FALSE     FALSE       FALSE        FALSE
##  [174,] FALSE     FALSE       FALSE        FALSE
##  [175,] FALSE     FALSE       FALSE        FALSE
##  [176,] FALSE     FALSE       FALSE        FALSE
##  [177,] FALSE     FALSE       FALSE        FALSE
##  [178,] FALSE     FALSE       FALSE        FALSE
##  [179,] FALSE     FALSE       FALSE        FALSE
##  [180,] FALSE     FALSE       FALSE        FALSE
##  [181,] FALSE     FALSE       FALSE        FALSE
##  [182,] FALSE     FALSE       FALSE        FALSE
##  [183,] FALSE     FALSE       FALSE        FALSE
##  [184,] FALSE     FALSE       FALSE        FALSE
##  [185,] FALSE     FALSE       FALSE        FALSE
##  [186,] FALSE     FALSE       FALSE        FALSE
##  [187,] FALSE     FALSE       FALSE        FALSE
##  [188,] FALSE     FALSE       FALSE        FALSE
##  [189,] FALSE     FALSE       FALSE        FALSE
##  [190,] FALSE     FALSE       FALSE        FALSE
##  [191,] FALSE     FALSE       FALSE        FALSE
##  [192,] FALSE     FALSE       FALSE        FALSE
##  [193,] FALSE     FALSE       FALSE        FALSE
##  [194,] FALSE     FALSE       FALSE        FALSE
##  [195,] FALSE     FALSE       FALSE        FALSE
##  [196,] FALSE     FALSE       FALSE        FALSE
##  [197,] FALSE     FALSE       FALSE        FALSE
##  [198,] FALSE     FALSE       FALSE        FALSE
##  [199,] FALSE     FALSE       FALSE        FALSE
##  [200,] FALSE     FALSE       FALSE        FALSE
##  [201,] FALSE     FALSE       FALSE        FALSE
##  [202,] FALSE     FALSE       FALSE        FALSE
##  [203,] FALSE     FALSE       FALSE        FALSE
##  [204,] FALSE     FALSE       FALSE        FALSE
##  [205,] FALSE     FALSE       FALSE        FALSE
##  [206,] FALSE     FALSE       FALSE        FALSE
##  [207,] FALSE     FALSE       FALSE        FALSE
##  [208,] FALSE     FALSE       FALSE        FALSE
##  [209,] FALSE     FALSE       FALSE        FALSE
##  [210,] FALSE     FALSE       FALSE        FALSE
##  [211,] FALSE     FALSE       FALSE        FALSE
##  [212,] FALSE     FALSE       FALSE        FALSE
##  [213,] FALSE     FALSE       FALSE        FALSE
##  [214,] FALSE     FALSE       FALSE        FALSE
##  [215,] FALSE     FALSE       FALSE        FALSE
##  [216,] FALSE     FALSE       FALSE        FALSE
##  [217,] FALSE     FALSE       FALSE        FALSE
##  [218,] FALSE     FALSE       FALSE        FALSE
##  [219,] FALSE     FALSE       FALSE        FALSE
##  [220,] FALSE     FALSE       FALSE        FALSE
##  [221,] FALSE     FALSE       FALSE        FALSE
##  [222,] FALSE     FALSE       FALSE        FALSE
##  [223,] FALSE     FALSE       FALSE        FALSE
##  [224,] FALSE     FALSE       FALSE        FALSE
##  [225,] FALSE     FALSE       FALSE        FALSE
##  [226,] FALSE     FALSE       FALSE        FALSE
##  [227,] FALSE     FALSE       FALSE        FALSE
##  [228,] FALSE     FALSE       FALSE        FALSE
##  [229,] FALSE     FALSE       FALSE        FALSE
##  [230,] FALSE     FALSE       FALSE        FALSE
##  [231,] FALSE     FALSE       FALSE        FALSE
##  [232,] FALSE     FALSE       FALSE        FALSE
##  [233,] FALSE     FALSE       FALSE        FALSE
##  [234,] FALSE     FALSE       FALSE        FALSE
##  [235,] FALSE     FALSE       FALSE        FALSE
##  [236,] FALSE     FALSE       FALSE        FALSE
##  [237,] FALSE     FALSE       FALSE        FALSE
##  [238,] FALSE     FALSE       FALSE        FALSE
##  [239,] FALSE     FALSE       FALSE        FALSE
##  [240,] FALSE     FALSE       FALSE        FALSE
##  [241,] FALSE     FALSE       FALSE        FALSE
##  [242,] FALSE     FALSE       FALSE        FALSE
##  [243,] FALSE     FALSE       FALSE        FALSE
##  [244,] FALSE     FALSE       FALSE        FALSE
##  [245,] FALSE     FALSE       FALSE        FALSE
##  [246,] FALSE     FALSE       FALSE        FALSE
##  [247,] FALSE     FALSE       FALSE        FALSE
##  [248,] FALSE     FALSE       FALSE        FALSE
##  [249,] FALSE     FALSE       FALSE        FALSE
##  [250,] FALSE     FALSE       FALSE        FALSE
##  [251,] FALSE     FALSE       FALSE        FALSE
##  [252,] FALSE     FALSE       FALSE        FALSE
##  [253,] FALSE     FALSE       FALSE        FALSE
##  [254,] FALSE     FALSE       FALSE        FALSE
##  [255,] FALSE     FALSE       FALSE        FALSE
##  [256,] FALSE     FALSE       FALSE        FALSE
##  [257,] FALSE     FALSE       FALSE        FALSE
##  [258,] FALSE     FALSE       FALSE        FALSE
##  [259,] FALSE     FALSE       FALSE        FALSE
##  [260,] FALSE     FALSE       FALSE        FALSE
##  [261,] FALSE     FALSE       FALSE        FALSE
##  [262,] FALSE     FALSE       FALSE        FALSE
##  [263,] FALSE     FALSE       FALSE        FALSE
##  [264,] FALSE     FALSE       FALSE        FALSE
##  [265,] FALSE     FALSE       FALSE        FALSE
##  [266,] FALSE     FALSE       FALSE        FALSE
##  [267,] FALSE     FALSE       FALSE        FALSE
##  [268,] FALSE     FALSE       FALSE        FALSE
##  [269,] FALSE     FALSE       FALSE        FALSE
##  [270,] FALSE     FALSE       FALSE        FALSE
##  [271,] FALSE     FALSE       FALSE        FALSE
##  [272,] FALSE     FALSE       FALSE        FALSE
##  [273,] FALSE     FALSE       FALSE        FALSE
##  [274,] FALSE     FALSE       FALSE        FALSE
##  [275,] FALSE     FALSE       FALSE        FALSE
##  [276,] FALSE     FALSE       FALSE        FALSE
##  [277,] FALSE     FALSE       FALSE        FALSE
##  [278,] FALSE     FALSE       FALSE        FALSE
##  [279,] FALSE     FALSE       FALSE        FALSE
##  [280,] FALSE     FALSE       FALSE        FALSE
##  [281,] FALSE     FALSE       FALSE        FALSE
##  [282,] FALSE     FALSE       FALSE        FALSE
##  [283,] FALSE     FALSE       FALSE        FALSE
##  [284,] FALSE     FALSE       FALSE        FALSE
##  [285,] FALSE     FALSE       FALSE        FALSE
##  [286,] FALSE     FALSE       FALSE        FALSE
##  [287,] FALSE     FALSE       FALSE        FALSE
##  [288,] FALSE     FALSE       FALSE        FALSE
##  [289,] FALSE     FALSE       FALSE        FALSE
##  [290,] FALSE     FALSE       FALSE        FALSE
##  [291,] FALSE     FALSE       FALSE        FALSE
##  [292,] FALSE     FALSE       FALSE        FALSE
##  [293,] FALSE     FALSE       FALSE        FALSE
##  [294,] FALSE     FALSE       FALSE        FALSE
##  [295,] FALSE     FALSE       FALSE        FALSE
##  [296,] FALSE     FALSE       FALSE        FALSE
##  [297,] FALSE     FALSE       FALSE        FALSE
##  [298,] FALSE     FALSE       FALSE        FALSE
##  [299,] FALSE     FALSE       FALSE        FALSE
##  [300,] FALSE     FALSE       FALSE        FALSE
##  [301,] FALSE     FALSE       FALSE        FALSE
##  [302,] FALSE     FALSE       FALSE        FALSE
##  [303,] FALSE     FALSE       FALSE        FALSE
##  [304,] FALSE     FALSE       FALSE        FALSE
##  [305,] FALSE     FALSE       FALSE        FALSE
##  [306,] FALSE     FALSE       FALSE        FALSE
##  [307,] FALSE     FALSE       FALSE        FALSE
##  [308,] FALSE     FALSE       FALSE        FALSE
##  [309,] FALSE     FALSE       FALSE        FALSE
##  [310,] FALSE     FALSE       FALSE        FALSE
##  [311,] FALSE     FALSE       FALSE        FALSE
##  [312,] FALSE     FALSE       FALSE        FALSE
##  [313,] FALSE     FALSE       FALSE        FALSE
##  [314,] FALSE     FALSE       FALSE        FALSE
##  [315,] FALSE     FALSE       FALSE        FALSE
##  [316,] FALSE     FALSE       FALSE        FALSE
##  [317,] FALSE     FALSE       FALSE        FALSE
##  [318,] FALSE     FALSE       FALSE        FALSE
##  [319,] FALSE     FALSE       FALSE        FALSE
##  [320,] FALSE     FALSE       FALSE        FALSE
##  [321,] FALSE     FALSE       FALSE        FALSE
##  [322,] FALSE     FALSE       FALSE        FALSE
##  [323,] FALSE     FALSE       FALSE        FALSE
##  [324,] FALSE     FALSE       FALSE        FALSE
##  [325,] FALSE     FALSE       FALSE        FALSE
##  [326,] FALSE     FALSE       FALSE        FALSE
##  [327,] FALSE     FALSE       FALSE        FALSE
##  [328,] FALSE     FALSE       FALSE        FALSE
##  [329,] FALSE     FALSE       FALSE        FALSE
##  [330,] FALSE     FALSE       FALSE        FALSE
##  [331,] FALSE     FALSE       FALSE        FALSE
##  [332,] FALSE     FALSE       FALSE        FALSE
##  [333,] FALSE     FALSE       FALSE        FALSE
##  [334,] FALSE     FALSE       FALSE        FALSE
##  [335,] FALSE     FALSE       FALSE        FALSE
##  [336,] FALSE     FALSE       FALSE        FALSE
##  [337,] FALSE     FALSE       FALSE        FALSE
##  [338,] FALSE     FALSE       FALSE        FALSE
##  [339,] FALSE     FALSE       FALSE        FALSE
##  [340,] FALSE     FALSE       FALSE        FALSE
##  [341,] FALSE     FALSE       FALSE        FALSE
##  [342,] FALSE     FALSE       FALSE        FALSE
##  [343,] FALSE     FALSE       FALSE        FALSE
##  [344,] FALSE     FALSE       FALSE        FALSE
##  [345,] FALSE     FALSE       FALSE        FALSE
##  [346,] FALSE     FALSE       FALSE        FALSE
##  [347,] FALSE     FALSE       FALSE        FALSE
##  [348,] FALSE     FALSE       FALSE        FALSE
##  [349,] FALSE     FALSE       FALSE        FALSE
##  [350,] FALSE     FALSE       FALSE        FALSE
##  [351,] FALSE     FALSE       FALSE        FALSE
##  [352,] FALSE     FALSE       FALSE        FALSE
##  [353,] FALSE     FALSE       FALSE        FALSE
##  [354,] FALSE     FALSE       FALSE        FALSE
##  [355,] FALSE     FALSE       FALSE        FALSE
##  [356,] FALSE     FALSE       FALSE        FALSE
##  [357,] FALSE     FALSE       FALSE        FALSE
##  [358,] FALSE     FALSE       FALSE        FALSE
##  [359,] FALSE     FALSE       FALSE        FALSE
##  [360,] FALSE     FALSE       FALSE        FALSE
##  [361,] FALSE     FALSE       FALSE        FALSE
##  [362,] FALSE     FALSE       FALSE        FALSE
##  [363,] FALSE     FALSE       FALSE        FALSE
##  [364,] FALSE     FALSE       FALSE        FALSE
##  [365,] FALSE     FALSE       FALSE        FALSE
##  [366,] FALSE     FALSE       FALSE        FALSE
##  [367,] FALSE     FALSE       FALSE        FALSE
##  [368,] FALSE     FALSE       FALSE        FALSE
##  [369,] FALSE     FALSE       FALSE        FALSE
##  [370,] FALSE     FALSE       FALSE        FALSE
##  [371,] FALSE     FALSE       FALSE        FALSE
##  [372,] FALSE     FALSE       FALSE        FALSE
##  [373,] FALSE     FALSE       FALSE        FALSE
##  [374,] FALSE     FALSE       FALSE        FALSE
##  [375,] FALSE     FALSE       FALSE        FALSE
##  [376,] FALSE     FALSE       FALSE        FALSE
##  [377,] FALSE     FALSE       FALSE        FALSE
##  [378,] FALSE     FALSE       FALSE        FALSE
##  [379,] FALSE     FALSE       FALSE        FALSE
##  [380,] FALSE     FALSE       FALSE        FALSE
##  [381,] FALSE     FALSE       FALSE        FALSE
##  [382,] FALSE     FALSE       FALSE        FALSE
##  [383,] FALSE     FALSE       FALSE        FALSE
##  [384,] FALSE     FALSE       FALSE        FALSE
##  [385,] FALSE     FALSE       FALSE        FALSE
##  [386,] FALSE     FALSE       FALSE        FALSE
##  [387,] FALSE     FALSE       FALSE        FALSE
##  [388,] FALSE     FALSE       FALSE        FALSE
##  [389,] FALSE     FALSE       FALSE        FALSE
##  [390,] FALSE     FALSE       FALSE        FALSE
##  [391,] FALSE     FALSE       FALSE        FALSE
##  [392,] FALSE     FALSE       FALSE        FALSE
##  [393,] FALSE     FALSE       FALSE        FALSE
##  [394,] FALSE     FALSE       FALSE        FALSE
##  [395,] FALSE     FALSE       FALSE        FALSE
##  [396,] FALSE     FALSE       FALSE        FALSE
##  [397,] FALSE     FALSE       FALSE        FALSE
##  [398,] FALSE     FALSE       FALSE        FALSE
##  [399,] FALSE     FALSE       FALSE        FALSE
##  [400,] FALSE     FALSE       FALSE        FALSE
##  [401,] FALSE     FALSE       FALSE        FALSE
##  [402,] FALSE     FALSE       FALSE        FALSE
##  [403,] FALSE     FALSE       FALSE        FALSE
##  [404,] FALSE     FALSE       FALSE        FALSE
##  [405,] FALSE     FALSE       FALSE        FALSE
##  [406,] FALSE     FALSE       FALSE        FALSE
##  [407,] FALSE     FALSE       FALSE        FALSE
##  [408,] FALSE     FALSE       FALSE        FALSE
##  [409,] FALSE     FALSE       FALSE        FALSE
##  [410,] FALSE     FALSE       FALSE        FALSE
##  [411,] FALSE     FALSE       FALSE        FALSE
##  [412,] FALSE     FALSE       FALSE        FALSE
##  [413,] FALSE     FALSE       FALSE        FALSE
##  [414,] FALSE     FALSE       FALSE        FALSE
##  [415,] FALSE     FALSE       FALSE        FALSE
##  [416,] FALSE     FALSE       FALSE        FALSE
##  [417,] FALSE     FALSE       FALSE        FALSE
##  [418,] FALSE     FALSE       FALSE        FALSE
##  [419,] FALSE     FALSE       FALSE        FALSE
##  [420,] FALSE     FALSE       FALSE        FALSE
##  [421,] FALSE     FALSE       FALSE        FALSE
##  [422,] FALSE     FALSE       FALSE        FALSE
##  [423,] FALSE     FALSE       FALSE        FALSE
##  [424,] FALSE     FALSE       FALSE        FALSE
##  [425,] FALSE     FALSE       FALSE        FALSE
##  [426,] FALSE     FALSE       FALSE        FALSE
##  [427,] FALSE     FALSE       FALSE        FALSE
##  [428,] FALSE     FALSE       FALSE        FALSE
##  [429,] FALSE     FALSE       FALSE        FALSE
##  [430,] FALSE     FALSE       FALSE        FALSE
##  [431,] FALSE     FALSE       FALSE        FALSE
##  [432,] FALSE     FALSE       FALSE        FALSE
##  [433,] FALSE     FALSE       FALSE        FALSE
##  [434,] FALSE     FALSE       FALSE        FALSE
##  [435,] FALSE     FALSE       FALSE        FALSE
##  [436,] FALSE     FALSE       FALSE        FALSE
##  [437,] FALSE     FALSE       FALSE        FALSE
##  [438,] FALSE     FALSE       FALSE        FALSE
##  [439,] FALSE     FALSE       FALSE        FALSE
##  [440,] FALSE     FALSE       FALSE        FALSE
##  [441,] FALSE     FALSE       FALSE        FALSE
##  [442,] FALSE     FALSE       FALSE        FALSE
##  [443,] FALSE     FALSE       FALSE        FALSE
##  [444,] FALSE     FALSE       FALSE        FALSE
##  [445,] FALSE     FALSE       FALSE        FALSE
##  [446,] FALSE     FALSE       FALSE        FALSE
##  [447,] FALSE     FALSE       FALSE        FALSE
##  [448,] FALSE     FALSE       FALSE        FALSE
##  [449,] FALSE     FALSE       FALSE        FALSE
##  [450,] FALSE     FALSE       FALSE        FALSE
##  [451,] FALSE     FALSE       FALSE        FALSE
##  [452,] FALSE     FALSE       FALSE        FALSE
##  [453,] FALSE     FALSE       FALSE        FALSE
##  [454,] FALSE     FALSE       FALSE        FALSE
##  [455,] FALSE     FALSE       FALSE        FALSE
##  [456,] FALSE     FALSE       FALSE        FALSE
##  [457,] FALSE     FALSE       FALSE        FALSE
##  [458,] FALSE     FALSE       FALSE        FALSE
##  [459,] FALSE     FALSE       FALSE        FALSE
##  [460,] FALSE     FALSE       FALSE        FALSE
##  [461,] FALSE     FALSE       FALSE        FALSE
##  [462,] FALSE     FALSE       FALSE        FALSE
##  [463,] FALSE     FALSE       FALSE        FALSE
##  [464,] FALSE     FALSE       FALSE        FALSE
##  [465,] FALSE     FALSE       FALSE        FALSE
##  [466,] FALSE     FALSE       FALSE        FALSE
##  [467,] FALSE     FALSE       FALSE        FALSE
##  [468,] FALSE     FALSE       FALSE        FALSE
##  [469,] FALSE     FALSE       FALSE        FALSE
##  [470,] FALSE     FALSE       FALSE        FALSE
##  [471,] FALSE     FALSE       FALSE        FALSE
##  [472,] FALSE     FALSE       FALSE        FALSE
##  [473,] FALSE     FALSE       FALSE        FALSE
##  [474,] FALSE     FALSE       FALSE        FALSE
##  [475,] FALSE     FALSE       FALSE        FALSE
##  [476,] FALSE     FALSE       FALSE        FALSE
##  [477,] FALSE     FALSE       FALSE        FALSE
##  [478,] FALSE     FALSE       FALSE        FALSE
##  [479,] FALSE     FALSE       FALSE        FALSE
##  [480,] FALSE     FALSE       FALSE        FALSE
##  [481,] FALSE     FALSE       FALSE        FALSE
##  [482,] FALSE     FALSE       FALSE        FALSE
##  [483,] FALSE     FALSE       FALSE        FALSE
##  [484,] FALSE     FALSE       FALSE        FALSE
##  [485,] FALSE     FALSE       FALSE        FALSE
##  [486,] FALSE     FALSE       FALSE        FALSE
##  [487,] FALSE     FALSE       FALSE        FALSE
##  [488,] FALSE     FALSE       FALSE        FALSE
##  [489,] FALSE     FALSE       FALSE        FALSE
##  [490,] FALSE     FALSE       FALSE        FALSE
##  [491,] FALSE     FALSE       FALSE        FALSE
##  [492,] FALSE     FALSE       FALSE        FALSE
##  [493,] FALSE     FALSE       FALSE        FALSE
##  [494,] FALSE     FALSE       FALSE        FALSE
##  [495,] FALSE     FALSE       FALSE        FALSE
##  [496,] FALSE     FALSE       FALSE        FALSE
##  [497,] FALSE     FALSE       FALSE        FALSE
##  [498,] FALSE     FALSE       FALSE        FALSE
##  [499,] FALSE     FALSE       FALSE        FALSE
##  [500,] FALSE     FALSE       FALSE        FALSE
##  [501,] FALSE     FALSE       FALSE        FALSE
##  [502,] FALSE     FALSE       FALSE        FALSE
##  [503,] FALSE     FALSE       FALSE        FALSE
##  [504,] FALSE     FALSE       FALSE        FALSE
##  [505,] FALSE     FALSE       FALSE        FALSE
##  [506,] FALSE     FALSE       FALSE        FALSE
##  [507,] FALSE     FALSE       FALSE        FALSE
##  [508,] FALSE     FALSE       FALSE        FALSE
##  [509,] FALSE     FALSE       FALSE        FALSE
##  [510,] FALSE     FALSE       FALSE        FALSE
##  [511,] FALSE     FALSE       FALSE        FALSE
##  [512,] FALSE     FALSE       FALSE        FALSE
##  [513,] FALSE     FALSE       FALSE        FALSE
##  [514,] FALSE     FALSE       FALSE        FALSE
##  [515,] FALSE     FALSE       FALSE        FALSE
##  [516,] FALSE     FALSE       FALSE        FALSE
##  [517,] FALSE     FALSE       FALSE        FALSE
##  [518,] FALSE     FALSE       FALSE        FALSE
##  [519,] FALSE     FALSE       FALSE        FALSE
##  [520,] FALSE     FALSE       FALSE        FALSE
##  [521,] FALSE     FALSE       FALSE        FALSE
##  [522,] FALSE     FALSE       FALSE        FALSE
##  [523,] FALSE     FALSE       FALSE        FALSE
##  [524,] FALSE     FALSE       FALSE        FALSE
##  [525,] FALSE     FALSE       FALSE        FALSE
##  [526,] FALSE     FALSE       FALSE        FALSE
##  [527,] FALSE     FALSE       FALSE        FALSE
##  [528,] FALSE     FALSE       FALSE        FALSE
##  [529,] FALSE     FALSE       FALSE        FALSE
##  [530,] FALSE     FALSE       FALSE        FALSE
##  [531,] FALSE     FALSE       FALSE        FALSE
##  [532,] FALSE     FALSE       FALSE        FALSE
##  [533,] FALSE     FALSE       FALSE        FALSE
##  [534,] FALSE     FALSE       FALSE        FALSE
##  [535,] FALSE     FALSE       FALSE        FALSE
##  [536,] FALSE     FALSE       FALSE        FALSE
##  [537,] FALSE     FALSE       FALSE        FALSE
##  [538,] FALSE     FALSE       FALSE        FALSE
##  [539,] FALSE     FALSE       FALSE        FALSE
##  [540,] FALSE     FALSE       FALSE        FALSE
##  [541,] FALSE     FALSE       FALSE        FALSE
##  [542,] FALSE     FALSE       FALSE        FALSE
##  [543,] FALSE     FALSE       FALSE        FALSE
##  [544,] FALSE     FALSE       FALSE        FALSE
##  [545,] FALSE     FALSE       FALSE        FALSE
##  [546,] FALSE     FALSE       FALSE        FALSE
##  [547,] FALSE     FALSE       FALSE        FALSE
##  [548,] FALSE     FALSE       FALSE        FALSE
##  [549,] FALSE     FALSE       FALSE        FALSE
##  [550,] FALSE     FALSE       FALSE        FALSE
##  [551,] FALSE     FALSE       FALSE        FALSE
##  [552,] FALSE     FALSE       FALSE        FALSE
##  [553,] FALSE     FALSE       FALSE        FALSE
##  [554,] FALSE     FALSE       FALSE        FALSE
##  [555,] FALSE     FALSE       FALSE        FALSE
##  [556,] FALSE     FALSE       FALSE        FALSE
##  [557,] FALSE     FALSE       FALSE        FALSE
##  [558,] FALSE     FALSE       FALSE        FALSE
##  [559,] FALSE     FALSE       FALSE        FALSE
##  [560,] FALSE     FALSE       FALSE        FALSE
##  [561,] FALSE     FALSE       FALSE        FALSE
##  [562,] FALSE     FALSE       FALSE        FALSE
##  [563,] FALSE     FALSE       FALSE        FALSE
##  [564,] FALSE     FALSE       FALSE        FALSE
##  [565,] FALSE     FALSE       FALSE        FALSE
##  [566,] FALSE     FALSE       FALSE        FALSE
##  [567,] FALSE     FALSE       FALSE        FALSE
##  [568,] FALSE     FALSE       FALSE        FALSE
##  [569,] FALSE     FALSE       FALSE        FALSE
##  [570,] FALSE     FALSE       FALSE        FALSE
##  [571,] FALSE     FALSE       FALSE        FALSE
##  [572,] FALSE     FALSE       FALSE        FALSE
##  [573,] FALSE     FALSE       FALSE        FALSE
##  [574,] FALSE     FALSE       FALSE        FALSE
##  [575,] FALSE     FALSE       FALSE        FALSE
##  [576,] FALSE     FALSE       FALSE        FALSE
##  [577,] FALSE     FALSE       FALSE        FALSE
##  [578,] FALSE     FALSE       FALSE        FALSE
##  [579,] FALSE     FALSE       FALSE        FALSE
##  [580,] FALSE     FALSE       FALSE        FALSE
##  [581,] FALSE     FALSE       FALSE        FALSE
##  [582,] FALSE     FALSE       FALSE        FALSE
##  [583,] FALSE     FALSE       FALSE        FALSE
##  [584,] FALSE     FALSE       FALSE        FALSE
##  [585,] FALSE     FALSE       FALSE        FALSE
##  [586,] FALSE     FALSE       FALSE        FALSE
##  [587,] FALSE     FALSE       FALSE        FALSE
##  [588,] FALSE     FALSE       FALSE        FALSE
##  [589,] FALSE     FALSE       FALSE        FALSE
##  [590,] FALSE     FALSE       FALSE        FALSE
##  [591,] FALSE     FALSE       FALSE        FALSE
##  [592,] FALSE     FALSE       FALSE        FALSE
##  [593,] FALSE     FALSE       FALSE        FALSE
##  [594,] FALSE     FALSE       FALSE        FALSE
##  [595,] FALSE     FALSE       FALSE        FALSE
##  [596,] FALSE     FALSE       FALSE        FALSE
##  [597,] FALSE     FALSE       FALSE        FALSE
##  [598,] FALSE     FALSE       FALSE        FALSE
##  [599,] FALSE     FALSE       FALSE        FALSE
##  [600,] FALSE     FALSE       FALSE        FALSE
##  [601,] FALSE     FALSE       FALSE        FALSE
##  [602,] FALSE     FALSE       FALSE        FALSE
##  [603,] FALSE     FALSE       FALSE        FALSE
##  [604,] FALSE     FALSE       FALSE        FALSE
##  [605,] FALSE     FALSE       FALSE        FALSE
##  [606,] FALSE     FALSE       FALSE        FALSE
##  [607,] FALSE     FALSE       FALSE        FALSE
##  [608,] FALSE     FALSE       FALSE        FALSE
##  [609,] FALSE     FALSE       FALSE        FALSE
##  [610,] FALSE     FALSE       FALSE        FALSE
##  [611,] FALSE     FALSE       FALSE        FALSE
##  [612,] FALSE     FALSE       FALSE        FALSE
##  [613,] FALSE     FALSE       FALSE        FALSE
##  [614,] FALSE     FALSE       FALSE        FALSE
##  [615,] FALSE     FALSE       FALSE        FALSE
##  [616,] FALSE     FALSE       FALSE        FALSE
##  [617,] FALSE     FALSE       FALSE        FALSE
##  [618,] FALSE     FALSE       FALSE        FALSE
##  [619,] FALSE     FALSE       FALSE        FALSE
##  [620,] FALSE     FALSE       FALSE        FALSE
##  [621,] FALSE     FALSE       FALSE        FALSE
##  [622,] FALSE     FALSE       FALSE        FALSE
##  [623,] FALSE     FALSE       FALSE        FALSE
##  [624,] FALSE     FALSE       FALSE        FALSE
##  [625,] FALSE     FALSE       FALSE        FALSE
##  [626,] FALSE     FALSE       FALSE        FALSE
##  [627,] FALSE     FALSE       FALSE        FALSE
##  [628,] FALSE     FALSE       FALSE        FALSE
##  [629,] FALSE     FALSE       FALSE        FALSE
##  [630,] FALSE     FALSE       FALSE        FALSE
##  [631,] FALSE     FALSE       FALSE        FALSE
##  [632,] FALSE     FALSE       FALSE        FALSE
##  [633,] FALSE     FALSE       FALSE        FALSE
##  [634,] FALSE     FALSE       FALSE        FALSE
##  [635,] FALSE     FALSE       FALSE        FALSE
##  [636,] FALSE     FALSE       FALSE        FALSE
##  [637,] FALSE     FALSE       FALSE        FALSE
##  [638,] FALSE     FALSE       FALSE        FALSE
##  [639,] FALSE     FALSE       FALSE        FALSE
##  [640,] FALSE     FALSE       FALSE        FALSE
##  [641,] FALSE     FALSE       FALSE        FALSE
##  [642,] FALSE     FALSE       FALSE        FALSE
##  [643,] FALSE     FALSE       FALSE        FALSE
##  [644,] FALSE     FALSE       FALSE        FALSE
##  [645,] FALSE     FALSE       FALSE        FALSE
##  [646,] FALSE     FALSE       FALSE        FALSE
##  [647,] FALSE     FALSE       FALSE        FALSE
##  [648,] FALSE     FALSE       FALSE        FALSE
##  [649,] FALSE     FALSE       FALSE        FALSE
##  [650,] FALSE     FALSE       FALSE        FALSE
##  [651,] FALSE     FALSE       FALSE        FALSE
##  [652,] FALSE     FALSE       FALSE        FALSE
##  [653,] FALSE     FALSE       FALSE        FALSE
##  [654,] FALSE     FALSE       FALSE        FALSE
##  [655,] FALSE     FALSE       FALSE        FALSE
##  [656,] FALSE     FALSE       FALSE        FALSE
##  [657,] FALSE     FALSE       FALSE        FALSE
##  [658,] FALSE     FALSE       FALSE        FALSE
##  [659,] FALSE     FALSE       FALSE        FALSE
##  [660,] FALSE     FALSE       FALSE        FALSE
##  [661,] FALSE     FALSE       FALSE        FALSE
##  [662,] FALSE     FALSE       FALSE        FALSE
##  [663,] FALSE     FALSE       FALSE        FALSE
##  [664,] FALSE     FALSE       FALSE        FALSE
##  [665,] FALSE     FALSE       FALSE        FALSE
##  [666,] FALSE     FALSE       FALSE        FALSE
##  [667,] FALSE     FALSE       FALSE        FALSE
##  [668,] FALSE     FALSE       FALSE        FALSE
##  [669,] FALSE     FALSE       FALSE        FALSE
##  [670,] FALSE     FALSE       FALSE        FALSE
##  [671,] FALSE     FALSE       FALSE        FALSE
##  [672,] FALSE     FALSE       FALSE        FALSE
##  [673,] FALSE     FALSE       FALSE        FALSE
##  [674,] FALSE     FALSE       FALSE        FALSE
##  [675,] FALSE     FALSE       FALSE        FALSE
##  [676,] FALSE     FALSE       FALSE        FALSE
##  [677,] FALSE     FALSE       FALSE        FALSE
##  [678,] FALSE     FALSE       FALSE        FALSE
##  [679,] FALSE     FALSE       FALSE        FALSE
##  [680,] FALSE     FALSE       FALSE        FALSE
##  [681,] FALSE     FALSE       FALSE        FALSE
##  [682,] FALSE     FALSE       FALSE        FALSE
##  [683,] FALSE     FALSE       FALSE        FALSE
##  [684,] FALSE     FALSE       FALSE        FALSE
##  [685,] FALSE     FALSE       FALSE        FALSE
##  [686,] FALSE     FALSE       FALSE        FALSE
##  [687,] FALSE     FALSE       FALSE        FALSE
##  [688,] FALSE     FALSE       FALSE        FALSE
##  [689,] FALSE     FALSE       FALSE        FALSE
##  [690,] FALSE     FALSE       FALSE        FALSE
##  [691,] FALSE     FALSE       FALSE        FALSE
##  [692,] FALSE     FALSE       FALSE        FALSE
##  [693,] FALSE     FALSE       FALSE        FALSE
##  [694,] FALSE     FALSE       FALSE        FALSE
##  [695,] FALSE     FALSE       FALSE        FALSE
##  [696,] FALSE     FALSE       FALSE        FALSE
##  [697,] FALSE     FALSE       FALSE        FALSE
##  [698,] FALSE     FALSE       FALSE        FALSE
##  [699,] FALSE     FALSE       FALSE        FALSE
##  [700,] FALSE     FALSE       FALSE        FALSE
##  [701,] FALSE     FALSE       FALSE        FALSE
##  [702,] FALSE     FALSE       FALSE        FALSE
##  [703,] FALSE     FALSE       FALSE        FALSE
##  [704,] FALSE     FALSE       FALSE        FALSE
##  [705,] FALSE     FALSE       FALSE        FALSE
##  [706,] FALSE     FALSE       FALSE        FALSE
##  [707,] FALSE     FALSE       FALSE        FALSE
##  [708,] FALSE     FALSE       FALSE        FALSE
##  [709,] FALSE     FALSE       FALSE        FALSE
##  [710,] FALSE     FALSE       FALSE        FALSE
##  [711,] FALSE     FALSE       FALSE        FALSE
##  [712,] FALSE     FALSE       FALSE        FALSE
##  [713,] FALSE     FALSE       FALSE        FALSE
##  [714,] FALSE     FALSE       FALSE        FALSE
##  [715,] FALSE     FALSE       FALSE        FALSE
##  [716,] FALSE     FALSE       FALSE        FALSE
##  [717,] FALSE     FALSE       FALSE        FALSE
##  [718,] FALSE     FALSE       FALSE        FALSE
##  [719,] FALSE     FALSE       FALSE        FALSE
##  [720,] FALSE     FALSE       FALSE        FALSE
##  [721,] FALSE     FALSE       FALSE        FALSE
##  [722,] FALSE     FALSE       FALSE        FALSE
##  [723,] FALSE     FALSE       FALSE        FALSE
##  [724,] FALSE     FALSE       FALSE        FALSE
##  [725,] FALSE     FALSE       FALSE        FALSE
##  [726,] FALSE     FALSE       FALSE        FALSE
##  [727,] FALSE     FALSE       FALSE        FALSE
##  [728,] FALSE     FALSE       FALSE        FALSE
##  [729,] FALSE     FALSE       FALSE        FALSE
##  [730,] FALSE     FALSE       FALSE        FALSE
##  [731,] FALSE     FALSE       FALSE        FALSE
##  [732,] FALSE     FALSE       FALSE        FALSE
##  [733,] FALSE     FALSE       FALSE        FALSE
##  [734,] FALSE     FALSE       FALSE        FALSE
##  [735,] FALSE     FALSE       FALSE        FALSE
##  [736,] FALSE     FALSE       FALSE        FALSE
##  [737,] FALSE     FALSE       FALSE        FALSE
##  [738,] FALSE     FALSE       FALSE        FALSE
##  [739,] FALSE     FALSE       FALSE        FALSE
##  [740,] FALSE     FALSE       FALSE        FALSE
##  [741,] FALSE     FALSE       FALSE        FALSE
##  [742,] FALSE     FALSE       FALSE        FALSE
##  [743,] FALSE     FALSE       FALSE        FALSE
##  [744,] FALSE     FALSE       FALSE        FALSE
##  [745,] FALSE     FALSE       FALSE        FALSE
##  [746,] FALSE     FALSE       FALSE        FALSE
##  [747,] FALSE     FALSE       FALSE        FALSE
##  [748,] FALSE     FALSE       FALSE        FALSE
##  [749,] FALSE     FALSE       FALSE        FALSE
##  [750,] FALSE     FALSE       FALSE        FALSE
##  [751,] FALSE     FALSE       FALSE        FALSE
##  [752,] FALSE     FALSE       FALSE        FALSE
##  [753,] FALSE     FALSE       FALSE        FALSE
##  [754,] FALSE     FALSE       FALSE        FALSE
##  [755,] FALSE     FALSE       FALSE        FALSE
##  [756,] FALSE     FALSE       FALSE        FALSE
##  [757,] FALSE     FALSE       FALSE        FALSE
##  [758,] FALSE     FALSE       FALSE        FALSE
##  [759,] FALSE     FALSE       FALSE        FALSE
##  [760,] FALSE     FALSE       FALSE        FALSE
##  [761,] FALSE     FALSE       FALSE        FALSE
##  [762,] FALSE     FALSE       FALSE        FALSE
##  [763,] FALSE     FALSE       FALSE        FALSE
##  [764,] FALSE     FALSE       FALSE        FALSE
##  [765,] FALSE     FALSE       FALSE        FALSE
##  [766,] FALSE     FALSE       FALSE        FALSE
##  [767,] FALSE     FALSE       FALSE        FALSE
##  [768,] FALSE     FALSE       FALSE        FALSE
##  [769,] FALSE     FALSE       FALSE        FALSE
##  [770,] FALSE     FALSE       FALSE        FALSE
##  [771,] FALSE     FALSE       FALSE        FALSE
##  [772,] FALSE     FALSE       FALSE        FALSE
##  [773,] FALSE     FALSE       FALSE        FALSE
##  [774,] FALSE     FALSE       FALSE        FALSE
##  [775,] FALSE     FALSE       FALSE        FALSE
##  [776,] FALSE     FALSE       FALSE        FALSE
##  [777,] FALSE     FALSE       FALSE        FALSE
##  [778,] FALSE     FALSE       FALSE        FALSE
##  [779,] FALSE     FALSE       FALSE        FALSE
##  [780,] FALSE     FALSE       FALSE        FALSE
##  [781,] FALSE     FALSE       FALSE        FALSE
##  [782,] FALSE     FALSE       FALSE        FALSE
##  [783,] FALSE     FALSE       FALSE        FALSE
##  [784,] FALSE     FALSE       FALSE        FALSE
##  [785,] FALSE     FALSE       FALSE        FALSE
##  [786,] FALSE     FALSE       FALSE        FALSE
##  [787,] FALSE     FALSE       FALSE        FALSE
##  [788,] FALSE     FALSE       FALSE        FALSE
##  [789,] FALSE     FALSE       FALSE        FALSE
##  [790,] FALSE     FALSE       FALSE        FALSE
##  [791,] FALSE     FALSE       FALSE        FALSE
##  [792,] FALSE     FALSE       FALSE        FALSE
##  [793,] FALSE     FALSE       FALSE        FALSE
##  [794,] FALSE     FALSE       FALSE        FALSE
##  [795,] FALSE     FALSE       FALSE        FALSE
##  [796,] FALSE     FALSE       FALSE        FALSE
##  [797,] FALSE     FALSE       FALSE        FALSE
##  [798,] FALSE     FALSE       FALSE        FALSE
##  [799,] FALSE     FALSE       FALSE        FALSE
##  [800,] FALSE     FALSE       FALSE        FALSE
##  [801,] FALSE     FALSE       FALSE        FALSE
##  [802,] FALSE     FALSE       FALSE        FALSE
##  [803,] FALSE     FALSE       FALSE        FALSE
##  [804,] FALSE     FALSE       FALSE        FALSE
##  [805,] FALSE     FALSE       FALSE        FALSE
##  [806,] FALSE     FALSE       FALSE        FALSE
##  [807,] FALSE     FALSE       FALSE        FALSE
##  [808,] FALSE     FALSE       FALSE        FALSE
##  [809,] FALSE     FALSE       FALSE        FALSE
##  [810,] FALSE     FALSE       FALSE        FALSE
##  [811,] FALSE     FALSE       FALSE        FALSE
##  [812,] FALSE     FALSE       FALSE        FALSE
##  [813,] FALSE     FALSE       FALSE        FALSE
##  [814,] FALSE     FALSE       FALSE        FALSE
##  [815,] FALSE     FALSE       FALSE        FALSE
##  [816,] FALSE     FALSE       FALSE        FALSE
##  [817,] FALSE     FALSE       FALSE        FALSE
##  [818,] FALSE     FALSE       FALSE        FALSE
##  [819,] FALSE     FALSE       FALSE        FALSE
##  [820,] FALSE     FALSE       FALSE        FALSE
##  [821,] FALSE     FALSE       FALSE        FALSE
##  [822,] FALSE     FALSE       FALSE        FALSE
##  [823,] FALSE     FALSE       FALSE        FALSE
##  [824,] FALSE     FALSE       FALSE        FALSE
##  [825,] FALSE     FALSE       FALSE        FALSE
##  [826,] FALSE     FALSE       FALSE        FALSE
##  [827,] FALSE     FALSE       FALSE        FALSE
##  [828,] FALSE     FALSE       FALSE        FALSE
##  [829,] FALSE     FALSE       FALSE        FALSE
##  [830,] FALSE     FALSE       FALSE        FALSE
##  [831,] FALSE     FALSE       FALSE        FALSE
##  [832,] FALSE     FALSE       FALSE        FALSE
##  [833,] FALSE     FALSE       FALSE        FALSE
##  [834,] FALSE     FALSE       FALSE        FALSE
##  [835,] FALSE     FALSE       FALSE        FALSE
##  [836,] FALSE     FALSE       FALSE        FALSE
##  [837,] FALSE     FALSE       FALSE        FALSE
##  [838,] FALSE     FALSE       FALSE        FALSE
##  [839,] FALSE     FALSE       FALSE        FALSE
##  [840,] FALSE     FALSE       FALSE        FALSE
##  [841,] FALSE     FALSE       FALSE        FALSE
##  [842,] FALSE     FALSE       FALSE        FALSE
##  [843,] FALSE     FALSE       FALSE        FALSE
##  [844,] FALSE     FALSE       FALSE        FALSE
##  [845,] FALSE     FALSE       FALSE        FALSE
##  [846,] FALSE     FALSE       FALSE        FALSE
##  [847,] FALSE     FALSE       FALSE        FALSE
##  [848,] FALSE     FALSE       FALSE        FALSE
##  [849,] FALSE     FALSE       FALSE        FALSE
##  [850,] FALSE     FALSE       FALSE        FALSE
##  [851,] FALSE     FALSE       FALSE        FALSE
##  [852,] FALSE     FALSE       FALSE        FALSE
##  [853,] FALSE     FALSE       FALSE        FALSE
##  [854,] FALSE     FALSE       FALSE        FALSE
##  [855,] FALSE     FALSE       FALSE        FALSE
##  [856,] FALSE     FALSE       FALSE        FALSE
##  [857,] FALSE     FALSE       FALSE        FALSE
##  [858,] FALSE     FALSE       FALSE        FALSE
##  [859,] FALSE     FALSE       FALSE        FALSE
##  [860,] FALSE     FALSE       FALSE        FALSE
##  [861,] FALSE     FALSE       FALSE        FALSE
##  [862,] FALSE     FALSE       FALSE        FALSE
##  [863,] FALSE     FALSE       FALSE        FALSE
##  [864,] FALSE     FALSE       FALSE        FALSE
##  [865,] FALSE     FALSE       FALSE        FALSE
##  [866,] FALSE     FALSE       FALSE        FALSE
##  [867,] FALSE     FALSE       FALSE        FALSE
##  [868,] FALSE     FALSE       FALSE        FALSE
##  [869,] FALSE     FALSE       FALSE        FALSE
##  [870,] FALSE     FALSE       FALSE        FALSE
##  [871,] FALSE     FALSE       FALSE        FALSE
##  [872,] FALSE     FALSE       FALSE        FALSE
##  [873,] FALSE     FALSE       FALSE        FALSE
##  [874,] FALSE     FALSE       FALSE        FALSE
##  [875,] FALSE     FALSE       FALSE        FALSE
##  [876,] FALSE     FALSE       FALSE        FALSE
##  [877,] FALSE     FALSE       FALSE        FALSE
##  [878,] FALSE     FALSE       FALSE        FALSE
##  [879,] FALSE     FALSE       FALSE        FALSE
##  [880,] FALSE     FALSE       FALSE        FALSE
##  [881,] FALSE     FALSE       FALSE        FALSE
##  [882,] FALSE     FALSE       FALSE        FALSE
##  [883,] FALSE     FALSE       FALSE        FALSE
##  [884,] FALSE     FALSE       FALSE        FALSE
##  [885,] FALSE     FALSE       FALSE        FALSE
##  [886,] FALSE     FALSE       FALSE        FALSE
##  [887,] FALSE     FALSE       FALSE        FALSE
##  [888,] FALSE     FALSE       FALSE        FALSE
##  [889,] FALSE     FALSE       FALSE        FALSE
##  [890,] FALSE     FALSE       FALSE        FALSE
##  [891,] FALSE     FALSE       FALSE        FALSE
##  [892,] FALSE     FALSE       FALSE        FALSE
##  [893,] FALSE     FALSE       FALSE        FALSE
##  [894,] FALSE     FALSE       FALSE        FALSE
##  [895,] FALSE     FALSE       FALSE        FALSE
##  [896,] FALSE     FALSE       FALSE        FALSE
##  [897,] FALSE     FALSE       FALSE        FALSE
##  [898,] FALSE     FALSE       FALSE        FALSE
##  [899,] FALSE     FALSE       FALSE        FALSE
##  [900,] FALSE     FALSE       FALSE        FALSE
##  [901,] FALSE     FALSE       FALSE        FALSE
##  [902,] FALSE     FALSE       FALSE        FALSE
##  [903,] FALSE     FALSE       FALSE        FALSE
##  [904,] FALSE     FALSE       FALSE        FALSE
##  [905,] FALSE     FALSE       FALSE        FALSE
##  [906,] FALSE     FALSE       FALSE        FALSE
##  [907,] FALSE     FALSE       FALSE        FALSE
##  [908,] FALSE     FALSE       FALSE        FALSE
##  [909,] FALSE     FALSE       FALSE        FALSE
##  [910,] FALSE     FALSE       FALSE        FALSE
##  [911,] FALSE     FALSE       FALSE        FALSE
##  [912,] FALSE     FALSE       FALSE        FALSE
##  [913,] FALSE     FALSE       FALSE        FALSE
##  [914,] FALSE     FALSE       FALSE        FALSE
##  [915,] FALSE     FALSE       FALSE        FALSE
##  [916,] FALSE     FALSE       FALSE        FALSE
##  [917,] FALSE     FALSE       FALSE        FALSE
##  [918,] FALSE     FALSE       FALSE        FALSE
##  [919,] FALSE     FALSE       FALSE        FALSE
##  [920,] FALSE     FALSE       FALSE        FALSE
##  [921,] FALSE     FALSE       FALSE        FALSE
##  [922,] FALSE     FALSE       FALSE        FALSE
##  [923,] FALSE     FALSE       FALSE        FALSE
##  [924,] FALSE     FALSE       FALSE        FALSE
##  [925,] FALSE     FALSE       FALSE        FALSE
##  [926,] FALSE     FALSE       FALSE        FALSE
##  [927,] FALSE     FALSE       FALSE        FALSE
##  [928,] FALSE     FALSE       FALSE        FALSE
##  [929,] FALSE     FALSE       FALSE        FALSE
##  [930,] FALSE     FALSE       FALSE        FALSE
##  [931,] FALSE     FALSE       FALSE        FALSE
##  [932,] FALSE     FALSE       FALSE        FALSE
##  [933,] FALSE     FALSE       FALSE        FALSE
##  [934,] FALSE     FALSE       FALSE        FALSE
##  [935,] FALSE     FALSE       FALSE        FALSE
##  [936,] FALSE     FALSE       FALSE        FALSE
##  [937,] FALSE     FALSE       FALSE        FALSE
##  [938,] FALSE     FALSE       FALSE        FALSE
##  [939,] FALSE     FALSE       FALSE        FALSE
##  [940,] FALSE     FALSE       FALSE        FALSE
##  [941,] FALSE     FALSE       FALSE        FALSE
##  [942,] FALSE     FALSE       FALSE        FALSE
##  [943,] FALSE     FALSE       FALSE        FALSE
##  [944,] FALSE     FALSE       FALSE        FALSE
##  [945,] FALSE     FALSE       FALSE        FALSE
##  [946,] FALSE     FALSE       FALSE        FALSE
##  [947,] FALSE     FALSE       FALSE        FALSE
##  [948,] FALSE     FALSE       FALSE        FALSE
##  [949,] FALSE     FALSE       FALSE        FALSE
##  [950,] FALSE     FALSE       FALSE        FALSE
##  [951,] FALSE     FALSE       FALSE        FALSE
##  [952,] FALSE     FALSE       FALSE        FALSE
##  [953,] FALSE     FALSE       FALSE        FALSE
##  [954,] FALSE     FALSE       FALSE        FALSE
##  [955,] FALSE     FALSE       FALSE        FALSE
##  [956,] FALSE     FALSE       FALSE        FALSE
##  [957,] FALSE     FALSE       FALSE        FALSE
##  [958,] FALSE     FALSE       FALSE        FALSE
##  [959,] FALSE     FALSE       FALSE        FALSE
##  [960,] FALSE     FALSE       FALSE        FALSE
##  [961,] FALSE     FALSE       FALSE        FALSE
##  [962,] FALSE     FALSE       FALSE        FALSE
##  [963,] FALSE     FALSE       FALSE        FALSE
##  [964,] FALSE     FALSE       FALSE        FALSE
##  [965,] FALSE     FALSE       FALSE        FALSE
##  [966,] FALSE     FALSE       FALSE        FALSE
##  [967,] FALSE     FALSE       FALSE        FALSE
##  [968,] FALSE     FALSE       FALSE        FALSE
##  [969,] FALSE     FALSE       FALSE        FALSE
##  [970,] FALSE     FALSE       FALSE        FALSE
##  [971,] FALSE     FALSE       FALSE        FALSE
##  [972,] FALSE     FALSE       FALSE        FALSE
##  [973,] FALSE     FALSE       FALSE        FALSE
##  [974,] FALSE     FALSE       FALSE        FALSE
##  [975,] FALSE     FALSE       FALSE        FALSE
##  [976,] FALSE     FALSE       FALSE        FALSE
##  [977,] FALSE     FALSE       FALSE        FALSE
##  [978,] FALSE     FALSE       FALSE        FALSE
##  [979,] FALSE     FALSE       FALSE        FALSE
##  [980,] FALSE     FALSE       FALSE        FALSE
##  [981,] FALSE     FALSE       FALSE        FALSE
##  [982,] FALSE     FALSE       FALSE        FALSE
##  [983,] FALSE     FALSE       FALSE        FALSE
##  [984,] FALSE     FALSE       FALSE        FALSE
##  [985,] FALSE     FALSE       FALSE        FALSE
##  [986,] FALSE     FALSE       FALSE        FALSE
##  [987,] FALSE     FALSE       FALSE        FALSE
##  [988,] FALSE     FALSE       FALSE        FALSE
##  [989,] FALSE     FALSE       FALSE        FALSE
##  [990,] FALSE     FALSE       FALSE        FALSE
##  [991,] FALSE     FALSE       FALSE        FALSE
##  [992,] FALSE     FALSE       FALSE        FALSE
##  [993,] FALSE     FALSE       FALSE        FALSE
##  [994,] FALSE     FALSE       FALSE        FALSE
##  [995,] FALSE     FALSE       FALSE        FALSE
##  [996,] FALSE     FALSE       FALSE        FALSE
##  [997,] FALSE     FALSE       FALSE        FALSE
##  [998,] FALSE     FALSE       FALSE        FALSE
##  [999,] FALSE     FALSE       FALSE        FALSE
## [1000,] FALSE     FALSE       FALSE        FALSE
## [1001,] FALSE     FALSE       FALSE        FALSE
## [1002,] FALSE     FALSE       FALSE        FALSE
## [1003,] FALSE     FALSE       FALSE        FALSE
## [1004,] FALSE     FALSE       FALSE        FALSE
## [1005,] FALSE     FALSE       FALSE        FALSE
## [1006,] FALSE     FALSE       FALSE        FALSE
## [1007,] FALSE     FALSE       FALSE        FALSE
## [1008,] FALSE     FALSE       FALSE        FALSE
## [1009,] FALSE     FALSE       FALSE        FALSE
## [1010,] FALSE     FALSE       FALSE        FALSE
## [1011,] FALSE     FALSE       FALSE        FALSE
## [1012,] FALSE     FALSE       FALSE        FALSE
## [1013,] FALSE     FALSE       FALSE        FALSE
## [1014,] FALSE     FALSE       FALSE        FALSE
## [1015,] FALSE     FALSE       FALSE        FALSE
## [1016,] FALSE     FALSE       FALSE        FALSE
## [1017,] FALSE     FALSE       FALSE        FALSE
## [1018,] FALSE     FALSE       FALSE        FALSE
## [1019,] FALSE     FALSE       FALSE        FALSE
## [1020,] FALSE     FALSE       FALSE        FALSE
## [1021,] FALSE     FALSE       FALSE        FALSE
## [1022,] FALSE     FALSE       FALSE        FALSE
## [1023,] FALSE     FALSE       FALSE        FALSE
## [1024,] FALSE     FALSE       FALSE        FALSE
## [1025,] FALSE     FALSE       FALSE        FALSE
## [1026,] FALSE     FALSE       FALSE        FALSE
## [1027,] FALSE     FALSE       FALSE        FALSE
## [1028,] FALSE     FALSE       FALSE        FALSE
## [1029,] FALSE     FALSE       FALSE        FALSE
## [1030,] FALSE     FALSE       FALSE        FALSE
## [1031,] FALSE     FALSE       FALSE        FALSE
## [1032,] FALSE     FALSE       FALSE        FALSE
## [1033,] FALSE     FALSE       FALSE        FALSE
## [1034,] FALSE     FALSE       FALSE        FALSE
## [1035,] FALSE     FALSE       FALSE        FALSE
## [1036,] FALSE     FALSE       FALSE        FALSE
## [1037,] FALSE     FALSE       FALSE        FALSE
## [1038,] FALSE     FALSE       FALSE        FALSE
## [1039,] FALSE     FALSE       FALSE        FALSE
## [1040,] FALSE     FALSE       FALSE        FALSE
## [1041,] FALSE     FALSE       FALSE        FALSE
## [1042,] FALSE     FALSE       FALSE        FALSE
## [1043,] FALSE     FALSE       FALSE        FALSE
## [1044,] FALSE     FALSE       FALSE        FALSE
## [1045,] FALSE     FALSE       FALSE        FALSE
## [1046,] FALSE     FALSE       FALSE        FALSE
## [1047,] FALSE     FALSE       FALSE        FALSE
## [1048,] FALSE     FALSE       FALSE        FALSE
## [1049,] FALSE     FALSE       FALSE        FALSE
## [1050,] FALSE     FALSE       FALSE        FALSE
## [1051,] FALSE     FALSE       FALSE        FALSE
## [1052,] FALSE     FALSE       FALSE        FALSE
## [1053,] FALSE     FALSE       FALSE        FALSE
## [1054,] FALSE     FALSE       FALSE        FALSE
## [1055,] FALSE     FALSE       FALSE        FALSE
## [1056,] FALSE     FALSE       FALSE        FALSE
## [1057,] FALSE     FALSE       FALSE        FALSE
## [1058,] FALSE     FALSE       FALSE        FALSE
## [1059,] FALSE     FALSE       FALSE        FALSE
## [1060,] FALSE     FALSE       FALSE        FALSE
## [1061,] FALSE     FALSE       FALSE        FALSE
## [1062,] FALSE     FALSE       FALSE        FALSE
## [1063,] FALSE     FALSE       FALSE        FALSE
## [1064,] FALSE     FALSE       FALSE        FALSE
## [1065,] FALSE     FALSE       FALSE        FALSE
## [1066,] FALSE     FALSE       FALSE        FALSE
## [1067,] FALSE     FALSE       FALSE        FALSE
## [1068,] FALSE     FALSE       FALSE        FALSE
## [1069,] FALSE     FALSE       FALSE        FALSE
## [1070,] FALSE     FALSE       FALSE        FALSE
## [1071,] FALSE     FALSE       FALSE        FALSE
## [1072,] FALSE     FALSE       FALSE        FALSE
## [1073,] FALSE     FALSE       FALSE        FALSE
## [1074,] FALSE     FALSE       FALSE        FALSE
## [1075,] FALSE     FALSE       FALSE        FALSE
## [1076,] FALSE     FALSE       FALSE        FALSE
## [1077,] FALSE     FALSE       FALSE        FALSE
## [1078,] FALSE     FALSE       FALSE        FALSE
## [1079,] FALSE     FALSE       FALSE        FALSE
## [1080,] FALSE     FALSE       FALSE        FALSE
## [1081,] FALSE     FALSE       FALSE        FALSE
## [1082,] FALSE     FALSE       FALSE        FALSE
## [1083,] FALSE     FALSE       FALSE        FALSE
## [1084,] FALSE     FALSE       FALSE        FALSE
## [1085,] FALSE     FALSE       FALSE        FALSE
## [1086,] FALSE     FALSE       FALSE        FALSE
## [1087,] FALSE     FALSE       FALSE        FALSE
## [1088,] FALSE     FALSE       FALSE        FALSE
## [1089,] FALSE     FALSE       FALSE        FALSE
## [1090,] FALSE     FALSE       FALSE        FALSE
## [1091,] FALSE     FALSE       FALSE        FALSE
## [1092,] FALSE     FALSE       FALSE        FALSE
## [1093,] FALSE     FALSE       FALSE        FALSE
## [1094,] FALSE     FALSE       FALSE        FALSE
## [1095,] FALSE     FALSE       FALSE        FALSE
## [1096,] FALSE     FALSE       FALSE        FALSE
## [1097,] FALSE     FALSE       FALSE        FALSE
## [1098,] FALSE     FALSE       FALSE        FALSE
## [1099,] FALSE     FALSE       FALSE        FALSE
## [1100,] FALSE     FALSE       FALSE        FALSE
## [1101,] FALSE     FALSE       FALSE        FALSE
## [1102,] FALSE     FALSE       FALSE        FALSE
## [1103,] FALSE     FALSE       FALSE        FALSE
## [1104,] FALSE     FALSE       FALSE        FALSE
## [1105,] FALSE     FALSE       FALSE        FALSE
## [1106,] FALSE     FALSE       FALSE        FALSE
## [1107,] FALSE     FALSE       FALSE        FALSE
## [1108,] FALSE     FALSE       FALSE        FALSE
## [1109,] FALSE     FALSE       FALSE        FALSE
## [1110,] FALSE     FALSE       FALSE        FALSE
## [1111,] FALSE     FALSE       FALSE        FALSE
## [1112,] FALSE     FALSE       FALSE        FALSE
## [1113,] FALSE     FALSE       FALSE        FALSE
## [1114,] FALSE     FALSE       FALSE        FALSE
## [1115,] FALSE     FALSE       FALSE        FALSE
## [1116,] FALSE     FALSE       FALSE        FALSE
## [1117,] FALSE     FALSE       FALSE        FALSE
## [1118,] FALSE     FALSE       FALSE        FALSE
## [1119,] FALSE     FALSE       FALSE        FALSE
## [1120,] FALSE     FALSE       FALSE        FALSE
## [1121,] FALSE     FALSE       FALSE        FALSE
## [1122,] FALSE     FALSE       FALSE        FALSE
## [1123,] FALSE     FALSE       FALSE        FALSE
## [1124,] FALSE     FALSE       FALSE        FALSE
## [1125,] FALSE     FALSE       FALSE        FALSE
## [1126,] FALSE     FALSE       FALSE        FALSE
## [1127,] FALSE     FALSE       FALSE        FALSE
## [1128,] FALSE     FALSE       FALSE        FALSE
## [1129,] FALSE     FALSE       FALSE        FALSE
## [1130,] FALSE     FALSE       FALSE        FALSE
## [1131,] FALSE     FALSE       FALSE        FALSE
## [1132,] FALSE     FALSE       FALSE        FALSE
## [1133,] FALSE     FALSE       FALSE        FALSE
## [1134,] FALSE     FALSE       FALSE        FALSE
## [1135,] FALSE     FALSE       FALSE        FALSE
## [1136,] FALSE     FALSE       FALSE        FALSE
## [1137,] FALSE     FALSE       FALSE        FALSE
## [1138,] FALSE     FALSE       FALSE        FALSE
## [1139,] FALSE     FALSE       FALSE        FALSE
## [1140,] FALSE     FALSE       FALSE        FALSE
## [1141,] FALSE     FALSE       FALSE        FALSE
## [1142,] FALSE     FALSE       FALSE        FALSE
## [1143,] FALSE     FALSE       FALSE        FALSE
## [1144,] FALSE     FALSE       FALSE        FALSE
## [1145,] FALSE     FALSE       FALSE        FALSE
## [1146,] FALSE     FALSE       FALSE        FALSE
## [1147,] FALSE     FALSE       FALSE        FALSE
## [1148,] FALSE     FALSE       FALSE        FALSE
## [1149,] FALSE     FALSE       FALSE        FALSE
## [1150,] FALSE     FALSE       FALSE        FALSE
## [1151,] FALSE     FALSE       FALSE        FALSE
## [1152,] FALSE     FALSE       FALSE        FALSE
## [1153,] FALSE     FALSE       FALSE        FALSE
## [1154,] FALSE     FALSE       FALSE        FALSE
## [1155,] FALSE     FALSE       FALSE        FALSE
## [1156,] FALSE     FALSE       FALSE        FALSE
## [1157,] FALSE     FALSE       FALSE        FALSE
## [1158,] FALSE     FALSE       FALSE        FALSE
## [1159,] FALSE     FALSE       FALSE        FALSE
## [1160,] FALSE     FALSE       FALSE        FALSE
## [1161,] FALSE     FALSE       FALSE        FALSE
## [1162,] FALSE     FALSE       FALSE        FALSE
## [1163,] FALSE     FALSE       FALSE        FALSE
## [1164,] FALSE     FALSE       FALSE        FALSE
## [1165,] FALSE     FALSE       FALSE        FALSE
## [1166,] FALSE     FALSE       FALSE        FALSE
## [1167,] FALSE     FALSE       FALSE        FALSE
## [1168,] FALSE     FALSE       FALSE        FALSE
## [1169,] FALSE     FALSE       FALSE        FALSE
## [1170,] FALSE     FALSE       FALSE        FALSE
## [1171,] FALSE     FALSE       FALSE        FALSE
## [1172,] FALSE     FALSE       FALSE        FALSE
## [1173,] FALSE     FALSE       FALSE        FALSE
## [1174,] FALSE     FALSE       FALSE        FALSE
## [1175,] FALSE     FALSE       FALSE        FALSE
## [1176,] FALSE     FALSE       FALSE        FALSE
## [1177,] FALSE     FALSE       FALSE        FALSE
## [1178,] FALSE     FALSE       FALSE        FALSE
## [1179,] FALSE     FALSE       FALSE        FALSE
## [1180,] FALSE     FALSE       FALSE        FALSE
## [1181,] FALSE     FALSE       FALSE        FALSE
## [1182,] FALSE     FALSE       FALSE        FALSE
## [1183,] FALSE     FALSE       FALSE        FALSE
## [1184,] FALSE     FALSE       FALSE        FALSE
## [1185,] FALSE     FALSE       FALSE        FALSE
## [1186,] FALSE     FALSE       FALSE        FALSE
## [1187,] FALSE     FALSE       FALSE        FALSE
## [1188,] FALSE     FALSE       FALSE        FALSE
## [1189,] FALSE     FALSE       FALSE        FALSE
## [1190,] FALSE     FALSE       FALSE        FALSE
## [1191,] FALSE     FALSE       FALSE        FALSE
## [1192,] FALSE     FALSE       FALSE        FALSE
## [1193,] FALSE     FALSE       FALSE        FALSE
## [1194,] FALSE     FALSE       FALSE        FALSE
## [1195,] FALSE     FALSE       FALSE        FALSE
## [1196,] FALSE     FALSE       FALSE        FALSE
## [1197,] FALSE     FALSE       FALSE        FALSE
## [1198,] FALSE     FALSE       FALSE        FALSE
## [1199,] FALSE     FALSE       FALSE        FALSE
## [1200,] FALSE     FALSE       FALSE        FALSE
## [1201,] FALSE     FALSE       FALSE        FALSE
## [1202,] FALSE     FALSE       FALSE        FALSE
## [1203,] FALSE     FALSE       FALSE        FALSE
## [1204,] FALSE     FALSE       FALSE        FALSE
## [1205,] FALSE     FALSE       FALSE        FALSE
## [1206,] FALSE     FALSE       FALSE        FALSE
## [1207,] FALSE     FALSE       FALSE        FALSE
## [1208,] FALSE     FALSE       FALSE        FALSE
## [1209,] FALSE     FALSE       FALSE        FALSE
## [1210,] FALSE     FALSE       FALSE        FALSE
## [1211,] FALSE     FALSE       FALSE        FALSE
## [1212,] FALSE     FALSE       FALSE        FALSE
## [1213,] FALSE     FALSE       FALSE        FALSE
## [1214,] FALSE     FALSE       FALSE        FALSE
## [1215,] FALSE     FALSE       FALSE        FALSE
## [1216,] FALSE     FALSE       FALSE        FALSE
## [1217,] FALSE     FALSE       FALSE        FALSE
## [1218,] FALSE     FALSE       FALSE        FALSE
## [1219,] FALSE     FALSE       FALSE        FALSE
## [1220,] FALSE     FALSE       FALSE        FALSE
## [1221,] FALSE     FALSE       FALSE        FALSE
## [1222,] FALSE     FALSE       FALSE        FALSE
## [1223,] FALSE     FALSE       FALSE        FALSE
## [1224,] FALSE     FALSE       FALSE        FALSE
## [1225,] FALSE     FALSE       FALSE        FALSE
## [1226,] FALSE     FALSE       FALSE        FALSE
## [1227,] FALSE     FALSE       FALSE        FALSE
## [1228,] FALSE     FALSE       FALSE        FALSE
## [1229,] FALSE     FALSE       FALSE        FALSE
## [1230,] FALSE     FALSE       FALSE        FALSE
## [1231,] FALSE     FALSE       FALSE        FALSE
## [1232,] FALSE     FALSE       FALSE        FALSE
## [1233,] FALSE     FALSE       FALSE        FALSE
## [1234,] FALSE     FALSE       FALSE        FALSE
## [1235,] FALSE     FALSE       FALSE        FALSE
## [1236,] FALSE     FALSE       FALSE        FALSE
## [1237,] FALSE     FALSE       FALSE        FALSE
## [1238,] FALSE     FALSE       FALSE        FALSE
## [1239,] FALSE     FALSE       FALSE        FALSE
## [1240,] FALSE     FALSE       FALSE        FALSE
## [1241,] FALSE     FALSE       FALSE        FALSE
## [1242,] FALSE     FALSE       FALSE        FALSE
## [1243,] FALSE     FALSE       FALSE        FALSE
## [1244,] FALSE     FALSE       FALSE        FALSE
## [1245,] FALSE     FALSE       FALSE        FALSE
## [1246,] FALSE     FALSE       FALSE        FALSE
## [1247,] FALSE     FALSE       FALSE        FALSE
## [1248,] FALSE     FALSE       FALSE        FALSE
## [1249,] FALSE     FALSE       FALSE        FALSE
## [1250,] FALSE     FALSE       FALSE        FALSE
## [1251,] FALSE     FALSE       FALSE        FALSE
## [1252,] FALSE     FALSE       FALSE        FALSE
## [1253,] FALSE     FALSE       FALSE        FALSE
## [1254,] FALSE     FALSE       FALSE        FALSE
## [1255,] FALSE     FALSE       FALSE        FALSE
## [1256,] FALSE     FALSE       FALSE        FALSE
## [1257,] FALSE     FALSE       FALSE        FALSE
## [1258,] FALSE     FALSE       FALSE        FALSE
## [1259,] FALSE     FALSE       FALSE        FALSE
## [1260,] FALSE     FALSE       FALSE        FALSE
## [1261,] FALSE     FALSE       FALSE        FALSE
## [1262,] FALSE     FALSE       FALSE        FALSE
## [1263,] FALSE     FALSE       FALSE        FALSE
## [1264,] FALSE     FALSE       FALSE        FALSE
## [1265,] FALSE     FALSE       FALSE        FALSE
## [1266,] FALSE     FALSE       FALSE        FALSE
## [1267,] FALSE     FALSE       FALSE        FALSE
## [1268,] FALSE     FALSE       FALSE        FALSE
## [1269,] FALSE     FALSE       FALSE        FALSE
## [1270,] FALSE     FALSE       FALSE        FALSE
## [1271,] FALSE     FALSE       FALSE        FALSE
## [1272,] FALSE     FALSE       FALSE        FALSE
## [1273,] FALSE     FALSE       FALSE        FALSE
## [1274,] FALSE     FALSE       FALSE        FALSE
## [1275,] FALSE     FALSE       FALSE        FALSE
## [1276,] FALSE     FALSE       FALSE        FALSE
## [1277,] FALSE     FALSE       FALSE        FALSE
## [1278,] FALSE     FALSE       FALSE        FALSE
## [1279,] FALSE     FALSE       FALSE        FALSE
## [1280,] FALSE     FALSE       FALSE        FALSE
## [1281,] FALSE     FALSE       FALSE        FALSE
## [1282,] FALSE     FALSE       FALSE        FALSE
## [1283,] FALSE     FALSE       FALSE        FALSE
## [1284,] FALSE     FALSE       FALSE        FALSE
## [1285,] FALSE     FALSE       FALSE        FALSE
## [1286,] FALSE     FALSE       FALSE        FALSE
## [1287,] FALSE     FALSE       FALSE        FALSE
## [1288,] FALSE     FALSE       FALSE        FALSE
## [1289,] FALSE     FALSE       FALSE        FALSE
## [1290,] FALSE     FALSE       FALSE        FALSE
## [1291,] FALSE     FALSE       FALSE        FALSE
## [1292,] FALSE     FALSE       FALSE        FALSE
## [1293,] FALSE     FALSE       FALSE        FALSE
## [1294,] FALSE     FALSE       FALSE        FALSE
## [1295,] FALSE     FALSE       FALSE        FALSE
## [1296,] FALSE     FALSE       FALSE        FALSE
## [1297,] FALSE     FALSE       FALSE        FALSE
## [1298,] FALSE     FALSE       FALSE        FALSE
## [1299,] FALSE     FALSE       FALSE        FALSE
## [1300,] FALSE     FALSE       FALSE        FALSE
## [1301,] FALSE     FALSE       FALSE        FALSE
## [1302,] FALSE     FALSE       FALSE        FALSE
## [1303,] FALSE     FALSE       FALSE        FALSE
## [1304,] FALSE     FALSE       FALSE        FALSE
## [1305,] FALSE     FALSE       FALSE        FALSE
## [1306,] FALSE     FALSE       FALSE        FALSE
## [1307,] FALSE     FALSE       FALSE        FALSE
## [1308,] FALSE     FALSE       FALSE        FALSE
## [1309,] FALSE     FALSE       FALSE        FALSE
## [1310,] FALSE     FALSE       FALSE        FALSE
## [1311,] FALSE     FALSE       FALSE        FALSE
## [1312,] FALSE     FALSE       FALSE        FALSE
## [1313,] FALSE     FALSE       FALSE        FALSE
## [1314,] FALSE     FALSE       FALSE        FALSE
## [1315,] FALSE     FALSE       FALSE        FALSE
## [1316,] FALSE     FALSE       FALSE        FALSE
## [1317,] FALSE     FALSE       FALSE        FALSE
## [1318,] FALSE     FALSE       FALSE        FALSE
## [1319,] FALSE     FALSE       FALSE        FALSE
## [1320,] FALSE     FALSE       FALSE        FALSE
## [1321,] FALSE     FALSE       FALSE        FALSE
## [1322,] FALSE     FALSE       FALSE        FALSE
## [1323,] FALSE     FALSE       FALSE        FALSE
## [1324,] FALSE     FALSE       FALSE        FALSE
## [1325,] FALSE     FALSE       FALSE        FALSE
## [1326,] FALSE     FALSE       FALSE        FALSE
## [1327,] FALSE     FALSE       FALSE        FALSE
## [1328,] FALSE     FALSE       FALSE        FALSE
## [1329,] FALSE     FALSE       FALSE        FALSE
## [1330,] FALSE     FALSE       FALSE        FALSE
## [1331,] FALSE     FALSE       FALSE        FALSE
## [1332,] FALSE     FALSE       FALSE        FALSE
## [1333,] FALSE     FALSE       FALSE        FALSE
## [1334,] FALSE     FALSE       FALSE        FALSE
## [1335,] FALSE     FALSE       FALSE        FALSE
## [1336,] FALSE     FALSE       FALSE        FALSE
## [1337,] FALSE     FALSE       FALSE        FALSE
## [1338,] FALSE     FALSE       FALSE        FALSE
## [1339,] FALSE     FALSE       FALSE        FALSE
## [1340,] FALSE     FALSE       FALSE        FALSE
## [1341,] FALSE     FALSE       FALSE        FALSE
## [1342,] FALSE     FALSE       FALSE        FALSE
## [1343,] FALSE     FALSE       FALSE        FALSE
## [1344,] FALSE     FALSE       FALSE        FALSE
## [1345,] FALSE     FALSE       FALSE        FALSE
## [1346,] FALSE     FALSE       FALSE        FALSE
## [1347,] FALSE     FALSE       FALSE        FALSE
## [1348,] FALSE     FALSE       FALSE        FALSE
## [1349,] FALSE     FALSE       FALSE        FALSE
## [1350,] FALSE     FALSE       FALSE        FALSE
## [1351,] FALSE     FALSE       FALSE        FALSE
## [1352,] FALSE     FALSE       FALSE        FALSE
## [1353,] FALSE     FALSE       FALSE        FALSE
## [1354,] FALSE     FALSE       FALSE        FALSE
## [1355,] FALSE     FALSE       FALSE        FALSE
## [1356,] FALSE     FALSE       FALSE        FALSE
## [1357,] FALSE     FALSE       FALSE        FALSE
## [1358,] FALSE     FALSE       FALSE        FALSE
## [1359,] FALSE     FALSE       FALSE        FALSE
## [1360,] FALSE     FALSE       FALSE        FALSE
## [1361,] FALSE     FALSE       FALSE        FALSE
## [1362,] FALSE     FALSE       FALSE        FALSE
## [1363,] FALSE     FALSE       FALSE        FALSE
## [1364,] FALSE     FALSE       FALSE        FALSE
## [1365,] FALSE     FALSE       FALSE        FALSE
## [1366,] FALSE     FALSE       FALSE        FALSE
## [1367,] FALSE     FALSE       FALSE        FALSE
## [1368,] FALSE     FALSE       FALSE        FALSE
## [1369,] FALSE     FALSE       FALSE        FALSE
## [1370,] FALSE     FALSE       FALSE        FALSE
## [1371,] FALSE     FALSE       FALSE        FALSE
## [1372,] FALSE     FALSE       FALSE        FALSE
## [1373,] FALSE     FALSE       FALSE        FALSE
## [1374,] FALSE     FALSE       FALSE        FALSE
## [1375,] FALSE     FALSE       FALSE        FALSE
## [1376,] FALSE     FALSE       FALSE        FALSE
## [1377,] FALSE     FALSE       FALSE        FALSE
## [1378,] FALSE     FALSE       FALSE        FALSE
## [1379,] FALSE     FALSE       FALSE        FALSE
## [1380,] FALSE     FALSE       FALSE        FALSE
## [1381,] FALSE     FALSE       FALSE        FALSE
## [1382,] FALSE     FALSE       FALSE        FALSE
## [1383,] FALSE     FALSE       FALSE        FALSE
## [1384,] FALSE     FALSE       FALSE        FALSE
## [1385,] FALSE     FALSE       FALSE        FALSE
## [1386,] FALSE     FALSE       FALSE        FALSE
## [1387,] FALSE     FALSE       FALSE        FALSE
## [1388,] FALSE     FALSE       FALSE        FALSE
## [1389,] FALSE     FALSE       FALSE        FALSE
## [1390,] FALSE     FALSE       FALSE        FALSE
## [1391,] FALSE     FALSE       FALSE        FALSE
## [1392,] FALSE     FALSE       FALSE        FALSE
## [1393,] FALSE     FALSE       FALSE        FALSE
## [1394,] FALSE     FALSE       FALSE        FALSE
## [1395,] FALSE     FALSE       FALSE        FALSE
## [1396,] FALSE     FALSE       FALSE        FALSE
## [1397,] FALSE     FALSE       FALSE        FALSE
## [1398,] FALSE     FALSE       FALSE        FALSE
## [1399,] FALSE     FALSE       FALSE        FALSE
## [1400,] FALSE     FALSE       FALSE        FALSE
## [1401,] FALSE     FALSE       FALSE        FALSE
## [1402,] FALSE     FALSE       FALSE        FALSE
## [1403,] FALSE     FALSE       FALSE        FALSE
## [1404,] FALSE     FALSE       FALSE        FALSE
## [1405,] FALSE     FALSE       FALSE        FALSE
## [1406,] FALSE     FALSE       FALSE        FALSE
## [1407,] FALSE     FALSE       FALSE        FALSE
## [1408,] FALSE     FALSE       FALSE        FALSE
## [1409,] FALSE     FALSE       FALSE        FALSE
## [1410,] FALSE     FALSE       FALSE        FALSE
## [1411,] FALSE     FALSE       FALSE        FALSE
## [1412,] FALSE     FALSE       FALSE        FALSE
## [1413,] FALSE     FALSE       FALSE        FALSE
## [1414,] FALSE     FALSE       FALSE        FALSE
## [1415,] FALSE     FALSE       FALSE        FALSE
## [1416,] FALSE     FALSE       FALSE        FALSE
## [1417,] FALSE     FALSE       FALSE        FALSE
## [1418,] FALSE     FALSE       FALSE        FALSE
## [1419,] FALSE     FALSE       FALSE        FALSE
## [1420,] FALSE     FALSE       FALSE        FALSE
## [1421,] FALSE     FALSE       FALSE        FALSE
## [1422,] FALSE     FALSE       FALSE        FALSE
## [1423,] FALSE     FALSE       FALSE        FALSE
## [1424,] FALSE     FALSE       FALSE        FALSE
## [1425,] FALSE     FALSE       FALSE        FALSE
## [1426,] FALSE     FALSE       FALSE        FALSE
## [1427,] FALSE     FALSE       FALSE        FALSE
## [1428,] FALSE     FALSE       FALSE        FALSE
## [1429,] FALSE     FALSE       FALSE        FALSE
## [1430,] FALSE     FALSE       FALSE        FALSE
## [1431,] FALSE     FALSE       FALSE        FALSE
## [1432,] FALSE     FALSE       FALSE        FALSE
## [1433,] FALSE     FALSE       FALSE        FALSE
## [1434,] FALSE     FALSE       FALSE        FALSE
## [1435,] FALSE     FALSE       FALSE        FALSE
## [1436,] FALSE     FALSE       FALSE        FALSE
## [1437,] FALSE     FALSE       FALSE        FALSE
## [1438,] FALSE     FALSE       FALSE        FALSE
## [1439,] FALSE     FALSE       FALSE        FALSE
## [1440,] FALSE     FALSE       FALSE        FALSE
```

```r
#no
```

3. What are the names of the columns in the `mammals` data?

```r
colnames(mammals)
```

```
##  [1] "order"        "family"       "Genus"        "species"      "mass"        
##  [6] "gestation"    "newborn"      "weaning"      "wean mass"    "AFR"         
## [11] "max. life"    "litter size"  "litters/year"
```

4. Rename any columns that have capitol letters or punctuation issues.  

```r
names(mammals)[names(sleep) == 'Genus'] <- "genus"
names(mammals)[names(sleep) == 'wean mass'] <- "wean_mass"
names(mammals)[names(sleep) == 'AFR'] <- "afr"
names(mammals)[names(sleep) == 'max. life'] <- "max_life"
names(mammals)[names(sleep) == 'litter size'] <- "litter_size"
names(mammals)[names(sleep) == 'litters/year'] <- "litters_per_year"
```

5. We are only interested in the variables `genus`, `species`, and `mass`. Use `select()` to build a new dataframe `mass` focused on these variables.

```r
mass <- select(mammals, contains("genus"), contains("species"), contains("mass"))
```

6. What if we only wanted to exclude `order` and `family`? Use the `-` operator to make the code efficient.

```r
select(mammals, -order, -family)
```

```
## # A tibble: 1,440 × 11
##    Genus species   mass gestation newborn weaning `wean mass`    AFR `max. life`
##    <chr> <chr>    <dbl>     <dbl>   <dbl>   <dbl>       <dbl>  <dbl>       <dbl>
##  1 Anti… americ… 4.54e4      8.13   3246.    3           8900   13.5         142
##  2 Addax nasoma… 1.82e5      9.39   5480     6.5         -999   27.3         308
##  3 Aepy… melamp… 4.15e4      6.35   5093     5.63       15900   16.7         213
##  4 Alce… busela… 1.5 e5      7.9   10167.    6.5         -999   23.0         240
##  5 Ammo… clarkei 2.85e4      6.8    -999  -999           -999 -999          -999
##  6 Ammo… lervia  5.55e4      5.08   3810     4           -999   14.9         251
##  7 Anti… marsup… 3   e4      5.72   3910     4.04        -999   10.2         228
##  8 Anti… cervic… 3.75e4      5.5    3846     2.13        -999   20.1         255
##  9 Bison bison   4.98e5      8.93  20000    10.7       157500   29.4         300
## 10 Bison bonasus 5   e5      9.14  23000.    6.6         -999   30.0         324
## # … with 1,430 more rows, and 2 more variables: litter size <dbl>,
## #   litters/year <dbl>
```

7. Select the columns that include "mass" as part of the name.  

```r
select(mammals, contains("mass"))
```

```
## # A tibble: 1,440 × 2
##       mass `wean mass`
##      <dbl>       <dbl>
##  1  45375         8900
##  2 182375         -999
##  3  41480        15900
##  4 150000         -999
##  5  28500         -999
##  6  55500         -999
##  7  30000         -999
##  8  37500         -999
##  9 497667.      157500
## 10 500000         -999
## # … with 1,430 more rows
```

8. Select all of the columns that are of class `character`.  

```r
select_if(mammals, is.character)
```

```
## # A tibble: 1,440 × 4
##    order        family         Genus       species      
##    <chr>        <chr>          <chr>       <chr>        
##  1 Artiodactyla Antilocapridae Antilocapra americana    
##  2 Artiodactyla Bovidae        Addax       nasomaculatus
##  3 Artiodactyla Bovidae        Aepyceros   melampus     
##  4 Artiodactyla Bovidae        Alcelaphus  buselaphus   
##  5 Artiodactyla Bovidae        Ammodorcas  clarkei      
##  6 Artiodactyla Bovidae        Ammotragus  lervia       
##  7 Artiodactyla Bovidae        Antidorcas  marsupialis  
##  8 Artiodactyla Bovidae        Antilope    cervicapra   
##  9 Artiodactyla Bovidae        Bison       bison        
## 10 Artiodactyla Bovidae        Bison       bonasus      
## # … with 1,430 more rows
```

## Other
Here are two examples of code that are super helpful to have in your bag of tricks.  

Imported data frames often have a mix of lower and uppercase column names. Use `toupper()` or `tolower()` to fix this issue. I always try to use lowercase to keep things consistent.  

```r
#mammals <- select_all(mammals, tolower)
```

When naming columns, blank spaces are often added (don't do this, please). Here is a trick to remove these.  

```r
#select_all(mammals, ~str_replace(., " ", "_"))
```

## That's it! Take a break and I will see you on Zoom!  

-->[Home](https://jmledford3115.github.io/datascibiol/)  
