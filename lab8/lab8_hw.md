---
title: "Lab 8 Homework"
author: "Sujung Chung"
date: "2022-02-14"
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
```

## Install `here`
The package `here` is a nice option for keeping directories clear when loading files. I will demonstrate below and let you decide if it is something you want to use.  

```r
#install.packages("here")
```

## Data
For this homework, we will use a data set compiled by the Office of Environment and Heritage in New South Whales, Australia. It contains the enterococci counts in water samples obtained from Sydney beaches as part of the Beachwatch Water Quality Program. Enterococci are bacteria common in the intestines of mammals; they are rarely present in clean water. So, enterococci values are a measurement of pollution. `cfu` stands for colony forming units and measures the number of viable bacteria in a sample [cfu](https://en.wikipedia.org/wiki/Colony-forming_unit).   

This homework loosely follows the tutorial of [R Ladies Sydney](https://rladiessydney.org/). If you get stuck, check it out!  

1. Start by loading the data `sydneybeaches`. Do some exploratory analysis to get an idea of the data structure.

```r
sydneybeaches <- readr::read_csv(file = "data/sydneybeaches.csv")
```

```
## Rows: 3690 Columns: 8
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
summary(sydneybeaches)
```

```
##     BeachId         Region            Council              Site          
##  Min.   :22.00   Length:3690        Length:3690        Length:3690       
##  1st Qu.:24.00   Class :character   Class :character   Class :character  
##  Median :26.00   Mode  :character   Mode  :character   Mode  :character  
##  Mean   :25.87                                                           
##  3rd Qu.:27.40                                                           
##  Max.   :29.00                                                           
##                                                                          
##    Longitude        Latitude          Date           Enterococci (cfu/100ml)
##  Min.   :151.3   Min.   :-33.98   Length:3690        Min.   :   0.00        
##  1st Qu.:151.3   1st Qu.:-33.95   Class :character   1st Qu.:   1.00        
##  Median :151.3   Median :-33.92   Mode  :character   Median :   5.00        
##  Mean   :151.3   Mean   :-33.93                      Mean   :  33.92        
##  3rd Qu.:151.3   3rd Qu.:-33.90                      3rd Qu.:  17.00        
##  Max.   :151.3   Max.   :-33.89                      Max.   :4900.00        
##                                                      NA's   :29
```

```r
sydneybeaches
```

```
## # A tibble: 3,690 x 8
##    BeachId Region        Council Site  Longitude Latitude Date  `Enterococci (~`
##      <dbl> <chr>         <chr>   <chr>     <dbl>    <dbl> <chr>            <dbl>
##  1      25 Sydney City ~ Randwi~ Clov~      151.    -33.9 02/0~               19
##  2      25 Sydney City ~ Randwi~ Clov~      151.    -33.9 06/0~                3
##  3      25 Sydney City ~ Randwi~ Clov~      151.    -33.9 12/0~                2
##  4      25 Sydney City ~ Randwi~ Clov~      151.    -33.9 18/0~               13
##  5      25 Sydney City ~ Randwi~ Clov~      151.    -33.9 30/0~                8
##  6      25 Sydney City ~ Randwi~ Clov~      151.    -33.9 05/0~                7
##  7      25 Sydney City ~ Randwi~ Clov~      151.    -33.9 11/0~               11
##  8      25 Sydney City ~ Randwi~ Clov~      151.    -33.9 23/0~               97
##  9      25 Sydney City ~ Randwi~ Clov~      151.    -33.9 07/0~                3
## 10      25 Sydney City ~ Randwi~ Clov~      151.    -33.9 25/0~                0
## # ... with 3,680 more rows
```

If you want to try `here`, first notice the output when you load the `here` library. It gives you information on the current working directory. You can then use it to easily and intuitively load files.

```r
library(here)
```

```
## here() starts at C:/Users/sujun/Documents/GitHub Winter 2022/BIS15W2022_schung
```

The quotes show the folder structure from the root directory.

```r
sydneybeaches <-read_csv(here("lab8", "data", "sydneybeaches.csv")) %>% janitor::clean_names()
```

```
## Rows: 3690 Columns: 8
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

2. Are these data "tidy" per the definitions of the tidyverse? How do you know? Are they in wide or long format?

The data is tidy because each variable has its own column, there are no columns with several values, and that each row is for an observation.  The data is long.

3. We are only interested in the variables site, date, and enterococci_cfu_100ml. Make a new object focused on these variables only. Name the object `sydneybeaches_long`

```r
sydneybeaches_long<- sydneybeaches%>%
  select(site, date, enterococci_cfu_100ml)
sydneybeaches_long
```

```
## # A tibble: 3,690 x 3
##    site           date       enterococci_cfu_100ml
##    <chr>          <chr>                      <dbl>
##  1 Clovelly Beach 02/01/2013                    19
##  2 Clovelly Beach 06/01/2013                     3
##  3 Clovelly Beach 12/01/2013                     2
##  4 Clovelly Beach 18/01/2013                    13
##  5 Clovelly Beach 30/01/2013                     8
##  6 Clovelly Beach 05/02/2013                     7
##  7 Clovelly Beach 11/02/2013                    11
##  8 Clovelly Beach 23/02/2013                    97
##  9 Clovelly Beach 07/03/2013                     3
## 10 Clovelly Beach 25/03/2013                     0
## # ... with 3,680 more rows
```

4. Pivot the data such that the dates are column names and each beach only appears once. Name the object `sydneybeaches_wide`

```r
sydneybeaches_wide <- sydneybeaches_long %>%
  pivot_wider(names_from = "date",
              values_from = "enterococci_cfu_100ml")
sydneybeaches_wide
```

```
## # A tibble: 11 x 345
##    site         `02/01/2013` `06/01/2013` `12/01/2013` `18/01/2013` `30/01/2013`
##    <chr>               <dbl>        <dbl>        <dbl>        <dbl>        <dbl>
##  1 Clovelly Be~           19            3            2           13            8
##  2 Coogee Beach           15            4           17           18           22
##  3 Gordons Bay~           NA           NA           NA           NA           NA
##  4 Little Bay ~            9            3           72            1           44
##  5 Malabar Bea~            2            4          390           15           13
##  6 Maroubra Be~            1            1           20            2           11
##  7 South Marou~            1            0           33            2           13
##  8 South Marou~           12            2          110           13          100
##  9 Bondi Beach             3            1            2            1            6
## 10 Bronte Beach            4            2           38            3           25
## 11 Tamarama Be~            1            0            7           22           23
## # ... with 339 more variables: `05/02/2013` <dbl>, `11/02/2013` <dbl>,
## #   `23/02/2013` <dbl>, `07/03/2013` <dbl>, `25/03/2013` <dbl>,
## #   `02/04/2013` <dbl>, `12/04/2013` <dbl>, `18/04/2013` <dbl>,
## #   `24/04/2013` <dbl>, `01/05/2013` <dbl>, `20/05/2013` <dbl>,
## #   `31/05/2013` <dbl>, `06/06/2013` <dbl>, `12/06/2013` <dbl>,
## #   `24/06/2013` <dbl>, `06/07/2013` <dbl>, `18/07/2013` <dbl>,
## #   `24/07/2013` <dbl>, `08/08/2013` <dbl>, `22/08/2013` <dbl>, ...
```

5. Pivot the data back so that the dates are data and not column names.

```r
sydneybeaches_wide%>%
  pivot_longer(-site,
               names_to="date",
               values_to="enterococci_cfu_100ml")
```

```
## # A tibble: 3,784 x 3
##    site           date       enterococci_cfu_100ml
##    <chr>          <chr>                      <dbl>
##  1 Clovelly Beach 02/01/2013                    19
##  2 Clovelly Beach 06/01/2013                     3
##  3 Clovelly Beach 12/01/2013                     2
##  4 Clovelly Beach 18/01/2013                    13
##  5 Clovelly Beach 30/01/2013                     8
##  6 Clovelly Beach 05/02/2013                     7
##  7 Clovelly Beach 11/02/2013                    11
##  8 Clovelly Beach 23/02/2013                    97
##  9 Clovelly Beach 07/03/2013                     3
## 10 Clovelly Beach 25/03/2013                     0
## # ... with 3,774 more rows
```

6. We haven't dealt much with dates yet, but separate the date into columns day, month, and year. Do this on the `sydneybeaches_long` data.

```r
sydneybeaches_long_split<- sydneybeaches_long%>%
  separate(date, into=c("day", "month", "year"), sep= "/")
sydneybeaches_long_split
```

```
## # A tibble: 3,690 x 5
##    site           day   month year  enterococci_cfu_100ml
##    <chr>          <chr> <chr> <chr>                 <dbl>
##  1 Clovelly Beach 02    01    2013                     19
##  2 Clovelly Beach 06    01    2013                      3
##  3 Clovelly Beach 12    01    2013                      2
##  4 Clovelly Beach 18    01    2013                     13
##  5 Clovelly Beach 30    01    2013                      8
##  6 Clovelly Beach 05    02    2013                      7
##  7 Clovelly Beach 11    02    2013                     11
##  8 Clovelly Beach 23    02    2013                     97
##  9 Clovelly Beach 07    03    2013                      3
## 10 Clovelly Beach 25    03    2013                      0
## # ... with 3,680 more rows
```


7. What is the average `enterococci_cfu_100ml` by year for each beach. Think about which data you will use- long or wide.

```r
mean_sbls<- sydneybeaches_long_split%>%
  group_by(site, year)%>%
  summarize(mean_enterococci_cfu_100ml = mean(enterococci_cfu_100ml, na.rm = T))
```

```
## `summarise()` has grouped output by 'site'. You can override using the `.groups` argument.
```

```r
mean_sbls
```

```
## # A tibble: 66 x 3
## # Groups:   site [11]
##    site         year  mean_enterococci_cfu_100ml
##    <chr>        <chr>                      <dbl>
##  1 Bondi Beach  2013                        32.2
##  2 Bondi Beach  2014                        11.1
##  3 Bondi Beach  2015                        14.3
##  4 Bondi Beach  2016                        19.4
##  5 Bondi Beach  2017                        13.2
##  6 Bondi Beach  2018                        22.9
##  7 Bronte Beach 2013                        26.8
##  8 Bronte Beach 2014                        17.5
##  9 Bronte Beach 2015                        23.6
## 10 Bronte Beach 2016                        61.3
## # ... with 56 more rows
```


8. Make the output from question 7 easier to read by pivoting it to wide format.

```r
mean_sbls%>%
  pivot_wider(names_from = "site",
              values_from = "mean_enterococci_cfu_100ml")
```

```
## # A tibble: 6 x 12
##   year  `Bondi Beach` `Bronte Beach` `Clovelly Beach` `Coogee Beach`
##   <chr>         <dbl>          <dbl>            <dbl>          <dbl>
## 1 2013           32.2           26.8             9.28           39.7
## 2 2014           11.1           17.5            13.8            52.6
## 3 2015           14.3           23.6             8.82           40.3
## 4 2016           19.4           61.3            11.3            59.5
## 5 2017           13.2           16.8             7.93           20.7
## 6 2018           22.9           43.4            10.6            21.6
## # ... with 7 more variables: `Gordons Bay (East)` <dbl>,
## #   `Little Bay Beach` <dbl>, `Malabar Beach` <dbl>, `Maroubra Beach` <dbl>,
## #   `South Maroubra Beach` <dbl>, `South Maroubra Rockpool` <dbl>,
## #   `Tamarama Beach` <dbl>
```


9. What was the most polluted beach in 2018?

```r
mean_sbls%>%
  filter(year=="2018")%>%
  arrange(desc(mean_enterococci_cfu_100ml))
```

```
## # A tibble: 11 x 3
## # Groups:   site [11]
##    site                    year  mean_enterococci_cfu_100ml
##    <chr>                   <chr>                      <dbl>
##  1 South Maroubra Rockpool 2018                      112.  
##  2 Little Bay Beach        2018                       59.1 
##  3 Bronte Beach            2018                       43.4 
##  4 Malabar Beach           2018                       38.0 
##  5 Bondi Beach             2018                       22.9 
##  6 Coogee Beach            2018                       21.6 
##  7 Gordons Bay (East)      2018                       17.6 
##  8 Tamarama Beach          2018                       15.5 
##  9 South Maroubra Beach    2018                       12.5 
## 10 Clovelly Beach          2018                       10.6 
## 11 Maroubra Beach          2018                        9.21
```


10. Please complete the class project survey at: [BIS 15L Group Project](https://forms.gle/H2j69Z3ZtbLH3efW6)


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
