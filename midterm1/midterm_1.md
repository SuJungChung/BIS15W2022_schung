---
title: "Midterm 1"
author: "Sujung Chung"
date: "2022-01-25"
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
## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --
```

```
## v ggplot2 3.3.5     v purrr   0.3.4
## v tibble  3.1.6     v dplyr   1.0.7
## v tidyr   1.1.4     v stringr 1.4.0
## v readr   2.1.1     v forcats 0.5.1
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
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
library(skimr)
```

## Questions  
Wikipedia's definition of [data science](https://en.wikipedia.org/wiki/Data_science): "Data science is an interdisciplinary field that uses scientific methods, processes, algorithms and systems to extract knowledge and insights from noisy, structured and unstructured data, and apply knowledge and actionable insights from data across a broad range of application domains."  

1. (2 points) Consider the definition of data science above. Although we are only part-way through the quarter, what specific elements of data science do you feel we have practiced? Provide at least one specific example.  

We have practiced extracting knowledge and insights from data.  For example, when we use the summary function, it can tell us the dimensions of the data, the type of variables present, and how many variables are present.  This is one way that can help us organize large sets of data to understand what type of information we can extract from it.


2. (2 points) What is the most helpful or interesting thing you have learned so far in BIS 15L? What is something that you think needs more work or practice?  I think the most helpful thing we have learned are pipes.  It reduces the number of lines of code we have to write and helps me organize my thought process.  I think something I'd like to work towards is being able to recall functions without having to look back.  I will remember how to start a function, but then I have to look back to get the finer details like punctuation.

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.

```r
elephants <- readr::read_csv("data/ElephantsMF.csv")
```

```
## Rows: 288 Columns: 3
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (1): Sex
## dbl (2): Age, Height
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.

```r
#changed to lower case
elephants <- clean_names(elephants)

#changed 'sex' variable to factor
elephants$sex <- as.factor(elephants$sex)
class(elephants$sex)
```

```
## [1] "factor"
```

```r
elephants
```

```
## # A tibble: 288 x 3
##      age height sex  
##    <dbl>  <dbl> <fct>
##  1   1.4   120  M    
##  2  17.5   227  M    
##  3  12.8   235  M    
##  4  11.2   210  M    
##  5  12.7   220  M    
##  6  12.7   189  M    
##  7  12.2   225  M    
##  8  12.2   204  M    
##  9  28.2   266. M    
## 10  11.7   233  M    
## # ... with 278 more rows
```

5. (2 points) How many male and female elephants are represented in the data?

```r
elephants%>%
  count(sex)
```

```
## # A tibble: 2 x 2
##   sex       n
##   <fct> <int>
## 1 F       150
## 2 M       138
```

6. (2 points) What is the average age all elephants in the data?

```r
mean(elephants$age)
```

```
## [1] 10.97132
```

7. (2 points) How does the average age and height of elephants compare by sex?

```r
elephants%>%
  group_by(sex)%>%
  summarize(average_age = mean(age), average_height = mean(height))
```

```
## # A tibble: 2 x 3
##   sex   average_age average_height
##   <fct>       <dbl>          <dbl>
## 1 F           12.8            190.
## 2 M            8.95           185.
```
On average, females are older and taller than males.

8. (2 points) How does the average height of elephants compare by sex for individuals over 20 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.  

```r
elephants%>%
  group_by(sex)%>%
  filter(age > 20)%>%
  summarize(min_height = min(height),
            max_height = max(height),
            mean_height = mean(height),
            n=n())
```

```
## # A tibble: 2 x 5
##   sex   min_height max_height mean_height     n
##   <fct>      <dbl>      <dbl>       <dbl> <int>
## 1 F           193.       278.        232.    37
## 2 M           229.       304.        270.    13
```
On average, male elephants are taller than female elephants over the gae of 20 years old.

For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.

```r
vertebrate <- readr::read_csv("data/IvindoData_DryadVersion.csv")
```

```
## Rows: 24 Columns: 26
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr  (2): HuntCat, LandUse
## dbl (24): TransectID, Distance, NumHouseholds, Veg_Rich, Veg_Stems, Veg_lian...
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
vertebrate <- clean_names(vertebrate)
```

```r
skim(vertebrate)
```


Table: Data summary

|                         |           |
|:------------------------|:----------|
|Name                     |vertebrate |
|Number of rows           |24         |
|Number of columns        |26         |
|_______________________  |           |
|Column type frequency:   |           |
|character                |2          |
|numeric                  |24         |
|________________________ |           |
|Group variables          |None       |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|hunt_cat      |         0|             1|   4|   8|     0|        3|          0|
|land_use      |         0|             1|   4|   7|     0|        3|          0|


**Variable type: numeric**

|skim_variable            | n_missing| complete_rate|  mean|    sd|    p0|   p25|   p50|   p75|  p100|hist                                     |
|:------------------------|---------:|-------------:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|:----------------------------------------|
|transect_id              |         0|             1| 13.50|  8.51|  1.00|  5.75| 14.50| 20.25| 27.00|▇▃▅▆▆ |
|distance                 |         0|             1| 11.88|  7.28|  2.70|  5.67|  9.72| 17.68| 26.76|▇▂▂▅▂ |
|num_households           |         0|             1| 37.88| 17.80| 13.00| 24.75| 29.00| 54.00| 73.00|▇▇▂▇▂ |
|veg_rich                 |         0|             1| 14.83|  2.07| 10.88| 13.10| 14.94| 16.54| 18.75|▃▂▃▇▁ |
|veg_stems                |         0|             1| 32.80|  5.96| 23.44| 28.69| 32.44| 37.08| 47.56|▆▇▆▃▁ |
|veg_liana                |         0|             1| 11.04|  3.29|  4.75|  9.03| 11.94| 13.25| 16.38|▃▂▃▇▃ |
|veg_dbh                  |         0|             1| 46.09| 10.67| 28.45| 40.65| 43.90| 50.57| 76.48|▂▇▃▁▁ |
|veg_canopy               |         0|             1|  3.47|  0.35|  2.50|  3.25|  3.43|  3.75|  4.00|▁▁▇▅▇ |
|veg_understory           |         0|             1|  3.02|  0.34|  2.38|  2.88|  3.00|  3.17|  3.88|▂▆▇▂▁ |
|ra_apes                  |         0|             1|  2.04|  3.03|  0.00|  0.00|  0.48|  3.82| 12.93|▇▂▁▁▁ |
|ra_birds                 |         0|             1| 58.64| 14.71| 31.56| 52.51| 57.89| 68.18| 85.03|▅▅▇▇▃ |
|ra_elephant              |         0|             1|  0.54|  0.67|  0.00|  0.00|  0.36|  0.89|  2.30|▇▂▂▁▁ |
|ra_monkeys               |         0|             1| 31.30| 12.38|  5.84| 22.70| 31.74| 39.88| 54.12|▂▅▃▇▂ |
|ra_rodent                |         0|             1|  3.28|  1.47|  1.06|  2.05|  3.23|  4.09|  6.31|▇▅▇▃▃ |
|ra_ungulate              |         0|             1|  4.17|  4.31|  0.00|  1.23|  2.54|  5.16| 13.86|▇▂▁▁▂ |
|rich_all_species         |         0|             1| 20.21|  2.06| 15.00| 19.00| 20.00| 22.00| 24.00|▁▁▇▅▁ |
|evenness_all_species     |         0|             1|  0.77|  0.05|  0.67|  0.75|  0.78|  0.81|  0.83|▃▁▅▇▇ |
|diversity_all_species    |         0|             1|  2.31|  0.15|  1.97|  2.25|  2.32|  2.43|  2.57|▂▃▇▆▅ |
|rich_bird_species        |         0|             1| 10.33|  1.24|  8.00| 10.00| 11.00| 11.00| 13.00|▃▅▇▁▁ |
|evenness_bird_species    |         0|             1|  0.71|  0.08|  0.56|  0.68|  0.72|  0.77|  0.82|▅▁▇▆▇ |
|diversity_bird_species   |         0|             1|  1.66|  0.20|  1.16|  1.60|  1.68|  1.78|  2.01|▂▂▅▇▃ |
|rich_mammal_species      |         0|             1|  9.88|  1.68|  6.00|  9.00| 10.00| 11.00| 12.00|▂▂▃▅▇ |
|evenness_mammal_species  |         0|             1|  0.75|  0.06|  0.62|  0.71|  0.74|  0.78|  0.86|▂▃▇▂▅ |
|diversity_mammal_species |         0|             1|  1.70|  0.17|  1.38|  1.57|  1.70|  1.81|  2.06|▅▇▇▇▃ |

```r
vertebrate$hunt_cat <- as.factor(vertebrate$hunt_cat)
vertebrate$land_use <- as.factor(vertebrate$land_use)
```


10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?

```r
vertebrate%>%
  filter(hunt_cat == "High" | hunt_cat == "Moderate")%>%
  summarize(avg_diversity_bird_species = mean(diversity_bird_species),
            avg_diversity_mammal_species = mean(diversity_mammal_species))
```

```
## # A tibble: 1 x 2
##   avg_diversity_bird_species avg_diversity_mammal_species
##                        <dbl>                        <dbl>
## 1                       1.64                         1.71
```
On average, mammals species have higher diversity than bird species.


11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 3km from a village to sites that are greater than 25km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.  

```r
close <- vertebrate%>%
  select(distance, ra_apes, ra_birds, ra_elephant, ra_monkeys, ra_rodent, ra_ungulate)%>%
  filter(distance < 3)%>%
  summarize(mean(distance), 
            across(contains("ra"), mean, na.rm = T))
  

far <- vertebrate%>%
  select(distance, ra_apes, ra_birds, ra_elephant, ra_monkeys, ra_rodent, ra_ungulate)%>%
  filter(distance > 25)

close
```

```
## # A tibble: 1 x 7
##   `mean(distance)` ra_apes ra_birds ra_elephant ra_monkeys ra_rodent ra_ungulate
##              <dbl>   <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1             2.81    0.12     76.6       0.145       17.3      3.90        1.87
```

```r
far
```

```
## # A tibble: 1 x 7
##   distance ra_apes ra_birds ra_elephant ra_monkeys ra_rodent ra_ungulate
##      <dbl>   <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1     26.8    4.91     31.6           0       54.1      1.29        8.12
```
For apes, a farther distance from the village showed higher diversity.  For birds, diversity increased the closer to the village.  For elephants, diversity was higher near the village.  For monkeys, diversity was higher farther from the village. Rodent diversity was higher near the village.  Ungulates had higher diversity away from the village. 

12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`

```r
levels(vertebrate$land_use)
```

```
## [1] "Logging" "Neither" "Park"
```

```r
vertebrate%>%
  select(land_use, diversity_all_species)%>%
  group_by(land_use)%>%
  summarize(mean(diversity_all_species))
```

```
## # A tibble: 3 x 2
##   land_use `mean(diversity_all_species)`
##   <fct>                            <dbl>
## 1 Logging                           2.23
## 2 Neither                           2.36
## 3 Park                              2.43
```

How does land use affect the overall diversity of species?
Surprisingly, overall species diversity is not significantly affected by land use.  Logging has the lowest average diversity while parks have the highest average diversity.  
