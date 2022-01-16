---
title: "Lab 4 Homework"
author: "Please Add Your Name Here"
date: "2022-01-15"
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
getwd()
```

```
## [1] "C:/Users/sujun/Documents/GitHub Winter 2022/BIS15W2022_schung/lab4"
```

```r
homerange <- readr::read_csv("data/Tamburelloetal_HomeRangeDatabase.csv")
```

```
## Rows: 569 Columns: 24
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (16): taxon, common.name, class, order, family, genus, species, primarym...
## dbl  (8): mean.mass.g, log10.mass, mean.hra.m2, log10.hra, dimension, preyma...
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
library(janitor)
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```


```r
homerange
```

```
## # A tibble: 569 x 24
##    taxon  common.name   class   order   family genus species primarymethod N    
##    <chr>  <chr>         <chr>   <chr>   <chr>  <chr> <chr>   <chr>         <chr>
##  1 lake ~ american eel  actino~ anguil~ angui~ angu~ rostra~ telemetry     16   
##  2 river~ blacktail re~ actino~ cyprin~ catos~ moxo~ poecil~ mark-recaptu~ <NA> 
##  3 river~ central ston~ actino~ cyprin~ cypri~ camp~ anomal~ mark-recaptu~ 20   
##  4 river~ rosyside dace actino~ cyprin~ cypri~ clin~ fundul~ mark-recaptu~ 26   
##  5 river~ longnose dace actino~ cyprin~ cypri~ rhin~ catara~ mark-recaptu~ 17   
##  6 river~ muskellunge   actino~ esocif~ esoci~ esox  masqui~ telemetry     5    
##  7 marin~ pollack       actino~ gadifo~ gadid~ poll~ pollac~ telemetry     2    
##  8 marin~ saithe        actino~ gadifo~ gadid~ poll~ virens  telemetry     2    
##  9 marin~ lined surgeo~ actino~ percif~ acant~ acan~ lineat~ direct obser~ <NA> 
## 10 marin~ orangespine ~ actino~ percif~ acant~ naso  litura~ telemetry     8    
## # ... with 559 more rows, and 15 more variables: mean.mass.g <dbl>,
## #   log10.mass <dbl>, alternative.mass.reference <chr>, mean.hra.m2 <dbl>,
## #   log10.hra <dbl>, hra.reference <chr>, realm <chr>, thermoregulation <chr>,
## #   locomotion <chr>, trophic.guild <chr>, dimension <dbl>, preymass <dbl>,
## #   log10.preymass <dbl>, PPMR <dbl>, prey.size.reference <chr>
```

**2. Explore the data. Show the dimensions, column names, classes for each variable, and a statistical summary. Keep these as separate code chunks.**  

```r
dim(homerange)
```

```
## [1] 569  24
```

```r
new_homerange <- clean_names(homerange)
```

```r
names(new_homerange)
```

```
##  [1] "taxon"                      "common_name"               
##  [3] "class"                      "order"                     
##  [5] "family"                     "genus"                     
##  [7] "species"                    "primarymethod"             
##  [9] "n"                          "mean_mass_g"               
## [11] "log10_mass"                 "alternative_mass_reference"
## [13] "mean_hra_m2"                "log10_hra"                 
## [15] "hra_reference"              "realm"                     
## [17] "thermoregulation"           "locomotion"                
## [19] "trophic_guild"              "dimension"                 
## [21] "preymass"                   "log10_preymass"            
## [23] "ppmr"                       "prey_size_reference"
```

```r
str(new_homerange)
```

```
## spec_tbl_df [569 x 24] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ taxon                     : chr [1:569] "lake fishes" "river fishes" "river fishes" "river fishes" ...
##  $ common_name               : chr [1:569] "american eel" "blacktail redhorse" "central stoneroller" "rosyside dace" ...
##  $ class                     : chr [1:569] "actinopterygii" "actinopterygii" "actinopterygii" "actinopterygii" ...
##  $ order                     : chr [1:569] "anguilliformes" "cypriniformes" "cypriniformes" "cypriniformes" ...
##  $ family                    : chr [1:569] "anguillidae" "catostomidae" "cyprinidae" "cyprinidae" ...
##  $ genus                     : chr [1:569] "anguilla" "moxostoma" "campostoma" "clinostomus" ...
##  $ species                   : chr [1:569] "rostrata" "poecilura" "anomalum" "funduloides" ...
##  $ primarymethod             : chr [1:569] "telemetry" "mark-recapture" "mark-recapture" "mark-recapture" ...
##  $ n                         : chr [1:569] "16" NA "20" "26" ...
##  $ mean_mass_g               : num [1:569] 887 562 34 4 4 ...
##  $ log10_mass                : num [1:569] 2.948 2.75 1.531 0.602 0.602 ...
##  $ alternative_mass_reference: chr [1:569] NA NA NA NA ...
##  $ mean_hra_m2               : num [1:569] 282750 282.1 116.1 125.5 87.1 ...
##  $ log10_hra                 : num [1:569] 5.45 2.45 2.06 2.1 1.94 ...
##  $ hra_reference             : chr [1:569] "Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aqu"| __truncated__ "Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aqu"| __truncated__ "Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aqu"| __truncated__ "Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aqu"| __truncated__ ...
##  $ realm                     : chr [1:569] "aquatic" "aquatic" "aquatic" "aquatic" ...
##  $ thermoregulation          : chr [1:569] "ectotherm" "ectotherm" "ectotherm" "ectotherm" ...
##  $ locomotion                : chr [1:569] "swimming" "swimming" "swimming" "swimming" ...
##  $ trophic_guild             : chr [1:569] "carnivore" "carnivore" "carnivore" "carnivore" ...
##  $ dimension                 : num [1:569] 3 2 2 2 2 2 2 2 2 2 ...
##  $ preymass                  : num [1:569] NA NA NA NA NA NA 1.39 NA NA NA ...
##  $ log10_preymass            : num [1:569] NA NA NA NA NA ...
##  $ ppmr                      : num [1:569] NA NA NA NA NA NA 530 NA NA NA ...
##  $ prey_size_reference       : chr [1:569] NA NA NA NA ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   taxon = col_character(),
##   ..   common.name = col_character(),
##   ..   class = col_character(),
##   ..   order = col_character(),
##   ..   family = col_character(),
##   ..   genus = col_character(),
##   ..   species = col_character(),
##   ..   primarymethod = col_character(),
##   ..   N = col_character(),
##   ..   mean.mass.g = col_double(),
##   ..   log10.mass = col_double(),
##   ..   alternative.mass.reference = col_character(),
##   ..   mean.hra.m2 = col_double(),
##   ..   log10.hra = col_double(),
##   ..   hra.reference = col_character(),
##   ..   realm = col_character(),
##   ..   thermoregulation = col_character(),
##   ..   locomotion = col_character(),
##   ..   trophic.guild = col_character(),
##   ..   dimension = col_double(),
##   ..   preymass = col_double(),
##   ..   log10.preymass = col_double(),
##   ..   PPMR = col_double(),
##   ..   prey.size.reference = col_character()
##   .. )
##  - attr(*, "problems")=<externalptr>
```


```r
summary(new_homerange)
```

```
##     taxon           common_name           class              order          
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     family             genus             species          primarymethod     
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##       n              mean_mass_g        log10_mass     
##  Length:569         Min.   :      0   Min.   :-0.6576  
##  Class :character   1st Qu.:     50   1st Qu.: 1.6990  
##  Mode  :character   Median :    330   Median : 2.5185  
##                     Mean   :  34602   Mean   : 2.5947  
##                     3rd Qu.:   2150   3rd Qu.: 3.3324  
##                     Max.   :4000000   Max.   : 6.6021  
##                                                        
##  alternative_mass_reference  mean_hra_m2          log10_hra     
##  Length:569                 Min.   :0.000e+00   Min.   :-1.523  
##  Class :character           1st Qu.:4.500e+03   1st Qu.: 3.653  
##  Mode  :character           Median :3.934e+04   Median : 4.595  
##                             Mean   :2.146e+07   Mean   : 4.709  
##                             3rd Qu.:1.038e+06   3rd Qu.: 6.016  
##                             Max.   :3.551e+09   Max.   : 9.550  
##                                                                 
##  hra_reference         realm           thermoregulation    locomotion       
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  trophic_guild        dimension        preymass         log10_preymass   
##  Length:569         Min.   :2.000   Min.   :     0.67   Min.   :-0.1739  
##  Class :character   1st Qu.:2.000   1st Qu.:    20.02   1st Qu.: 1.3014  
##  Mode  :character   Median :2.000   Median :    53.75   Median : 1.7304  
##                     Mean   :2.218   Mean   :  3989.88   Mean   : 2.0188  
##                     3rd Qu.:2.000   3rd Qu.:   363.35   3rd Qu.: 2.5603  
##                     Max.   :3.000   Max.   :130233.20   Max.   : 5.1147  
##                                     NA's   :502         NA's   :502      
##       ppmr         prey_size_reference
##  Min.   :  0.380   Length:569         
##  1st Qu.:  3.315   Class :character   
##  Median :  7.190   Mode  :character   
##  Mean   : 31.752                      
##  3rd Qu.: 15.966                      
##  Max.   :530.000                      
##  NA's   :502
```

**3. Change the class of the variables `taxon` and `order` to factors and display their levels.**  

```r
new_homerange$taxon <- as.factor(new_homerange$taxon)
class(new_homerange$taxon)
```

```
## [1] "factor"
```

```r
levels(new_homerange$taxon)
```

```
## [1] "birds"         "lake fishes"   "lizards"       "mammals"      
## [5] "marine fishes" "river fishes"  "snakes"        "tortoises"    
## [9] "turtles"
```

```r
new_homerange$order <- as.factor(new_homerange$order)
class(new_homerange$order)
```

```
## [1] "factor"
```

```r
levels(new_homerange$order)
```

```
##  [1] "accipitriformes"    "afrosoricida"       "anguilliformes"    
##  [4] "anseriformes"       "apterygiformes"     "artiodactyla"      
##  [7] "caprimulgiformes"   "carnivora"          "charadriiformes"   
## [10] "columbidormes"      "columbiformes"      "coraciiformes"     
## [13] "cuculiformes"       "cypriniformes"      "dasyuromorpha"     
## [16] "dasyuromorpia"      "didelphimorphia"    "diprodontia"       
## [19] "diprotodontia"      "erinaceomorpha"     "esociformes"       
## [22] "falconiformes"      "gadiformes"         "galliformes"       
## [25] "gruiformes"         "lagomorpha"         "macroscelidea"     
## [28] "monotrematae"       "passeriformes"      "pelecaniformes"    
## [31] "peramelemorphia"    "perciformes"        "perissodactyla"    
## [34] "piciformes"         "pilosa"             "proboscidea"       
## [37] "psittaciformes"     "rheiformes"         "roden"             
## [40] "rodentia"           "salmoniformes"      "scorpaeniformes"   
## [43] "siluriformes"       "soricomorpha"       "squamata"          
## [46] "strigiformes"       "struthioniformes"   "syngnathiformes"   
## [49] "testudines"         "tetraodontiformes<U+00A0>" "tinamiformes"
```

**4. What taxa are represented in the `homerange` data frame? Make a new data frame `taxa` that is restricted to taxon, common name, class, order, family, genus, species.**  

```r
taxa<- select(new_homerange, "taxon","common_name","class","order","family","genus","species")
taxa
```

```
## # A tibble: 569 x 7
##    taxon         common_name             class   order   family   genus  species
##    <fct>         <chr>                   <chr>   <fct>   <chr>    <chr>  <chr>  
##  1 lake fishes   american eel            actino~ anguil~ anguill~ angui~ rostra~
##  2 river fishes  blacktail redhorse      actino~ cyprin~ catosto~ moxos~ poecil~
##  3 river fishes  central stoneroller     actino~ cyprin~ cyprini~ campo~ anomal~
##  4 river fishes  rosyside dace           actino~ cyprin~ cyprini~ clino~ fundul~
##  5 river fishes  longnose dace           actino~ cyprin~ cyprini~ rhini~ catara~
##  6 river fishes  muskellunge             actino~ esocif~ esocidae esox   masqui~
##  7 marine fishes pollack                 actino~ gadifo~ gadidae  polla~ pollac~
##  8 marine fishes saithe                  actino~ gadifo~ gadidae  polla~ virens 
##  9 marine fishes lined surgeonfish       actino~ percif~ acanthu~ acant~ lineat~
## 10 marine fishes orangespine unicornfish actino~ percif~ acanthu~ naso   litura~
## # ... with 559 more rows
```

**5. The variable `taxon` identifies the large, common name groups of the species represented in `homerange`. Make a table the shows the counts for each of these `taxon`.**  

```r
count(taxa, c(taxon))
```

```
## # A tibble: 9 x 2
##   `c(taxon)`        n
##   <fct>         <int>
## 1 birds           140
## 2 lake fishes       9
## 3 lizards          11
## 4 mammals         238
## 5 marine fishes    90
## 6 river fishes     14
## 7 snakes           41
## 8 tortoises        12
## 9 turtles          14
```

**6. The species in `homerange` are also classified into trophic guilds. How many species are represented in each trophic guild.**  

```r
count(new_homerange, c(trophic_guild))
```

```
## # A tibble: 2 x 2
##   `c(trophic_guild)`     n
##   <chr>              <int>
## 1 carnivore            342
## 2 herbivore            227
```

**7. Make two new data frames, one which is restricted to carnivores and another that is restricted to herbivores.**  

```r
carnivore <- filter(new_homerange, trophic_guild=="carnivore")
carnivore
```

```
## # A tibble: 342 x 24
##    taxon   common_name   class   order  family genus species primarymethod n    
##    <fct>   <chr>         <chr>   <fct>  <chr>  <chr> <chr>   <chr>         <chr>
##  1 lake f~ american eel  actino~ angui~ angui~ angu~ rostra~ telemetry     16   
##  2 river ~ blacktail re~ actino~ cypri~ catos~ moxo~ poecil~ mark-recaptu~ <NA> 
##  3 river ~ central ston~ actino~ cypri~ cypri~ camp~ anomal~ mark-recaptu~ 20   
##  4 river ~ rosyside dace actino~ cypri~ cypri~ clin~ fundul~ mark-recaptu~ 26   
##  5 river ~ longnose dace actino~ cypri~ cypri~ rhin~ catara~ mark-recaptu~ 17   
##  6 river ~ muskellunge   actino~ esoci~ esoci~ esox  masqui~ telemetry     5    
##  7 marine~ pollack       actino~ gadif~ gadid~ poll~ pollac~ telemetry     2    
##  8 marine~ saithe        actino~ gadif~ gadid~ poll~ virens  telemetry     2    
##  9 marine~ giant treval~ actino~ perci~ caran~ cara~ ignobi~ telemetry     4    
## 10 lake f~ rock bass     actino~ perci~ centr~ ambl~ rupest~ mark-recaptu~ 16   
## # ... with 332 more rows, and 15 more variables: mean_mass_g <dbl>,
## #   log10_mass <dbl>, alternative_mass_reference <chr>, mean_hra_m2 <dbl>,
## #   log10_hra <dbl>, hra_reference <chr>, realm <chr>, thermoregulation <chr>,
## #   locomotion <chr>, trophic_guild <chr>, dimension <dbl>, preymass <dbl>,
## #   log10_preymass <dbl>, ppmr <dbl>, prey_size_reference <chr>
```

```r
herbivore <- filter(new_homerange, trophic_guild=="herbivore")
herbivore
```

```
## # A tibble: 227 x 24
##    taxon  common_name   class   order  family  genus species primarymethod n    
##    <fct>  <chr>         <chr>   <fct>  <chr>   <chr> <chr>   <chr>         <chr>
##  1 marin~ lined surgeo~ actino~ perci~ acanth~ acan~ lineat~ direct obser~ <NA> 
##  2 marin~ orangespine ~ actino~ perci~ acanth~ naso  litura~ telemetry     8    
##  3 marin~ bluespine un~ actino~ perci~ acanth~ naso  unicor~ telemetry     7    
##  4 marin~ redlip blenny actino~ perci~ blenni~ ophi~ atlant~ direct obser~ 20   
##  5 marin~ bermuda chub  actino~ perci~ kyphos~ kyph~ sectat~ telemetry     11   
##  6 marin~ cherubfish    actino~ perci~ pomaca~ cent~ argi    direct obser~ <NA> 
##  7 marin~ damselfish    actino~ perci~ pomace~ chro~ chromis direct obser~ <NA> 
##  8 marin~ twinspot dam~ actino~ perci~ pomace~ chry~ biocel~ direct obser~ 18   
##  9 marin~ wards damsel  actino~ perci~ pomace~ poma~ wardi   direct obser~ <NA> 
## 10 marin~ australian g~ actino~ perci~ pomace~ steg~ apical~ direct obser~ <NA> 
## # ... with 217 more rows, and 15 more variables: mean_mass_g <dbl>,
## #   log10_mass <dbl>, alternative_mass_reference <chr>, mean_hra_m2 <dbl>,
## #   log10_hra <dbl>, hra_reference <chr>, realm <chr>, thermoregulation <chr>,
## #   locomotion <chr>, trophic_guild <chr>, dimension <dbl>, preymass <dbl>,
## #   log10_preymass <dbl>, ppmr <dbl>, prey_size_reference <chr>
```

**8. Do herbivores or carnivores have, on average, a larger `mean.hra.m2`? Remove any NAs from the data.**  

```r
mean(herbivore$mean_hra_m2, na.rm=T)
```

```
## [1] 34137012
```

```r
mean(carnivore$mean_hra_m2, na.rm=T)
```

```
## [1] 13039918
```

**9. Make a new dataframe `deer` that is limited to the mean mass, log10 mass, family, genus, and species of deer in the database. The family for deer is cervidae. Arrange the data in descending order by log10 mass. Which is the largest deer? What is its common name?**  

```r
deer<- select(new_homerange,"mean_mass_g","log10_mass","family","genus","common_name")
cervidae <- filter(deer, family == "cervidae")
cervidae[order(cervidae$log10_mass),]
```

```
## # A tibble: 12 x 5
##    mean_mass_g log10_mass family   genus      common_name      
##          <dbl>      <dbl> <chr>    <chr>      <chr>            
##  1       7500.       3.88 cervidae pudu       pudu             
##  2      13500.       4.13 cervidae muntiacus  Reeves's muntjac 
##  3      24050.       4.38 cervidae capreolus  roe deer         
##  4      29450.       4.47 cervidae cervus     sika deer        
##  5      35000.       4.54 cervidae ozotoceros pampas deer      
##  6      53864.       4.73 cervidae odocoileus mule deer        
##  7      62823.       4.80 cervidae axis       chital           
##  8      71450.       4.85 cervidae dama       fallow deer      
##  9      87884.       4.94 cervidae odocoileus white-tailed deer
## 10     102059.       5.01 cervidae rangifer   reindeer         
## 11     234758.       5.37 cervidae cervus     red deer         
## 12     307227.       5.49 cervidae alces      moose
```

```r
cervidae$common_name[which.max(cervidae$log10_mass)]
```

```
## [1] "moose"
```

**10. As measured by the data, which snake species has the smallest homerange? Show all of your work, please. Look this species up online and tell me about it!** **Snake is found in taxon column**    

```r
small <- select(new_homerange, "taxon","mean_hra_m2", "common_name")
snake <- filter(small, taxon =="snakes")
snake[order(snake$mean_hra_m2),]
```

```
## # A tibble: 41 x 3
##    taxon  mean_hra_m2 common_name         
##    <fct>        <dbl> <chr>               
##  1 snakes        200  namaqua dwarf adder 
##  2 snakes        253  eastern worm snake  
##  3 snakes        600  butlers garter snake
##  4 snakes        700  western worm snake  
##  5 snakes       2400  snubnosed viper     
##  6 snakes       2614. chinese pit viper   
##  7 snakes       6476  ringneck snake      
##  8 snakes      10655  cottonmouth         
##  9 snakes      15400  redbacked ratsnake  
## 10 snakes      17400  gopher snake        
## # ... with 31 more rows
```

```r
snake$common_name[which.max(snake$mean_hra_m2)]
```

```
## [1] "timber rattlesnake"
```
The timber rattlesnake is found primarily on the eastern half of the US.  They feed on small birds, frogs, small snakes, and other small animals. It is one of the most dangerous snakes in North America due to their venom.  Before striking, they will rattle their tail.  They can be found in a diverse range of habitats from mountainous forests to river floodplains and agricultural fields.  They are usually found coiled up, waiting for prey to come to them before striking. 

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
