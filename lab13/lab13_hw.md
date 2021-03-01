---
title: "Lab 13 Homework"
author: "Allison Andre"
date: "2021-02-27"
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
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.0 ──
```

```
## ✓ ggplot2 3.3.3     ✓ purrr   0.3.4
## ✓ tibble  3.0.6     ✓ dplyr   1.0.4
## ✓ tidyr   1.1.2     ✓ stringr 1.4.0
## ✓ readr   1.4.0     ✓ forcats 0.5.1
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```


```r
library(tidyverse)
library(shiny)
library(shinydashboard)
library(janitor)
library(skimr)
```

## Data
The data for this assignment come from the [University of California Information Center](https://www.universityofcalifornia.edu/infocenter). Admissions data were collected for the years 2010-2019 for each UC campus. Admissions are broken down into three categories: applications, admits, and enrollees. The number of individuals in each category are presented by demographic.  

```r
UC_admit <- readr::read_csv("data/UC_admit.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   Campus = col_character(),
##   Academic_Yr = col_double(),
##   Category = col_character(),
##   Ethnicity = col_character(),
##   `Perc FR` = col_character(),
##   FilteredCountFR = col_double()
## )
```

**1. Use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine if there are NA's and how they are treated.**  

```r
glimpse(UC_admit)
```

```
## Rows: 2,160
## Columns: 6
## $ Campus          <chr> "Davis", "Davis", "Davis", "Davis", "Davis", "Davis", …
## $ Academic_Yr     <dbl> 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2018, …
## $ Category        <chr> "Applicants", "Applicants", "Applicants", "Applicants"…
## $ Ethnicity       <chr> "International", "Unknown", "White", "Asian", "Chicano…
## $ `Perc FR`       <chr> "21.16%", "2.51%", "18.39%", "30.76%", "22.44%", "0.35…
## $ FilteredCountFR <dbl> 16522, 1959, 14360, 24024, 17526, 277, 3425, 78093, 15…
```

```r
summary(UC_admit)
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
UC_admit_tidy <- 
  clean_names(UC_admit) %>% 
  filter(ethnicity != "All")
names(UC_admit_tidy)
```

```
## [1] "campus"            "academic_yr"       "category"         
## [4] "ethnicity"         "perc_fr"           "filtered_count_fr"
```

Looking at NAs. There are 2 total in the data, in addition to Unknowns in the ethnicity data (which still give us information)

```r
UC_admit_tidy %>% 
  summarize(num_nas = sum(is.na(UC_admit_tidy)))
```

```
## # A tibble: 1 x 1
##   num_nas
##     <int>
## 1       2
```

Making the character columns into factors

```r
UC_admit_tidy$campus <- as.factor(UC_admit_tidy$campus)
UC_admit_tidy$ethnicity <- as.factor(UC_admit_tidy$ethnicity)
UC_admit_tidy$perc_fr <- as.factor(UC_admit_tidy$perc_fr)
UC_admit_tidy$category <- as.factor(UC_admit_tidy$category)
UC_admit_tidy$academic_yr <- as.factor(UC_admit_tidy$academic_yr)
```


**2. The president of UC has asked you to build a shiny app that shows admissions by ethnicity across all UC campuses. Your app should allow users to explore year, campus, and admit category as interactive variables. Use shiny dashboard and try to incorporate the aesthetics you have learned in ggplot to make the app neat and clean.**


```r
thesis_palette <- c("hotpink4", "steelblue4", "sienna3", "pink3", "cadetblue3", "darkolivegreen4", "goldenrod3", "darkcyan", "bisque4", "brown", "seashell3", "darkslateblue")
pie(rep(1, 12), col = thesis_palette)
```

![](lab13_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->



```r
ui <- dashboardPage(
  dashboardHeader(title = "UC Admissions Statistics"),
  dashboardSidebar(disable = T),
  dashboardBody(
  fluidRow(
  box(title = "Plot Options", width = 3,
  selectInput("x", "Select Admissions Variable", choices = c("academic_yr", "category", "campus"), 
              selected = "academic_yr"),
      hr(),
      helpText("Reference: University of California Information Center")
  ), # close the first box
  box(title = "UC Admissions Statistics", width = 6,
  plotOutput("plot", width = "600px", height = "500px")
  ) # close the second box
  ) # close the row
  ) # close the dashboard body
) # close the ui

server <- function(input, output, session) { 
  
  output$plot <- renderPlot({
  ggplot(UC_admit_tidy, aes_string(x = input$x, y =  "filtered_count_fr", fill = "ethnicity")) + geom_col() + scale_fill_manual(values = thesis_palette) + labs(x = NULL, y = NULL) + theme_light(base_size = 18) + theme(axis.text.x = element_text(angle = 60, hjust = 1))
  })
  
  # stop the app when we close it
  session$onSessionEnded(stopApp)
  }

shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}


**3. Make alternate version of your app above by tracking enrollment at a campus over all of the represented years while allowing users to interact with campus, category, and ethnicity.**


```r
ui <- dashboardPage(
  dashboardHeader(title = "UC Enrollment Statistics"),
  dashboardSidebar(disable = T),
  dashboardBody(
  fluidRow(
  box(title = "Plot Options", width = 3,
  selectInput("x", "Select Enrollment Variable", choices = c("category", "ethnicity", "campus"), 
              selected = "category"),
      hr(),
      helpText("Reference: University of California Information Center")
  ), # close the first box
  box(title = "UC Enrollment Statistics", width = 6,
  plotOutput("plot", width = "600px", height = "500px")
  ) # close the second box
  ) # close the row
  ) # close the dashboard body
) # close the ui

server <- function(input, output, session) { 
  
  output$plot <- renderPlot({
  ggplot(UC_admit_tidy, aes_string(x = "academic_yr", y =  "filtered_count_fr", fill = input$x)) + geom_col() + scale_fill_manual(values = thesis_palette) + labs(x = NULL, y = NULL) + theme_light(base_size = 18) + theme(axis.text.x = element_text(angle = 60, hjust = 1))
  })
  
  # stop the app when we close it
  session$onSessionEnded(stopApp)
  }

shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
