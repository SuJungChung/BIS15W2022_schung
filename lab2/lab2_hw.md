---
title: "Lab 2 Homework"
author: "Sujung Chung"
date: "2022-01-11"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

1. What is a vector in R?  
A simple set of data that can be listed.

2. What is a data matrix in R?  
A series of organized vectors that are organized in rows and columns.

3. Below are data collected by three scientists (Jill, Steve, Susan in order) measuring temperatures of eight hot springs. Run this code chunk to create the vectors.  

```r
spring_1 <- c(36.25, 35.40, 35.30)
spring_2 <- c(35.15, 35.35, 33.35)
spring_3 <- c(30.70, 29.65, 29.20)
spring_4 <- c(39.70, 40.05, 38.65)
spring_5 <- c(31.85, 31.40, 29.30)
spring_6 <- c(30.20, 30.65, 29.75)
spring_7 <- c(32.90, 32.50, 32.80)
spring_8 <- c(36.80, 36.45, 33.15)
```

4. Build a data matrix that has the springs as rows and the columns as scientists. 


```r
total_spring <- c(spring_1, spring_2, spring_3, spring_4, spring_5, spring_6, spring_7, spring_8)
total_spring
```

```
##  [1] 36.25 35.40 35.30 35.15 35.35 33.35 30.70 29.65 29.20 39.70 40.05 38.65
## [13] 31.85 31.40 29.30 30.20 30.65 29.75 32.90 32.50 32.80 36.80 36.45 33.15
```


```r
data_matrix <- matrix(total_spring, nrow = 8, byrow = T)
data_matrix
```

```
##       [,1]  [,2]  [,3]
## [1,] 36.25 35.40 35.30
## [2,] 35.15 35.35 33.35
## [3,] 30.70 29.65 29.20
## [4,] 39.70 40.05 38.65
## [5,] 31.85 31.40 29.30
## [6,] 30.20 30.65 29.75
## [7,] 32.90 32.50 32.80
## [8,] 36.80 36.45 33.15
```

```r
scientist <- c("Jill","Steve","Susan")
scientist
```

```
## [1] "Jill"  "Steve" "Susan"
```

```r
spring <- c("Spring 1", "Spring 2", "Spring 3", "Spring 4", "Spring 5", "Spring 6", "Spring 7", "Spring 8")
spring
```

```
## [1] "Spring 1" "Spring 2" "Spring 3" "Spring 4" "Spring 5" "Spring 6" "Spring 7"
## [8] "Spring 8"
```

```r
colnames(data_matrix) <- scientist
```

```r
rownames(data_matrix) <- spring
```

```r
data_matrix
```

```
##           Jill Steve Susan
## Spring 1 36.25 35.40 35.30
## Spring 2 35.15 35.35 33.35
## Spring 3 30.70 29.65 29.20
## Spring 4 39.70 40.05 38.65
## Spring 5 31.85 31.40 29.30
## Spring 6 30.20 30.65 29.75
## Spring 7 32.90 32.50 32.80
## Spring 8 36.80 36.45 33.15
```
5. The names of the springs are 1.Bluebell Spring, 2.Opal Spring, 3.Riverside Spring, 4.Too Hot Spring, 5.Mystery Spring, 6.Emerald Spring, 7.Black Spring, 8.Pearl Spring. Name the rows and columns in the data matrix. Start by making two new vectors with the names, then use `colnames()` and `rownames()` to name the columns and rows.

```r
spring <- c("Bluebell Spring", "Opal Spring", "Riverside Spring", "TOO HOT Spring", "Mystery Spring", "Emerald Spring", "Black Spring", "Pearl Spring")
spring
```

```
## [1] "Bluebell Spring"  "Opal Spring"      "Riverside Spring" "TOO HOT Spring"  
## [5] "Mystery Spring"   "Emerald Spring"   "Black Spring"     "Pearl Spring"
```

```r
rownames(data_matrix) <- spring
```

```r
data_matrix
```

```
##                   Jill Steve Susan
## Bluebell Spring  36.25 35.40 35.30
## Opal Spring      35.15 35.35 33.35
## Riverside Spring 30.70 29.65 29.20
## TOO HOT Spring   39.70 40.05 38.65
## Mystery Spring   31.85 31.40 29.30
## Emerald Spring   30.20 30.65 29.75
## Black Spring     32.90 32.50 32.80
## Pearl Spring     36.80 36.45 33.15
```

6. Calculate the mean temperature of all eight springs.

```r
Bluebell_Spring_All <- data_matrix[1,]
Bluebell_Spring_All
```

```
##  Jill Steve Susan 
## 36.25 35.40 35.30
```

```r
mean(Bluebell_Spring_All)
```

```
## [1] 35.65
```



```r
Opal_Spring_All <- data_matrix[2,]
Opal_Spring_All
```

```
##  Jill Steve Susan 
## 35.15 35.35 33.35
```

```r
mean(Opal_Spring_All)
```

```
## [1] 34.61667
```

```r
Riverside_Spring_All <- data_matrix[3,]
Riverside_Spring_All
```

```
##  Jill Steve Susan 
## 30.70 29.65 29.20
```

```r
mean(Riverside_Spring_All)
```

```
## [1] 29.85
```

```r
TOO_HOT_Spring_All <- data_matrix[4,]
TOO_HOT_Spring_All
```

```
##  Jill Steve Susan 
## 39.70 40.05 38.65
```

```r
mean(TOO_HOT_Spring_All)
```

```
## [1] 39.46667
```

```r
Mystery_Spring_All <- data_matrix[5,]
Mystery_Spring_All
```

```
##  Jill Steve Susan 
## 31.85 31.40 29.30
```

```r
mean(Mystery_Spring_All)
```

```
## [1] 30.85
```

```r
Emerald_Spring_All <- data_matrix[6,]
Emerald_Spring_All
```

```
##  Jill Steve Susan 
## 30.20 30.65 29.75
```

```r
mean(Emerald_Spring_All)
```

```
## [1] 30.2
```

```r
Black_Spring_All <- data_matrix[7,]
Black_Spring_All
```

```
##  Jill Steve Susan 
##  32.9  32.5  32.8
```

```r
mean(Black_Spring_All)
```

```
## [1] 32.73333
```

```r
Pearl_Spring_All <- data_matrix[8,]
Pearl_Spring_All
```

```
##  Jill Steve Susan 
## 36.80 36.45 33.15
```

```r
mean(Pearl_Spring_All)
```

```
## [1] 35.46667
```

```r
Average <- rowMeans(data_matrix)
Average
```

```
##  Bluebell Spring      Opal Spring Riverside Spring   TOO HOT Spring 
##         35.65000         34.61667         29.85000         39.46667 
##   Mystery Spring   Emerald Spring     Black Spring     Pearl Spring 
##         30.85000         30.20000         32.73333         35.46667
```


7. Add this as a new column in the data matrix.  

```r
final_data_matrix <- cbind(data_matrix, Average)
final_data_matrix
```

```
##                   Jill Steve Susan  Average
## Bluebell Spring  36.25 35.40 35.30 35.65000
## Opal Spring      35.15 35.35 33.35 34.61667
## Riverside Spring 30.70 29.65 29.20 29.85000
## TOO HOT Spring   39.70 40.05 38.65 39.46667
## Mystery Spring   31.85 31.40 29.30 30.85000
## Emerald Spring   30.20 30.65 29.75 30.20000
## Black Spring     32.90 32.50 32.80 32.73333
## Pearl Spring     36.80 36.45 33.15 35.46667
```

8. Show Susan's value for Opal Spring only.

```r
final_data_matrix[1,3]
```

```
## [1] 35.3
```

9. Calculate the mean for Jill's column only.  

```r
Jill_Average <- mean(final_data_matrix[,1])
Jill_Average
```

```
## [1] 34.19375
```

10. Use the data matrix to perform one calculation or operation of your interest.

```r
Average_All_Springs <- mean (final_data_matrix[,4])
Average_All_Springs
```

```
## [1] 33.60417
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
