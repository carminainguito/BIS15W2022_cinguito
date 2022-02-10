---
title: "Lab 10 Homework"
author: "Carmina Inguito"
date: "`2022-10-02`"
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
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (6): species_id, sex, genus, species, taxa, plot_type
## dbl (7): record_id, month, day, year, plot_id, hindfoot_length, weight
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

**1. Use the function(s) of your choice to get an idea of its structure, including how NA's are treated. Are the data tidy?**

```r
glimpse(deserts)
```

```
## Rows: 34,786
## Columns: 13
## $ record_id       <dbl> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16,~
## $ month           <dbl> 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, ~
## $ day             <dbl> 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16~
## $ year            <dbl> 1977, 1977, 1977, 1977, 1977, 1977, 1977, 1977, 1977, ~
## $ plot_id         <dbl> 2, 3, 2, 7, 3, 1, 2, 1, 1, 6, 5, 7, 3, 8, 6, 4, 3, 2, ~
## $ species_id      <chr> "NL", "NL", "DM", "DM", "DM", "PF", "PE", "DM", "DM", ~
## $ sex             <chr> "M", "M", "F", "M", "M", "M", "F", "M", "F", "F", "F",~
## $ hindfoot_length <dbl> 32, 33, 37, 36, 35, 14, NA, 37, 34, 20, 53, 38, 35, NA~
## $ weight          <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA~
## $ genus           <chr> "Neotoma", "Neotoma", "Dipodomys", "Dipodomys", "Dipod~
## $ species         <chr> "albigula", "albigula", "merriami", "merriami", "merri~
## $ taxa            <chr> "Rodent", "Rodent", "Rodent", "Rodent", "Rodent", "Rod~
## $ plot_type       <chr> "Control", "Long-term Krat Exclosure", "Control", "Rod~
```


```r
names(deserts)
```

```
##  [1] "record_id"       "month"           "day"             "year"           
##  [5] "plot_id"         "species_id"      "sex"             "hindfoot_length"
##  [9] "weight"          "genus"           "species"         "taxa"           
## [13] "plot_type"
```

**2. How many genera and species are represented in the data? What are the total number of observations? Which species is most/ least frequently sampled in the study?**

```r
deserts %>%
  summarise(num_genera=n_distinct(genus),
            num_species=n_distinct(species),
            n_total=n())
```

```
## # A tibble: 1 x 3
##   num_genera num_species n_total
##        <int>       <int>   <int>
## 1         26          40   34786
```

**3. What is the proportion of taxa included in this study? Show a table and plot that reflects this count.**

```r
deserts %>%
  count(taxa)
```

```
## # A tibble: 4 x 2
##   taxa        n
##   <chr>   <int>
## 1 Bird      450
## 2 Rabbit     75
## 3 Reptile    14
## 4 Rodent  34247
```


```r
deserts %>%
  ggplot(aes(x= taxa, fill= taxa)) +
  geom_bar() +
  scale_y_log10()+
  theme(plot.title=element_text(hjust=0.5)) +
  labs(title= "Proportion of Taxa",
       x= "taxa",
       y="count")
```

![](lab10_hw_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

**4. For the taxa included in the study, use the fill option to show the proportion of individuals sampled by `plot_type.`**

```r
deserts %>%
  ggplot(aes(x=taxa, fill=plot_type))+
  geom_bar(position="dodge")+
  labs(title= "Proportion of Taxa by Plot Type",
       x="taxa",
       y="ratio")+
  scale_y_log10()+
  theme(plot.title = element_text(hjust=0.5))
```

![](lab10_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

**5. What is the range of weight for each species included in the study? Remove any observations of weight that are NA so they do not show up in the plot.**

```r
deserts %>%
  filter(!is.na(weight)) %>%
  ggplot(aes(x=species, y=weight))+
  geom_boxplot()+
  theme(axis.text.x= element_text(angle=65, hjust=1))+
  labs(title="Weight of Species within Study",
       x="species",
       y="weight")+
  theme(plot.title=element_text(hjust=0.5))
```

![](lab10_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

**6. Add another layer to your answer from #4 using `geom_point` to get an idea of how many measurements were taken for each species.**

```r
deserts %>%
  filter(!is.na(weight)) %>%
  ggplot(aes(x=species_id, y=weight))+
  geom_boxplot()+
  geom_point(alpha=0.3, color="turquoise", position="jitter")+
  coord_flip()+
  labs(title= "Range of Weight for Each Species",
       x="species ID",
       y="weight")+
  theme(plot.title=element_text(hjust=0.5))
```

![](lab10_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

**7. [Dipodomys merriami](https://en.wikipedia.org/wiki/Merriam's_kangaroo_rat) is the most frequently sampled animal in the study. How have the number of observations of this species changed over the years included in the study?**

```r
deserts %>% 
  group_by(year) %>% 
  filter(species_id=="DM") %>% 
  summarise(n_samples=n()) %>% 
  ggplot(aes(x=as.factor(year), y=n_samples))+geom_col()+
  theme(axis.text.x=element_text(angle=60, hjust=1))+
  labs(title = "Dipodomys merriami Observations",
       x=NULL,
       y="n")+
theme(plot.title=element_text(hjust=0.5))
```

![](lab10_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

**8. What is the relationship between `weight` and `hindfoot` length? Consider whether or not over plotting is an issue.**

```r
deserts %>% 
  ggplot(aes(x=weight, y=hindfoot_length, color=species_id))+
  geom_jitter(na.rm=T)+
  labs(title="Weight vs Hindfoot Length",
       x="weight",
       y="hindfoot length")
```

![](lab10_hw_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

**9. Which two species have, on average, the highest weight? Once you have identified them, make a new column that is a ratio of `weight` to `hindfoot_length`. Make a plot that shows the range of this new ratio and fill by sex.**

```r
deserts %>% 
  filter(weight!="NA") %>% 
  group_by(species_id) %>% 
  summarize(mean_weight=mean(weight)) %>% 
  arrange(desc(mean_weight))
```

```
## # A tibble: 25 x 2
##    species_id mean_weight
##    <chr>            <dbl>
##  1 NL               159. 
##  2 DS               120. 
##  3 SS                93.5
##  4 SH                73.1
##  5 SF                58.9
##  6 SO                55.4
##  7 DO                48.9
##  8 DM                43.2
##  9 PB                31.7
## 10 OL                31.6
## # ... with 15 more rows
```


```r
deserts %>% 
  filter(species_id=="NL"|species_id=="DS") %>% 
  filter(weight!="NA" & hindfoot_length!="NA")%>% 
  mutate(ratio=weight/hindfoot_length) %>% 
  select(species_id, sex, weight, hindfoot_length, ratio)
```

```
## # A tibble: 3,072 x 5
##    species_id sex   weight hindfoot_length ratio
##    <chr>      <chr>  <dbl>           <dbl> <dbl>
##  1 DS         F        117              50  2.34
##  2 DS         F        121              51  2.37
##  3 DS         M        115              51  2.25
##  4 DS         F        120              48  2.5 
##  5 DS         F        118              48  2.46
##  6 DS         F        126              52  2.42
##  7 DS         M        132              50  2.64
##  8 DS         F        122              53  2.30
##  9 DS         F        107              48  2.23
## 10 DS         F        115              50  2.3 
## # ... with 3,062 more rows
```


```r
deserts %>% 
  filter(species_id=="NL"|species_id=="DS") %>% 
  filter(weight!="NA" & hindfoot_length!="NA")%>% 
  mutate(ratio=weight/hindfoot_length) %>% 
  select(species_id, sex, weight, hindfoot_length, ratio) %>% 
  ggplot(aes(x=species_id, y=ratio, fill=sex))+
  geom_boxplot()+
  labs(title="Range of Weight/Hindfoot Length for species NL and DS",
       x="species ID",
       y="ratio")
```

![](lab10_hw_files/figure-html/unnamed-chunk-15-1.png)<!-- -->


**10. Make one plot of your choice! Make sure to include at least two of the aesthetics options you have learned.**



```r
deserts%>%
  filter(weight!="NA")%>%
  filter(species=="ochrognathus")%>%
  ggplot(aes(x=year,y=weight,fill=year))+geom_col()+
  scale_y_log10()+
  theme(plot.title = element_text(size=rel(1.5),hjust=0.5))+
  labs(title="Changes of Weight for Ochrognathus over Years",
       x="year",
       y="weight")
```

![](lab10_hw_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
