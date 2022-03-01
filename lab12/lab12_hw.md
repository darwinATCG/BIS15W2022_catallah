---
title: "Lab 12 Homework"
author: "Flynn Atallah"
date: "2022-03-01"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(ggmap)
```

## Load the Data
We will use two separate data sets for this homework.  

1. The first [data set](https://rcweb.dartmouth.edu/~f002d69/workshops/index_rspatial.html) represent sightings of grizzly bears (Ursos arctos) in Alaska.  
2. The second data set is from Brandell, Ellen E (2021), Serological dataset and R code for: Patterns and processes of pathogen exposure in gray wolves across North America, Dryad, [Dataset](https://doi.org/10.5061/dryad.5hqbzkh51).  

1. Load the `grizzly` data and evaluate its structure. As part of this step, produce a summary that provides the range of latitude and longitude so you can build an appropriate bounding box.

```r
Grizzle <- read.csv('~/Midtermbis15/bear-sightings.csv')
```


```r
summary(Grizzle)
```

```
##     bear.id       longitude         latitude    
##  Min.   :   7   Min.   :-166.2   Min.   :55.02  
##  1st Qu.:2569   1st Qu.:-154.2   1st Qu.:58.13  
##  Median :4822   Median :-151.0   Median :60.97  
##  Mean   :4935   Mean   :-149.1   Mean   :61.41  
##  3rd Qu.:7387   3rd Qu.:-145.6   3rd Qu.:64.13  
##  Max.   :9996   Max.   :-131.3   Max.   :70.37
```

2. Use the range of the latitude and longitude to build an appropriate bounding box for your map.

```r
latitude <- c(55.02, 70.37)
longitude <- c(-131.3, -166.2)
bounding_box <- make_bbox(longitude, latitude, f = 0.05)
```

3. Load a map from `stamen` in a terrain style projection and display the map.

```r
stamen_map <- get_map(bounding_box, maptype = "terrain", source = "stamen")
```

```
## Source : http://tile.stamen.com/terrain/5/1/6.png
```

```
## Source : http://tile.stamen.com/terrain/5/2/6.png
```

```
## Source : http://tile.stamen.com/terrain/5/3/6.png
```

```
## Source : http://tile.stamen.com/terrain/5/4/6.png
```

```
## Source : http://tile.stamen.com/terrain/5/1/7.png
```

```
## Source : http://tile.stamen.com/terrain/5/2/7.png
```

```
## Source : http://tile.stamen.com/terrain/5/3/7.png
```

```
## Source : http://tile.stamen.com/terrain/5/4/7.png
```

```
## Source : http://tile.stamen.com/terrain/5/1/8.png
```

```
## Source : http://tile.stamen.com/terrain/5/2/8.png
```

```
## Source : http://tile.stamen.com/terrain/5/3/8.png
```

```
## Source : http://tile.stamen.com/terrain/5/4/8.png
```

```
## Source : http://tile.stamen.com/terrain/5/1/9.png
```

```
## Source : http://tile.stamen.com/terrain/5/2/9.png
```

```
## Source : http://tile.stamen.com/terrain/5/3/9.png
```

```
## Source : http://tile.stamen.com/terrain/5/4/9.png
```

```
## Source : http://tile.stamen.com/terrain/5/1/10.png
```

```
## Source : http://tile.stamen.com/terrain/5/2/10.png
```

```
## Source : http://tile.stamen.com/terrain/5/3/10.png
```

```
## Source : http://tile.stamen.com/terrain/5/4/10.png
```

```r
ggmap(stamen_map)
```

![](lab12_hw_files/figure-html/unnamed-chunk-5-1.png)<!-- -->



4. Build a final map that overlays the recorded observations of grizzly bears in Alaska.

```r
ggmap(stamen_map) + 
  geom_point(data = Grizzle, aes(longitude, latitude), size=0.2) +
  labs(x = "longitude", y = "latitude", title = "Bear Sightings in Alaska")
```

![](lab12_hw_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

5. Let's switch to the wolves data. Load the data and evaluate its structure.

```r
Woolf <- read.csv('~/Midtermbis15/wolves_data/wolves_dataset.csv')
glimpse(Woolf)
```

```
## Rows: 1,986
## Columns: 23
## $ pop                <chr> "AK.PEN", "AK.PEN", "AK.PEN", "AK.PEN", "AK.PEN", "…
## $ year               <int> 2006, 2006, 2006, 2006, 2006, 2006, 2006, 2006, 200…
## $ age.cat            <chr> "S", "S", "A", "S", "A", "A", "A", "P", "S", "P", "…
## $ sex                <chr> "F", "M", "F", "M", "M", "M", "F", "M", "F", "M", "…
## $ color              <chr> "G", "G", "G", "B", "B", "G", "G", "G", "G", "G", "…
## $ lat                <dbl> 57.03983, 57.03983, 57.03983, 57.03983, 57.03983, 5…
## $ long               <dbl> -157.8427, -157.8427, -157.8427, -157.8427, -157.84…
## $ habitat            <dbl> 254.08, 254.08, 254.08, 254.08, 254.08, 254.08, 254…
## $ human              <dbl> 10.42, 10.42, 10.42, 10.42, 10.42, 10.42, 10.42, 10…
## $ pop.density        <dbl> 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, …
## $ pack.size          <dbl> 8.78, 8.78, 8.78, 8.78, 8.78, 8.78, 8.78, 8.78, 8.7…
## $ standard.habitat   <dbl> -1.6339, -1.6339, -1.6339, -1.6339, -1.6339, -1.633…
## $ standard.human     <dbl> -0.9784, -0.9784, -0.9784, -0.9784, -0.9784, -0.978…
## $ standard.pop       <dbl> -0.6827, -0.6827, -0.6827, -0.6827, -0.6827, -0.682…
## $ standard.packsize  <dbl> 1.3157, 1.3157, 1.3157, 1.3157, 1.3157, 1.3157, 1.3…
## $ standard.latitude  <dbl> 0.7214, 0.7214, 0.7214, 0.7214, 0.7214, 0.7214, 0.7…
## $ standard.longitude <dbl> -2.1441, -2.1441, -2.1441, -2.1441, -2.1441, -2.144…
## $ cav.binary         <int> 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
## $ cdv.binary         <int> 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
## $ cpv.binary         <int> 0, 0, 1, 1, 0, 1, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, …
## $ chv.binary         <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, …
## $ neo.binary         <int> NA, NA, NA, 0, 0, NA, NA, 1, 0, 1, NA, 0, NA, NA, N…
## $ toxo.binary        <int> NA, NA, NA, 1, 0, NA, NA, 1, 0, 0, NA, 0, NA, NA, N…
```

6. How many distinct wolf populations are included in this study? Make a new object that restricts the data to the wolf populations in the lower 48 US states.

```r
n_distinct(Woolf$pop)
```

```
## [1] 17
```


```r
lower_fourty_eight <- Woolf %>% filter(pop %in% c("MI", "MT", "GTNP", "MEXICAN", "SNF", "YNP"))
```

7.Use the range of the latitude and longitude to build an appropriate bounding box for your map.



```r
latitude_two <- c(33.69, 50)
longitude_two <- c(-125.84, -78)
box_two <- make_bbox(longitude_two, latitude_two, f = 0.05)
```

8.  Load a map from `stamen` in a `terrain-lines` projection and display the map.

```r
stamen_map_two <- get_map(box_two, maptype = "terrain-lines", source = "stamen")
```

```
## Source : http://tile.stamen.com/terrain/5/5/10.png
```

```
## Source : http://tile.stamen.com/terrain/5/6/10.png
```

```
## Source : http://tile.stamen.com/terrain/5/7/10.png
```

```
## Source : http://tile.stamen.com/terrain/5/8/10.png
```

```
## Source : http://tile.stamen.com/terrain/5/9/10.png
```

```
## Source : http://tile.stamen.com/terrain/5/4/11.png
```

```
## Source : http://tile.stamen.com/terrain/5/5/11.png
```

```
## Source : http://tile.stamen.com/terrain/5/6/11.png
```

```
## Source : http://tile.stamen.com/terrain/5/7/11.png
```

```
## Source : http://tile.stamen.com/terrain/5/8/11.png
```

```
## Source : http://tile.stamen.com/terrain/5/9/11.png
```

```
## Source : http://tile.stamen.com/terrain/5/4/12.png
```

```
## Source : http://tile.stamen.com/terrain/5/5/12.png
```

```
## Source : http://tile.stamen.com/terrain/5/6/12.png
```

```
## Source : http://tile.stamen.com/terrain/5/7/12.png
```

```
## Source : http://tile.stamen.com/terrain/5/8/12.png
```

```
## Source : http://tile.stamen.com/terrain/5/9/12.png
```

```r
ggmap(stamen_map_two)
```

![](lab12_hw_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

9.  Build a final map that overlays the recorded observations of wolves in the lower 48 states.


```r
ggmap(stamen_map_two) + 
  geom_point(Woolf, mapping=aes(x=long, y=lat), size=2)+
  labs(x = "longitude", y = "latitude", title = "Woolf Observations in lower 48 US states")
```

```
## Warning: Removed 757 rows containing missing values (geom_point).
```

![](lab12_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->



10. Use the map from #9 above, but add some aesthetics. Try to `fill` and `color` by population.


```r
ggmap(stamen_map_two) + 
  geom_point(Woolf, mapping=aes(x=long, y=lat, fill=pop, color=pop), size=2)+
  labs(x = "longitude", y = "latitude", title = "Woolf Observations in lower 48 US states")
```

```
## Warning: Removed 757 rows containing missing values (geom_point).
```

![](lab12_hw_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
