---
title: "Lab 7 Homework"
author: "Joel Ledford"
date: "2022-02-15"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(skimr)
```

## Data
**1. For this homework, we will use two different data sets. Please load `amniota` and `amphibio`.**  

`amniota` data:  
Myhrvold N, Baldridge E, Chan B, Sivam D, Freeman DL, Ernest SKM (2015). “An amniote life-history
database to perform comparative analyses with birds, mammals, and reptiles.” _Ecology_, *96*, 3109.
doi: 10.1890/15-0846.1 (URL: https://doi.org/10.1890/15-0846.1).

```r
amniota <- read.csv('~/Midtermbis15/amniota.csv')
```

`amphibio` data:  
Oliveira BF, São-Pedro VA, Santos-Barrera G, Penone C, Costa GC (2017). “AmphiBIO, a global database
for amphibian ecological traits.” _Scientific Data_, *4*, 170123. doi: 10.1038/sdata.2017.123 (URL:
https://doi.org/10.1038/sdata.2017.123).

```r
amphibio <- read.csv('~/Midtermbis15/amphibio.csv')
```

## Questions  
**2. Do some exploratory analysis of the `amniota` data set. Use the function(s) of your choice. Try to get an idea of how NA's are represented in the data.**  

```r
glimpse(amniota)
```

```
## Rows: 21,322
## Columns: 36
## $ class                                 <chr> "Aves", "Aves", "Aves", "Aves", …
## $ order                                 <chr> "Accipitriformes", "Accipitrifor…
## $ family                                <chr> "Accipitridae", "Accipitridae", …
## $ genus                                 <chr> "Accipiter", "Accipiter", "Accip…
## $ species                               <chr> "albogularis", "badius", "bicolo…
## $ subspecies                            <int> -999, -999, -999, -999, -999, -9…
## $ common_name                           <chr> "Pied Goshawk", "Shikra", "Bicol…
## $ female_maturity_d                     <dbl> -999.000, 363.468, -999.000, -99…
## $ litter_or_clutch_size_n               <dbl> -999.000, 3.250, 2.700, -999.000…
## $ litters_or_clutches_per_y             <dbl> -999, 1, -999, -999, 1, -999, -9…
## $ adult_body_mass_g                     <dbl> 251.500, 140.000, 345.000, 142.0…
## $ maximum_longevity_y                   <dbl> -999.00000, -999.00000, -999.000…
## $ gestation_d                           <dbl> -999, -999, -999, -999, -999, -9…
## $ weaning_d                             <dbl> -999, -999, -999, -999, -999, -9…
## $ birth_or_hatching_weight_g            <dbl> -999, -999, -999, -999, -999, -9…
## $ weaning_weight_g                      <dbl> -999, -999, -999, -999, -999, -9…
## $ egg_mass_g                            <dbl> -999.00, 21.00, 32.00, -999.00, …
## $ incubation_d                          <dbl> -999.00, 30.00, -999.00, -999.00…
## $ fledging_age_d                        <dbl> -999.00, 32.00, -999.00, -999.00…
## $ longevity_y                           <dbl> -999.00000, -999.00000, -999.000…
## $ male_maturity_d                       <dbl> -999, -999, -999, -999, -999, -9…
## $ inter_litter_or_interbirth_interval_y <dbl> -999, -999, -999, -999, -999, -9…
## $ female_body_mass_g                    <dbl> 352.500, 168.500, 390.000, -999.…
## $ male_body_mass_g                      <dbl> 223.000, 125.000, 212.000, 142.0…
## $ no_sex_body_mass_g                    <dbl> -999.0, 123.0, -999.0, -999.0, -…
## $ egg_width_mm                          <dbl> -999, -999, -999, -999, -999, -9…
## $ egg_length_mm                         <dbl> -999, -999, -999, -999, -999, -9…
## $ fledging_mass_g                       <dbl> -999, -999, -999, -999, -999, -9…
## $ adult_svl_cm                          <dbl> -999.00, 30.00, 39.50, -999.00, …
## $ male_svl_cm                           <dbl> -999, -999, -999, -999, -999, -9…
## $ female_svl_cm                         <dbl> -999, -999, -999, -999, -999, -9…
## $ birth_or_hatching_svl_cm              <dbl> -999, -999, -999, -999, -999, -9…
## $ female_svl_at_maturity_cm             <dbl> -999, -999, -999, -999, -999, -9…
## $ female_body_mass_at_maturity_g        <int> -999, -999, -999, -999, -999, -9…
## $ no_sex_svl_cm                         <dbl> -999, -999, -999, -999, -999, -9…
## $ no_sex_maturity_d                     <dbl> -999, -999, -999, -999, -999, -9…
```

```r
#NAs represented by -999
```

**3. Do some exploratory analysis of the `amphibio` data set. Use the function(s) of your choice. Try to get an idea of how NA's are represented in the data.**  

```r
glimpse(amphibio)
```

```
## Rows: 6,776
## Columns: 38
## $ id                      <chr> "Anf0001", "Anf0002", "Anf0003", "Anf0004", "A…
## $ Order                   <chr> "Anura", "Anura", "Anura", "Anura", "Anura", "…
## $ Family                  <chr> "Allophrynidae", "Alytidae", "Alytidae", "Alyt…
## $ Genus                   <chr> "Allophryne", "Alytes", "Alytes", "Alytes", "A…
## $ Species                 <chr> "Allophryne ruthveni", "Alytes cisternasii", "…
## $ Fos                     <int> NA, NA, NA, NA, NA, 1, 1, 1, 1, 1, 1, 1, 1, NA…
## $ Ter                     <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1…
## $ Aqu                     <int> 1, 1, 1, 1, NA, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
## $ Arb                     <int> 1, 1, 1, 1, 1, 1, NA, NA, NA, NA, NA, NA, NA, …
## $ Leaves                  <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ Flowers                 <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ Seeds                   <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ Fruits                  <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ Arthro                  <int> 1, 1, 1, NA, 1, 1, 1, 1, 1, NA, 1, 1, NA, NA, …
## $ Vert                    <int> NA, NA, NA, NA, NA, NA, 1, NA, NA, NA, 1, 1, N…
## $ Diu                     <int> 1, NA, NA, NA, NA, NA, 1, 1, 1, NA, 1, 1, NA, …
## $ Noc                     <int> 1, 1, 1, NA, 1, 1, 1, 1, 1, NA, 1, 1, 1, NA, N…
## $ Crepu                   <int> 1, NA, NA, NA, NA, 1, NA, NA, NA, NA, NA, NA, …
## $ Wet_warm                <int> NA, NA, NA, NA, 1, 1, NA, NA, NA, NA, 1, NA, N…
## $ Wet_cold                <int> 1, NA, NA, NA, NA, NA, 1, NA, NA, NA, NA, NA, …
## $ Dry_warm                <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ Dry_cold                <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ Body_mass_g             <dbl> 31.00, 6.10, NA, NA, 2.31, 13.40, 21.80, NA, N…
## $ Age_at_maturity_min_y   <dbl> NA, 2.0, 2.0, NA, 3.0, 2.0, 3.0, NA, NA, NA, 4…
## $ Age_at_maturity_max_y   <dbl> NA, 2.0, 2.0, NA, 3.0, 3.0, 5.0, NA, NA, NA, 4…
## $ Body_size_mm            <dbl> 31.0, 50.0, 55.0, NA, 40.0, 55.0, 80.0, 60.0, …
## $ Size_at_maturity_min_mm <dbl> NA, 27, NA, NA, NA, 35, NA, NA, NA, NA, NA, NA…
## $ Size_at_maturity_max_mm <dbl> NA, 36.0, NA, NA, NA, 40.5, NA, NA, NA, NA, NA…
## $ Longevity_max_y         <dbl> NA, 6, NA, NA, NA, 7, 9, NA, NA, NA, NA, NA, N…
## $ Litter_size_min_n       <dbl> 300, 60, 40, NA, 7, 53, 300, 1500, 1000, NA, 2…
## $ Litter_size_max_n       <dbl> 300, 180, 40, NA, 20, 171, 1500, 1500, 1000, N…
## $ Reproductive_output_y   <dbl> 1, 4, 1, 4, 1, 4, 6, 1, 1, 1, 1, 1, 1, 1, NA, …
## $ Offspring_size_min_mm   <dbl> NA, 2.6, NA, NA, 5.4, 2.6, 1.5, NA, 1.5, NA, 1…
## $ Offspring_size_max_mm   <dbl> NA, 3.5, NA, NA, 7.0, 5.0, 2.0, NA, 1.5, NA, 1…
## $ Dir                     <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, N…
## $ Lar                     <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, N…
## $ Viv                     <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, N…
## $ OBS                     <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
```

```r
#NAs represented by NAs
```

**4. How many total NA's are in each data set? Do these values make sense? Are NA's represented by values?**   

`amniota`  

```r
length(which(amniota=='-999'))
```

```
## [1] 528196
```

```r
#528196
```

`amphibio`  

```r
length(which(amphibio== is.na(TRUE)))
```

```
## [1] 11394
```

```r
#11394
```

**5. Make any necessary replacements in the data such that all NA's appear as "NA".**   

```r
amniota[amniota== "-999"] = NA
```

**6. Use the package `naniar` to produce a summary, including percentages, of missing data in each column for the `amniota` data.**  

```r
#gg_miss_var is missing so using visdata package
#vis_miss(amniota)
```


```r
?naniar
```

```
## No documentation for 'naniar' in specified packages and libraries:
## you could try '??naniar'
```

**7. Use the package `naniar` to produce a summary, including percentages, of missing data in each column for the `amphibio` data.**

```r
#vis_miss(amphibio)
```

**8. For the `amniota` data, calculate the number of NAs in the `egg_mass_g` column sorted by taxonomic class; i.e. how many NA's are present in the `egg_mass_g` column in birds, mammals, and reptiles? Does this results make sense biologically? How do these results affect your interpretation of NA's?**  

```r
birds <- filter(amniota, class == 'Aves')
sum(is.na(birds$egg_mass_g))
```

```
## [1] 4914
```

```r
#4914
mammals <- filter(amniota, class == 'Mammalia')
sum(is.na(mammals$egg_mass_g))
```

```
## [1] 4953
```

```r
#4953
reptiles <- filter(amniota, class == 'Reptilia')
sum(is.na(reptiles$egg_mass_g))
```

```
## [1] 6040
```

```r
#6040
#This is wierd forsure I  dont know what to make of it
```

**9. The `amphibio` data have variables that classify species as fossorial (burrowing), terrestrial, aquatic, or arboreal.Calculate the number of NA's in each of these variables. Do you think that the authors intend us to think that there are NA's in these columns or could they represent something else? Explain.**

```r
burrowers <- filter(amphibio, Fos == '1')
landers <- filter(amphibio, Ter == '1')
waterers <- filter(amphibio, Aqu == '1')
treerers <- filter(amphibio, Arb == '1')
#6053
#1104
#2810
#4347
```

**10. Now that we know how NA's are represented in the `amniota` data, how would you load the data such that the values which represent NA's are automatically converted?**

```r
amniota[amniota== "NA"] = 0
amphibio[amphibio== "NA"] = 0
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
