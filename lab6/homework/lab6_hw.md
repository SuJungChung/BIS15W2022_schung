---
title: "Lab 6 Homework"
author: "Joel Ledford"
date: "2022-01-23"
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

For this assignment we are going to work with a large data set from the [United Nations Food and Agriculture Organization](http://www.fao.org/about/en/) on world fisheries. These data are pretty wild, so we need to do some cleaning. First, load the data.  

Load the data `FAO_1950to2012_111914.csv` as a new object titled `fisheries`.

```r
fisheries <- readr::read_csv(file = "data/FAO_1950to2012_111914.csv")
```

```
## Rows: 17692 Columns: 71
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (69): Country, Common name, ISSCAAP taxonomic group, ASFIS species#, ASF...
## dbl  (2): ISSCAAP group#, FAO major fishing area
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. Do an exploratory analysis of the data (your choice). What are the names of the variables, what are the dimensions, are there any NA's, what are the classes of the variables?  

```r
skim(fisheries)
```


Table: Data summary

|                         |          |
|:------------------------|:---------|
|Name                     |fisheries |
|Number of rows           |17692     |
|Number of columns        |71        |
|_______________________  |          |
|Column type frequency:   |          |
|character                |69        |
|numeric                  |2         |
|________________________ |          |
|Group variables          |None      |


**Variable type: character**

|skim_variable           | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-----------------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|Country                 |         0|          1.00|   4|  25|     0|      204|          0|
|Common name             |         0|          1.00|   3|  30|     0|     1553|          0|
|ISSCAAP taxonomic group |         0|          1.00|   5|  36|     0|       30|          0|
|ASFIS species#          |         0|          1.00|  10|  13|     0|     1553|          0|
|ASFIS species name      |         0|          1.00|   6|  32|     0|     1548|          0|
|Measure                 |         0|          1.00|  17|  17|     0|        1|          0|
|1950                    |     15561|          0.12|   1|   6|     0|      513|          0|
|1951                    |     15550|          0.12|   1|   7|     0|      536|          0|
|1952                    |     15501|          0.12|   1|   7|     0|      553|          0|
|1953                    |     15439|          0.13|   1|   6|     0|      570|          0|
|1954                    |     15417|          0.13|   1|   7|     0|      588|          0|
|1955                    |     15382|          0.13|   1|   7|     0|      589|          0|
|1956                    |     15331|          0.13|   1|   7|     0|      633|          0|
|1957                    |     15253|          0.14|   1|   7|     0|      627|          0|
|1958                    |     15138|          0.14|   1|   6|     0|      643|          0|
|1959                    |     15110|          0.15|   1|   7|     0|      641|          0|
|1960                    |     15016|          0.15|   1|   7|     0|      673|          0|
|1961                    |     14922|          0.16|   1|   8|     0|      713|          0|
|1962                    |     14801|          0.16|   1|   8|     0|      729|          0|
|1963                    |     14707|          0.17|   1|   8|     0|      760|          0|
|1964                    |     14349|          0.19|   1|   8|     0|      759|          0|
|1965                    |     14241|          0.20|   1|   8|     0|      798|          0|
|1966                    |     14187|          0.20|   1|   8|     0|      801|          0|
|1967                    |     14047|          0.21|   1|   8|     0|      815|          0|
|1968                    |     13963|          0.21|   1|   8|     0|      829|          0|
|1969                    |     13920|          0.21|   1|   8|     0|      853|          0|
|1970                    |     13113|          0.26|   1|   8|     0|     1225|          0|
|1971                    |     12925|          0.27|   1|   8|     0|     1326|          0|
|1972                    |     12749|          0.28|   1|   8|     0|     1372|          0|
|1973                    |     12673|          0.28|   1|   8|     0|     1432|          0|
|1974                    |     12583|          0.29|   1|   8|     0|     2601|          0|
|1975                    |     12333|          0.30|   1|   8|     0|     2767|          0|
|1976                    |     12177|          0.31|   1|   8|     0|     2804|          0|
|1977                    |     12014|          0.32|   1|   8|     0|     2867|          0|
|1978                    |     11847|          0.33|   1|   8|     0|     2901|          0|
|1979                    |     11820|          0.33|   1|   8|     0|     2932|          0|
|1980                    |     11747|          0.34|   1|   8|     0|     2956|          0|
|1981                    |     11713|          0.34|   1|   8|     0|     2996|          0|
|1982                    |     11558|          0.35|   1|   9|     0|     3030|          0|
|1983                    |     11453|          0.35|   1|   8|     0|     3031|          0|
|1984                    |     11309|          0.36|   1|   8|     0|     3076|          0|
|1985                    |     11212|          0.37|   1|   8|     0|     3161|          0|
|1986                    |     11086|          0.37|   1|   8|     0|     3242|          0|
|1987                    |     10930|          0.38|   1|   8|     0|     3255|          0|
|1988                    |     10493|          0.41|   1|   8|     0|     3435|          0|
|1989                    |     10435|          0.41|   1|   8|     0|     3425|          0|
|1990                    |     10293|          0.42|   1|   8|     0|     3446|          0|
|1991                    |     10364|          0.41|   1|   8|     0|     3420|          0|
|1992                    |     10435|          0.41|   1|   8|     0|     3322|          0|
|1993                    |     10522|          0.41|   1|   8|     0|     3313|          0|
|1994                    |     10400|          0.41|   1|   8|     0|     3313|          0|
|1995                    |     10148|          0.43|   1|   8|     0|     3329|          0|
|1996                    |      9990|          0.44|   1|   7|     0|     3358|          0|
|1997                    |      9773|          0.45|   1|   9|     0|     3393|          0|
|1998                    |      9579|          0.46|   1|   9|     0|     3399|          0|
|1999                    |      9265|          0.48|   1|   9|     0|     3428|          0|
|2000                    |      8899|          0.50|   1|   9|     0|     3481|          0|
|2001                    |      8646|          0.51|   1|   9|     0|     3490|          0|
|2002                    |      8590|          0.51|   1|   9|     0|     3507|          0|
|2003                    |      8383|          0.53|   1|   9|     0|     3482|          0|
|2004                    |      7977|          0.55|   1|   9|     0|     3505|          0|
|2005                    |      7822|          0.56|   1|   9|     0|     3532|          0|
|2006                    |      7699|          0.56|   1|   9|     0|     3565|          0|
|2007                    |      7589|          0.57|   1|   8|     0|     3551|          0|
|2008                    |      7667|          0.57|   1|   8|     0|     3537|          0|
|2009                    |      7573|          0.57|   1|   8|     0|     3572|          0|
|2010                    |      7499|          0.58|   1|   8|     0|     3590|          0|
|2011                    |      7371|          0.58|   1|   8|     0|     3618|          0|
|2012                    |      7336|          0.59|   1|   8|     0|     3609|          0|


**Variable type: numeric**

|skim_variable          | n_missing| complete_rate|  mean|    sd| p0| p25| p50| p75| p100|hist                                     |
|:----------------------|---------:|-------------:|-----:|-----:|--:|---:|---:|---:|----:|:----------------------------------------|
|ISSCAAP group#         |         0|             1| 37.38|  7.88| 11|  33|  36|  38|   77|▁▇▂▁▁ |
|FAO major fishing area |         0|             1| 45.34| 18.33| 18|  31|  37|  57|   88|▇▇▆▃▃ |
There are 71 columns and 17692 rows. The variables are common names, country name, ISSCAAP taxonomic group, ASFIS species#, ASFIS species name, Measure, the dates 1950-2012, ISSCAAP group #, FAO major fishing area. NAs are present for the date variables. All the variables are characters except for ISSCAAP group # and FAO major fishing area which are numeric variables. 

2. Use `janitor` to rename the columns and make them easier to use. As part of this cleaning step, change `country`, `isscaap_group_number`, `asfis_species_number`, and `fao_major_fishing_area` to data class factor. 

```r
fisheries <- clean_names(fisheries)
fisheries$country <- as.factor(fisheries$country)
fisheries$isscaap_group_number <- as.factor(fisheries$isscaap_group_number)
fisheries$asfis_species_name <- as.factor(fisheries$asfis_species_name)
fisheries$fao_major_fishing_area <- as.factor(fisheries$fao_major_fishing_area)
```

We need to deal with the years because they are being treated as characters and start with an X. We also have the problem that the column names that are years actually represent data. We haven't discussed tidy data yet, so here is some help. You should run this ugly chunk to transform the data for the rest of the homework. It will only work if you have used janitor to rename the variables in question 2!

```r
fisheries_tidy <- fisheries %>% 
  pivot_longer(-c(country,common_name,isscaap_group_number,isscaap_taxonomic_group,asfis_species_number,asfis_species_name,fao_major_fishing_area,measure),
               names_to = "year",
               values_to = "catch",
               values_drop_na = TRUE) %>% 
  mutate(year= as.numeric(str_replace(year, 'x', ''))) %>% 
  mutate(catch= str_replace(catch, c(' F'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('...'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('-'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('0 0'), replacement = ''))

fisheries_tidy$catch <- as.numeric(fisheries_tidy$catch)
```

3. How many countries are represented in the data? Provide a count and list their names.

```r
fisheries_tidy%>%
  count(country, sort = T)
```

```
## # A tibble: 203 x 2
##    country                      n
##    <fct>                    <int>
##  1 United States of America 18080
##  2 Spain                    17482
##  3 Japan                    15429
##  4 Portugal                 11570
##  5 Korea, Republic of       10824
##  6 France                   10639
##  7 Taiwan Province of China  9927
##  8 Indonesia                 9274
##  9 Australia                 8183
## 10 Un. Sov. Soc. Rep.        7084
## # ... with 193 more rows
```
There are 204 countries in the data set.

4. Refocus the data only to include country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch.

```r
fisheries_tidy%>%
  select(country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch)
```

```
## # A tibble: 376,771 x 6
##    country isscaap_taxonomic_group asfis_species_n~ asfis_species_n~  year catch
##    <fct>   <chr>                   <fct>            <chr>            <dbl> <dbl>
##  1 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        1995    NA
##  2 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        1996    53
##  3 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        1997    20
##  4 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        1998    31
##  5 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        1999    30
##  6 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        2000    30
##  7 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        2001    16
##  8 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        2002    79
##  9 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        2003     1
## 10 Albania Sharks, rays, chimaeras Squatinidae      10903XXXXX        2004     4
## # ... with 376,761 more rows
```

5. Based on the asfis_species_number, how many distinct fish species were caught as part of these data?

```r
fisheries_tidy%>%
  count(asfis_species_name, sort = T)
```

```
## # A tibble: 1,546 x 2
##    asfis_species_name     n
##    <fct>              <int>
##  1 Osteichthyes       16204
##  2 Thunnus albacares   6866
##  3 Elasmobranchii      6405
##  4 Katsuwonus pelamis  5785
##  5 Thunnus obesus      5341
##  6 Xiphias gladius     5143
##  7 Thunnus alalunga    4441
##  8 Mugilidae           4141
##  9 Rajiformes          3706
## 10 Mollusca            3516
## # ... with 1,536 more rows
```
1546 different fish species were collected.

6. Which country had the largest overall catch in the year 2000?

```r
fisheries_tidy%>%
  filter(year == "2000")%>%
  arrange(desc(catch))
```

```
## # A tibble: 8,793 x 10
##    country   common_name    isscaap_group_nu~ isscaap_taxonomi~ asfis_species_n~
##    <fct>     <chr>          <fct>             <chr>             <chr>           
##  1 China     Marine fishes~ 39                Marine fishes no~ 199XXXXXXX010   
##  2 Peru      Anchoveta(=Pe~ 35                Herrings, sardin~ 1210600208      
##  3 Russian ~ Alaska polloc~ 32                Cods, hakes, had~ 1480401601      
##  4 Viet Nam  Marine fishes~ 39                Marine fishes no~ 199XXXXXXX010   
##  5 Chile     Chilean jack ~ 37                Miscellaneous pe~ 1702300405      
##  6 China     Marine mollus~ 58                Miscellaneous ma~ 399XXXXXXX016   
##  7 China     Largehead hai~ 34                Miscellaneous de~ 1750600302      
##  8 United S~ Alaska polloc~ 32                Cods, hakes, had~ 1480401601      
##  9 China     Marine crusta~ 47                Miscellaneous ma~ 299XXXXXXX013   
## 10 Philippi~ Scads nei      37                Miscellaneous pe~ 17023043XX      
## # ... with 8,783 more rows, and 5 more variables: asfis_species_name <fct>,
## #   fao_major_fishing_area <fct>, measure <chr>, year <dbl>, catch <dbl>
```
China had the largest overall catch in the year 2000.

7. Which country caught the most sardines (_Sardina pilchardus_) between the years 1990-2000?

```r
fisheries_tidy%>%
  filter(between(year, 1990, 2000) & asfis_species_name == "Sardina pilchardus")%>%
  arrange(desc(catch))
```

```
## # A tibble: 336 x 10
##    country   common_name    isscaap_group_nu~ isscaap_taxonomi~ asfis_species_n~
##    <fct>     <chr>          <fct>             <chr>             <chr>           
##  1 Morocco   European pilc~ 35                Herrings, sardin~ 1210506401      
##  2 Morocco   European pilc~ 35                Herrings, sardin~ 1210506401      
##  3 Spain     European pilc~ 35                Herrings, sardin~ 1210506401      
##  4 Morocco   European pilc~ 35                Herrings, sardin~ 1210506401      
##  5 Morocco   European pilc~ 35                Herrings, sardin~ 1210506401      
##  6 Morocco   European pilc~ 35                Herrings, sardin~ 1210506401      
##  7 Morocco   European pilc~ 35                Herrings, sardin~ 1210506401      
##  8 Morocco   European pilc~ 35                Herrings, sardin~ 1210506401      
##  9 Russian ~ European pilc~ 35                Herrings, sardin~ 1210506401      
## 10 Russian ~ European pilc~ 35                Herrings, sardin~ 1210506401      
## # ... with 326 more rows, and 5 more variables: asfis_species_name <fct>,
## #   fao_major_fishing_area <fct>, measure <chr>, year <dbl>, catch <dbl>
```
Morocco caught the most number of Sardines between 1990 and 2000.

8. Which five countries caught the most cephalopods between 2008-2012?

```r
fisheries_tidy%>%
  filter(between(year, 2008, 2012) & isscaap_taxonomic_group == "Squids, cuttlefishes, octopuses")%>%
  arrange(desc(catch))
```

```
## # A tibble: 1,801 x 10
##    country    common_name    isscaap_group_n~ isscaap_taxonomi~ asfis_species_n~
##    <fct>      <chr>          <fct>            <chr>             <chr>           
##  1 Indonesia  Common squids~ 57               Squids, cuttlefi~ 32104001XX      
##  2 China      Various squid~ 57               Squids, cuttlefi~ 32105XXXXX036   
##  3 Chile      Jumbo flying ~ 57               Squids, cuttlefi~ 3210502301      
##  4 United St~ Opalescent in~ 57               Squids, cuttlefi~ 3210400103      
##  5 China      Various squid~ 57               Squids, cuttlefi~ 32105XXXXX036   
##  6 Japan      Japanese flyi~ 57               Squids, cuttlefi~ 3210505803      
##  7 China      Cuttlefish, b~ 57               Squids, cuttlefi~ 32102XXXXX026   
##  8 Peru       Jumbo flying ~ 57               Squids, cuttlefi~ 3210502301      
##  9 Korea, Re~ Argentine sho~ 57               Squids, cuttlefi~ 3210501003      
## 10 Peru       Jumbo flying ~ 57               Squids, cuttlefi~ 3210502301      
## # ... with 1,791 more rows, and 5 more variables: asfis_species_name <fct>,
## #   fao_major_fishing_area <fct>, measure <chr>, year <dbl>, catch <dbl>
```
The top 5 countries to catch cephalopods were Indonesia, China, Chile, USA, and Japan.

9. Which species had the highest catch total between 2008-2012? (hint: Osteichthyes is not a species)

```r
fisheries_tidy%>%
  filter(between(year, 2008, 2012))%>%
  arrange(desc(catch))
```

```
## # A tibble: 51,014 x 10
##    country   common_name    isscaap_group_nu~ isscaap_taxonomi~ asfis_species_n~
##    <fct>     <chr>          <fct>             <chr>             <chr>           
##  1 Viet Nam  Marine fishes~ 39                Marine fishes no~ 199XXXXXXX010   
##  2 Myanmar   Marine fishes~ 39                Marine fishes no~ 199XXXXXXX010   
##  3 Viet Nam  Marine fishes~ 39                Marine fishes no~ 199XXXXXXX010   
##  4 China     Largehead hai~ 34                Miscellaneous de~ 1750600302      
##  5 Russian ~ Alaska polloc~ 32                Cods, hakes, had~ 1480401601      
##  6 Peru      Anchoveta(=Pe~ 35                Herrings, sardin~ 1210600208      
##  7 Norway    Atlantic herr~ 35                Herrings, sardin~ 1210500105      
##  8 Norway    Atlantic herr~ 35                Herrings, sardin~ 1210500105      
##  9 Peru      Anchoveta(=Pe~ 35                Herrings, sardin~ 1210600208      
## 10 China     Largehead hai~ 34                Miscellaneous de~ 1750600302      
## # ... with 51,004 more rows, and 5 more variables: asfis_species_name <fct>,
## #   fao_major_fishing_area <fct>, measure <chr>, year <dbl>, catch <dbl>
```
The fish with the highest catch (that was able to be identified) was Tirchirus lepturus aka the Largehead hairtail.
10. Use the data to do at least one analysis of your choice.

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
