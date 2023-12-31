Mini Data-Analysis Deliverable 2
================

*To complete this milestone, you can either edit [this `.rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are commented out with
`<!--- start your work here--->`. When you are done, make sure to knit
to an `.md` file by changing the output in the YAML header to
`github_document`, before submitting a tagged release on canvas.*

# Welcome to the rest of your mini data analysis project!

In Milestone 1, you explored your data. and came up with research
questions. This time, we will finish up our mini data analysis and
obtain results for your data by:

- Making summary tables and graphs
- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

We will also explore more in depth the concept of *tidy data.*

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class.

# Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to your mini-analysis GitHub repository, and tag a
release on GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 50 points: 45 for your analysis, and
5 for overall reproducibility, cleanliness, and coherence of the Github
submission.

**Research Questions**: In Milestone 1, you chose two research questions
to focus on. Wherever realistic, your work in this milestone should
relate to these research questions whenever we ask for justification
behind your work. In the case that some tasks in this milestone don’t
align well with one of your research questions, feel free to discuss
your results in the context of a different research question.

# Learning Objectives

By the end of this milestone, you should:

- Understand what *tidy* data is, and how to create it using `tidyr`.
- Generate a reproducible and clear report using R Markdown.
- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

# Setup

Begin by loading your data and the tidyverse package below:

``` r
library(datateachr) # <- might contain the data you picked!
library(tidyverse)
```

# Task 1: Process and summarize your data

From milestone 1, you should have an idea of the basic structure of your
dataset (e.g. number of rows and columns, class types, etc.). Here, we
will start investigating your data more in-depth using various data
manipulation functions.

### 1.1 (1 point)

First, write out the 4 research questions you defined in milestone 1
were. This will guide your work through milestone 2:

<!-------------------------- Start your work below ---------------------------->

1.  Which ward(s) have the most fire-safe apartment buildings? Is this
    correlated with year built?
2.  Which ward(s) have the most barrier-free, accessible apartment
    buildings? Is this correlated with year built?
3.  What are the most common window types? Is this correlated with ward
    or year built?
4.  Does the year that the apartment building was built influence
    whether or not it has air conditioning?
    <!----------------------------------------------------------------------------->

Here, we will investigate your data using various data manipulation and
graphing functions.

### 1.2 (8 points)

Now, for each of your four research questions, choose one task from
options 1-4 (summarizing), and one other task from 4-8 (graphing). You
should have 2 tasks done for each research question (8 total). Make sure
it makes sense to do them! (e.g. don’t use a numerical variables for a
task that needs a categorical variable.). Comment on why each task helps
(or doesn’t!) answer the corresponding research question.

Ensure that the output of each operation is printed!

Also make sure that you’re using dplyr and ggplot2 rather than base R.
Outside of this project, you may find that you prefer using base R
functions for certain tasks, and that’s just fine! But part of this
project is for you to practice the tools we learned in class, which is
dplyr and ggplot2.

**Summarizing:**

1.  Compute the *range*, *mean*, and *two other summary statistics* of
    **one numerical variable** across the groups of **one categorical
    variable** from your data.
2.  Compute the number of observations for at least one of your
    categorical variables. Do not use the function `table()`!
3.  Create a categorical variable with 3 or more groups from an existing
    numerical variable. You can use this new variable in the other
    tasks! *An example: age in years into “child, teen, adult, senior”.*
4.  Compute the proportion and counts in each category of one
    categorical variable across the groups of another categorical
    variable from your data. Do not use the function `table()`!

**Graphing:**

6.  Create a graph of your choosing, make one of the axes logarithmic,
    and format the axes labels so that they are “pretty” or easier to
    read.
7.  Make a graph where it makes sense to customize the alpha
    transparency.

Using variables and/or tables you made in one of the “Summarizing”
tasks:

8.  Create a graph that has at least two geom layers.
9.  Create 3 histograms, with each histogram having different sized
    bins. Pick the “best” one and explain why it is the best.

Make sure it’s clear what research question you are doing each operation
for!

<!------------------------- Start your work below ----------------------------->

#### Which ward(s) have the most fire-safe apartment buildings? Is this correlated with year built?

**Summarizing:** I will compute the number of observations for a new
categorical variable that indicates whether an apartment building is or
is not fire-safe; here, fire-safe is defined as a building with fire
alarms, exterior fire escapes, and sprinkler systems. By doing this,
I’ll be able to see the ward(s) that have the most fire-safe apartment
buildings. Moreover, I’ll be able to see the breakdown of fire-safe,
partially fire-safe, or not fire-safe buildings in each ward.

``` r
# creates a new categorical variable that says if an apartment building is or is not fire-safe
apt_buildings <- apt_buildings %>%
  filter(!ward == "YY") %>%
  mutate(fire_safe = 
           # filters according to criteria
           case_when(fire_alarm == "YES" & sprinkler_system == "YES" & exterior_fire_escape == "YES" ~ "YES",
                     fire_alarm == "YES" & sprinkler_system == "YES" ~ "PARTIAL",
                     fire_alarm == "YES" & exterior_fire_escape == "YES" ~ "PARTIAL",
                     TRUE ~ "NO")) 

# counts the number of observations for the fire-safe variable per ward
fire_safe <- apt_buildings %>%
  count(ward, fire_safe) # counts the number of buildings that are or are not fire-safe 
print(fire_safe)
```

    ## # A tibble: 74 × 3
    ##    ward  fire_safe     n
    ##    <chr> <chr>     <int>
    ##  1 01    NO           14
    ##  2 01    PARTIAL      55
    ##  3 01    YES          12
    ##  4 02    NO           39
    ##  5 02    PARTIAL      81
    ##  6 02    YES          10
    ##  7 03    NO          122
    ##  8 03    PARTIAL     105
    ##  9 03    YES          13
    ## 10 04    NO           42
    ## # ℹ 64 more rows

**Graphing:** I will create a bar graph as this will allow me to see the
breakdown of fire-safe, partially fire-safe, or not fire-safe buildings
in each ward. I will also have labels that will display the “n” value at
the top of each bar for increased clarity.

``` r
bar_fire <- fire_safe %>%
  ggplot(aes(x = ward, # plots with wards as the x-axis
             y = n, # plots with the count as the y-axis
             fill = fire_safe)) + # groups by fire-safe categorization
  geom_bar(stat = "identity", 
           position = position_dodge(), # staggers bars side by side
           width = 0.8, # adjusts bar width
           alpha = 0.5) + # creates a bar plot at half transparency
  geom_text(aes(label = n), # labels each bar with the n
            position = position_dodge(0.9), # adjusts placement of labels
            size = 2, # adjusts size of labels
            vjust = -0.5) # adjusts vertical placement of labels

print(bar_fire)
```

![](milestone_2_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

#### Which ward(s) have the most barrier-free, accessible apartment buildings? Is this correlated with year built?

**Summarizing:** I will compute the number of observations for a new
categorical variable that indicates whether an apartment building is or
is not barrier-free and accessible; this is defined as a building with
barrier-free accessibility entrances, accessible parking spaces, an
elevator(s), as well as at least one barrier-free accessible unit. By
doing this, I’ll be able to see the ward(s) that have the most
barrier-free, accessible apartment buildings.

``` r
# creates a new categorical variable that says if an apartment building is or is not accessible
apt_buildings <- apt_buildings %>%
  filter(!ward == "YY") %>% 
  mutate(accessible = 
           # filters according to criteria
           case_when(barrier_free_accessibilty_entr == "YES" & 
                       no_of_accessible_parking_spaces >0 & 
                       no_of_elevators >0 &
                       no_barrier_free_accessible_units >0 ~ "YES",
                     TRUE ~ "NO")) 

# counts the number of observations for the accessible variable per ward
accessible <- apt_buildings %>%
  count(ward, accessible) # counts the number of buildings that are or are not accessible 
print(accessible)
```

    ## # A tibble: 50 × 3
    ##    ward  accessible     n
    ##    <chr> <chr>      <int>
    ##  1 01    NO            59
    ##  2 01    YES           22
    ##  3 02    NO           111
    ##  4 02    YES           19
    ##  5 03    NO           211
    ##  6 03    YES           29
    ##  7 04    NO           174
    ##  8 04    YES           24
    ##  9 05    NO           196
    ## 10 05    YES           37
    ## # ℹ 40 more rows

**Graphing:** I will create a bar graph as this will allow me to see the
breakdown of barrier free, accessible buildings in each ward. I will
also have labels that will display the “n” value at the top of each bar
for increased clarity.

``` r
bar_accessible <- accessible %>%
  ggplot(aes(x = ward, # plots with wards as the x-axis
             y = n, # plots with the count as the y-axis
             fill = accessible)) + # groups by accessible categorization
  geom_bar(stat = "identity", 
           position = position_dodge(), # staggers bars side by side
           width = 0.8, # adjusts bar width
           alpha = 0.5) + # creates a bar plot at half transparency
  geom_text(aes(label = n), # labels each bar with the n
            position = position_dodge(0.9), # adjusts placement of labels
            size = 2, # adjusts size of labels
            vjust = -0.5) # adjusts vertical placement of labels

print(bar_accessible)
```

![](milestone_2_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

#### What are the most common window types? Is this correlated with ward or year built?

**Summarizing:** I will compute the range, mean, and median of years
built across the groups of window type. I will also count the number of
buildings within each. Altogether, this will allow me to examine if
there is a trend between when a certain window type is included and the
year the apartment building was built, as well as its popularity.

``` r
window_summary <- apt_buildings %>%
  group_by(window_type) %>% # groups by window type
  summarise(year_earliest = min(year_built, na.rm = TRUE), # identifies earliest year 
            year_latest = max(year_built, na.rm = TRUE), # identifies latest year
            year_mean = mean(year_built, na.rm = TRUE), # identifies average year 
            year_med = median(year_built, na.rm = TRUE), # identifies median year 
            year_n = n()) # identifies number of buildings 

print(window_summary)
```

    ## # A tibble: 4 × 6
    ##   window_type year_earliest year_latest year_mean year_med year_n
    ##   <chr>               <dbl>       <dbl>     <dbl>    <dbl>  <int>
    ## 1 DOUBLE PANE          1805        2019     1962.     1961   2890
    ## 2 SINGLE PANE          1885        2006     1959.     1962    469
    ## 3 THERMAL              1900        2017     1968.     1966     87
    ## 4 <NA>                 1929        2019     1995.     2018      8

**Graphing:** I will create a density graph of the window types as this
will allow me to visualize the distribution of data over the years
built. As there will be overlap, I will customize the alpha transparency
to ensure that all three graphs will be visible.

``` r
density_window <- apt_buildings %>%
  filter(!is.na(window_type) & !is.na(year_built)) %>% # removes any missing data
  ggplot(aes(x = year_built, # plots with years as the x-axis
             fill = window_type, colour = window_type)) + # groups based on window type
  geom_density(alpha = 0.25) # customizes alpha transparency of density plot

print(density_window)
```

![](milestone_2_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

#### Does the year that the apartment building was built influence whether or not it has air conditioning?

**Summarizing:** I will compute the range, mean, and median of years
built across the types of air conditioning. I will also count the number
of buildings within each. Altogether, this will allow me to examine if
there is a trend between whether a building has air conditioning and the
year the apartment building was built, as well as which air conditioning
method is most employed.

``` r
ac_summary <- apt_buildings %>%
  group_by(air_conditioning) %>% # groups by window type
  summarise(year_earliest = min(year_built, na.rm = TRUE), # identifies earliest year 
            year_latest = max(year_built, na.rm = TRUE), # identifies latest year
            year_mean = mean(year_built, na.rm = TRUE), # identifies average year 
            year_med = median(year_built, na.rm = TRUE), # identifies median year 
            year_n = n()) # identifies number of buildings 

print(ac_summary)
```

    ## # A tibble: 4 × 6
    ##   air_conditioning year_earliest year_latest year_mean year_med year_n
    ##   <chr>                    <dbl>       <dbl>     <dbl>    <dbl>  <int>
    ## 1 CENTRAL AIR               1928        2019     1987.    1989     210
    ## 2 INDIVIDUAL UNITS          1888        2019     1963.    1965     289
    ## 3 NONE                      1805        2015     1960.    1960    2870
    ## 4 <NA>                      1902        2019     1951.    1954.     85

**Graphing:** I will create a density graph of the air conditioning
methods as this will allow me to visualize the distribution of data over
the years built. As there will be overlap, I will customize the alpha
transparency to ensure that all three graphs will be visible.

``` r
density_ac <- apt_buildings %>%
  filter(!is.na(air_conditioning) & !is.na(year_built)) %>% # removes any missing data
  ggplot(aes(x = year_built, # plots with years as the x-axis
             fill = air_conditioning, colour = air_conditioning)) + # groups based on air conditioning
  geom_density(alpha = 0.25) # customizes alpha transparency of density plot

print(density_ac)
```

![](milestone_2_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

<!----------------------------------------------------------------------------->

### 1.3 (2 points)

Based on the operations that you’ve completed, how much closer are you
to answering your research questions? Think about what aspects of your
research questions remain unclear. Can your research questions be
refined, now that you’ve investigated your data a bit more? Which
research questions are yielding interesting results?

<!------------------------- Write your answer here ---------------------------->

Based on the activities I’ve done, I feel that I am somewhat closer to
answering my research questions. I have gained a better insight into the
dataset, as well as the relationship between my variables of interest;
for example, while double pane windows have been commonly employed since
the early 1800s, it appears that the use of single pane windows has
ceased. Though much of the knowledge is preliminary, I am now able to
further refine my research questions and identify which questions are
worthy of further investigation. Moving forward, I would like to delve
into my definitions of “fire-safe” and “accessible” buildings by looking
at the year they were built. My current hypothesis is that those who
were built later are more fire-safe and accessible. This is because
unlike with window type and air conditioning, these are traits that are
less likely to be implemented after the building is built.
<!----------------------------------------------------------------------------->

# Task 2: Tidy your data

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

- Each row is an **observation**
- Each column is a **variable**
- Each cell is a **value**

### 2.1 (2 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have \>8 variables, just
pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->

Based on the definition above, I feel that my data is semi-tidy. The
eight variables I chose from the dataset are “air_conditioning”,
“amenities”, “balconies”, “barrier_free_accessibility_entr”,
“bike_parking”, “exterior_fire_escape”, “fire_alarm”, and
“garbage_chutes”. Of these, six of the eight are tidy; the two untidy
ones are “amenities” and “bike_parking” as each cell has multiple
values. It would therefore be of use to separate the data within each of
them into additional, more specific columns.
<!----------------------------------------------------------------------------->

### 2.2 (4 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

**Reasoning:** I will be tidying the data and then untidying it to its
original state. As I mentioned above, “amenities” and “bike_parking” are
untidy variables. This is because both consists of multiple
observations; for example, bike_parking lists both inside and outside
bike parking. Therefore, to tidy this I will be splitting amenities to
five separate columns - one for child play area, one for exercise rooms,
one for sauna, one for pools, and one for recreation. I will also be
splitting bike_parking to two separate columns - one for indoor bike
parking, and one for outdoor bike parking.

#### Subsetting Data

``` r
# creates a separate dataset with the eight variables of interest from apt_buildings
apt_buildings_sub <- apt_buildings %>%
  select(air_conditioning:garbage_chutes)
print(apt_buildings_sub)
```

    ## # A tibble: 3,454 × 8
    ##    air_conditioning amenities      balconies barrier_free_accessi…¹ bike_parking
    ##    <chr>            <chr>          <chr>     <chr>                  <chr>       
    ##  1 NONE             Outdoor rec f… YES       YES                    0 indoor pa…
    ##  2 NONE             Outdoor pool   YES       NO                     0 indoor pa…
    ##  3 NONE             <NA>           YES       NO                     Not Availab…
    ##  4 NONE             <NA>           YES       YES                    Not Availab…
    ##  5 NONE             <NA>           NO        NO                     12 indoor p…
    ##  6 NONE             <NA>           NO        NO                     Not Availab…
    ##  7 NONE             <NA>           NO        YES                    Not Availab…
    ##  8 CENTRAL AIR      Indoor pool ,… YES       NO                     Not Availab…
    ##  9 NONE             <NA>           YES       YES                    0 indoor pa…
    ## 10 NONE             Indoor recrea… YES       YES                    Not Availab…
    ## # ℹ 3,444 more rows
    ## # ℹ abbreviated name: ¹​barrier_free_accessibilty_entr
    ## # ℹ 3 more variables: exterior_fire_escape <chr>, fire_alarm <chr>,
    ## #   garbage_chutes <chr>

#### Tidying Data

``` r
# subsetting
apt_buildings_tidy <- apt_buildings_sub %>%
  # amenities 
  mutate(amenities_child = # creates a column specific for "child play area"
           case_when(amenities = str_detect(amenities, "Child play area") ~ "YES",
                     TRUE ~ "NO"),
         amenities_exercise = # creates a column specific for "indoor exercise room"
           case_when(amenities = str_detect(amenities, "Indoor exercise room") ~ "INDOOR",
                     TRUE ~ "NO"),
         amenities_sauna = # creates a column specific for "sauna"
           case_when(amenities = str_detect(amenities, "Sauna") ~ "YES",
                     TRUE ~ "NO"),
         amenities_pool = # creates a column specific for "indoor pool" and "outdoor pool"
           case_when(amenities = str_detect(amenities, "Indoor pool") ~ "INDOOR",
                     amenities = str_detect(amenities, "Outdoor pool") ~ "OUTDOOR",
                     TRUE ~ "NO"),
         amenities_rec = # creates a column specific for "indoor recreation room", "outdoor rec facilities"
           case_when(amenities = str_detect(amenities, "Indoor recreation room") ~ "INDOOR",
                     amenities = str_detect(amenities, "Outdoor rec facilities") ~ "OUTDOOR",
                     TRUE ~ "NO"),
         ) %>%
  # bike parking
  mutate(# creates a column specific for the number of indoor bike parking spots
         bike_parking_in = str_c(str_match(bike_parking, "(.*) indoor parking spots")[, 2]),
         # creates a column specific for the number of outdoor bike parking spots
         bike_parking_out = str_c(str_match(bike_parking, "(.*) outdoor parking spots$")[, 2]),
         bike_parking_out = str_sub(bike_parking_out, -2)
         ) %>%
  # removing untidy columns
  select(-c(amenities, bike_parking))
```

``` r
# previews tidied dataset
print(apt_buildings_tidy)
```

    ## # A tibble: 3,454 × 13
    ##    air_conditioning balconies barrier_free_accessibilty_e…¹ exterior_fire_escape
    ##    <chr>            <chr>     <chr>                         <chr>               
    ##  1 NONE             YES       YES                           NO                  
    ##  2 NONE             YES       NO                            NO                  
    ##  3 NONE             YES       NO                            NO                  
    ##  4 NONE             YES       YES                           YES                 
    ##  5 NONE             NO        NO                            NO                  
    ##  6 NONE             NO        NO                            <NA>                
    ##  7 NONE             NO        YES                           NO                  
    ##  8 CENTRAL AIR      YES       NO                            NO                  
    ##  9 NONE             YES       YES                           NO                  
    ## 10 NONE             YES       YES                           NO                  
    ## # ℹ 3,444 more rows
    ## # ℹ abbreviated name: ¹​barrier_free_accessibilty_entr
    ## # ℹ 9 more variables: fire_alarm <chr>, garbage_chutes <chr>,
    ## #   amenities_child <chr>, amenities_exercise <chr>, amenities_sauna <chr>,
    ## #   amenities_pool <chr>, amenities_rec <chr>, bike_parking_in <chr>,
    ## #   bike_parking_out <chr>

#### Untidying Data

``` r
apt_buildings_untidy <- apt_buildings_tidy %>%
  # amenities 
  mutate(amenities_child = # reverts text back to "Child play area"
           case_when(amenities_child = str_detect(amenities_child, "YES") ~ "Child play area",
                     TRUE ~ NA_character_),
         amenities_exercise = # reverts text back to "Indoor exercise room"
           case_when(amenities_exercise = str_detect(amenities_exercise, "INDOOR") ~ "Indoor exercise room",
                     TRUE ~ NA_character_),
         amenities_sauna = # reverts text back to "Sauna"
           case_when(amenities_sauna = str_detect(amenities_sauna, "YES") ~ "Sauna",
                     TRUE ~ NA_character_),
         amenities_pool = # reverts text back to "Indoor pool" and "Outdoor pool"
           case_when(amenities_pool = str_detect(amenities_pool, "INDOOR") ~ "Indoor pool",
                     amenities_pool = str_detect(amenities_pool, "OUTDOOR") ~ "Outdoor pool",
                     TRUE ~ NA_character_),
         amenities_rec = # reverts text back to "Indoor recreation room" and "Outdoor rec facilities"
           case_when(amenities_rec = str_detect(amenities_rec, "INDOOR") ~ "Indoor recreation room",
                     amenities_rec = str_detect(amenities_rec, "OUTDOOR") ~ "Outdoor rec facilities",
                     TRUE ~ NA_character_),) %>%
  # bike parking
  mutate(bike_parking_in = # includes "indoor parking spots" after number
           case_when(bike_parking_in >=0 ~ paste0(bike_parking_in, " indoor parking spots"),
                     TRUE ~ NA_character_),
         bike_parking_out = # includes "outdoor parking spots" after number
           case_when(bike_parking_out >=0 ~ paste0(bike_parking_out, " outdoor parking spots"),
                     TRUE ~ NA_character_),
         ) %>%
  # collapses observations into a single column for amenities
  unite("amenities", amenities_child:amenities_rec, na.rm = TRUE, remove = TRUE, sep = ", ") %>%
  # collapses observations into a single column for bike_parking
  unite("bike_parking", bike_parking_in:bike_parking_out, na.rm = TRUE, remove = TRUE, sep = " and ")
```

``` r
# previews untidied dataset
print(apt_buildings_untidy)
```

    ## # A tibble: 3,454 × 8
    ##    air_conditioning balconies barrier_free_accessibilty_e…¹ exterior_fire_escape
    ##    <chr>            <chr>     <chr>                         <chr>               
    ##  1 NONE             YES       YES                           NO                  
    ##  2 NONE             YES       NO                            NO                  
    ##  3 NONE             YES       NO                            NO                  
    ##  4 NONE             YES       YES                           YES                 
    ##  5 NONE             NO        NO                            NO                  
    ##  6 NONE             NO        NO                            <NA>                
    ##  7 NONE             NO        YES                           NO                  
    ##  8 CENTRAL AIR      YES       NO                            NO                  
    ##  9 NONE             YES       YES                           NO                  
    ## 10 NONE             YES       YES                           NO                  
    ## # ℹ 3,444 more rows
    ## # ℹ abbreviated name: ¹​barrier_free_accessibilty_entr
    ## # ℹ 4 more variables: fire_alarm <chr>, garbage_chutes <chr>, amenities <chr>,
    ## #   bike_parking <chr>

<!----------------------------------------------------------------------------->

### 2.3 (4 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the remaining tasks:

<!-------------------------- Start your work below ---------------------------->

1.  Which ward(s) have the most fire-safe apartment buildings?
2.  Which ward(s) have the most barrier-free, accessible apartment
    buildings?

<!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->

As mentioned above, I would like to delve into my definitions of
“fire-safe” and “accessible” buildings by looking at the year they were
built moving forward. My current hypothesis is that those who were built
later are more fire-safe and accessible. This is because unlike with my
other questions relating window type and air conditioning to year built,
these are traits that are more inherent to the building’s construction.
Moreover, the two questions I chose yielded more interesting results in
Exercise 1.
<!----------------------------------------------------------------------------->

Now, try to choose a version of your data that you think will be
appropriate to answer these 2 questions. Use between 4 and 8 functions
that we’ve covered so far (i.e. by filtering, cleaning, tidying,
dropping irrelevant columns, etc.).

(If it makes more sense, then you can make/pick two versions of your
data, one for each research question.)

<!--------------------------- Start your work below --------------------------->

``` r
apt_buildings_fire <- apt_buildings %>%
  # selecting variables of interest
  select(id, year_built, ward, fire_alarm, sprinkler_system, exterior_fire_escape) %>%
  # removes ward YY as it's not a real ward
  filter(!ward == "YY") %>% 
  # creates fire_safe variable according to definition
  mutate(fire_safe = 
           # filters according to criteria
           case_when(fire_alarm == "YES" & sprinkler_system == "YES" & exterior_fire_escape == "YES" ~ "YES",
                     fire_alarm == "YES" & sprinkler_system == "YES" ~ "PARTIAL",
                     fire_alarm == "YES" & exterior_fire_escape == "YES" ~ "PARTIAL",
                     TRUE ~ "NO")) %>%
  # removes observations that are missing data required for fire-safe definition
  subset(!is.na(fire_alarm) & !is.na(sprinkler_system) & !is.na(exterior_fire_escape))

print(apt_buildings_fire)
```

    ## # A tibble: 3,358 × 7
    ##       id year_built ward  fire_alarm sprinkler_system exterior_fire_escape
    ##    <dbl>      <dbl> <chr> <chr>      <chr>            <chr>               
    ##  1 10359       1967 17    YES        YES              NO                  
    ##  2 10360       1970 17    YES        YES              NO                  
    ##  3 10361       1927 03    YES        NO               NO                  
    ##  4 10362       1959 03    YES        YES              YES                 
    ##  5 10363       1943 02    YES        NO               NO                  
    ##  6 10365       1959 02    YES        NO               NO                  
    ##  7 10366       1971 02    YES        YES              NO                  
    ##  8 10367       1969 13    YES        YES              NO                  
    ##  9 10368       1972 07    YES        YES              NO                  
    ## 10 10369       1945 09    YES        NO               NO                  
    ## # ℹ 3,348 more rows
    ## # ℹ 1 more variable: fire_safe <chr>

``` r
apt_buildings_accessible <- apt_buildings %>%
  # selecting variables of interest
  select(id, year_built, ward, barrier_free_accessibilty_entr, no_of_accessible_parking_spaces, 
         no_of_elevators, no_barrier_free_accessible_units) %>%
  # removes ward YY as it's not a real ward
  filter(!ward == "YY") %>% 
  # creates accessible variable according to definition
  mutate(accessible = 
           # filters according to criteria
           case_when(barrier_free_accessibilty_entr == "YES" & 
                       no_of_accessible_parking_spaces >0 & 
                       no_of_elevators >0 &
                       no_barrier_free_accessible_units >0 ~ "YES",
                     TRUE ~ "NO")) %>%
  # removes observations that are missing data required for accessible definition
  subset(!is.na(barrier_free_accessibilty_entr) & !is.na(no_of_accessible_parking_spaces) 
         & !is.na(no_of_elevators) & !is.na(no_barrier_free_accessible_units))

print(apt_buildings_accessible)
```

    ## # A tibble: 3,279 × 8
    ##       id year_built ward  barrier_free_accessibilty_entr no_of_accessible_park…¹
    ##    <dbl>      <dbl> <chr> <chr>                                            <dbl>
    ##  1 10359       1967 17    YES                                                  8
    ##  2 10360       1970 17    NO                                                  10
    ##  3 10361       1927 03    NO                                                  20
    ##  4 10362       1959 03    YES                                                 42
    ##  5 10363       1943 02    NO                                                  12
    ##  6 10365       1959 02    YES                                                  5
    ##  7 10366       1971 02    NO                                                   1
    ##  8 10367       1969 13    YES                                                  1
    ##  9 10368       1972 07    YES                                                  6
    ## 10 10369       1945 09    YES                                                 12
    ## # ℹ 3,269 more rows
    ## # ℹ abbreviated name: ¹​no_of_accessible_parking_spaces
    ## # ℹ 3 more variables: no_of_elevators <dbl>,
    ## #   no_barrier_free_accessible_units <dbl>, accessible <chr>

<!----------------------------------------------------------------------------->

# Task 3: Modelling

## 3.0 (no points)

Pick a research question from 1.2, and pick a variable of interest
(we’ll call it “Y”) that’s relevant to the research question. Indicate
these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**: Which ward(s) have the most barrier-free,
accessible apartment buildings? Is this correlated with year built?

**Variable of interest**: no_barrier_free_accessible_units

<!----------------------------------------------------------------------------->

## 3.1 (3 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

- **Note**: It’s OK if you don’t know how these models/tests work. Here
  are some examples of things you can do here, but the sky’s the limit.

  - You could fit a model that makes predictions on Y using another
    variable, by using the `lm()` function.
  - You could test whether the mean of Y equals 0 using `t.test()`, or
    maybe the mean across two groups are different using `t.test()`, or
    maybe the mean across multiple groups are different using `anova()`
    (you may have to pivot your data for the latter two).
  - You could use `lm()` to test for significance of regression
    coefficients.

<!-------------------------- Start your work below ---------------------------->

``` r
# fits a linear model between the number of barrier free accessible units (Y) and year built (X)
model <- lm(no_barrier_free_accessible_units ~ year_built, apt_buildings_accessible)

print(model)
```

    ## 
    ## Call:
    ## lm(formula = no_barrier_free_accessible_units ~ year_built, data = apt_buildings_accessible)
    ## 
    ## Coefficients:
    ## (Intercept)   year_built  
    ##    -385.035        0.201

<!----------------------------------------------------------------------------->

## 3.2 (3 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

- Be sure to indicate in writing what you chose to produce.
- Your code should either output a tibble (in which case you should
  indicate the column that contains the thing you’re looking for), or
  the thing you’re looking for itself.
- Obtain your results using the `broom` package if possible. If your
  model is not compatible with the broom function you’re needing, then
  you can obtain your results by some other means, but first indicate
  which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

I chose to look at the p-value for the intercept and slope of the linear
model I created. The results can be seen in the fifth column (titled
“p-value”) in the first and second row respectively.

``` r
library(broom) # loads the broom library
tidy(model) # this function constructs a tibble that summarizes the model’s statistical findings
```

    ## # A tibble: 2 × 5
    ##   term        estimate std.error statistic      p.value
    ##   <chr>          <dbl>     <dbl>     <dbl>        <dbl>
    ## 1 (Intercept) -385.      72.1        -5.34 0.0000000997
    ## 2 year_built     0.201    0.0367      5.47 0.0000000484

<!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 4.1 (3 points)

Take a summary table that you made from Task 1, and write it as a csv
file in your `output` folder. Use the `here::here()` function.

- **Robustness criteria**: You should be able to move your Mini Project
  repository / project folder to some other location on your computer,
  or move this very Rmd file to another location within your project
  repository / folder, and your code should still work.
- **Reproducibility criteria**: You should be able to delete the csv
  file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

``` r
# loads relevant library
library(here)
```

    ## here() starts at /Users/cynthiachung/Library/CloudStorage/OneDrive-UBC/STATS 545A/Mini-Data Analysis/mda-cynthiaachung

``` r
# exporting csv
write.csv(accessible, here("output", "apt_buildings_accessible_summ.csv")) # "here" dictates file location
```

<!----------------------------------------------------------------------------->

## 4.2 (3 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

- The same robustness and reproducibility criteria as in 4.1 apply here.

<!-------------------------- Start your work below ---------------------------->

``` r
# exports R binary file
saveRDS(model, here("output", "model.RDS")) # "here" dictates file location
```

``` r
# loading R binary file
readRDS(here("output", "model.RDS"), refhook = NULL) # "here" locates file of interest
```

    ## 
    ## Call:
    ## lm(formula = no_barrier_free_accessible_units ~ year_built, data = apt_buildings_accessible)
    ## 
    ## Coefficients:
    ## (Intercept)   year_built  
    ##    -385.035        0.201

<!----------------------------------------------------------------------------->

# Overall Reproducibility/Cleanliness/Coherence Checklist

Here are the criteria we’re looking for.

## Coherence (0.5 points)

The document should read sensibly from top to bottom, with no major
continuity errors.

The README file should still satisfy the criteria from the last
milestone, i.e. it has been updated to match the changes to the
repository made in this milestone.

## File and folder structure (1 points)

You should have at least three folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

## Output (1 point)

All output is recent and relevant:

- All Rmd files have been `knit`ted to their output md files.
- All knitted md files are viewable without errors on Github. Examples
  of errors: Missing plots, “Sorry about that, but we can’t show files
  that are this big right now” messages, error messages from broken R
  code
- All of these output files are up-to-date – that is, they haven’t
  fallen behind after the source (Rmd) files have been updated.
- There should be no relic output files. For example, if you were
  knitting an Rmd to html, but then changed the output to be only a
  markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

## Tagged release (0.5 point)

You’ve tagged a release for Milestone 2.

### Attribution

Thanks to Victor Yuan for mostly putting this together.
