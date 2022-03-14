---
title: "Lab 13 Homework"
author: "Sujung Chung"
date: "2022-03-14"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Libraries

```r
library(tidyverse)
#install.packages("shiny")
library(shiny)
library(shinydashboard)
library(janitor)
```

## Choose Your Adventure!
For this homework assignment, you have two choices of data. You only need to build an app for one of them. The first dataset is focused on UC Admissions and the second build on the Gabon data that we used for midterm 1.  

## Option 1
The data for this assignment come from the [University of California Information Center](https://www.universityofcalifornia.edu/infocenter). Admissions data were collected for the years 2010-2019 for each UC campus. Admissions are broken down into three categories: applications, admits, and enrollees. The number of individuals in each category are presented by demographic.  

**1. Load the `UC_admit.csv` data and use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine if there are NA's and how they are treated.**  

```r
uc<-readr::read_csv("data/uc_data/UC_admit.csv")
```

```
## Rows: 2160 Columns: 6
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (4): Campus, Category, Ethnicity, Perc FR
## dbl (2): Academic_Yr, FilteredCountFR
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
summary(uc)
```

```
##     Campus           Academic_Yr     Category          Ethnicity        
##  Length:2160        Min.   :2010   Length:2160        Length:2160       
##  Class :character   1st Qu.:2012   Class :character   Class :character  
##  Mode  :character   Median :2014   Mode  :character   Mode  :character  
##                     Mean   :2014                                        
##                     3rd Qu.:2017                                        
##                     Max.   :2019                                        
##                                                                         
##    Perc FR          FilteredCountFR   
##  Length:2160        Min.   :     1.0  
##  Class :character   1st Qu.:   447.5  
##  Mode  :character   Median :  1837.0  
##                     Mean   :  7142.6  
##                     3rd Qu.:  6899.5  
##                     Max.   :113755.0  
##                     NA's   :1
```

```r
uc <- clean_names(uc)
uc
```

```
## # A tibble: 2,160 x 6
##    campus academic_yr category   ethnicity        perc_fr filtered_count_fr
##    <chr>        <dbl> <chr>      <chr>            <chr>               <dbl>
##  1 Davis         2019 Applicants International    21.16%              16522
##  2 Davis         2019 Applicants Unknown          2.51%                1959
##  3 Davis         2019 Applicants White            18.39%              14360
##  4 Davis         2019 Applicants Asian            30.76%              24024
##  5 Davis         2019 Applicants Chicano/Latino   22.44%              17526
##  6 Davis         2019 Applicants American Indian  0.35%                 277
##  7 Davis         2019 Applicants African American 4.39%                3425
##  8 Davis         2019 Applicants All              100.00%             78093
##  9 Davis         2018 Applicants International    19.87%              15507
## 10 Davis         2018 Applicants Unknown          2.83%                2208
## # ... with 2,150 more rows
```

```r
uc %>% 
  summarise_all(~(sum(is.na(.))))
```

```
## # A tibble: 1 x 6
##   campus academic_yr category ethnicity perc_fr filtered_count_fr
##    <int>       <int>    <int>     <int>   <int>             <int>
## 1      0           0        0         0       1                 1
```

**2. The president of UC has asked you to build a shiny app that shows admissions by ethnicity across all UC campuses. Your app should allow users to explore year, campus, and admit category as interactive variables. Use shiny dashboard and try to incorporate the aesthetics you have learned in ggplot to make the app neat and clean.**

```r
uc%>%
  distinct(academic_yr)
```

```
## # A tibble: 10 x 1
##    academic_yr
##          <dbl>
##  1        2019
##  2        2018
##  3        2017
##  4        2016
##  5        2015
##  6        2014
##  7        2013
##  8        2012
##  9        2011
## 10        2010
```

```r
uc%>%
  distinct(campus)
```

```
## # A tibble: 9 x 1
##   campus       
##   <chr>        
## 1 Davis        
## 2 Berkeley     
## 3 Irvine       
## 4 Los_Angeles  
## 5 Merced       
## 6 Riverside    
## 7 San_Diego    
## 8 Santa_Barbara
## 9 Santa_Cruz
```

```r
uc%>%
  distinct(category)
```

```
## # A tibble: 3 x 1
##   category  
##   <chr>     
## 1 Applicants
## 2 Admits    
## 3 Enrollees
```


```r
library(shiny)

ui <- dashboardPage(
  dashboardHeader(title ="Admissions across all UC campuses"),
  dashboardSidebar(disable = T),
  dashboardBody(
    fluidRow(
      box(title = "Interactive Variables", width = 4,
        selectInput("x", "Select Year", choices = c("2010", "2011", "2012", "2013", "2014" , "2015", "2016", "2017", "2018", "2019")),
   selectInput("y", "Select Campus", choices = c("Davis", "Berkeley", "Irvine", "Los_Angeles","Merced","Riverside","San_Diego","Santa_Barbara", "Santa_Cruz")),
    selectInput("z", "Select Category", choices = c("Applicants", "Admits", "Enrollees"))),
box(title = "Admission by Ethnicity", width = 7,
          plotOutput("plot", width ="700px", height ="800px"))
)
)
)

server <- function(input, output, session) {
  output$plot <- renderPlot({
    
    uc%>%
      filter(academic_yr==input$x & campus ==input$y & category ==input$z)%>%
  ggplot(aes(x = ethnicity, y = filtered_count_fr)) +
      geom_col() + 
      theme_light(base_size = 18) +
      labs(x="Ethnicity",
           y="Number of Individuals")
  })
  session$onSessionEnded(stopApp)
}

shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}



**3. Make alternate version of your app above by tracking enrollment at a campus over all of the represented years while allowing users to interact with campus, category, and ethnicity.**


```r
uc%>%
  distinct(ethnicity)
```

```
## # A tibble: 8 x 1
##   ethnicity       
##   <chr>           
## 1 International   
## 2 Unknown         
## 3 White           
## 4 Asian           
## 5 Chicano/Latino  
## 6 American Indian 
## 7 African American
## 8 All
```


```r
library(shiny)

ui <- dashboardPage(
  dashboardHeader(title ="Admission Stats for UC Schools"),
  dashboardSidebar(disable = T),
  dashboardBody(
    fluidRow(
      box(title = "Interactive Variables", width = 4,
        selectInput("x", "Select Ethnicity", choices = c("International", "Unknown", "White", "Asian", "Chicano/Latino", "American Indian", "African America", "All")),
   selectInput("y", "Select Campus", choices = c("Davis", "Berkeley", "Irvine", "Los_Angeles","Merced","Riverside","San_Diego","Santa_Barbara", "Santa_Cruz")),
    selectInput("z", "Select Category", choices = c("Applicants", "Admits", "Enrollees"))),
box(title = "Admission by Ethnicity", width = 7,
          plotOutput("plot", width ="700px", height ="800px"))
)
)
)

server <- function(input, output, session) {
  output$plot <- renderPlot({
    
    uc%>%
      filter(ethnicity==input$x & campus == input$y & category ==input$z)%>%
  ggplot(aes(x = academic_yr, y = filtered_count_fr)) +
      geom_col() + 
      theme_light(base_size = 18) +
      labs(x="Year",
           y="Number of Individuals")
  })
  session$onSessionEnded(stopApp)
}

shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}


## Option 2
We will use data from a study on vertebrate community composition and impacts from defaunation in Gabon, Africa. Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016.   

**1. Load the `IvindoData_DryadVersion.csv` data and use the function(s) of your choice to get an idea of the overall structure, including its dimensions, column names, variable classes, etc. As part of this, determine if NA's are present and how they are treated.**  

**2. Build an app that re-creates the plots shown on page 810 of this paper. The paper is included in the folder. It compares the relative abundance % to the distance from villages in rural Gabon. Use shiny dashboard and add aesthetics to the plot.  **  

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
