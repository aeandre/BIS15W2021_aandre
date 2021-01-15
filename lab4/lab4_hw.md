---
title: "Lab 4 Homework"
author: "Allison Andre"
date: "2021-01-15"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Data
For the homework, we will use data about vertebrate home range sizes. The data are in the class folder, but the reference is below.  

**Database of vertebrate home range sizes.**  
Reference: Tamburello N, Cote IM, Dulvy NK (2015) Energy and the scaling of animal space use. The American Naturalist 186(2):196-211. http://dx.doi.org/10.1086/682070.  
Data: http://datadryad.org/resource/doi:10.5061/dryad.q5j65/1  

**1. Load the data into a new object called `homerange`.**

```r
setwd("~/Desktop/BIS 15L/lab4/data")
homerange <- readr::read_csv("Tamburelloetal_HomeRangeDatabase.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   .default = col_character(),
##   mean.mass.g = col_double(),
##   log10.mass = col_double(),
##   mean.hra.m2 = col_double(),
##   log10.hra = col_double(),
##   preymass = col_double(),
##   log10.preymass = col_double(),
##   PPMR = col_double()
## )
## ℹ Use `spec()` for the full column specifications.
```

**2. Explore the data. Show the dimensions, column names, classes for each variable, and a statistical summary. Keep these as separate code chunks.**  

```r
dim(homerange)
```

```
## [1] 569  24
```


```r
names(homerange)
```

```
##  [1] "taxon"                      "common.name"               
##  [3] "class"                      "order"                     
##  [5] "family"                     "genus"                     
##  [7] "species"                    "primarymethod"             
##  [9] "N"                          "mean.mass.g"               
## [11] "log10.mass"                 "alternative.mass.reference"
## [13] "mean.hra.m2"                "log10.hra"                 
## [15] "hra.reference"              "realm"                     
## [17] "thermoregulation"           "locomotion"                
## [19] "trophic.guild"              "dimension"                 
## [21] "preymass"                   "log10.preymass"            
## [23] "PPMR"                       "prey.size.reference"
```


```r
glimpse(homerange)
```

```
## Rows: 569
## Columns: 24
## $ taxon                      <chr> "lake fishes", "river fishes", "river fish…
## $ common.name                <chr> "american eel", "blacktail redhorse", "cen…
## $ class                      <chr> "actinopterygii", "actinopterygii", "actin…
## $ order                      <chr> "anguilliformes", "cypriniformes", "cyprin…
## $ family                     <chr> "anguillidae", "catostomidae", "cyprinidae…
## $ genus                      <chr> "anguilla", "moxostoma", "campostoma", "cl…
## $ species                    <chr> "rostrata", "poecilura", "anomalum", "fund…
## $ primarymethod              <chr> "telemetry", "mark-recapture", "mark-recap…
## $ N                          <chr> "16", NA, "20", "26", "17", "5", "2", "2",…
## $ mean.mass.g                <dbl> 887.00, 562.00, 34.00, 4.00, 4.00, 3525.00…
## $ log10.mass                 <dbl> 2.9479236, 2.7497363, 1.5314789, 0.6020600…
## $ alternative.mass.reference <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ mean.hra.m2                <dbl> 282750.00, 282.10, 116.11, 125.50, 87.10, …
## $ log10.hra                  <dbl> 5.4514026, 2.4504031, 2.0648696, 2.0986437…
## $ hra.reference              <chr> "Minns, C. K. 1995. Allometry of home rang…
## $ realm                      <chr> "aquatic", "aquatic", "aquatic", "aquatic"…
## $ thermoregulation           <chr> "ectotherm", "ectotherm", "ectotherm", "ec…
## $ locomotion                 <chr> "swimming", "swimming", "swimming", "swimm…
## $ trophic.guild              <chr> "carnivore", "carnivore", "carnivore", "ca…
## $ dimension                  <chr> "3D", "2D", "2D", "2D", "2D", "2D", "2D", …
## $ preymass                   <dbl> NA, NA, NA, NA, NA, NA, 1.39, NA, NA, NA, …
## $ log10.preymass             <dbl> NA, NA, NA, NA, NA, NA, 0.1430148, NA, NA,…
## $ PPMR                       <dbl> NA, NA, NA, NA, NA, NA, 530, NA, NA, NA, N…
## $ prey.size.reference        <chr> NA, NA, NA, NA, NA, NA, "Brose U, et al. 2…
```


```r
summary(homerange)
```

```
##     taxon           common.name           class              order          
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     family             genus             species          primarymethod     
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##       N              mean.mass.g        log10.mass     
##  Length:569         Min.   :      0   Min.   :-0.6576  
##  Class :character   1st Qu.:     50   1st Qu.: 1.6990  
##  Mode  :character   Median :    330   Median : 2.5185  
##                     Mean   :  34602   Mean   : 2.5947  
##                     3rd Qu.:   2150   3rd Qu.: 3.3324  
##                     Max.   :4000000   Max.   : 6.6021  
##                                                        
##  alternative.mass.reference  mean.hra.m2          log10.hra     
##  Length:569                 Min.   :0.000e+00   Min.   :-1.523  
##  Class :character           1st Qu.:4.500e+03   1st Qu.: 3.653  
##  Mode  :character           Median :3.934e+04   Median : 4.595  
##                             Mean   :2.146e+07   Mean   : 4.709  
##                             3rd Qu.:1.038e+06   3rd Qu.: 6.016  
##                             Max.   :3.551e+09   Max.   : 9.550  
##                                                                 
##  hra.reference         realm           thermoregulation    locomotion       
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  trophic.guild       dimension            preymass         log10.preymass   
##  Length:569         Length:569         Min.   :     0.67   Min.   :-0.1739  
##  Class :character   Class :character   1st Qu.:    20.02   1st Qu.: 1.3014  
##  Mode  :character   Mode  :character   Median :    53.75   Median : 1.7304  
##                                        Mean   :  3989.88   Mean   : 2.0188  
##                                        3rd Qu.:   363.35   3rd Qu.: 2.5603  
##                                        Max.   :130233.20   Max.   : 5.1147  
##                                        NA's   :502         NA's   :502      
##       PPMR         prey.size.reference
##  Min.   :  0.380   Length:569         
##  1st Qu.:  3.315   Class :character   
##  Median :  7.190   Mode  :character   
##  Mean   : 31.752                      
##  3rd Qu.: 15.966                      
##  Max.   :530.000                      
##  NA's   :502
```


```r
homerange <- rename(homerange, primary_method = "primarymethod", mean_mass_g = "mean.mass.g", log10_prey_mass = "log10.preymass", trophic_guild = "trophic.guild", prey_mass = "preymass", hra_reference = "hra.reference", prey_size_reference = "prey.size.reference", alternative_mass_reference = "alternative.mass.reference",  mean_hra_m2 ="mean.hra.m2",  log10_prey_mass = "log10.preymass", log10_mass = "log10.mass", common_name = "common.name")
```

```r
view(homerange)
```

**3. Change the class of the variables `taxon` and `order` to factors and display their levels.**  

```r
homerange$taxon <-as.factor(homerange$taxon)
class(homerange$taxon)
```

```
## [1] "factor"
```

```r
homerange$order <- as.factor(homerange$order)
class(homerange$order)
```

```
## [1] "factor"
```


```r
levels(homerange$taxon)
```

```
## [1] "birds"         "lake fishes"   "lizards"       "mammals"      
## [5] "marine fishes" "river fishes"  "snakes"        "tortoises"    
## [9] "turtles"
```

```r
levels(homerange$order)
```

```
##  [1] "accipitriformes"    "afrosoricida"       "anguilliformes"    
##  [4] "anseriformes"       "apterygiformes"     "artiodactyla"      
##  [7] "caprimulgiformes"   "carnivora"          "charadriiformes"   
## [10] "columbidormes"      "columbiformes"      "coraciiformes"     
## [13] "cuculiformes"       "cypriniformes"      "dasyuromorpha"     
## [16] "dasyuromorpia"      "didelphimorphia"    "diprodontia"       
## [19] "diprotodontia"      "erinaceomorpha"     "esociformes"       
## [22] "falconiformes"      "gadiformes"         "galliformes"       
## [25] "gruiformes"         "lagomorpha"         "macroscelidea"     
## [28] "monotrematae"       "passeriformes"      "pelecaniformes"    
## [31] "peramelemorphia"    "perciformes"        "perissodactyla"    
## [34] "piciformes"         "pilosa"             "proboscidea"       
## [37] "psittaciformes"     "rheiformes"         "roden"             
## [40] "rodentia"           "salmoniformes"      "scorpaeniformes"   
## [43] "siluriformes"       "soricomorpha"       "squamata"          
## [46] "strigiformes"       "struthioniformes"   "syngnathiformes"   
## [49] "testudines"         "tetraodontiformes\xa0" "tinamiformes"
```


**4. What taxa are represented in the `homerange` data frame? Make a new data frame `taxa` that is restricted to taxon, common name, class, order, family, genus, species.**  

```r
taxa <- select(homerange, taxon, common_name, order, family, genus, species)
taxa
```

```
## # A tibble: 569 x 6
##    taxon       common_name          order        family      genus     species  
##    <fct>       <chr>                <fct>        <chr>       <chr>     <chr>    
##  1 lake fishes american eel         anguillifor… anguillidae anguilla  rostrata 
##  2 river fish… blacktail redhorse   cypriniform… catostomid… moxostoma poecilura
##  3 river fish… central stoneroller  cypriniform… cyprinidae  camposto… anomalum 
##  4 river fish… rosyside dace        cypriniform… cyprinidae  clinosto… funduloi…
##  5 river fish… longnose dace        cypriniform… cyprinidae  rhinicht… cataract…
##  6 river fish… muskellunge          esociformes  esocidae    esox      masquino…
##  7 marine fis… pollack              gadiformes   gadidae     pollachi… pollachi…
##  8 marine fis… saithe               gadiformes   gadidae     pollachi… virens   
##  9 marine fis… lined surgeonfish    perciformes  acanthurid… acanthur… lineatus 
## 10 marine fis… orangespine unicorn… perciformes  acanthurid… naso      lituratus
## # … with 559 more rows
```

**5. The variable `taxon` identifies the large, common name groups of the species represented in `homerange`. Make a table the shows the counts for each of these `taxon`.**  

```r
#Determining counts of each taxon
dim(filter(taxa, taxon == "birds"))
```

```
## [1] 140   6
```

```r
dim(filter(taxa, taxon == "lake fishes"))
```

```
## [1] 9 6
```

```r
dim(filter(taxa, taxon == "lizards"))
```

```
## [1] 11  6
```

```r
dim(filter(taxa, taxon =="mammals"))
```

```
## [1] 238   6
```

```r
dim(filter(taxa, taxon == "marine fishes"))
```

```
## [1] 90  6
```

```r
dim(filter(taxa, taxon == "river fishes"))
```

```
## [1] 14  6
```

```r
dim(filter(taxa, taxon == "snakes"))
```

```
## [1] 41  6
```

```r
dim(filter(taxa, taxon == "tortoises"))
```

```
## [1] 12  6
```

```r
dim(filter(taxa, taxon == "turtles"))
```

```
## [1] 14  6
```


```r
taxa_names <- c("birds", "lake fishes", "lizards", "mammals", "marine fishes", "river fishes", "snakes", "tortoises", "turtles")
taxa_numbers <- c(140, 9, 11, 238, 90, 14, 41, 12, 14)
taxa_counts <- data_frame(taxa_names, taxa_numbers)
```

```
## Warning: `data_frame()` is deprecated as of tibble 1.1.0.
## Please use `tibble()` instead.
## This warning is displayed once every 8 hours.
## Call `lifecycle::last_warnings()` to see where this warning was generated.
```

```r
taxa_counts
```

```
## # A tibble: 9 x 2
##   taxa_names    taxa_numbers
##   <chr>                <dbl>
## 1 birds                  140
## 2 lake fishes              9
## 3 lizards                 11
## 4 mammals                238
## 5 marine fishes           90
## 6 river fishes            14
## 7 snakes                  41
## 8 tortoises               12
## 9 turtles                 14
```

**6. The species in `homerange` are also classified into trophic guilds. How many species are represented in each trophic guild.**  

```r
homerange$trophic_guild <- as.factor(homerange$trophic_guild)
levels(homerange$trophic_guild)
```

```
## [1] "carnivore" "herbivore"
```

```r
table(homerange$trophic_guild)
```

```
## 
## carnivore herbivore 
##       342       227
```

**7. Make two new data frames, one which is restricted to carnivores and another that is restricted to herbivores.**  

```r
carnivores <- data_frame(filter(homerange, trophic_guild == "carnivore"))
carnivores
```

```
## # A tibble: 342 x 24
##    taxon common_name class order family genus species primary_method N    
##    <fct> <chr>       <chr> <fct> <chr>  <chr> <chr>   <chr>          <chr>
##  1 lake… american e… acti… angu… angui… angu… rostra… telemetry      16   
##  2 rive… blacktail … acti… cypr… catos… moxo… poecil… mark-recapture <NA> 
##  3 rive… central st… acti… cypr… cypri… camp… anomal… mark-recapture 20   
##  4 rive… rosyside d… acti… cypr… cypri… clin… fundul… mark-recapture 26   
##  5 rive… longnose d… acti… cypr… cypri… rhin… catara… mark-recapture 17   
##  6 rive… muskellunge acti… esoc… esoci… esox  masqui… telemetry      5    
##  7 mari… pollack     acti… gadi… gadid… poll… pollac… telemetry      2    
##  8 mari… saithe      acti… gadi… gadid… poll… virens  telemetry      2    
##  9 mari… giant trev… acti… perc… caran… cara… ignobi… telemetry      4    
## 10 lake… rock bass   acti… perc… centr… ambl… rupest… mark-recapture 16   
## # … with 332 more rows, and 15 more variables: mean_mass_g <dbl>,
## #   log10_mass <dbl>, alternative_mass_reference <chr>, mean_hra_m2 <dbl>,
## #   log10.hra <dbl>, hra_reference <chr>, realm <chr>, thermoregulation <chr>,
## #   locomotion <chr>, trophic_guild <fct>, dimension <chr>, prey_mass <dbl>,
## #   log10_prey_mass <dbl>, PPMR <dbl>, prey_size_reference <chr>
```

```r
herbivores <- data_frame(filter(homerange, trophic_guild =="herbivore"))
herbivores
```

```
## # A tibble: 227 x 24
##    taxon common_name class order family genus species primary_method N    
##    <fct> <chr>       <chr> <fct> <chr>  <chr> <chr>   <chr>          <chr>
##  1 mari… lined surg… acti… perc… acant… acan… lineat… direct observ… <NA> 
##  2 mari… orangespin… acti… perc… acant… naso  litura… telemetry      8    
##  3 mari… bluespine … acti… perc… acant… naso  unicor… telemetry      7    
##  4 mari… redlip ble… acti… perc… blenn… ophi… atlant… direct observ… 20   
##  5 mari… bermuda ch… acti… perc… kypho… kyph… sectat… telemetry      11   
##  6 mari… cherubfish  acti… perc… pomac… cent… argi    direct observ… <NA> 
##  7 mari… damselfish  acti… perc… pomac… chro… chromis direct observ… <NA> 
##  8 mari… twinspot d… acti… perc… pomac… chry… biocel… direct observ… 18   
##  9 mari… wards dams… acti… perc… pomac… poma… wardi   direct observ… <NA> 
## 10 mari… australian… acti… perc… pomac… steg… apical… direct observ… <NA> 
## # … with 217 more rows, and 15 more variables: mean_mass_g <dbl>,
## #   log10_mass <dbl>, alternative_mass_reference <chr>, mean_hra_m2 <dbl>,
## #   log10.hra <dbl>, hra_reference <chr>, realm <chr>, thermoregulation <chr>,
## #   locomotion <chr>, trophic_guild <fct>, dimension <chr>, prey_mass <dbl>,
## #   log10_prey_mass <dbl>, PPMR <dbl>, prey_size_reference <chr>
```

**8. Do herbivores or carnivores have, on average, a larger `mean.hra.m2`? Remove any NAs from the data.**  Herbivores

```r
mean(herbivores$mean_hra_m2, na.rm = TRUE)
```

```
## [1] 34137012
```

```r
mean(carnivores$mean_hra_m2, na.rm = TRUE)
```

```
## [1] 13039918
```

**9. Make a new dataframe `deer` that is limited to the mean mass, log10 mass, family, genus, and species of deer in the database. The family for deer is cervidae. Arrange the data in descending order by log10 mass. Which is the largest deer? What is its common name?**  

```r
homerange$family <- as.factor(homerange$family)
deer <- data_frame(filter(homerange, family == "cervidae"))
arrange(deer, desc(log10_mass))
```

```
## # A tibble: 12 x 24
##    taxon common_name class order family genus species primary_method N    
##    <fct> <chr>       <chr> <fct> <fct>  <chr> <chr>   <chr>          <chr>
##  1 mamm… moose       mamm… arti… cervi… alces alces   telemetry*     <NA> 
##  2 mamm… red deer    mamm… arti… cervi… cerv… elaphus telemetry*     <NA> 
##  3 mamm… reindeer    mamm… arti… cervi… rang… tarand… telemetry*     <NA> 
##  4 mamm… white-tail… mamm… arti… cervi… odoc… virgin… telemetry*     <NA> 
##  5 mamm… fallow deer mamm… arti… cervi… dama  dama    telemetry*     <NA> 
##  6 mamm… chital      mamm… arti… cervi… axis  axis    telemetry*     <NA> 
##  7 mamm… mule deer   mamm… arti… cervi… odoc… hemion… telemetry*     <NA> 
##  8 mamm… pampas deer mamm… arti… cervi… ozot… bezoar… telemetry*     <NA> 
##  9 mamm… sika deer   mamm… arti… cervi… cerv… nippon  telemetry*     <NA> 
## 10 mamm… roe deer    mamm… arti… cervi… capr… capreo… telemetry*     <NA> 
## 11 mamm… Reeves's m… mamm… arti… cervi… munt… reevesi telemetry*     <NA> 
## 12 mamm… pudu        mamm… arti… cervi… pudu  puda    telemetry*     <NA> 
## # … with 15 more variables: mean_mass_g <dbl>, log10_mass <dbl>,
## #   alternative_mass_reference <chr>, mean_hra_m2 <dbl>, log10.hra <dbl>,
## #   hra_reference <chr>, realm <chr>, thermoregulation <chr>, locomotion <chr>,
## #   trophic_guild <fct>, dimension <chr>, prey_mass <dbl>,
## #   log10_prey_mass <dbl>, PPMR <dbl>, prey_size_reference <chr>
```

```r
deer
```

```
## # A tibble: 12 x 24
##    taxon common_name class order family genus species primary_method N    
##    <fct> <chr>       <chr> <fct> <fct>  <chr> <chr>   <chr>          <chr>
##  1 mamm… moose       mamm… arti… cervi… alces alces   telemetry*     <NA> 
##  2 mamm… chital      mamm… arti… cervi… axis  axis    telemetry*     <NA> 
##  3 mamm… roe deer    mamm… arti… cervi… capr… capreo… telemetry*     <NA> 
##  4 mamm… red deer    mamm… arti… cervi… cerv… elaphus telemetry*     <NA> 
##  5 mamm… sika deer   mamm… arti… cervi… cerv… nippon  telemetry*     <NA> 
##  6 mamm… fallow deer mamm… arti… cervi… dama  dama    telemetry*     <NA> 
##  7 mamm… Reeves's m… mamm… arti… cervi… munt… reevesi telemetry*     <NA> 
##  8 mamm… mule deer   mamm… arti… cervi… odoc… hemion… telemetry*     <NA> 
##  9 mamm… white-tail… mamm… arti… cervi… odoc… virgin… telemetry*     <NA> 
## 10 mamm… pampas deer mamm… arti… cervi… ozot… bezoar… telemetry*     <NA> 
## 11 mamm… pudu        mamm… arti… cervi… pudu  puda    telemetry*     <NA> 
## 12 mamm… reindeer    mamm… arti… cervi… rang… tarand… telemetry*     <NA> 
## # … with 15 more variables: mean_mass_g <dbl>, log10_mass <dbl>,
## #   alternative_mass_reference <chr>, mean_hra_m2 <dbl>, log10.hra <dbl>,
## #   hra_reference <chr>, realm <chr>, thermoregulation <chr>, locomotion <chr>,
## #   trophic_guild <fct>, dimension <chr>, prey_mass <dbl>,
## #   log10_prey_mass <dbl>, PPMR <dbl>, prey_size_reference <chr>
```

```r
filter(deer, log10_mass == max(deer$log10_mass))
```

```
## # A tibble: 1 x 24
##   taxon common_name class order family genus species primary_method N    
##   <fct> <chr>       <chr> <fct> <fct>  <chr> <chr>   <chr>          <chr>
## 1 mamm… moose       mamm… arti… cervi… alces alces   telemetry*     <NA> 
## # … with 15 more variables: mean_mass_g <dbl>, log10_mass <dbl>,
## #   alternative_mass_reference <chr>, mean_hra_m2 <dbl>, log10.hra <dbl>,
## #   hra_reference <chr>, realm <chr>, thermoregulation <chr>, locomotion <chr>,
## #   trophic_guild <fct>, dimension <chr>, prey_mass <dbl>,
## #   log10_prey_mass <dbl>, PPMR <dbl>, prey_size_reference <chr>
```

**10. As measured by the data, which snake species has the smallest homerange? Show all of your work, please. Look this species up online and tell me about it!** **Snake is found in taxon column**    

```r
taxa_2 <- select(homerange, taxon, common_name, mean_hra_m2)
snakes <- filter(taxa_2, taxon == "snakes")
filter(snakes, mean_hra_m2 == min(snakes$mean_hra_m2))
```

```
## # A tibble: 1 x 3
##   taxon  common_name         mean_hra_m2
##   <fct>  <chr>                     <dbl>
## 1 snakes namaqua dwarf adder         200
```

Namaqua dwarf adder is a tiny venomous snake native to Namibia and South Africa. Hobbies include slithering under the sand and eating frogs and lizards. Don't tick off this snake, because its bites are mildly toxic to humans!

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
