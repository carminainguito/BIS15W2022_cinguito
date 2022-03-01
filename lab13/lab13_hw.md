---
title: "Lab 13 Homework"
author: "Carmina Inguito"
date: "`2022-03-01`"
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
if (!require("tidyverse")) install.packages('tidyverse')
```

```
## Loading required package: tidyverse
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --
```

```
## v ggplot2 3.3.5     v purrr   0.3.4
## v tibble  3.1.6     v dplyr   1.0.8
## v tidyr   1.2.0     v stringr 1.4.0
## v readr   2.1.2     v forcats 0.5.1
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```


```r
library(tidyverse)
library(shiny)
library(shinydashboard)
library(skimr)
library(janitor)
```

## Choose Your Adventure!
For this homework assignment, you have two choices of data. You only need to build an app for one of them. The first dataset is focused on UC Admissions and the second build on the Gabon data that we used for midterm 1.  

## Option 1
The data for this assignment come from the [University of California Information Center](https://www.universityofcalifornia.edu/infocenter). Admissions data were collected for the years 2010-2019 for each UC campus. Admissions are broken down into three categories: applications, admits, and enrollees. The number of individuals in each category are presented by demographic.  

**1. Load the `UC_admit.csv` data and use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine if there are NA's and how they are treated.**  

```r
UC_admit <- readr::read_csv("data/UC_admit.csv") %>% clean_names()
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
skim(UC_admit)
```


Table: Data summary

|                         |         |
|:------------------------|:--------|
|Name                     |UC_admit |
|Number of rows           |2160     |
|Number of columns        |6        |
|_______________________  |         |
|Column type frequency:   |         |
|character                |4        |
|numeric                  |2        |
|________________________ |         |
|Group variables          |None     |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|campus        |         0|             1|   5|  13|     0|        9|          0|
|category      |         0|             1|   6|  10|     0|        3|          0|
|ethnicity     |         0|             1|   3|  16|     0|        8|          0|
|perc_fr       |         1|             1|   5|   7|     0|     1293|          0|


**Variable type: numeric**

|skim_variable     | n_missing| complete_rate|    mean|       sd|   p0|    p25|    p50|    p75|   p100|hist                                     |
|:-----------------|---------:|-------------:|-------:|--------:|----:|------:|------:|------:|------:|:----------------------------------------|
|academic_yr       |         0|             1| 2014.50|     2.87| 2010| 2012.0| 2014.5| 2017.0|   2019|▇▇▇▇▇ |
|filtered_count_fr |         1|             1| 7142.63| 13808.91|    1|  447.5| 1837.0| 6899.5| 113755|▇▁▁▁▁ |


```r
glimpse(UC_admit)
```

```
## Rows: 2,160
## Columns: 6
## $ campus            <chr> "Davis", "Davis", "Davis", "Davis", "Davis", "Davis"~
## $ academic_yr       <dbl> 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2018~
## $ category          <chr> "Applicants", "Applicants", "Applicants", "Applicant~
## $ ethnicity         <chr> "International", "Unknown", "White", "Asian", "Chica~
## $ perc_fr           <chr> "21.16%", "2.51%", "18.39%", "30.76%", "22.44%", "0.~
## $ filtered_count_fr <dbl> 16522, 1959, 14360, 24024, 17526, 277, 3425, 78093, ~
```


```r
summary(UC_admit)
```

```
##     campus           academic_yr     category          ethnicity        
##  Length:2160        Min.   :2010   Length:2160        Length:2160       
##  Class :character   1st Qu.:2012   Class :character   Class :character  
##  Mode  :character   Median :2014   Mode  :character   Mode  :character  
##                     Mean   :2014                                        
##                     3rd Qu.:2017                                        
##                     Max.   :2019                                        
##                                                                         
##    perc_fr          filtered_count_fr 
##  Length:2160        Min.   :     1.0  
##  Class :character   1st Qu.:   447.5  
##  Mode  :character   Median :  1837.0  
##                     Mean   :  7142.6  
##                     3rd Qu.:  6899.5  
##                     Max.   :113755.0  
##                     NA's   :1
```


```r
naniar::miss_var_summary(UC_admit)
```

```
## # A tibble: 6 x 3
##   variable          n_miss pct_miss
##   <chr>              <int>    <dbl>
## 1 perc_fr                1   0.0463
## 2 filtered_count_fr      1   0.0463
## 3 campus                 0   0     
## 4 academic_yr            0   0     
## 5 category               0   0     
## 6 ethnicity              0   0
```


```r
names(UC_admit)
```

```
## [1] "campus"            "academic_yr"       "category"         
## [4] "ethnicity"         "perc_fr"           "filtered_count_fr"
```

**2. The president of UC has asked you to build a shiny app that shows admissions by ethnicity across all UC campuses. Your app should allow users to explore year, campus, and admit category as interactive variables. Use shiny dashboard and try to incorporate the aesthetics you have learned in ggplot to make the app neat and clean.**

```r
ui <- dashboardPage(
  dashboardHeader(title = "UC Campuses Admission"),
  dashboardSidebar(disable = T),
  dashboardBody(
    fluidRow(
      box( title = "Select Variable",width = 4,
    selectInput("x","Select Year",
                choices = c("2010","2011","2012","2013","2014","2015","2016","2017","2018","2019"),
                selected = "2010"),
    selectInput("y","Select Campus",
                choices = c("Berkeley","Davis","Irvine","Los_Angeles","Merced","Riverside","San_Diego","Santa_Barbara","Santa_Cruz"),
                selected = "Berkeley"),
    selectInput("z","Select Specific Category",
                choices = c("Admits","Enrollees","Applicants"),
                selected = "Admits")
      ),
    box(
      title = "UC Admission Plot",width = 7,
      plotOutput("plot",width = "600px",height = "700px")
    )
    )
    )
)

server <- function(input, output,session) { 
  output$plot<-renderPlot({
   UC_admit%>%
      filter(academic_yr==input$x&campus==input$y&category==input$z)%>%
      ggplot(aes_string(x="ethnicity",y="filtered_count_fr",fill="ethnicity"))+
      geom_col(alpha=0.8,color="Black")+
      theme_linedraw()+
      theme(axis.text.x = element_text(hjust = 1,angle = 60))+ 
      labs(x="Ethnicity", y="Flitered Count")
  })
  session$onSessionEnded(stopApp)
  }

shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}


**3. Make alternate version of your app above by tracking enrollment at a campus over all of the represented years while allowing users to interact with campus, category, and ethnicity.**  

```r
ui <- dashboardPage(skin="blue",
  dashboardHeader(),
  dashboardSidebar(disable=T),
  dashboardBody(box(width=3,
    selectInput("campus", "Select UC Campus", choices=c("Davis", "Berkeley", "Santa_Barbara", "San_Diego", "Merced", "Irvine", "Los_Angeles", "Riverside", "Santa_Cruz"), selected="Davis"),
    
    selectInput("ethnicity", "Select Ethnicity", choices=c("African American", "American Indian", "Asian", "Chicano/Latino", "International", "Unknown", "White"), selected="American Indian"),
    
    radioButtons("category", "Select Category of Applicants", choices=c("Enrollees", "Applicants", "Admits"))
    ),
    plotOutput("plot", width="600px", height="500px")
    
    )
)

server <- function(input, output, session) {
  output$plot <- renderPlot({
    UC_admit %>%
      filter(ethnicity==input$ethnicity) %>%
      filter(campus==input$campus & category==input$category) %>%
      ggplot(aes_string(x="academic_yr", y="perc_fr")) +geom_col(fill="red", alpha=0.4, color="black") +ylim(0,1) +labs(x="Year", y="Percent", title="Admissions Stats (2010-2019) by Ethnicity") +coord_flip() +scale_y_continuous(limits=c(2010, 2019), breaks = c(2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018, 2019)) +theme_linedraw()+ theme(plot.title=element_text(face="bold", hjust=0.5))
    
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
