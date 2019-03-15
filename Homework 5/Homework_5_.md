---
title: "HW 5"
author: "Jonathan Marquez "
date: "Winter 2019"
output:
  html_document:
    fig_caption: yes
    keep_md: yes
    theme: spacelab
    toc: yes
    toc_float: yes
  pdf_document:
    toc: yes
---

Load the tidyverse

```r
library(tidyverse)
```

```
## ── Attaching packages ────────────────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──
```

```
## ✔ ggplot2 3.1.0     ✔ purrr   0.2.5
## ✔ tibble  2.0.0     ✔ dplyr   0.7.8
## ✔ tidyr   0.8.2     ✔ stringr 1.3.1
## ✔ readr   1.3.1     ✔ forcats 0.3.0
```

```
## ── Conflicts ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
getwd()
```

```
## [1] "/Users/jonathanmarquez/Desktop/FRS 417"
```

library(tidyverse)
Mammals Life History
Let’s revisit the mammal life history data to practice our ggplot skills. Some of the tidy steps will be a repeat from the homework, but it is good practice. The data are from: S. K. Morgan Ernest. 2003. Life history characteristics of placental non-volant mammals. Ecology 84:3402.

1.Load the data.

```r
mammals <- readr::read_csv("mammal_lifehistories_v2.csv", na = c("", " ", "NA", "#N/A", "-999"))  ##make sure to put data in "" . won't work otherwise
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


2. Use your preferred function to have a look. Do you notice any problems?

```r
glimpse(mammals)
```

```
## Observations: 1,440
## Variables: 13
## $ order          <chr> "Artiodactyla", "Artiodactyla", "Artiodactyla", "…
## $ family         <chr> "Antilocapridae", "Bovidae", "Bovidae", "Bovidae"…
## $ Genus          <chr> "Antilocapra", "Addax", "Aepyceros", "Alcelaphus"…
## $ species        <chr> "americana", "nasomaculatus", "melampus", "busela…
## $ mass           <dbl> 45375.0, 182375.0, 41480.0, 150000.0, 28500.0, 55…
## $ gestation      <dbl> 8.13, 9.39, 6.35, 7.90, 6.80, 5.08, 5.72, 5.50, 8…
## $ newborn        <dbl> 3246.36, 5480.00, 5093.00, 10166.67, NA, 3810.00,…
## $ weaning        <dbl> 3.00, 6.50, 5.63, 6.50, NA, 4.00, 4.04, 2.13, 10.…
## $ `wean mass`    <dbl> 8900, NA, 15900, NA, NA, NA, NA, NA, 157500, NA, …
## $ AFR            <dbl> 13.53, 27.27, 16.66, 23.02, NA, 14.89, 10.23, 20.…
## $ `max. life`    <dbl> 142, 308, 213, 240, NA, 251, 228, 255, 300, 324, …
## $ `litter size`  <dbl> 1.85, 1.00, 1.00, 1.00, 1.00, 1.37, 1.00, 1.00, 1…
## $ `litters/year` <dbl> 1.00, 0.99, 0.95, NA, NA, 2.00, NA, 1.89, 1.00, 1…
```


3. There are NA’s. How are you going to deal with them?

```r
mammals <-mammals %>% ## changed all "-999" into NAs
  na_if("-999")
```


4. Where are the NA’s? This is important to keep in mind as you build plots.

```r
mammals %>%
  purrr::map_df(~sum(is.na(.))) %>% 
  tidyr:: gather(variables,num_nas ) %>% # made a new data frame using gather from our already established variables and num_nas
  arrange(desc(num_nas))
```

```
## # A tibble: 13 x 2
##    variables    num_nas
##    <chr>          <int>
##  1 wean mass       1039
##  2 max. life        841
##  3 litters/year     689
##  4 weaning          619
##  5 AFR              607
##  6 newborn          595
##  7 gestation        418
##  8 mass              85
##  9 litter size       84
## 10 order              0
## 11 family             0
## 12 Genus              0
## 13 species            0
```


5. Some of the variable names will be problematic. Let’s rename them here as a final tidy step.

```r
mammals <- 
  mammals %>%  
  dplyr::rename(
          genus        = Genus,
          wean_mass    = `wean mass`,
          max_life     = `max. life`,
          litter_size  = `litter size`,
          litters_yr   = `litters/year`
          )
mammals
```

```
## # A tibble: 1,440 x 13
##    order family genus species   mass gestation newborn weaning wean_mass
##    <chr> <chr>  <chr> <chr>    <dbl>     <dbl>   <dbl>   <dbl>     <dbl>
##  1 Arti… Antil… Anti… americ… 4.54e4      8.13   3246.    3         8900
##  2 Arti… Bovid… Addax nasoma… 1.82e5      9.39   5480     6.5         NA
##  3 Arti… Bovid… Aepy… melamp… 4.15e4      6.35   5093     5.63     15900
##  4 Arti… Bovid… Alce… busela… 1.50e5      7.9   10167.    6.5         NA
##  5 Arti… Bovid… Ammo… clarkei 2.85e4      6.8      NA    NA           NA
##  6 Arti… Bovid… Ammo… lervia  5.55e4      5.08   3810     4           NA
##  7 Arti… Bovid… Anti… marsup… 3.00e4      5.72   3910     4.04        NA
##  8 Arti… Bovid… Anti… cervic… 3.75e4      5.5    3846     2.13        NA
##  9 Arti… Bovid… Bison bison   4.98e5      8.93  20000    10.7     157500
## 10 Arti… Bovid… Bison bonasus 5.00e5      9.14  23000.    6.6         NA
## # … with 1,430 more rows, and 4 more variables: AFR <dbl>, max_life <dbl>,
## #   litter_size <dbl>, litters_yr <dbl>
```



ggplot()
For the questions below, try to use the aesthetics you have learned to make visually appealing and informative plots. Make sure to include labels for the axes and titles.

options(scipen=999) #cancels the use of scientific notation for the session
6. What is the relationship between newborn body mass and gestation? Make a scatterplot that shows this relationship.

```r
options(scipen=999) #cancels the use of scientific notation for the session
mammals %>%
  ggplot(aes(x=newborn,y=gestation )) +
  geom_jitter()
```

```
## Warning: Removed 673 rows containing missing values (geom_point).
```

![](Homework_5__files/figure-html/unnamed-chunk-7-1.png)<!-- -->

7. You should notice that because of the outliers in newborn mass, we need to make some changes. We didn’t talk about this in lab, but you can use scale_x_log10() as a layer to correct for this issue. This will log transform the y-axis values.

```r
mammals %>%
  ggplot(aes(x=newborn,y=gestation )) +
  geom_jitter() +
  scale_x_log10() ## added as an additional layer 
```

```
## Warning: Removed 673 rows containing missing values (geom_point).
```

![](Homework_5__files/figure-html/unnamed-chunk-8-1.png)<!-- -->

8. Now that you have the basic plot, color the points by taxonomic order.

```r
mammals %>%
  ggplot(aes(x=newborn,y=gestation, color=order )) + ## added color with"color=x" colors by different groups!
  geom_jitter() +
  scale_x_log10() 
```

```
## Warning: Removed 673 rows containing missing values (geom_point).
```

![](Homework_5__files/figure-html/unnamed-chunk-9-1.png)<!-- -->

9. Lastly, make the size of the points proportional to body mass.

```r
mammals %>%
  ggplot(aes(x=newborn,y=gestation, color=order, size= mass )) + # added proportional sizes using "size=x" 
  geom_jitter() +
  scale_x_log10() 
```

```
## Warning: Removed 691 rows containing missing values (geom_point).
```

![](Homework_5__files/figure-html/unnamed-chunk-10-1.png)<!-- -->


10. Make a plot that shows the range of lifespan by order.

```r
mammals %>%
  ggplot(aes(x=order, y=max_life, fill=order)) + ## would it be wrong to switch X and Y?
  geom_boxplot() + 
  coord_flip()
```

```
## Warning: Removed 841 rows containing non-finite values (stat_boxplot).
```

![](Homework_5__files/figure-html/unnamed-chunk-11-1.png)<!-- -->

