---
title: "Lab 4 Homework"
author: "Flynn Atallah"
date: "2022-03-09"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Data
For the homework, we will use data about vertebrate home range sizes. The data are in the class folder, but the reference is below.  

**Database of vertebrate home range sizes.**  
Reference: Tamburello N, Cote IM, Dulvy NK (2015) Energy and the scaling of animal space use. The American Naturalist 186(2):196-211. http://dx.doi.org/10.1086/682070.  
Data: http://datadryad.org/resource/doi:10.5061/dryad.q5j65/1  

**1. Load the data into a new object called `homerange`.**

```r
homerange <- read.csv('~/Midtermbis15/Tamburelloetal_HomeRangeDatabase.csv')
```
**2. Explore the data. Show the dimensions, column names, classes for each variable, and a statistical summary. Keep these as separate code chunks.**  

```r
glimpse(homerange)
```

```
## Rows: 569
## Columns: 24
## $ taxon                      <chr> "lake fishes", "river fishes", "river fishe…
## $ common.name                <chr> "american eel", "blacktail redhorse", "cent…
## $ class                      <chr> "actinopterygii", "actinopterygii", "actino…
## $ order                      <chr> "anguilliformes", "cypriniformes", "cyprini…
## $ family                     <chr> "anguillidae", "catostomidae", "cyprinidae"…
## $ genus                      <chr> "anguilla", "moxostoma", "campostoma", "cli…
## $ species                    <chr> "rostrata", "poecilura", "anomalum", "fundu…
## $ primarymethod              <chr> "telemetry", "mark-recapture", "mark-recapt…
## $ N                          <chr> "16", NA, "20", "26", "17", "5", "2", "2", …
## $ mean.mass.g                <dbl> 887.00, 562.00, 34.00, 4.00, 4.00, 3525.00,…
## $ log10.mass                 <dbl> 2.9479236, 2.7497363, 1.5314789, 0.6020600,…
## $ alternative.mass.reference <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,…
## $ mean.hra.m2                <dbl> 282750.00, 282.10, 116.11, 125.50, 87.10, 3…
## $ log10.hra                  <dbl> 5.4514026, 2.4504031, 2.0648696, 2.0986437,…
## $ hra.reference              <chr> "Minns, C. K. 1995. Allometry of home range…
## $ realm                      <chr> "aquatic", "aquatic", "aquatic", "aquatic",…
## $ thermoregulation           <chr> "ectotherm", "ectotherm", "ectotherm", "ect…
## $ locomotion                 <chr> "swimming", "swimming", "swimming", "swimmi…
## $ trophic.guild              <chr> "carnivore", "carnivore", "carnivore", "car…
## $ dimension                  <chr> "3D", "2D", "2D", "2D", "2D", "2D", "2D", "…
## $ preymass                   <dbl> NA, NA, NA, NA, NA, NA, 1.39, NA, NA, NA, N…
## $ log10.preymass             <dbl> NA, NA, NA, NA, NA, NA, 0.1430148, NA, NA, …
## $ PPMR                       <dbl> NA, NA, NA, NA, NA, NA, 530, NA, NA, NA, NA…
## $ prey.size.reference        <chr> NA, NA, NA, NA, NA, NA, "Brose U, et al. 20…
```

```r
dim(homerange)
```

```
## [1] 569  24
```

```r
colnames(homerange)
```

```
##  [1] "taxon"                      "common.name"               
##  [3] "class"                      "order"                     
##  [5] "family"                     "genus"                     
##  [7] "species"                    "primarymethod"             
##  [9] "N"                          "mean.mass.g"               
## [11] "log10.mass"                 "alternative.mass.reference"
## [13] "mean.hra.m2"                "log10.hra"                 
## [15] "hra.reference"              "realm"                     
## [17] "thermoregulation"           "locomotion"                
## [19] "trophic.guild"              "dimension"                 
## [21] "preymass"                   "log10.preymass"            
## [23] "PPMR"                       "prey.size.reference"
```
**3. Change the class of the variables `taxon` and `order` to factors and display their levels.**  

```r
homerange$taxon <- as.factor(homerange$taxon)
homerange$order <- as.factor(homerange$order)
class(homerange$taxon)
```

```
## [1] "factor"
```

```r
class(homerange$order)
```

```
## [1] "factor"
```
**4. What taxa are represented in the `homerange` data frame? Make a new data frame `taxa` that is restricted to taxon, common name, class, order, family, genus, species.**  

```r
levels(homerange$taxon)
```

```
## [1] "birds"         "lake fishes"   "lizards"       "mammals"      
## [5] "marine fishes" "river fishes"  "snakes"        "tortoises"    
## [9] "turtles"
```

```r
new_taxa <- select(homerange, "taxon", "class", "common.name", "order", "genus", "family", "species")
```
**5. The variable `taxon` identifies the large, common name groups of the species represented in `homerange`. Make a table the shows the counts for each of these `taxon`.**  

```r
table(homerange$taxon)
```

```
## 
##         birds   lake fishes       lizards       mammals marine fishes 
##           140             9            11           238            90 
##  river fishes        snakes     tortoises       turtles 
##            14            41            12            14
```
**6. The species in `homerange` are also classified into trophic guilds. How many species are represented in each trophic guild.**  

```r
table(homerange$trophic.guild)
```

```
## 
## carnivore herbivore 
##       342       227
```

```r
#two trophic guilds, 342 in carnivores and 227 in herbivores
```
**7. Make two new data frames, one which is restricted to carnivores and another that is restricted to herbivores.**  

```r
meat_lovers <- filter(homerange, trophic.guild=="carnivore")
vegans <- filter(homerange, trophic.guild=="herbivore")
```
**8. Do herbivores or carnivores have, on average, a larger `mean.hra.m2`? Remove any NAs from the data.**  

```r
carnivore_mean_hra <- meat_lovers[ ,13]
mean(carnivore_mean_hra, na.rm=T)
```

```
## [1] 13039918
```

```r
herb_mean_hra <- vegans[ ,13]
mean(herb_mean_hra, na.rm=T)
```

```
## [1] 34137012
```

```r
#herbivories have higher mean hra
```
**9. Make a new dataframe `deer` that is limited to the mean mass, log10 mass, family, genus, and species of deer in the database. The family for deer is cervidae. Arrange the data in descending order by log10 mass. Which is the largest deer? What is its common name?**  

```r
deer <- select(homerange, "family", "genus", "species", "mean.mass.g", "log10.mass", "common.name")
deer <- filter(deer, family=="cervidae")
arrange(deer, desc(log10.mass))
```

```
##      family      genus     species mean.mass.g log10.mass       common.name
## 1  cervidae      alces       alces   307227.44    5.48746             moose
## 2  cervidae     cervus     elaphus   234757.78    5.37062          red deer
## 3  cervidae   rangifer    tarandus   102058.69    5.00885          reindeer
## 4  cervidae odocoileus virginianus    87884.04    4.94391 white-tailed deer
## 5  cervidae       dama        dama    71449.63    4.85400       fallow deer
## 6  cervidae       axis        axis    62823.19    4.79812            chital
## 7  cervidae odocoileus    hemionus    53864.17    4.73130         mule deer
## 8  cervidae ozotoceros bezoarticus    35000.16    4.54407       pampas deer
## 9  cervidae     cervus      nippon    29450.32    4.46909         sika deer
## 10 cervidae  capreolus   capreolus    24050.27    4.38112          roe deer
## 11 cervidae  muntiacus     reevesi    13499.88    4.13033  Reeves's muntjac
## 12 cervidae       pudu        puda     7499.98    3.87506              pudu
```

```r
#issa moose
```
**10. As measured by the data, which snake species has the smallest homerange? Show all of your work, please. Look this species up online and tell me about it!** **Snake is found in taxon column**    

```r
snek <- filter(homerange, taxon=="snakes") %>% select("common.name", "genus", "species", "mean.hra.m2")
arrange(snek, mean.hra.m2)
```

```
##                   common.name         genus                  species
## 1         namaqua dwarf adder         bitis               schneideri
## 2          eastern worm snake     carphopis                  viridis
## 3        butlers garter snake    thamnophis                  butleri
## 4          western worm snake     carphopis                   vermis
## 5             snubnosed viper        vipera                 latastei
## 6           chinese pit viper      gloydius              shedaoensis
## 7              ringneck snake     diadophis                punctatus
## 8                 cottonmouth   agkistrodon              piscivorous
## 9          redbacked ratsnake    oocatochus             rufodorsatus
## 10               gopher snake     pituophis                catenifer
## 11   twin-spotted rattlesnake      crotalus                   pricei
## 12          broadheaded snake hoplocephalus              bungaroides
## 13                 sidewinder      crotalus                 cerastes
## 14          tiger rattlesnake      crotalus                   tigris
## 15    blacktailed rattlesnake      crotalus                 molossus
## 16                tiger snake      notechis                 scutatus
## 17        Northern watersnake       nerodia                 sipeodon
## 18           western ratsnake        elaphe                 obsoleta
## 19        western diamondback      crotalus                    atrox
## 20               fer-de-lance      bothrops                    asper
## 21          Aesculapian snake       zamenis              longissimus
## 22                 blacksnake    pseudechis             porphyriacus
## 23                grass snake        natrix                   natrix
## 24         European whipsnake     hierophis             viridiflavus
## 25       yellow bellied racer       coluber constrictor flaviventris
## 26                 copperhead   agkistrodon               contortrix
## 27     copperbelly watersnake       nerodia            erythrogaster
## 28      great plains ratsnake        elaphe           guttata emoryi
## 29                      racer       coluber              constrictor
## 30 southwestern carpet python       morelia        spilota imbricata
## 31                  milksnake  lampropeltis               triangulum
## 32             Armenian viper   montivipera                   raddei
## 33         Mojave rattlesnake      crotalus               scutulatus
## 34        giant garter snakes    thamnophis                    gigal
## 35                  coachwhip   masticophis                flagellum
## 36          Eastern kingsnake  lampropeltis            getula getula
## 37              hognose snake     heterodon              platirhinos
## 38                 pine snake     pituophis             melanoleucus
## 39   midget faded rattlesnake      crotalus        oreganus concolor
## 40       eastern indigo snake    drymarchon                  couperi
## 41         timber rattlesnake      crotalus                 horridus
##    mean.hra.m2
## 1       200.00
## 2       253.00
## 3       600.00
## 4       700.00
## 5      2400.00
## 6      2613.69
## 7      6476.00
## 8     10655.00
## 9     15400.00
## 10    17400.00
## 11    22900.00
## 12    27379.00
## 13    28000.00
## 14    34800.00
## 15    34900.00
## 16    38800.00
## 17    40000.00
## 18    46000.00
## 19    54200.00
## 20    60700.00
## 21    77400.00
## 22    96000.00
## 23    99000.00
## 24   110900.00
## 25   114500.00
## 26   119288.89
## 27   131000.00
## 28   150600.00
## 29   151000.00
## 30   171600.00
## 31   240400.00
## 32   245928.57
## 33   316000.00
## 34   374800.00
## 35   429300.00
## 36   495000.00
## 37   516375.00
## 38   701000.00
## 39  1178000.00
## 40  1853000.00
## 41  2579600.00
```

```r
#namaqua dwarf snake-  Bitis schneideri is a species of venomous snake in the subfamily Viperinae of the family Viperidae. The species is native to a small coastal region that straddles the border between Namibia and South Africa. B. scneideri is the smallest species in the genus Bitis and possibly the world's smallest viper.
#very cool snek
```
## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
