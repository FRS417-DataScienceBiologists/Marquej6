---
title: "Lab 3 Homework"
author: Jonathan marquez 
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
---


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
fisheries <- 
  readr::read_csv(file = "FAO_1950to2012_111914.csv") 
```

```
## Warning: Duplicated column names deduplicated: 'Species (ISSCAAP group)'
## => 'Species (ISSCAAP group)_1' [4], 'Species (ASFIS species)' => 'Species
## (ASFIS species)_1' [5], 'Species (ASFIS species)' => 'Species (ASFIS
## species)_2' [6]
```

```
## Parsed with column specification:
## cols(
##   .default = col_character(),
##   `Species (ISSCAAP group)` = col_double(),
##   `Fishing area (FAO major fishing area)` = col_double()
## )
```

```
## See spec(...) for full column specifications.
```

```r
getwd()
```

```
## [1] "/Users/jonathanmarquez/Desktop/FRS 417"
```


```r
spec(fisheries)
```

```
## cols(
##   `Country (Country)` = col_character(),
##   `Species (ASFIS species)` = col_character(),
##   `Species (ISSCAAP group)` = col_double(),
##   `Species (ISSCAAP group)_1` = col_character(),
##   `Species (ASFIS species)_1` = col_character(),
##   `Species (ASFIS species)_2` = col_character(),
##   `Fishing area (FAO major fishing area)` = col_double(),
##   `Measure (Measure)` = col_character(),
##   `1950` = col_character(),
##   `1951` = col_character(),
##   `1952` = col_character(),
##   `1953` = col_character(),
##   `1954` = col_character(),
##   `1955` = col_character(),
##   `1956` = col_character(),
##   `1957` = col_character(),
##   `1958` = col_character(),
##   `1959` = col_character(),
##   `1960` = col_character(),
##   `1961` = col_character(),
##   `1962` = col_character(),
##   `1963` = col_character(),
##   `1964` = col_character(),
##   `1965` = col_character(),
##   `1966` = col_character(),
##   `1967` = col_character(),
##   `1968` = col_character(),
##   `1969` = col_character(),
##   `1970` = col_character(),
##   `1971` = col_character(),
##   `1972` = col_character(),
##   `1973` = col_character(),
##   `1974` = col_character(),
##   `1975` = col_character(),
##   `1976` = col_character(),
##   `1977` = col_character(),
##   `1978` = col_character(),
##   `1979` = col_character(),
##   `1980` = col_character(),
##   `1981` = col_character(),
##   `1982` = col_character(),
##   `1983` = col_character(),
##   `1984` = col_character(),
##   `1985` = col_character(),
##   `1986` = col_character(),
##   `1987` = col_character(),
##   `1988` = col_character(),
##   `1989` = col_character(),
##   `1990` = col_character(),
##   `1991` = col_character(),
##   `1992` = col_character(),
##   `1993` = col_character(),
##   `1994` = col_character(),
##   `1995` = col_character(),
##   `1996` = col_character(),
##   `1997` = col_character(),
##   `1998` = col_character(),
##   `1999` = col_character(),
##   `2000` = col_character(),
##   `2001` = col_character(),
##   `2002` = col_character(),
##   `2003` = col_character(),
##   `2004` = col_character(),
##   `2005` = col_character(),
##   `2006` = col_character(),
##   `2007` = col_character(),
##   `2008` = col_character(),
##   `2009` = col_character(),
##   `2010` = col_character(),
##   `2011` = col_character(),
##   `2012` = col_character()
## )
```
1. Do you see any potential problems with the column names? Does the error message now make more sense?
Column names are duplicated so R manipulated them to unique names


```r
names(fisheries) ## the years are listed as separate columns themselves which is NOT what we want. 
```

```
##  [1] "Country (Country)"                    
##  [2] "Species (ASFIS species)"              
##  [3] "Species (ISSCAAP group)"              
##  [4] "Species (ISSCAAP group)_1"            
##  [5] "Species (ASFIS species)_1"            
##  [6] "Species (ASFIS species)_2"            
##  [7] "Fishing area (FAO major fishing area)"
##  [8] "Measure (Measure)"                    
##  [9] "1950"                                 
## [10] "1951"                                 
## [11] "1952"                                 
## [12] "1953"                                 
## [13] "1954"                                 
## [14] "1955"                                 
## [15] "1956"                                 
## [16] "1957"                                 
## [17] "1958"                                 
## [18] "1959"                                 
## [19] "1960"                                 
## [20] "1961"                                 
## [21] "1962"                                 
## [22] "1963"                                 
## [23] "1964"                                 
## [24] "1965"                                 
## [25] "1966"                                 
## [26] "1967"                                 
## [27] "1968"                                 
## [28] "1969"                                 
## [29] "1970"                                 
## [30] "1971"                                 
## [31] "1972"                                 
## [32] "1973"                                 
## [33] "1974"                                 
## [34] "1975"                                 
## [35] "1976"                                 
## [36] "1977"                                 
## [37] "1978"                                 
## [38] "1979"                                 
## [39] "1980"                                 
## [40] "1981"                                 
## [41] "1982"                                 
## [42] "1983"                                 
## [43] "1984"                                 
## [44] "1985"                                 
## [45] "1986"                                 
## [46] "1987"                                 
## [47] "1988"                                 
## [48] "1989"                                 
## [49] "1990"                                 
## [50] "1991"                                 
## [51] "1992"                                 
## [52] "1993"                                 
## [53] "1994"                                 
## [54] "1995"                                 
## [55] "1996"                                 
## [56] "1997"                                 
## [57] "1998"                                 
## [58] "1999"                                 
## [59] "2000"                                 
## [60] "2001"                                 
## [61] "2002"                                 
## [62] "2003"                                 
## [63] "2004"                                 
## [64] "2005"                                 
## [65] "2006"                                 
## [66] "2007"                                 
## [67] "2008"                                 
## [68] "2009"                                 
## [69] "2010"                                 
## [70] "2011"                                 
## [71] "2012"
```

2. The make.names() command is helpful when there are issues with column names. Notice that although the names are still cumbersome, much of the problemtatic syntax is removed.

```r
names(fisheries) = make.names(names(fisheries), unique=T) #changes the column names
names(fisheries) ## we'll see that the years now have a variable "x" in front of it. 
```

```
##  [1] "Country..Country."                    
##  [2] "Species..ASFIS.species."              
##  [3] "Species..ISSCAAP.group."              
##  [4] "Species..ISSCAAP.group._1"            
##  [5] "Species..ASFIS.species._1"            
##  [6] "Species..ASFIS.species._2"            
##  [7] "Fishing.area..FAO.major.fishing.area."
##  [8] "Measure..Measure."                    
##  [9] "X1950"                                
## [10] "X1951"                                
## [11] "X1952"                                
## [12] "X1953"                                
## [13] "X1954"                                
## [14] "X1955"                                
## [15] "X1956"                                
## [16] "X1957"                                
## [17] "X1958"                                
## [18] "X1959"                                
## [19] "X1960"                                
## [20] "X1961"                                
## [21] "X1962"                                
## [22] "X1963"                                
## [23] "X1964"                                
## [24] "X1965"                                
## [25] "X1966"                                
## [26] "X1967"                                
## [27] "X1968"                                
## [28] "X1969"                                
## [29] "X1970"                                
## [30] "X1971"                                
## [31] "X1972"                                
## [32] "X1973"                                
## [33] "X1974"                                
## [34] "X1975"                                
## [35] "X1976"                                
## [36] "X1977"                                
## [37] "X1978"                                
## [38] "X1979"                                
## [39] "X1980"                                
## [40] "X1981"                                
## [41] "X1982"                                
## [42] "X1983"                                
## [43] "X1984"                                
## [44] "X1985"                                
## [45] "X1986"                                
## [46] "X1987"                                
## [47] "X1988"                                
## [48] "X1989"                                
## [49] "X1990"                                
## [50] "X1991"                                
## [51] "X1992"                                
## [52] "X1993"                                
## [53] "X1994"                                
## [54] "X1995"                                
## [55] "X1996"                                
## [56] "X1997"                                
## [57] "X1998"                                
## [58] "X1999"                                
## [59] "X2000"                                
## [60] "X2001"                                
## [61] "X2002"                                
## [62] "X2003"                                
## [63] "X2004"                                
## [64] "X2005"                                
## [65] "X2006"                                
## [66] "X2007"                                
## [67] "X2008"                                
## [68] "X2009"                                
## [69] "X2010"                                
## [70] "X2011"                                
## [71] "X2012"
```

3. Let’s rename the columns. Use rename() to adjust the names as follows. Double check to make sure the rename worked correctly. Make sure to replace the old fisheries object with a new one so you can keep the column names.
country = Country..Country.
commname = Species..ASFIS.species.
sciname = Species..ASFIS.species._2
spcode = Species..ASFIS.species._1
spgroup = Species..ISSCAAP.group.
spgroupname = Species..ISSCAAP.group._1
region = Fishing.area..FAO.major.fishing.area.
unit = Measure..Measure.

```r
fisheries <-
fisheries %>%
  rename(
 country = Country..Country.,  ## I was having problems running the code because it turns out I already ran it once so I couldn't run it again. 
      commname    = Species..ASFIS.species., 
        sciname     = Species..ASFIS.species._2, 
     spcode      = Species..ASFIS.species._1, 
      spgroup     = Species..ISSCAAP.group., 
     spgroupname = Species..ISSCAAP.group._1, 
     region      = Fishing.area..FAO.major.fishing.area., 
     unit        = Measure..Measure.)
fisheries
```

```
## # A tibble: 17,692 x 71
##    country commname spgroup spgroupname spcode sciname region unit  X1950
##    <chr>   <chr>      <dbl> <chr>       <chr>  <chr>    <dbl> <chr> <chr>
##  1 Albania Angelsh…      38 Sharks, ra… 10903… Squati…     37 Quan… ...  
##  2 Albania Atlanti…      36 Tunas, bon… 17501… Sarda …     37 Quan… ...  
##  3 Albania Barracu…      37 Miscellane… 17710… Sphyra…     37 Quan… ...  
##  4 Albania Blue an…      45 Shrimps, p… 22802… Ariste…     37 Quan… ...  
##  5 Albania Blue wh…      32 Cods, hake… 14804… Microm…     37 Quan… ...  
##  6 Albania Bluefish      37 Miscellane… 17020… Pomato…     37 Quan… ...  
##  7 Albania Bogue         33 Miscellane… 17039… Boops …     37 Quan… ...  
##  8 Albania Caramot…      45 Shrimps, p… 22801… Penaeu…     37 Quan… ...  
##  9 Albania Catshar…      38 Sharks, ra… 10801… Scylio…     37 Quan… ...  
## 10 Albania Common …      57 Squids, cu… 32102… Sepia …     37 Quan… ...  
## # … with 17,682 more rows, and 62 more variables: X1951 <chr>,
## #   X1952 <chr>, X1953 <chr>, X1954 <chr>, X1955 <chr>, X1956 <chr>,
## #   X1957 <chr>, X1958 <chr>, X1959 <chr>, X1960 <chr>, X1961 <chr>,
## #   X1962 <chr>, X1963 <chr>, X1964 <chr>, X1965 <chr>, X1966 <chr>,
## #   X1967 <chr>, X1968 <chr>, X1969 <chr>, X1970 <chr>, X1971 <chr>,
## #   X1972 <chr>, X1973 <chr>, X1974 <chr>, X1975 <chr>, X1976 <chr>,
## #   X1977 <chr>, X1978 <chr>, X1979 <chr>, X1980 <chr>, X1981 <chr>,
## #   X1982 <chr>, X1983 <chr>, X1984 <chr>, X1985 <chr>, X1986 <chr>,
## #   X1987 <chr>, X1988 <chr>, X1989 <chr>, X1990 <chr>, X1991 <chr>,
## #   X1992 <chr>, X1993 <chr>, X1994 <chr>, X1995 <chr>, X1996 <chr>,
## #   X1997 <chr>, X1998 <chr>, X1999 <chr>, X2000 <chr>, X2001 <chr>,
## #   X2002 <chr>, X2003 <chr>, X2004 <chr>, X2005 <chr>, X2006 <chr>,
## #   X2007 <chr>, X2008 <chr>, X2009 <chr>, X2010 <chr>, X2011 <chr>,
## #   X2012 <chr>
```


4. Are these data tidy? Why or why not, and, how do you know? No, the column names are values of the variable date.

```r
glimpse(fisheries)## Looking at the data below we can determine it ins't tidy. Each year is listed as its own column and acts as a variable which is really problematic. 
```

```
## Observations: 17,692
## Variables: 71
## $ country     <chr> "Albania", "Albania", "Albania", "Albania", "Albania…
## $ commname    <chr> "Angelsharks, sand devils nei", "Atlantic bonito", "…
## $ spgroup     <dbl> 38, 36, 37, 45, 32, 37, 33, 45, 38, 57, 33, 57, 31, …
## $ spgroupname <chr> "Sharks, rays, chimaeras", "Tunas, bonitos, billfish…
## $ spcode      <chr> "10903XXXXX", "1750100101", "17710001XX", "228020310…
## $ sciname     <chr> "Squatinidae", "Sarda sarda", "Sphyraena spp", "Aris…
## $ region      <dbl> 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, …
## $ unit        <chr> "Quantity (tonnes)", "Quantity (tonnes)", "Quantity …
## $ X1950       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1951       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1952       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1953       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1954       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1955       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1956       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1957       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1958       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1959       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1960       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1961       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1962       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1963       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1964       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1965       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1966       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1967       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1968       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1969       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1970       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1971       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1972       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1973       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1974       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1975       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1976       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1977       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1978       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1979       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1980       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1981       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1982       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1983       <chr> "...", "...", "...", "...", "...", "...", "559", "..…
## $ X1984       <chr> "...", "...", "...", "...", "...", "...", "392", "..…
## $ X1985       <chr> "...", "...", "...", "...", "...", "...", "406", "..…
## $ X1986       <chr> "...", "...", "...", "...", "...", "...", "499", "..…
## $ X1987       <chr> "...", "...", "...", "...", "...", "...", "564", "..…
## $ X1988       <chr> "...", "...", "...", "...", "...", "...", "724", "..…
## $ X1989       <chr> "...", "...", "...", "...", "...", "...", "583", "..…
## $ X1990       <chr> "...", "...", "...", "...", "...", "...", "754", "..…
## $ X1991       <chr> "...", "...", "...", "...", "...", "...", "283", "..…
## $ X1992       <chr> "...", "...", "...", "...", "...", "...", "196", "..…
## $ X1993       <chr> "...", "...", "...", "...", "...", "...", "150 F", "…
## $ X1994       <chr> "...", "...", "...", "...", "...", "...", "100 F", "…
## $ X1995       <chr> "0 0", "1", "...", "0 0", "0 0", "...", "52", "30", …
## $ X1996       <chr> "53", "2", "...", "3", "2", "...", "104", "8", "..."…
## $ X1997       <chr> "20", "0 0", "...", "0 0", "0 0", "...", "65", "4", …
## $ X1998       <chr> "31", "12", "...", "-", "-", "...", "220", "18", "..…
## $ X1999       <chr> "30", "30", "...", "-", "-", "...", "220", "18", "..…
## $ X2000       <chr> "30", "25", "2", "-", "-", "...", "220", "20", "..."…
## $ X2001       <chr> "16", "30", "...", "-", "-", "...", "120", "23", "..…
## $ X2002       <chr> "79", "24", "...", "34", "6", "...", "150", "84", ".…
## $ X2003       <chr> "1", "4", "...", "22", "...", "...", "84", "178", ".…
## $ X2004       <chr> "4", "2", "2", "15", "1", "2", "76", "285", "1", "70…
## $ X2005       <chr> "68", "23", "4", "12", "5", "6", "68", "150", "2", "…
## $ X2006       <chr> "55", "30", "7", "18", "8", "9", "86", "102", "4", "…
## $ X2007       <chr> "12", "19", "...", "...", "-", "-", "132", "18", "..…
## $ X2008       <chr> "23", "27", "...", "...", "-", "-", "132", "23", "..…
## $ X2009       <chr> "14", "21", "...", "...", "-", "-", "154", "20", "..…
## $ X2010       <chr> "78", "23", "7", "...", "-", "-", "80", "228", "..."…
## $ X2011       <chr> "12", "12", "...", "...", "-", "-", "88", "9", "..."…
## $ X2012       <chr> "5", "5", "...", "...", "-", "-", "129", "290", "...…
```

5. We need to tidy the data using gather(). The code below will not run. I have added a bit of code that will prevent you from needing to type in each year from 1950-2012 but you need to complete the remainder.

```r
fisheries_tidy <- 
  fisheries %>% 
  gather(num_range('X',1950:2012), key='year', value='catch') ## we know have the year gathered in a single column 
fisheries_tidy ## the gather function created a "year" column as the key that's specifcally assigned to a  corresponding "value"
```

```
## # A tibble: 1,114,596 x 10
##    country commname spgroup spgroupname spcode sciname region unit  year 
##    <chr>   <chr>      <dbl> <chr>       <chr>  <chr>    <dbl> <chr> <chr>
##  1 Albania Angelsh…      38 Sharks, ra… 10903… Squati…     37 Quan… X1950
##  2 Albania Atlanti…      36 Tunas, bon… 17501… Sarda …     37 Quan… X1950
##  3 Albania Barracu…      37 Miscellane… 17710… Sphyra…     37 Quan… X1950
##  4 Albania Blue an…      45 Shrimps, p… 22802… Ariste…     37 Quan… X1950
##  5 Albania Blue wh…      32 Cods, hake… 14804… Microm…     37 Quan… X1950
##  6 Albania Bluefish      37 Miscellane… 17020… Pomato…     37 Quan… X1950
##  7 Albania Bogue         33 Miscellane… 17039… Boops …     37 Quan… X1950
##  8 Albania Caramot…      45 Shrimps, p… 22801… Penaeu…     37 Quan… X1950
##  9 Albania Catshar…      38 Sharks, ra… 10801… Scylio…     37 Quan… X1950
## 10 Albania Common …      57 Squids, cu… 32102… Sepia …     37 Quan… X1950
## # … with 1,114,586 more rows, and 1 more variable: catch <chr>
```

6. Use glimpse() to look at the categories of the variables. Pay particular attention to year and catch. What do you notice?
There are a lot of missing entries in the data

```r
glimpse(fisheries_tidy) ## Using glimpse, I can see that the year column is listed as a "chr" instead of "dbl". We also need to change blank values into NAs
```

```
## Observations: 1,114,596
## Variables: 10
## $ country     <chr> "Albania", "Albania", "Albania", "Albania", "Albania…
## $ commname    <chr> "Angelsharks, sand devils nei", "Atlantic bonito", "…
## $ spgroup     <dbl> 38, 36, 37, 45, 32, 37, 33, 45, 38, 57, 33, 57, 31, …
## $ spgroupname <chr> "Sharks, rays, chimaeras", "Tunas, bonitos, billfish…
## $ spcode      <chr> "10903XXXXX", "1750100101", "17710001XX", "228020310…
## $ sciname     <chr> "Squatinidae", "Sarda sarda", "Sphyraena spp", "Aris…
## $ region      <dbl> 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, …
## $ unit        <chr> "Quantity (tonnes)", "Quantity (tonnes)", "Quantity …
## $ year        <chr> "X1950", "X1950", "X1950", "X1950", "X1950", "X1950"…
## $ catch       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
```

7. From question 6 you should see that there are a lot of entries that are missing. In R, these are referred to as NA's but they can be coded in different ways by the scientists in a given study. In order to make the data tidy, we need to deal with them. As a preview to our next lab, run the following code. It removes the 'X' from the years and changes the catch column from a character into a numeric. This forces the blank entries to become NAs. The error "NAs introduced by coercion" indicates their replacement.

```r
fisheries_tidy <- 
  fisheries_tidy %>% 
  mutate(
    year= as.numeric(str_replace(year, 'X', '')), #remove the X from year. This will help us categorize it as a "chr"
    catch= as.numeric(str_replace(catch, c(' F','...','-'), replacement = '')) #replace character strings with NA
    )
```

```
## Warning in evalq(as.numeric(str_replace(catch, c(" F", "...", "-"),
## replacement = "")), : NAs introduced by coercion
```

8. Are the data tidy? Why?
The dates are organized into a new column called year and each observation has its own row.

```r
fisheries_tidy
```

```
## # A tibble: 1,114,596 x 10
##    country commname spgroup spgroupname spcode sciname region unit   year
##    <chr>   <chr>      <dbl> <chr>       <chr>  <chr>    <dbl> <chr> <dbl>
##  1 Albania Angelsh…      38 Sharks, ra… 10903… Squati…     37 Quan…  1950
##  2 Albania Atlanti…      36 Tunas, bon… 17501… Sarda …     37 Quan…  1950
##  3 Albania Barracu…      37 Miscellane… 17710… Sphyra…     37 Quan…  1950
##  4 Albania Blue an…      45 Shrimps, p… 22802… Ariste…     37 Quan…  1950
##  5 Albania Blue wh…      32 Cods, hake… 14804… Microm…     37 Quan…  1950
##  6 Albania Bluefish      37 Miscellane… 17020… Pomato…     37 Quan…  1950
##  7 Albania Bogue         33 Miscellane… 17039… Boops …     37 Quan…  1950
##  8 Albania Caramot…      45 Shrimps, p… 22801… Penaeu…     37 Quan…  1950
##  9 Albania Catshar…      38 Sharks, ra… 10801… Scylio…     37 Quan…  1950
## 10 Albania Common …      57 Squids, cu… 32102… Sepia …     37 Quan…  1950
## # … with 1,114,586 more rows, and 1 more variable: catch <dbl>
```

9. You are a fisheries scientist studying cephalopod catch during 2012. Identify the top five consumers (by country) of cephalopods (don't worry about species for now). Restrict the data frame only to our variables of interest.

```r
fisheries_tidy %>%
  select(sciname, country, year, catch) %>%
  filter( year==2012, sciname=="Cephalopoda")%>% 
  arrange(desc(catch))
```

```
## # A tibble: 23 x 4
##    sciname     country                   year catch
##    <chr>       <chr>                    <dbl> <dbl>
##  1 Cephalopoda India                     2012 82456
##  2 Cephalopoda China                     2012 61821
##  3 Cephalopoda India                     2012 14388
##  4 Cephalopoda Madagascar                2012  6206
##  5 Cephalopoda Mozambique                2012  2150
##  6 Cephalopoda Taiwan Province of China  2012  1050
##  7 Cephalopoda Somalia                   2012   600
##  8 Cephalopoda Mauritania                2012   107
##  9 Cephalopoda Italy                     2012    66
## 10 Cephalopoda France                    2012    27
## # … with 13 more rows
```

Top 5: India, Madagascar, Mozambique and Taiwan province of China 












