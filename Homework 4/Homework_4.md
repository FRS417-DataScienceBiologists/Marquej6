---
title: " HW 4"
author: "Jonathan Marquez"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
    toc: yes
    toc_float: yes
  pdf_document:
    toc: yes
---

Load the tidyverse
library(tidyverse)

```r
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.2.1 ──
```

```
## ✔ ggplot2 3.1.0     ✔ purrr   0.2.5
## ✔ tibble  2.0.0     ✔ dplyr   0.7.8
## ✔ tidyr   0.8.2     ✔ stringr 1.3.1
## ✔ readr   1.3.1     ✔ forcats 0.3.0
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
getwd()
```

```
## [1] "/Users/jonathanmarquez/Desktop/FRS 417"
```

Practice and Homework
We will work together on the next part (time permitting) and this will end up being your homework. Make sure that you save your work and copy and paste your responses into the RMarkdown homnework file.

Aren’t mammals fun? Let’s load up some more mammal data. This will be life history data for mammals. The data are from: S. K. Morgan Ernest. 2003. Life history characteristics of placental non-volant mammals. Ecology 84:3402.

life_history <- readr::read_csv("data/mammal_lifehistories_v2.csv")
##Loading Data

```r
life_history <- readr::read_csv("mammal_lifehistories_v2.csv")
```

```
## Parsed with column specification:
## cols(
##   order = col_character(),
##   family = col_character(),
##   Genus = col_character(),
##   species = col_character(),
##   mass = col_double(),
##   gestation = col_double(),
##   newborn = col_double(),
##   weaning = col_double(),
##   `wean mass` = col_double(),
##   AFR = col_double(),
##   `max. life` = col_double(),
##   `litter size` = col_double(),
##   `litters/year` = col_double()
## )
```


Rename some of the variables. Notice that I am replacing the old life_history data.
## Renaming some of the variables

```r
life_history <- 
  life_history %>% 
  dplyr::rename(
          genus        = Genus,
          wean_mass    = `wean mass`,
          max_life     = `max. life`,
          litter_size  = `litter size`,
          litters_yr   = `litters/year` )
names(life_history)
```

```
##  [1] "order"       "family"      "genus"       "species"     "mass"       
##  [6] "gestation"   "newborn"     "weaning"     "wean_mass"   "AFR"        
## [11] "max_life"    "litter_size" "litters_yr"
```

```r
glimpse(life_history)
```

```
## Observations: 1,440
## Variables: 13
## $ order       <chr> "Artiodactyla", "Artiodactyla", "Artiodactyla", "Art…
## $ family      <chr> "Antilocapridae", "Bovidae", "Bovidae", "Bovidae", "…
## $ genus       <chr> "Antilocapra", "Addax", "Aepyceros", "Alcelaphus", "…
## $ species     <chr> "americana", "nasomaculatus", "melampus", "buselaphu…
## $ mass        <dbl> 45375.0, 182375.0, 41480.0, 150000.0, 28500.0, 55500…
## $ gestation   <dbl> 8.13, 9.39, 6.35, 7.90, 6.80, 5.08, 5.72, 5.50, 8.93…
## $ newborn     <dbl> 3246.36, 5480.00, 5093.00, 10166.67, -999.00, 3810.0…
## $ weaning     <dbl> 3.00, 6.50, 5.63, 6.50, -999.00, 4.00, 4.04, 2.13, 1…
## $ wean_mass   <dbl> 8900, -999, 15900, -999, -999, -999, -999, -999, 157…
## $ AFR         <dbl> 13.53, 27.27, 16.66, 23.02, -999.00, 14.89, 10.23, 2…
## $ max_life    <dbl> 142, 308, 213, 240, -999, 251, 228, 255, 300, 324, 3…
## $ litter_size <dbl> 1.85, 1.00, 1.00, 1.00, 1.00, 1.37, 1.00, 1.00, 1.00…
## $ litters_yr  <dbl> 1.00, 0.99, 0.95, -999.00, -999.00, 2.00, -999.00, 1…
```


Explore the data using the method that you prefer. Below, I am using a new package called skimr. If you want to use this, make sure that it is installed.
## Installing "skimr" package

```r
#install.packages("skimr")
library(skimr)
life_history %>%
  skimr::skim()
```

```
## Skim summary statistics
##  n obs: 1440 
##  n variables: 13 
## 
## ── Variable type:character ─────────────────────────────────────────────────────
##  variable missing complete    n min max empty n_unique
##    family       0     1440 1440   6  15     0       96
##     genus       0     1440 1440   3  16     0      618
##     order       0     1440 1440   7  14     0       17
##   species       0     1440 1440   3  17     0     1191
## 
## ── Variable type:numeric ───────────────────────────────────────────────────────
##     variable missing complete    n      mean         sd   p0  p25     p50
##          AFR       0     1440 1440   -408.12     504.97 -999 -999    2.5 
##    gestation       0     1440 1440   -287.25     455.36 -999 -999    1.05
##  litter_size       0     1440 1440    -55.63     234.88 -999    1    2.27
##   litters_yr       0     1440 1440   -477.14     500.03 -999 -999    0.38
##         mass       0     1440 1440 383576.72 5055162.92 -999   50  403.02
##     max_life       0     1440 1440   -490.26     615.3  -999 -999 -999   
##      newborn       0     1440 1440   6703.15   90912.52 -999 -999    2.65
##    wean_mass       0     1440 1440  16048.93   5e+05    -999 -999 -999   
##      weaning       0     1440 1440   -427.17     496.71 -999 -999    0.73
##      p75          p100     hist
##    15.61     210       ▆▁▁▁▁▁▇▁
##     4.5       21.46    ▃▁▁▁▁▁▁▇
##     3.83      14.18    ▁▁▁▁▁▁▁▇
##     1.15       7.5     ▇▁▁▁▁▁▁▇
##  7009.17       1.5e+08 ▇▁▁▁▁▁▁▁
##   147.25    1368       ▇▁▁▃▂▁▁▁
##    98    2250000       ▇▁▁▁▁▁▁▁
##    10          1.9e+07 ▇▁▁▁▁▁▁▁
##     2         48       ▆▁▁▁▁▁▁▇
```

## Are there any NA’s in the data? Does this seem likely?

```r
msleep %>% 
  summarize(number_nas= sum(is.na(life_history))) ## doesn't know that na's are listed as "-999.0"
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1          0
```
## Using the code above there's no NAs in our data;however, the code was only programmed to specifically look for "NA"s when in reality NAs can be input under different characters/Values

Are there any missing data (NA’s) represented by different values? How much and where? In which variables do we have the most missing data? Can you think of a reason why so much data are missing in this variable?
## Yes, there is missing data. When I used the glimpse function I was actually able to see NAs input as "-999.0" . I'll run a code that will allow me to change the "-999.0" values into "Na" in which then we can recount the NA. 


```r
life_history_na <- 
  life_history %>% 
  na_if("-999") # changed "-999.0" to be recognized as "na"
life_history_na %>% 
  summarize(number_nas= sum(is.na(life_history)))
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1          0
```
## Using the purr (a tidyverse function) we can map all the nas into a new data frame that will specifically show us 

```r
 Life_history_na <- ##I used both the "purr" and "gather" function to execute a beatufully data visual. changed to Cap L!!
 life_history_na %>%
 purrr::map_df(~ sum(is.na(.))) %>% 
  tidyr::gather(key="variables", value="num_nas") %>% 
  arrange(desc(num_nas))
Life_history_na
```

```
## # A tibble: 13 x 2
##    variables   num_nas
##    <chr>         <int>
##  1 wean_mass      1039
##  2 max_life        841
##  3 litters_yr      689
##  4 weaning         619
##  5 AFR             607
##  6 newborn         595
##  7 gestation       418
##  8 mass             85
##  9 litter_size      84
## 10 order             0
## 11 family            0
## 12 genus             0
## 13 species           0
```
## Wean mass has the most missing information. It's tough distinguishing when animals wean completely sometimes. It's also tough being able to weigh some animals.Maybe many animals they observed didn't wean within their time of observation 


Compared to the msleep data, we have better representation among taxa. Produce a summary that shows the number of observations by taxonomic order.
## Lab 4_1 we were introduced to "group_by" function which is what i'll use to specify the number.  

```r
life_history %>%
group_by(order) %>%
  summarise(observations=n()) %>% ## we grouped by order and then further reduced our information to show how many observations we input for each order
  arrange(desc(observations))
```

```
## # A tibble: 17 x 2
##    order          observations
##    <chr>                 <int>
##  1 Rodentia                665
##  2 Carnivora               197
##  3 Artiodactyla            161
##  4 Primates                156
##  5 Insectivora              91
##  6 Cetacea                  55
##  7 Lagomorpha               42
##  8 Xenarthra                20
##  9 Perissodactyla           15
## 10 Macroscelidea            10
## 11 Pholidota                 7
## 12 Scandentia                7
## 13 Sirenia                   5
## 14 Hyracoidea                4
## 15 Dermoptera                2
## 16 Proboscidea               2
## 17 Tubulidentata             1
```


Mammals have a range of life histories, including lifespan. Produce a summary of lifespan in years by order. Be sure to include the minimum, maximum, mean, standard deviation, and total n.
## I didn't understand this part fully 

```r
life_history %>% 
  mutate(lifespan=max_life/12) %>% ## added data to a new column to data. why did we divide by 12?
  group_by(order) %>%
  summarize(min=min(lifespan, na.rm=TRUE),
            max=max(lifespan, na.rm=TRUE),
            mean=mean(lifespan, na.rm=TRUE),
            sd=sd(lifespan, na.rm=TRUE),
            total=n()) 
```

```
## # A tibble: 17 x 6
##    order            min    max  mean    sd total
##    <chr>          <dbl>  <dbl> <dbl> <dbl> <int>
##  1 Artiodactyla   -83.2  61    -19.4 51.1    161
##  2 Carnivora      -83.2  56    -10.2 48.5    197
##  3 Cetacea        -83.2 114     10.4 64.8     55
##  4 Dermoptera     -83.2  17.5  -32.9 71.2      2
##  5 Hyracoidea     -83.2  12.2  -35.8 54.8      4
##  6 Insectivora    -83.2  13    -45.0 43.5     91
##  7 Lagomorpha     -83.2  18    -65.7 36.7     42
##  8 Macroscelidea  -83.2   8.75 -47.7 45.9     10
##  9 Perissodactyla -83.2  50     11.8 50.0     15
## 10 Pholidota      -83.2  20    -68.5 39.0      7
## 11 Primates       -83.2  60.5  -31.5 55.1    156
## 12 Proboscidea     70    80     75    7.07     2
## 13 Rodentia       -83.2  35    -61.4 38.8    665
## 14 Scandentia     -83.2  12.4  -43.8 49.3      7
## 15 Sirenia        -83.2  73     -7.4 72.5      5
## 16 Tubulidentata   24    24     24   NA        1
## 17 Xenarthra      -83.2  40    -25.8 53.8     20
```


Let’s look closely at gestation and newborns. Summarize the mean gestation, newborn mass, and weaning mass by order. Add a new column that shows mean gestation in years and sort in descending order. Which group has the longest mean gestation? What is the common name for these mammals?

```r
life_history%>%
  group_by(order) %>%
 summarize(mean_gestation=mean(gestation, na.rm=TRUE), ##when we summarize does that automatically reduce order?
            mean_newborn_mass=mean(newborn, na.rm=TRUE),
            mean_wean_mass=mean(wean_mass, na.rm=TRUE),
            total=n()) %>% 
  arrange(desc(mean_gestation))
```

```
## # A tibble: 17 x 5
##    order          mean_gestation mean_newborn_mass mean_wean_mass total
##    <chr>                   <dbl>             <dbl>          <dbl> <int>
##  1 Proboscidea             21.3            99523.        299500.      2
##  2 Perissodactyla          13.0            21412.         75639.     15
##  3 Hyracoidea               7.4              -76.5         -624.      4
##  4 Tubulidentata            7.08            1734           6250       1
##  5 Dermoptera               2.75            -482.          -999       2
##  6 Artiodactyla           -61.5             5125.          7725.    161
##  7 Carnivora             -124.              2262.          7607.    197
##  8 Cetacea               -135.            149143.        347883.     55
##  9 Xenarthra             -196.              -342.          -928.     20
## 10 Lagomorpha            -237.              -345.          -428.     42
## 11 Primates              -259.               -83.8          -40.9   156
## 12 Macroscelidea         -298.              -385.          -778.     10
## 13 Rodentia              -391.              -484.          -673.    665
## 14 Sirenia               -393.             13327.         12701.      5
## 15 Scandentia            -427.              -276.          -684.      7
## 16 Insectivora           -504.              -502.          -727.     91
## 17 Pholidota             -569.               -88.0          718.      7
```



















