
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#3 - instructions

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.

# Lab 3: Avenger’s Peril

## As a team

Extract from the data below two data sets in long form `deaths` and
`returns`

``` r
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(tidyr)
library(readr)

# Select only relevant columns
deaths <- av %>%
  select(URL, starts_with("Death")) %>%
  pivot_longer(
    cols = starts_with("Death"),
    names_to = "Time",
    values_to = "Death"
  ) %>%
  mutate(
    Time = parse_number(Time),   
    Death = tolower(Death)       
  )

head(deaths)
```

    ## # A tibble: 6 × 3
    ##   URL                                                 Time Death
    ##   <chr>                                              <dbl> <chr>
    ## 1 http://marvel.wikia.com/Henry_Pym_(Earth-616)          1 "yes"
    ## 2 http://marvel.wikia.com/Henry_Pym_(Earth-616)          2 ""   
    ## 3 http://marvel.wikia.com/Henry_Pym_(Earth-616)          3 ""   
    ## 4 http://marvel.wikia.com/Henry_Pym_(Earth-616)          4 ""   
    ## 5 http://marvel.wikia.com/Henry_Pym_(Earth-616)          5 ""   
    ## 6 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)     1 "yes"

``` r
returns <- av %>%
  select(URL, starts_with("Return")) %>%
  pivot_longer(
    cols = starts_with("Return"),
    names_to = "Time",
    values_to = "Return"
  ) %>%
  mutate(
    Time = parse_number(Time),
    Return = tolower(Return)
  )

head(returns)
```

    ## # A tibble: 6 × 3
    ##   URL                                                 Time Return
    ##   <chr>                                              <dbl> <chr> 
    ## 1 http://marvel.wikia.com/Henry_Pym_(Earth-616)          1 "no"  
    ## 2 http://marvel.wikia.com/Henry_Pym_(Earth-616)          2 ""    
    ## 3 http://marvel.wikia.com/Henry_Pym_(Earth-616)          3 ""    
    ## 4 http://marvel.wikia.com/Henry_Pym_(Earth-616)          4 ""    
    ## 5 http://marvel.wikia.com/Henry_Pym_(Earth-616)          5 ""    
    ## 6 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)     1 "yes"

``` r
avg_deaths <- deaths %>%
  group_by(URL) %>%
  summarize(num_deaths = sum(Death == "yes", na.rm = TRUE)) %>%
  summarize(avg_deaths = mean(num_deaths))

avg_deaths
```

    ## # A tibble: 1 × 1
    ##   avg_deaths
    ##        <dbl>
    ## 1      0.514

Similarly, deal with the returns of characters.

Based on these datasets calculate the average number of deaths an
Avenger suffers.

## Individually

## Kenzie’s Work

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check. “If all the
> Avengers are sent to a pocket dimension after seemingly being killed
> by Onslaught but we then we follow their adventures on Counter-Earth,
> they’re not dead.” \### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

``` r
# Load required packages
library(dplyr)
library(tidyr)
library(readr)

# Load the Avengers dataset
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
# Create deaths dataset in long format
deaths <- av %>%
  select(URL, Name.Alias, starts_with("Death")) %>%
  pivot_longer(
    cols = starts_with("Death"),
    names_to = "Time",
    values_to = "Death"
  ) %>%
  mutate(
    Time = parse_number(Time),
    Death = tolower(Death)
  )

# Create returns dataset in long format
returns <- av %>%
  select(URL, Name.Alias, starts_with("Return")) %>%
  pivot_longer(
    cols = starts_with("Return"),
    names_to = "Time",
    values_to = "Return"
  ) %>%
  mutate(
    Time = parse_number(Time),
    Return = tolower(Return)
  )

# Merge deaths and returns
death_return <- left_join(deaths, returns, by = c("URL", "Name.Alias", "Time"))

# Identify Avengers who died and then returned
revived <- death_return %>%
  filter(Death == "yes", Return == "yes") %>%
  distinct(Name.Alias)

# Onslaught-era Avengers involved in the pocket dimension storyline
onslaught_avengers <- c("Captain America", "Iron Man", "Thor", "Scarlet Witch")

# Filter to see which of them both died and returned
revived_onslaught <- revived %>%
  filter(Name.Alias %in% onslaught_avengers)

# Derive a numeric fact: How many Onslaught-era Avengers returned?
num_revived <- nrow(revived_onslaught)
num_revived
```

    ## [1] 0

### Include your answer

## Dan’s Work

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check. “I counted 89
> total deaths — some unlucky Avengers7 are basically Meat Loaf with an
> E-ZPass — and on 57 occasions the individual made a comeback.” \###
> Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

``` r
library(dplyr)
library(tidyr)
library(readr)

# Load data
av <- read.csv(
  "https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv",
  stringsAsFactors = FALSE
)

# Long tables for deaths and returns
deaths <- av %>%
  select(URL, starts_with("Death")) %>%
  pivot_longer(
    cols = starts_with("Death"),
    names_to = "Time",
    values_to = "Death"
  ) %>%
  mutate(Death = tolower(Death))

returns <- av %>%
  select(URL, starts_with("Return")) %>%
  pivot_longer(
    cols = starts_with("Return"),
    names_to = "Time",
    values_to = "Return"
  ) %>%
  mutate(Return = tolower(Return))

# Totals across all events
total_deaths  <- sum(deaths$Death  == "yes", na.rm = TRUE)
total_returns <- sum(returns$Return == "yes", na.rm = TRUE)

list(
  total_deaths  = total_deaths,
  total_returns = total_returns
)
```

    ## $total_deaths
    ## [1] 89
    ## 
    ## $total_returns
    ## [1] 57

Include at least one sentence discussing the result of your
fact-checking endeavor. “Using the dataset, I count total_deaths total
death events and total_returns total return events; this supports
FiveThirtyEight’s claim of about 89 deaths and 57 returns.”
