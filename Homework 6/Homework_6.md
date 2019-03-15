---
title: "Lab 6 Homework"
author: Jonathan marquez 
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
---

##Load the libraries 

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
library(skimr)
library(RColorBrewer)
```


##install gapminder

```r
#install.packages("gapminder")
```


## Load the data into a new object called gapminder

```r
gapminder <-
  gapminder::gapminder
```

1. Explore the data using the various function you have learned. Is it tidy, are there any NA’s, what are its dimensions, what are the column names, etc.

```r
glimpse(gapminder)
```

```
## Observations: 1,704
## Variables: 6
## $ country   <fct> Afghanistan, Afghanistan, Afghanistan, Afghanistan, Af…
## $ continent <fct> Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, …
## $ year      <int> 1952, 1957, 1962, 1967, 1972, 1977, 1982, 1987, 1992, …
## $ lifeExp   <dbl> 28.801, 30.332, 31.997, 34.020, 36.088, 38.438, 39.854…
## $ pop       <int> 8425333, 9240934, 10267083, 11537966, 13079460, 148803…
## $ gdpPercap <dbl> 779.4453, 820.8530, 853.1007, 836.1971, 739.9811, 786.…
```

## How many NAs do I have?

```r
gapminder %>%
  summarize(number_nas= sum(is.na(gapminder)))
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1          0
```

```r
gapminder
```

```
## # A tibble: 1,704 x 6
##    country     continent  year lifeExp      pop gdpPercap
##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
##  1 Afghanistan Asia       1952    28.8  8425333      779.
##  2 Afghanistan Asia       1957    30.3  9240934      821.
##  3 Afghanistan Asia       1962    32.0 10267083      853.
##  4 Afghanistan Asia       1967    34.0 11537966      836.
##  5 Afghanistan Asia       1972    36.1 13079460      740.
##  6 Afghanistan Asia       1977    38.4 14880372      786.
##  7 Afghanistan Asia       1982    39.9 12881816      978.
##  8 Afghanistan Asia       1987    40.8 13867957      852.
##  9 Afghanistan Asia       1992    41.7 16317921      649.
## 10 Afghanistan Asia       1997    41.8 22227415      635.
## # … with 1,694 more rows
```

## Can't see nas straight from the summarize fucntion glimpse. Will use skimr instead.

```r
skimr::skim(gapminder)
```

```
## Skim summary statistics
##  n obs: 1704 
##  n variables: 6 
## 
## ── Variable type:factor ────────────────────────────────────────────────────────
##   variable missing complete    n n_unique
##  continent       0     1704 1704        5
##    country       0     1704 1704      142
##                              top_counts ordered
##  Afr: 624, Asi: 396, Eur: 360, Ame: 300   FALSE
##      Afg: 12, Alb: 12, Alg: 12, Ang: 12   FALSE
## 
## ── Variable type:integer ───────────────────────────────────────────────────────
##  variable missing complete    n    mean       sd    p0        p25     p50
##       pop       0     1704 1704 3e+07    1.1e+08 60011 2793664    7e+06  
##      year       0     1704 1704  1979.5 17.27     1952    1965.75  1979.5
##       p75       p100     hist
##  2e+07       1.3e+09 ▇▁▁▁▁▁▁▁
##   1993.25 2007       ▇▃▇▃▃▇▃▇
## 
## ── Variable type:numeric ───────────────────────────────────────────────────────
##   variable missing complete    n    mean      sd     p0     p25     p50
##  gdpPercap       0     1704 1704 7215.33 9857.45 241.17 1202.06 3531.85
##    lifeExp       0     1704 1704   59.47   12.92  23.6    48.2    60.71
##      p75      p100     hist
##  9325.46 113523.13 ▇▁▁▁▁▁▁▁
##    70.85     82.6  ▁▂▅▅▅▅▇▃
```
Looks like the data is tidy. 

2. We are interested in the relationship between per capita GDP and life expectancy; i.e. does having more money help you live longer on average. Make a quick plot below to visualize this relationship.

```r
gapminder %>%
  ggplot(aes(x=gdpPercap, y=lifeExp)) +
  geom_point() + ##used geom point to get a scatterplot of our data
  labs(title = "GDP vs. Life Expectancy",
       x = "GDP per Capita" ,
       y = "Life Expectancy")
```

![](Homework_6_files/figure-html/unnamed-chunk-7-1.png)<!-- -->
## According to our plot, having more money does correlate with longer life expectancy.

3. There is extreme disparity in per capita GDP. Rescale the x axis to make this easier to interpret. How would you characterize the relationship?

```r
gapminder %>%
  ggplot(aes(x=gdpPercap, y=lifeExp)) +
  geom_point() +
  scale_x_log10() + ## helps get a better view of our data by making our x-axis intoa base 10g 10 scale!
  labs(title = "GDP vs. Life Expectancy",
       x = "GDP per Capita" ,
       y = "Life Expectancy")
```

![](Homework_6_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

4. This should look pretty dense to you with significant overplotting. Try using a faceting approach to break this relationship down by year.

```r
gapminder %>%
  ggplot(aes(x=gdpPercap, y=lifeExp)) +
  geom_point() +
  facet_wrap(~year)+ ## Our previous code had condesed data from several year. This code helps us separate multiple graphs by year!
  scale_x_log10() +
  labs(title = "GDP vs. Life Expectancy",
       x = "GDP per Capita" ,
       y = "Life Expectancy")
```

![](Homework_6_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

5. Simplify the comparison by comparing only 1952 and 2007. Can you come to any conclusions?

```r
gapminder %>%
  filter(year=="1957"| year=="2007") %>%
  ggplot(aes(x=gdpPercap, y=lifeExp)) +
  geom_point() +
  facet_wrap(~year)+ ## Our previous code had condesed data from several year. This code helps us separate multiple graphs by year!
  scale_x_log10() +
  labs(title = "GDP vs. Life Expectancy",
       x = "GDP per Capita" ,
       y = "Life Expectancy")
```

![](Homework_6_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
## Tried to make two different barplots but I couldn't work around mainpulating the x-axis.Ended up using a scatter plot. Nonetheless, we can see that both GDP and Life Expectancy have both increased between the years 1957 and 2007. 

6. Let’s stick with the 1952 and 2007 comparison but make some aesthetic adjustments. First try to color by continent and adjust the size of the points by population. Add + scale_size(range = c(0.1, 10), guide = "none") as a layer to clean things up a bit.

```r
gapminder %>%
  filter(year=="1957"| year=="2007") %>%
  ggplot(aes(x=gdpPercap, y=lifeExp, color=continent, size= pop)) + ## added color by continent, and size by population 
  geom_point() +
   scale_size(range = c(0.1, 10), guide = "none") + ##copy and pasted code for cleaner plot. 
  facet_wrap(~year)+
  scale_x_log10() +
  labs(title = "GDP vs. Life Expectancy",
       x = "GDP per Capita" ,
       y = "Life Expectancy") 
```

![](Homework_6_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

7.Although we did not introduce them in lab, ggplot has a number of built-in themes that make things easier. I like the light theme for these data, but you can see lots of options. Apply one of these to your plot above.

```r
 ?theme_light
gapminder %>%
  filter(year=="1957"| year=="2007") %>%
  ggplot(aes(x=gdpPercap, y=lifeExp, color=continent, size= pop)) + 
  geom_point() +
   scale_size(range = c(0.1, 10), guide = "none") +
  facet_wrap(~year)+
  scale_x_log10() +
  labs(title = "GDP vs. Life Expectancy",
       x = "GDP per Capita" ,
       y = "Life Expectancy") +
  theme_minimal() + ## I like the cleanliness this code provides 
  theme_bw() ## This code complements the cleanliness of of the previous code and makes it more appealing on presentations. 
```

![](Homework_6_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

8. What is the population for all countries on the Asian continent in 2007? Show this as a barplot.

```r
gapminder %>%
 filter(year=="2007", continent=="Asia") %>%
  ggplot(aes(x=reorder(country, -pop), y=pop)) + ##don't fully understand what this code does. why do we add "-pop". does that mean by reorder by pop. 
           geom_col(fill="light green", alpha=1)+
  coord_flip()+ ## helps up read the names better 
  labs(title = "Poplation on Asian Continent (2007)", 
x= "Country ", 
y= "Population") +
  theme_minimal() +
  theme_bw() +
  theme(plot.title = element_text(size = rel(2),hjust = .4)) ##rel adjust size on title where as hjust adjust the position of the title.
```

![](Homework_6_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

9. You should see that China's population is the largest with India a close second. Let's focus on China only. Make a plot that shows how population has changed over the years.

```r
gapminder %>%
  filter(country=="China") %>% 
  ggplot(aes(x=factor(year), y=pop, color=pop))+
  geom_bar(stat = "identity")+ ##don't understand what stat does in this code. 
  labs(title = "Population Growth in China \n1952-2007",
       x = "Year",
       y = "Population")
```

![](Homework_6_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

```r
?stat
```

10. Let's compare China and India. Make a barplot that shows population growth by year using position=dodge. Apply a custom color theme using RColorBrewer.

```r
library(RColorBrewer)
gapminder %>%
  filter(country=="China"| country=="India") %>%
  ggplot(aes(x=factor(year), y=pop, fill=country)) +
  geom_bar(stat = "identity", position ="dodge") + ##without position dodge the two contries would be sitting on top of one another. 
  labs(title = "China vs India (population)",
       x= "Year",
       y= "Population")+
    scale_fill_brewer(palette = "Paired") ##In order for this to work we have to make sure that we have "fill" in aesthetic 
```

![](Homework_6_files/figure-html/unnamed-chunk-15-1.png)<!-- -->































