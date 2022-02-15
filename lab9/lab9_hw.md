---
title: "Lab 9 Homework"
author: "Sujung Chung"
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
library(here)
library(naniar)
```

For this homework, we will take a departure from biological data and use data about California colleges. These data are a subset of the national college scorecard (https://collegescorecard.ed.gov/data/). Load the `ca_college_data.csv` as a new object called `colleges`.

```r
colleges <- readr::read_csv("data/ca_college_data.csv")
```

```
## Rows: 341 Columns: 10
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (4): INSTNM, CITY, STABBR, ZIP
## dbl (6): ADM_RATE, SAT_AVG, PCIP26, COSTT4_A, C150_4_POOLED, PFTFTUG1_EF
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

The variables are a bit hard to decipher, here is a key:  

INSTNM: Institution name  
CITY: California city  
STABBR: Location state  
ZIP: Zip code  
ADM_RATE: Admission rate  
SAT_AVG: SAT average score  
PCIP26: Percentage of degrees awarded in Biological And Biomedical Sciences  
COSTT4_A: Annual cost of attendance  
C150_4_POOLED: 4-year completion rate  
PFTFTUG1_EF: Percentage of undergraduate students who are first-time, full-time degree/certificate-seeking undergraduate students  

1. Use your preferred function(s) to have a look at the data and get an idea of its structure. Make sure you summarize NA's and determine whether or not the data are tidy. You may also consider dealing with any naming issues.

```r
summary(colleges)
```

```
##     INSTNM              CITY              STABBR              ZIP           
##  Length:341         Length:341         Length:341         Length:341        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     ADM_RATE         SAT_AVG         PCIP26           COSTT4_A    
##  Min.   :0.0807   Min.   : 870   Min.   :0.00000   Min.   : 7956  
##  1st Qu.:0.4581   1st Qu.: 985   1st Qu.:0.00000   1st Qu.:12578  
##  Median :0.6370   Median :1078   Median :0.00000   Median :16591  
##  Mean   :0.5901   Mean   :1112   Mean   :0.01981   Mean   :26685  
##  3rd Qu.:0.7461   3rd Qu.:1237   3rd Qu.:0.02457   3rd Qu.:39289  
##  Max.   :1.0000   Max.   :1555   Max.   :0.21650   Max.   :69355  
##  NA's   :240      NA's   :276    NA's   :35        NA's   :124    
##  C150_4_POOLED     PFTFTUG1_EF    
##  Min.   :0.0625   Min.   :0.0064  
##  1st Qu.:0.4265   1st Qu.:0.3212  
##  Median :0.5845   Median :0.5016  
##  Mean   :0.5705   Mean   :0.5577  
##  3rd Qu.:0.7162   3rd Qu.:0.8117  
##  Max.   :0.9569   Max.   :1.0000  
##  NA's   :221      NA's   :53
```


```r
colleges%>%
  summarize(number_nas=sum(is.na(colleges)))
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1        949
```


```r
colleges<- clean_names(colleges)
colleges
```

```
## # A tibble: 341 x 10
##    instnm      city  stabbr zip   adm_rate sat_avg pcip26 costt4_a c150_4_pooled
##    <chr>       <chr> <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>         <dbl>
##  1 Grossmont ~ El C~ CA     9202~       NA      NA 0.0016     7956        NA    
##  2 College of~ Visa~ CA     9327~       NA      NA 0.0066     8109        NA    
##  3 College of~ San ~ CA     9440~       NA      NA 0.0038     8278        NA    
##  4 Ventura Co~ Vent~ CA     9300~       NA      NA 0.0035     8407        NA    
##  5 Oxnard Col~ Oxna~ CA     9303~       NA      NA 0.0085     8516        NA    
##  6 Moorpark C~ Moor~ CA     9302~       NA      NA 0.0151     8577        NA    
##  7 Skyline Co~ San ~ CA     9406~       NA      NA 0          8580         0.233
##  8 Glendale C~ Glen~ CA     9120~       NA      NA 0.002      9181        NA    
##  9 Citrus Col~ Glen~ CA     9174~       NA      NA 0.0021     9281        NA    
## 10 Fresno Cit~ Fres~ CA     93741       NA      NA 0.0324     9370        NA    
## # ... with 331 more rows, and 1 more variable: pftftug1_ef <dbl>
```

2. Which cities in California have the highest number of colleges?

```r
ncity<-colleges%>%
  count(city)%>%
  arrange(desc(n))
ncity
```

```
## # A tibble: 161 x 2
##    city              n
##    <chr>         <int>
##  1 Los Angeles      24
##  2 San Diego        18
##  3 San Francisco    15
##  4 Sacramento       10
##  5 Berkeley          9
##  6 Oakland           9
##  7 Claremont         7
##  8 Pasadena          6
##  9 Fresno            5
## 10 Irvine            5
## # ... with 151 more rows
```
Los Angeles has the highest number of colleges.

3. Based on your answer to #2, make a plot that shows the number of colleges in the top 10 cities.

```r
ncity%>%
  top_n(10,n)%>%
  ggplot(aes(x=city,y=n))+geom_col()+
  coord_flip()
```

![](lab9_hw_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

4. The column `COSTT4_A` is the annual cost of each institution. Which city has the highest average cost? Where is it located?

```r
colleges%>%
  group_by(city)%>%
  summarize(mean_cost = mean(costt4_a, na.rm = TRUE))%>%
  arrange(desc(mean_cost))
```

```
## # A tibble: 161 x 2
##    city                mean_cost
##    <chr>                   <dbl>
##  1 Claremont               66498
##  2 Malibu                  66152
##  3 Valencia                64686
##  4 Orange                  64501
##  5 Redlands                61542
##  6 Moraga                  61095
##  7 Atherton                56035
##  8 Thousand Oaks           54373
##  9 Rancho Palos Verdes     50758
## 10 La Verne                50603
## # ... with 151 more rows
```
Claremont has highest average cost.

5. Based on your answer to #4, make a plot that compares the cost of the individual colleges in the most expensive city. Bonus! Add UC Davis here to see how it compares :>).

```r
colleges%>%
  filter(city=="Claremont"|instnm=="University of California-Davis")%>%
  arrange(desc(costt4_a))
```

```
## # A tibble: 8 x 10
##   instnm      city  stabbr zip   adm_rate sat_avg  pcip26 costt4_a c150_4_pooled
##   <chr>       <chr> <chr>  <chr>    <dbl>   <dbl>   <dbl>    <dbl>         <dbl>
## 1 Harvey Mud~ Clar~ CA     91711   0.129     1496  0.0674    69355         0.925
## 2 Claremont ~ Clar~ CA     9171~   0.0944    1413  0.0681    66325         0.924
## 3 Scripps Co~ Clar~ CA     9171~   0.299     1353  0.152     66060         0.871
## 4 Pitzer Col~ Clar~ CA     9171~   0.137       NA  0.0888    65880         0.888
## 5 Pomona Col~ Clar~ CA     9171~   0.0944    1442  0.171     64870         0.957
## 6 University~ Davis CA     9561~   0.423     1218  0.198     33904         0.850
## 7 Claremont ~ Clar~ CA     9171~  NA           NA NA            NA        NA    
## 8 Claremont ~ Clar~ CA     9171~  NA           NA NA            NA        NA    
## # ... with 1 more variable: pftftug1_ef <dbl>
```

6. The column `ADM_RATE` is the admissions rate by college and `C150_4_POOLED` is the four-year completion rate. Use a scatterplot to show the relationship between these two variables. What do you think this means?

```r
colleges%>%
  ggplot(aes(x=adm_rate, y=c150_4_pooled))+geom_point()+geom_smooth(method=lm)
```

```
## `geom_smooth()` using formula 'y ~ x'
```

```
## Warning: Removed 251 rows containing non-finite values (stat_smooth).
```

```
## Warning: Removed 251 rows containing missing values (geom_point).
```

![](lab9_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
The lower the admission rate, the higher the rate of completion.

7. Is there a relationship between cost and four-year completion rate? (You don't need to do the stats, just produce a plot). What do you think this means?

```r
colleges%>%
  ggplot(aes(x=costt4_a, y=c150_4_pooled))+geom_point()+geom_smooth(method=lm)
```

```
## `geom_smooth()` using formula 'y ~ x'
```

```
## Warning: Removed 225 rows containing non-finite values (stat_smooth).
```

```
## Warning: Removed 225 rows containing missing values (geom_point).
```

![](lab9_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->
The more expensive the school, the higher the completion rate.

8. The column titled `INSTNM` is the institution name. We are only interested in the University of California colleges. Make a new data frame that is restricted to UC institutions. You can remove `Hastings College of Law` and `UC San Francisco` as we are only interested in undergraduate institutions.

```r
uc<-colleges%>%
  filter_all(any_vars(str_detect(.,pattern = "University of California")))
uc
```

```
## # A tibble: 10 x 10
##    instnm      city  stabbr zip   adm_rate sat_avg pcip26 costt4_a c150_4_pooled
##    <chr>       <chr> <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>         <dbl>
##  1 University~ La J~ CA     92093    0.357    1324  0.216    31043         0.872
##  2 University~ Irvi~ CA     92697    0.406    1206  0.107    31198         0.876
##  3 University~ Rive~ CA     92521    0.663    1078  0.149    31494         0.73 
##  4 University~ Los ~ CA     9009~    0.180    1334  0.155    33078         0.911
##  5 University~ Davis CA     9561~    0.423    1218  0.198    33904         0.850
##  6 University~ Sant~ CA     9506~    0.578    1201  0.193    34608         0.776
##  7 University~ Berk~ CA     94720    0.169    1422  0.105    34924         0.916
##  8 University~ Sant~ CA     93106    0.358    1281  0.108    34998         0.816
##  9 University~ San ~ CA     9410~   NA          NA NA           NA        NA    
## 10 University~ San ~ CA     9414~   NA          NA NA           NA        NA    
## # ... with 1 more variable: pftftug1_ef <dbl>
```

Remove `Hastings College of Law` and `UC San Francisco` and store the final data frame as a new object `univ_calif_final`.

```r
univ_calif_final <- uc%>%
  filter(city!="San Francisco"& city!="Hastings College of Law")
univ_calif_final
```

```
## # A tibble: 8 x 10
##   instnm       city  stabbr zip   adm_rate sat_avg pcip26 costt4_a c150_4_pooled
##   <chr>        <chr> <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>         <dbl>
## 1 University ~ La J~ CA     92093    0.357    1324  0.216    31043         0.872
## 2 University ~ Irvi~ CA     92697    0.406    1206  0.107    31198         0.876
## 3 University ~ Rive~ CA     92521    0.663    1078  0.149    31494         0.73 
## 4 University ~ Los ~ CA     9009~    0.180    1334  0.155    33078         0.911
## 5 University ~ Davis CA     9561~    0.423    1218  0.198    33904         0.850
## 6 University ~ Sant~ CA     9506~    0.578    1201  0.193    34608         0.776
## 7 University ~ Berk~ CA     94720    0.169    1422  0.105    34924         0.916
## 8 University ~ Sant~ CA     93106    0.358    1281  0.108    34998         0.816
## # ... with 1 more variable: pftftug1_ef <dbl>
```

Use `separate()` to separate institution name into two new columns "UNIV" and "CAMPUS".

```r
univ_calif_final<-univ_calif_final%>%
  separate(instnm, into=c("univ", "campus"), sep="-")
univ_calif_final
```

```
## # A tibble: 8 x 11
##   univ  campus city  stabbr zip   adm_rate sat_avg pcip26 costt4_a c150_4_pooled
##   <chr> <chr>  <chr> <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>         <dbl>
## 1 Univ~ San D~ La J~ CA     92093    0.357    1324  0.216    31043         0.872
## 2 Univ~ Irvine Irvi~ CA     92697    0.406    1206  0.107    31198         0.876
## 3 Univ~ River~ Rive~ CA     92521    0.663    1078  0.149    31494         0.73 
## 4 Univ~ Los A~ Los ~ CA     9009~    0.180    1334  0.155    33078         0.911
## 5 Univ~ Davis  Davis CA     9561~    0.423    1218  0.198    33904         0.850
## 6 Univ~ Santa~ Sant~ CA     9506~    0.578    1201  0.193    34608         0.776
## 7 Univ~ Berke~ Berk~ CA     94720    0.169    1422  0.105    34924         0.916
## 8 Univ~ Santa~ Sant~ CA     93106    0.358    1281  0.108    34998         0.816
## # ... with 1 more variable: pftftug1_ef <dbl>
```

9. The column `ADM_RATE` is the admissions rate by campus. Which UC has the lowest and highest admissions rates? Produce a numerical summary and an appropriate plot.

```r
univ_calif_final%>%
  arrange(desc(adm_rate))
```

```
## # A tibble: 8 x 11
##   univ  campus city  stabbr zip   adm_rate sat_avg pcip26 costt4_a c150_4_pooled
##   <chr> <chr>  <chr> <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>         <dbl>
## 1 Univ~ River~ Rive~ CA     92521    0.663    1078  0.149    31494         0.73 
## 2 Univ~ Santa~ Sant~ CA     9506~    0.578    1201  0.193    34608         0.776
## 3 Univ~ Davis  Davis CA     9561~    0.423    1218  0.198    33904         0.850
## 4 Univ~ Irvine Irvi~ CA     92697    0.406    1206  0.107    31198         0.876
## 5 Univ~ Santa~ Sant~ CA     93106    0.358    1281  0.108    34998         0.816
## 6 Univ~ San D~ La J~ CA     92093    0.357    1324  0.216    31043         0.872
## 7 Univ~ Los A~ Los ~ CA     9009~    0.180    1334  0.155    33078         0.911
## 8 Univ~ Berke~ Berk~ CA     94720    0.169    1422  0.105    34924         0.916
## # ... with 1 more variable: pftftug1_ef <dbl>
```


```r
univ_calif_final%>%
  ggplot(aes(x=campus, y=adm_rate))+
  geom_point()+
  coord_flip()
```

![](lab9_hw_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

10. If you wanted to get a degree in biological or biomedical sciences, which campus confers the majority of these degrees? Produce a numerical summary and an appropriate plot.

```r
univ_calif_final%>%
  arrange(desc(pcip26))
```

```
## # A tibble: 8 x 11
##   univ  campus city  stabbr zip   adm_rate sat_avg pcip26 costt4_a c150_4_pooled
##   <chr> <chr>  <chr> <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>         <dbl>
## 1 Univ~ San D~ La J~ CA     92093    0.357    1324  0.216    31043         0.872
## 2 Univ~ Davis  Davis CA     9561~    0.423    1218  0.198    33904         0.850
## 3 Univ~ Santa~ Sant~ CA     9506~    0.578    1201  0.193    34608         0.776
## 4 Univ~ Los A~ Los ~ CA     9009~    0.180    1334  0.155    33078         0.911
## 5 Univ~ River~ Rive~ CA     92521    0.663    1078  0.149    31494         0.73 
## 6 Univ~ Santa~ Sant~ CA     93106    0.358    1281  0.108    34998         0.816
## 7 Univ~ Irvine Irvi~ CA     92697    0.406    1206  0.107    31198         0.876
## 8 Univ~ Berke~ Berk~ CA     94720    0.169    1422  0.105    34924         0.916
## # ... with 1 more variable: pftftug1_ef <dbl>
```


```r
univ_calif_final%>%
  ggplot(aes(x=campus, y=pcip26))+
  geom_point()+
  coord_flip()
```

![](lab9_hw_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

## Knit Your Output and Post to [GitHub](https://github.com/FRS417-DataScienceBiologists)
