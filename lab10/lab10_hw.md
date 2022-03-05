---
title: "Lab 10 Homework"
author: "Sujung Chung"
date: "2022-03-04"
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
library(naniar)
```

## Desert Ecology
For this assignment, we are going to use a modified data set on [desert ecology](http://esapubs.org/archive/ecol/E090/118/). The data are from: S. K. Morgan Ernest, Thomas J. Valone, and James H. Brown. 2009. Long-term monitoring and experimental manipulation of a Chihuahuan Desert ecosystem near Portal, Arizona, USA. Ecology 90:1708.

```r
deserts <- read_csv(here("lab10", "data", "surveys_complete.csv"))
```

```
## Rows: 34786 Columns: 13
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (6): species_id, sex, genus, species, taxa, plot_type
## dbl (7): record_id, month, day, year, plot_id, hindfoot_length, weight
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. Use the function(s) of your choice to get an idea of its structure, including how NA's are treated. Are the data tidy?  

```r
summary(deserts)
```

```
##    record_id         month             day            year         plot_id     
##  Min.   :    1   Min.   : 1.000   Min.   : 1.0   Min.   :1977   Min.   : 1.00  
##  1st Qu.: 8964   1st Qu.: 4.000   1st Qu.: 9.0   1st Qu.:1984   1st Qu.: 5.00  
##  Median :17762   Median : 6.000   Median :16.0   Median :1990   Median :11.00  
##  Mean   :17804   Mean   : 6.474   Mean   :16.1   Mean   :1990   Mean   :11.34  
##  3rd Qu.:26655   3rd Qu.:10.000   3rd Qu.:23.0   3rd Qu.:1997   3rd Qu.:17.00  
##  Max.   :35548   Max.   :12.000   Max.   :31.0   Max.   :2002   Max.   :24.00  
##                                                                                
##   species_id            sex            hindfoot_length     weight      
##  Length:34786       Length:34786       Min.   : 2.00   Min.   :  4.00  
##  Class :character   Class :character   1st Qu.:21.00   1st Qu.: 20.00  
##  Mode  :character   Mode  :character   Median :32.00   Median : 37.00  
##                                        Mean   :29.29   Mean   : 42.67  
##                                        3rd Qu.:36.00   3rd Qu.: 48.00  
##                                        Max.   :70.00   Max.   :280.00  
##                                        NA's   :3348    NA's   :2503    
##     genus             species              taxa            plot_type        
##  Length:34786       Length:34786       Length:34786       Length:34786      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
## 
```

```r
deserts%>%
summarize(number_nas=sum(is.na(deserts)))
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1       7599
```

```r
deserts
```

```
## # A tibble: 34,786 x 13
##    record_id month   day  year plot_id species_id sex   hindfoot_length weight
##        <dbl> <dbl> <dbl> <dbl>   <dbl> <chr>      <chr>           <dbl>  <dbl>
##  1         1     7    16  1977       2 NL         M                  32     NA
##  2         2     7    16  1977       3 NL         M                  33     NA
##  3         3     7    16  1977       2 DM         F                  37     NA
##  4         4     7    16  1977       7 DM         M                  36     NA
##  5         5     7    16  1977       3 DM         M                  35     NA
##  6         6     7    16  1977       1 PF         M                  14     NA
##  7         7     7    16  1977       2 PE         F                  NA     NA
##  8         8     7    16  1977       1 DM         M                  37     NA
##  9         9     7    16  1977       1 DM         F                  34     NA
## 10        10     7    16  1977       6 PF         F                  20     NA
## # ... with 34,776 more rows, and 4 more variables: genus <chr>, species <chr>,
## #   taxa <chr>, plot_type <chr>
```

The NAs are input into the data as NA. Data is tidy!

2. How many genera and species are represented in the data? What are the total number of observations? Which species is most/ least frequently sampled in the study?

```r
deserts%>%
  summarize(genera=n_distinct(genus), species=n_distinct(species), n=n())
```

```
## # A tibble: 1 x 3
##   genera species     n
##    <int>   <int> <int>
## 1     26      40 34786
```


```r
deserts%>%
  count(species_id)%>%
  arrange(desc(n))
```

```
## # A tibble: 48 x 2
##    species_id     n
##    <chr>      <int>
##  1 DM         10596
##  2 PP          3123
##  3 DO          3027
##  4 PB          2891
##  5 RM          2609
##  6 DS          2504
##  7 OT          2249
##  8 PF          1597
##  9 PE          1299
## 10 NL          1252
## # ... with 38 more rows
```

3. What is the proportion of taxa included in this study? Show a table and plot that reflects this count.

```r
deserts%>%
  tabyl(taxa)
```

```
##     taxa     n      percent
##     Bird   450 0.0129362387
##   Rabbit    75 0.0021560398
##  Reptile    14 0.0004024608
##   Rodent 34247 0.9845052607
```

```r
deserts%>%
  ggplot(aes(x =taxa))+
  geom_bar()+
  scale_y_log10()+
  labs(title = "number of taxa",
       y = "count",
       x = "taxa")
```

![](lab10_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->


4. For the taxa included in the study, use the fill option to show the proportion of individuals sampled by `plot_type.`

```r
deserts%>%
  ggplot(aes(x =taxa, fill = plot_type))+geom_bar()+scale_y_log10()
```

![](lab10_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

5. What is the range of weight for each species included in the study? Remove any observations of weight that are NA so they do not show up in the plot.

```r
deserts%>%
  select(species, weight)%>%
  group_by(species)%>%
  arrange(desc(weight))
```

```
## # A tibble: 34,786 x 2
## # Groups:   species [40]
##    species  weight
##    <chr>     <dbl>
##  1 albigula    280
##  2 albigula    278
##  3 albigula    275
##  4 albigula    274
##  5 albigula    270
##  6 albigula    269
##  7 albigula    265
##  8 albigula    264
##  9 albigula    260
## 10 albigula    260
## # ... with 34,776 more rows
```

```r
deserts%>%
  filter(weight!="NA")%>%
  ggplot(aes(x=species, y=weight))+
  geom_boxplot()+
  coord_flip()
```

![](lab10_hw_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

6. Add another layer to your answer from #5 using `geom_point` to get an idea of how many measurements were taken for each species.

```r
deserts%>%
  filter(weight!="NA")%>%
  ggplot(aes(x=species, y=weight))+
  geom_boxplot(na.rm=T)+
  geom_point(alpha = 0.1, color = "steelblue")+
  coord_flip()+
  labs(title = "weight of species",
       x = "species",
       y = "weight")
```

![](lab10_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

7. [Dipodomys merriami](https://en.wikipedia.org/wiki/Merriam's_kangaroo_rat) is the most frequently sampled animal in the study. How have the number of observations of this species changed over the years included in the study?

```r
dm <- deserts%>%
  filter(species_id =="DM")%>%
  group_by(year)%>%
  summarize(n_samples=n())
dm
```

```
## # A tibble: 26 x 2
##     year n_samples
##    <dbl>     <int>
##  1  1977       264
##  2  1978       389
##  3  1979       209
##  4  1980       493
##  5  1981       559
##  6  1982       609
##  7  1983       528
##  8  1984       396
##  9  1985       667
## 10  1986       406
## # ... with 16 more rows
```

```r
dm%>%
  ggplot(aes(x = year, y = n_samples))+
  geom_point()+
  geom_line(color = "orange")+
  labs(title = "number of samples of DM through the years",
       x = "year",
       y = "number of samples")
```

![](lab10_hw_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

8. What is the relationship between `weight` and `hindfoot` length? Consider whether or not over plotting is an issue.

```r
deserts%>%
  ggplot(aes(x = weight, y = hindfoot_length))+
  geom_point()+
  geom_jitter()
```

```
## Warning: Removed 4048 rows containing missing values (geom_point).
## Removed 4048 rows containing missing values (geom_point).
```

![](lab10_hw_files/figure-html/unnamed-chunk-16-1.png)<!-- -->
You can't deduct anything from it becuase the data is too scattered in large clumps.

9. Which two species have, on average, the highest weight? Once you have identified them, make a new column that is a ratio of `weight` to `hindfoot_length`. Make a plot that shows the range of this new ratio and fill by sex.

```r
deserts%>% #if new column, use mutate
  filter(weight!="NA")%>%
  group_by(species)%>%
  summarize(mean_weight=mean(weight))%>%
  arrange(desc(mean_weight))
```

```
## # A tibble: 22 x 2
##    species      mean_weight
##    <chr>              <dbl>
##  1 albigula           159. 
##  2 spectabilis        120. 
##  3 spilosoma           93.5
##  4 hispidus            65.6
##  5 fulviventer         58.9
##  6 ochrognathus        55.4
##  7 ordii               48.9
##  8 merriami            43.2
##  9 baileyi             31.7
## 10 leucogaster         31.6
## # ... with 12 more rows
```

```r
deserts%>%
  filter(weight!="NA" & hindfoot_length!="NA")%>%
  filter(species=="albigula" | species=="spectabilis")%>%
  mutate(ratio=weight/hindfoot_length)%>%
  select(species, sex, weight, hindfoot_length, ratio)
```

```
## # A tibble: 3,072 x 5
##    species     sex   weight hindfoot_length ratio
##    <chr>       <chr>  <dbl>           <dbl> <dbl>
##  1 spectabilis F        117              50  2.34
##  2 spectabilis F        121              51  2.37
##  3 spectabilis M        115              51  2.25
##  4 spectabilis F        120              48  2.5 
##  5 spectabilis F        118              48  2.46
##  6 spectabilis F        126              52  2.42
##  7 spectabilis M        132              50  2.64
##  8 spectabilis F        122              53  2.30
##  9 spectabilis F        107              48  2.23
## 10 spectabilis F        115              50  2.3 
## # ... with 3,062 more rows
```


```r
deserts
```

```
## # A tibble: 34,786 x 13
##    record_id month   day  year plot_id species_id sex   hindfoot_length weight
##        <dbl> <dbl> <dbl> <dbl>   <dbl> <chr>      <chr>           <dbl>  <dbl>
##  1         1     7    16  1977       2 NL         M                  32     NA
##  2         2     7    16  1977       3 NL         M                  33     NA
##  3         3     7    16  1977       2 DM         F                  37     NA
##  4         4     7    16  1977       7 DM         M                  36     NA
##  5         5     7    16  1977       3 DM         M                  35     NA
##  6         6     7    16  1977       1 PF         M                  14     NA
##  7         7     7    16  1977       2 PE         F                  NA     NA
##  8         8     7    16  1977       1 DM         M                  37     NA
##  9         9     7    16  1977       1 DM         F                  34     NA
## 10        10     7    16  1977       6 PF         F                  20     NA
## # ... with 34,776 more rows, and 4 more variables: genus <chr>, species <chr>,
## #   taxa <chr>, plot_type <chr>
```


```r
deserts%>%
  filter(weight!="NA" & hindfoot_length!="NA" & sex!="NA")%>%
  filter(species=="albigula" | species=="spectabilis")%>%
  mutate(ratio=weight/hindfoot_length)%>%
  ggplot(aes(x=species, y = ratio, fill=sex))+geom_boxplot()+
  labs(title = "species vs weight and hindfoot ratio",
       x = "species",
       y = "weight:hindfoot length ratio")
```

![](lab10_hw_files/figure-html/unnamed-chunk-20-1.png)<!-- -->

10. Make one plot of your choice! Make sure to include at least two of the aesthetics options you have learned.

```r
deserts%>%
  filter(sex!="NA")%>%
  filter(taxa == "Rodent")%>%
  ggplot(aes(x=sex, y=weight, fill = sex))+
  geom_col(position = "dodge")+
  labs(title = "weight differences between males and females in rodents",
       x="sex",
       y = "weight")
```

```
## Warning: Removed 856 rows containing missing values (geom_col).
```

![](lab10_hw_files/figure-html/unnamed-chunk-21-1.png)<!-- -->

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
