---
title: "Midterm 1"
author: "Carmina Inguito"
date: "`2022-01-27`"
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
library("janitor")
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

**1. (2 points) Consider the definition of data science above. Although we are only part-way through the quarter, what specific elements of data science do you feel we have practiced? Provide at least one specific example.**  
In these past few weeks, we have practiced various elements of data science including manipulating variables, creating data frames, making simple computations, and overall learning how to organize data in a more straightforward way that would make sense to any individual who comes across it (no matter if they have/haven't had previous knowledge of the program). For instance, removing NAs (missing data) within the data by using `na.rm=T`. We can check beforehand for missing data by utilizing `is.na`to help deal with it and keep data neat. While we have removed multiple NAs throughout the quarter so far, one can refer to Lab 4 in regards to organizing herbivore/carnivore data. 

**2. (2 points) What is the most helpful or interesting thing you have learned so far in BIS 15L? What is something that you think needs more work or practice?**  
As I have experienced using RStudio in previous classes like STA100 and my MAT17A class at UC Davis, it was really helpful to be able to delve deeper into the organization of different data and being able to ultimately pull out a specific piece of information you're looking for. From being able to get sort of a refresher on how RStudio works to now being able to create new objects, make data frames, and explore new functions- it certainly has been a hands-on learning experience. I thought creating our own repository and navigating through GitHub was also an interesting first for me. 

Something that I think I need more practice on is getting used to utilizing pipes `%>%`, `if else` or the `|` function. 

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

**3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.**


```r
elephants <- readr::read_csv("data/ElephantsMF.csv") #Create a new object 'elephants' and load the data
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


```r
glimpse(elephants) #Summary of data
```

```
## Rows: 288
## Columns: 3
## $ Age    <dbl> 1.40, 17.50, 12.75, 11.17, 12.67, 12.67, 12.25, 12.17, 28.17, 1~
## $ Height <dbl> 120.00, 227.00, 235.00, 210.00, 220.00, 189.00, 225.00, 204.00,~
## $ Sex    <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M"~
```


```r
skim(elephants) #Another perspective of the data
```


Table: Data summary

|                         |          |
|:------------------------|:---------|
|Name                     |elephants |
|Number of rows           |288       |
|Number of columns        |3         |
|_______________________  |          |
|Column type frequency:   |          |
|character                |1         |
|numeric                  |2         |
|________________________ |          |
|Group variables          |None      |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|Sex           |         0|             1|   1|   1|     0|        2|          0|


**Variable type: numeric**

|skim_variable | n_missing| complete_rate|   mean|   sd|    p0|    p25|    p50|    p75|   p100|hist                                     |
|:-------------|---------:|-------------:|------:|----:|-----:|------:|------:|------:|------:|:----------------------------------------|
|Age           |         0|             1|  10.97|  8.4|  0.01|   4.58|   9.46|  16.50|  32.17|▆▇▂▂▂ |
|Height        |         0|             1| 187.68| 50.6| 75.46| 160.75| 200.00| 221.09| 304.06|▃▃▇▇▁ |

**4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.**

```r
elephants <- janitor::clean_names(elephants) #Removing an uppercase words by using the Janitor package
names(elephants)
```

```
## [1] "age"    "height" "sex"
```

```r
elephants <- elephants %>% #Changing class of variable 'sex' to a factor
  mutate(sex=as.factor(sex))
```


```r
class(elephants$sex)
```

```
## [1] "factor"
```

**5. (2 points) How many male and female elephants are represented in the data?**

```r
table(elephants$sex) 
```

```
## 
##   F   M 
## 150 138
```

There are 138 males and 150 female elephants represented in the data.

**6. (2 points) What is the average age all elephants in the data?**

```r
mean(elephants$age, na.rm=T) 
```

```
## [1] 10.97132
```
The average age of all elephants in the data is about 10.97 or 11 if rounded.

**7. (2 points) How does the average age and height of elephants compare by sex?**


```r
elephants %>%
  group_by(sex) %>%
  summarise(average_age=mean(age),
            average_height=mean(height))
```

```
## # A tibble: 2 x 3
##   sex   average_age average_height
##   <fct>       <dbl>          <dbl>
## 1 F           12.8            190.
## 2 M            8.95           185.
```
Females have a greater average height and age compared to the males. 

**8. (2 points) How does the average height of elephants compare by sex for individuals over 20 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.**  


```r
elephants %>%
  filter(age>20) %>%
  group_by(sex) %>%
  summarise(mean_height=mean(height),
            max_height=max(height), 
            min_height=min(height),
            n_elephants=n())
```

```
## # A tibble: 2 x 5
##   sex   mean_height max_height min_height n_elephants
##   <fct>       <dbl>      <dbl>      <dbl>       <int>
## 1 F            232.       278.       193.          37
## 2 M            270.       304.       229.          13
```
According to the data, females that are over 20 years old have an average height of 232.2 while males over 20 years old have an average height of 269.6 making them with the higher average. 

For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

**9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.**


```r
gabon_africa <-readr::read_csv("data/IvindoData_DryadVersion.csv")
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
summary(gabon_africa) #Getting an overview of the data
```

```
##    TransectID       Distance        HuntCat          NumHouseholds  
##  Min.   : 1.00   Min.   : 2.700   Length:24          Min.   :13.00  
##  1st Qu.: 5.75   1st Qu.: 5.668   Class :character   1st Qu.:24.75  
##  Median :14.50   Median : 9.720   Mode  :character   Median :29.00  
##  Mean   :13.50   Mean   :11.879                      Mean   :37.88  
##  3rd Qu.:20.25   3rd Qu.:17.683                      3rd Qu.:54.00  
##  Max.   :27.00   Max.   :26.760                      Max.   :73.00  
##    LandUse             Veg_Rich       Veg_Stems       Veg_liana     
##  Length:24          Min.   :10.88   Min.   :23.44   Min.   : 4.750  
##  Class :character   1st Qu.:13.10   1st Qu.:28.69   1st Qu.: 9.033  
##  Mode  :character   Median :14.94   Median :32.45   Median :11.940  
##                     Mean   :14.83   Mean   :32.80   Mean   :11.040  
##                     3rd Qu.:16.54   3rd Qu.:37.08   3rd Qu.:13.250  
##                     Max.   :18.75   Max.   :47.56   Max.   :16.380  
##     Veg_DBH        Veg_Canopy    Veg_Understory     RA_Apes      
##  Min.   :28.45   Min.   :2.500   Min.   :2.380   Min.   : 0.000  
##  1st Qu.:40.65   1st Qu.:3.250   1st Qu.:2.875   1st Qu.: 0.000  
##  Median :43.90   Median :3.430   Median :3.000   Median : 0.485  
##  Mean   :46.09   Mean   :3.469   Mean   :3.020   Mean   : 2.045  
##  3rd Qu.:50.58   3rd Qu.:3.750   3rd Qu.:3.167   3rd Qu.: 3.815  
##  Max.   :76.48   Max.   :4.000   Max.   :3.880   Max.   :12.930  
##     RA_Birds      RA_Elephant       RA_Monkeys      RA_Rodent    
##  Min.   :31.56   Min.   :0.0000   Min.   : 5.84   Min.   :1.060  
##  1st Qu.:52.51   1st Qu.:0.0000   1st Qu.:22.70   1st Qu.:2.047  
##  Median :57.90   Median :0.3600   Median :31.74   Median :3.230  
##  Mean   :58.64   Mean   :0.5450   Mean   :31.30   Mean   :3.278  
##  3rd Qu.:68.17   3rd Qu.:0.8925   3rd Qu.:39.88   3rd Qu.:4.093  
##  Max.   :85.03   Max.   :2.3000   Max.   :54.12   Max.   :6.310  
##   RA_Ungulate     Rich_AllSpecies Evenness_AllSpecies Diversity_AllSpecies
##  Min.   : 0.000   Min.   :15.00   Min.   :0.6680      Min.   :1.966       
##  1st Qu.: 1.232   1st Qu.:19.00   1st Qu.:0.7542      1st Qu.:2.248       
##  Median : 2.545   Median :20.00   Median :0.7760      Median :2.316       
##  Mean   : 4.166   Mean   :20.21   Mean   :0.7699      Mean   :2.310       
##  3rd Qu.: 5.157   3rd Qu.:22.00   3rd Qu.:0.8083      3rd Qu.:2.429       
##  Max.   :13.860   Max.   :24.00   Max.   :0.8330      Max.   :2.566       
##  Rich_BirdSpecies Evenness_BirdSpecies Diversity_BirdSpecies Rich_MammalSpecies
##  Min.   : 8.00    Min.   :0.5590       Min.   :1.162         Min.   : 6.000    
##  1st Qu.:10.00    1st Qu.:0.6825       1st Qu.:1.603         1st Qu.: 9.000    
##  Median :11.00    Median :0.7220       Median :1.680         Median :10.000    
##  Mean   :10.33    Mean   :0.7137       Mean   :1.661         Mean   : 9.875    
##  3rd Qu.:11.00    3rd Qu.:0.7722       3rd Qu.:1.784         3rd Qu.:11.000    
##  Max.   :13.00    Max.   :0.8240       Max.   :2.008         Max.   :12.000    
##  Evenness_MammalSpecies Diversity_MammalSpecies
##  Min.   :0.6190         Min.   :1.378          
##  1st Qu.:0.7073         1st Qu.:1.567          
##  Median :0.7390         Median :1.699          
##  Mean   :0.7477         Mean   :1.698          
##  3rd Qu.:0.7847         3rd Qu.:1.815          
##  Max.   :0.8610         Max.   :2.065
```


```r
gabon_africa <- janitor::clean_names(gabon_africa)
names(gabon_africa)
```

```
##  [1] "transect_id"              "distance"                
##  [3] "hunt_cat"                 "num_households"          
##  [5] "land_use"                 "veg_rich"                
##  [7] "veg_stems"                "veg_liana"               
##  [9] "veg_dbh"                  "veg_canopy"              
## [11] "veg_understory"           "ra_apes"                 
## [13] "ra_birds"                 "ra_elephant"             
## [15] "ra_monkeys"               "ra_rodent"               
## [17] "ra_ungulate"              "rich_all_species"        
## [19] "evenness_all_species"     "diversity_all_species"   
## [21] "rich_bird_species"        "evenness_bird_species"   
## [23] "diversity_bird_species"   "rich_mammal_species"     
## [25] "evenness_mammal_species"  "diversity_mammal_species"
```


```r
gabon_africa <- gabon_africa %>% #All names to lowercase
  mutate_all(tolower)
gabon_africa
```

```
## # A tibble: 24 x 26
##    transect_id distance hunt_cat num_households land_use veg_rich veg_stems
##    <chr>       <chr>    <chr>    <chr>          <chr>    <chr>    <chr>    
##  1 1           7.14     moderate 54             park     16.67    31.2     
##  2 2           17.31    none     54             park     15.75    37.44    
##  3 2           18.32    none     29             park     16.88    32.33    
##  4 3           20.85    none     29             logging  12.44    29.39    
##  5 4           15.95    none     29             park     17.13    36       
##  6 5           17.47    none     29             park     16.5     29.22    
##  7 6           24.06    none     29             park     14.75    31.22    
##  8 7           19.81    none     54             logging  13.25    32.56    
##  9 8           5.78     high     25             neither  12.63    23.67    
## 10 9           5.13     high     73             logging  16       27.11    
## # ... with 14 more rows, and 19 more variables: veg_liana <chr>, veg_dbh <chr>,
## #   veg_canopy <chr>, veg_understory <chr>, ra_apes <chr>, ra_birds <chr>,
## #   ra_elephant <chr>, ra_monkeys <chr>, ra_rodent <chr>, ra_ungulate <chr>,
## #   rich_all_species <chr>, evenness_all_species <chr>,
## #   diversity_all_species <chr>, rich_bird_species <chr>,
## #   evenness_bird_species <chr>, diversity_bird_species <chr>,
## #   rich_mammal_species <chr>, evenness_mammal_species <chr>, ...
```


```r
gabon_africa <- gabon_africa %>%
  mutate(land_use=as.factor(land_use), hunt_cat=as.factor(hunt_cat)) #Changing hunt_cat and land_use to factor
```


```r
class(gabon_africa$land_use) #Checking what the variable identifies as 
```

```
## [1] "factor"
```

```r
class(gabon_africa$hunt_cat) #Checking what the variable identifies as 
```

```
## [1] "factor"
```

**10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?**


```r
levels(gabon_africa$hunt_cat) #Ensuring the hunt_cat levels are spelled correctly and lowercase
```

```
## [1] "high"     "moderate" "none"
```

```r
gabon_africa$diversity_bird_species <- as.numeric(gabon_africa$diversity_bird_species)
class(gabon_africa$diversity_bird_species) #Checking the change from factor to numeric
```

```
## [1] "numeric"
```

```r
gabon_africa$diversity_mammal_species <- as.numeric(gabon_africa$diversity_mammal_species)
class(gabon_africa$diversity_mammal_species) #Checking the change from factor to numeric
```

```
## [1] "numeric"
```


```r
gabon_africa %>%
  filter(hunt_cat == "high" | hunt_cat == "moderate") %>% 
  summarise(avg_bird_diversity=mean(diversity_bird_species),avg_mammal_diversity=mean(diversity_mammal_species)) #Calculating the mean diversity
```

```
## # A tibble: 1 x 2
##   avg_bird_diversity avg_mammal_diversity
##                <dbl>                <dbl>
## 1               1.64                 1.71
```
The average diversity between the birds and the mammals are actually fairly close. Mammals have a slightly greater diversity mean at approximately 1.71 while the birds have a diversity mean of approximately 1.64.

**11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 3km from a village to sites that are greater than 25km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.**  


```r
names(gabon_africa)
```

```
##  [1] "transect_id"              "distance"                
##  [3] "hunt_cat"                 "num_households"          
##  [5] "land_use"                 "veg_rich"                
##  [7] "veg_stems"                "veg_liana"               
##  [9] "veg_dbh"                  "veg_canopy"              
## [11] "veg_understory"           "ra_apes"                 
## [13] "ra_birds"                 "ra_elephant"             
## [15] "ra_monkeys"               "ra_rodent"               
## [17] "ra_ungulate"              "rich_all_species"        
## [19] "evenness_all_species"     "diversity_all_species"   
## [21] "rich_bird_species"        "evenness_bird_species"   
## [23] "diversity_bird_species"   "rich_mammal_species"     
## [25] "evenness_mammal_species"  "diversity_mammal_species"
```


```r
gabon_africa$distance <- as.numeric(gabon_africa$distance) #factor to numeric
gabon_africa$ra_ungulate <- as.numeric(gabon_africa$ra_ungulate)
gabon_africa$ra_rodent <- as.numeric(gabon_africa$ra_rodent)
gabon_africa$ra_monkeys <- as.numeric(gabon_africa$ra_monkeys)
gabon_africa$ra_elephant <- as.numeric(gabon_africa$ra_elephant)
gabon_africa$ra_birds <- as.numeric(gabon_africa$ra_birds)
gabon_africa$ra_apes <- as.numeric(gabon_africa$ra_apes)
```


```r
gabon_africa %>%
  filter(distance<3) %>% #filtering for distance under 3km
  summarise((across(c(ra_apes, ra_birds, ra_elephant, ra_monkeys, ra_rodent, ra_ungulate), mean, na.rm=T)))
```

```
## # A tibble: 1 x 6
##   ra_apes ra_birds ra_elephant ra_monkeys ra_rodent ra_ungulate
##     <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1    0.12     76.6       0.145       17.3      3.90        1.87
```

```r
gabon_africa %>%
  filter(distance>25) %>% #filtering for distance over 25km 
  summarise((across(c(ra_apes, ra_birds, ra_elephant, ra_monkeys, ra_rodent, ra_ungulate), mean, na.rm=T)))
```

```
## # A tibble: 1 x 6
##   ra_apes ra_birds ra_elephant ra_monkeys ra_rodent ra_ungulate
##     <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1    4.91     31.6           0       54.1      1.29        8.12
```

**12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`**


```r
levels(gabon_africa$land_use)
```

```
## [1] "logging" "neither" "park"
```

Here, I am interested in finding the greatest amount (max) of species, least (min) amount of mammal species and the mean-- who utilize the park as their land.

```r
gabon_africa %>%
  filter(land_use=="park") %>%
  group_by(diversity_mammal_species) %>%
  summarise(avg_mammal_all=mean(diversity_mammal_species),min_mammal_all=min(diversity_mammal_species), max_mammal_all=max(diversity_mammal_species))
```

```
## # A tibble: 7 x 4
##   diversity_mammal_species avg_mammal_all min_mammal_all max_mammal_all
##                      <dbl>          <dbl>          <dbl>          <dbl>
## 1                     1.56           1.56           1.56           1.56
## 2                     1.62           1.62           1.62           1.62
## 3                     1.72           1.72           1.72           1.72
## 4                     1.76           1.76           1.76           1.76
## 5                     1.81           1.81           1.81           1.81
## 6                     1.83           1.83           1.83           1.83
## 7                     1.93           1.93           1.93           1.93
```

