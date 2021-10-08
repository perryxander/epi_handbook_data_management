Data Management
================

## Load Linelist

``` r
linelist_raw <- import("./data/linelist_raw.xlsx") 
```

    ## New names:
    ## * `` -> ...28

``` r
linelist <- linelist_raw %>%
  janitor::clean_names() %>%
  rename(
    date_infection = infection_date,
    date_hospitalization = hosp_date,
    date_outcome = date_of_outcome
  )

#Review
skimr::skim(linelist_raw)
```

|                                                  |               |
|:-------------------------------------------------|:--------------|
| Name                                             | linelist\_raw |
| Number of rows                                   | 6611          |
| Number of columns                                | 28            |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |               |
| Column type frequency:                           |               |
| character                                        | 17            |
| numeric                                          | 8             |
| POSIXct                                          | 3             |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |               |
| Group variables                                  | None          |

Data summary

**Variable type: character**

| skim\_variable  | n\_missing | complete\_rate | min | max | empty | n\_unique | whitespace |
|:----------------|-----------:|---------------:|----:|----:|------:|----------:|-----------:|
| case\_id        |        137 |           0.98 |   6 |   6 |     0 |      5888 |          0 |
| date onset      |        293 |           0.96 |  10 |  10 |     0 |       580 |          0 |
| outcome         |       1500 |           0.77 |   5 |   7 |     0 |         2 |          0 |
| gender          |        324 |           0.95 |   1 |   1 |     0 |         2 |          0 |
| hospital        |       1512 |           0.77 |   5 |  36 |     0 |        13 |          0 |
| infector        |       2323 |           0.65 |   6 |   6 |     0 |      2697 |          0 |
| source          |       2323 |           0.65 |   5 |   7 |     0 |         2 |          0 |
| age             |        107 |           0.98 |   1 |   2 |     0 |        75 |          0 |
| age\_unit       |          7 |           1.00 |   5 |   6 |     0 |         2 |          0 |
| fever           |        258 |           0.96 |   2 |   3 |     0 |         2 |          0 |
| chills          |        258 |           0.96 |   2 |   3 |     0 |         2 |          0 |
| cough           |        258 |           0.96 |   2 |   3 |     0 |         2 |          0 |
| aches           |        258 |           0.96 |   2 |   3 |     0 |         2 |          0 |
| vomit           |        258 |           0.96 |   2 |   3 |     0 |         2 |          0 |
| time\_admission |        844 |           0.87 |   5 |   5 |     0 |      1091 |          0 |
| merged\_header  |          0 |           1.00 |   1 |   1 |     0 |         1 |          0 |
| …28             |          0 |           1.00 |   1 |   1 |     0 |         1 |          0 |

**Variable type: numeric**

| skim\_variable | n\_missing | complete\_rate |    mean |      sd |     p0 |     p25 |     p50 |     p75 |    p100 | hist  |
|:---------------|-----------:|---------------:|--------:|--------:|-------:|--------:|--------:|--------:|--------:|:------|
| generation     |          7 |           1.00 |   16.60 |    5.71 |   0.00 |   13.00 |   16.00 |   20.00 |   37.00 | ▁▆▇▂▁ |
| lon            |          7 |           1.00 |  -13.23 |    0.02 | -13.27 |  -13.25 |  -13.23 |  -13.22 |  -13.21 | ▅▃▃▅▇ |
| lat            |          7 |           1.00 |    8.47 |    0.01 |   8.45 |    8.46 |    8.47 |    8.48 |    8.49 | ▅▇▇▇▆ |
| row\_num       |          0 |           1.00 | 3240.91 | 1857.83 |   1.00 | 1647.50 | 3241.00 | 4836.50 | 6481.00 | ▇▇▇▇▇ |
| wt\_kg         |          7 |           1.00 |   52.69 |   18.59 | -11.00 |   41.00 |   54.00 |   66.00 |  111.00 | ▁▃▇▅▁ |
| ht\_cm         |          7 |           1.00 |  125.25 |   49.57 |   4.00 |   91.00 |  130.00 |  159.00 |  295.00 | ▂▅▇▂▁ |
| ct\_blood      |          7 |           1.00 |   21.26 |    1.67 |  16.00 |   20.00 |   22.00 |   22.00 |   26.00 | ▁▃▇▃▁ |
| temp           |        158 |           0.98 |   38.60 |    0.95 |  35.20 |   38.30 |   38.80 |   39.20 |   40.80 | ▁▂▂▇▁ |

**Variable type: POSIXct**

| skim\_variable    | n\_missing | complete\_rate | min        | max        | median     | n\_unique |
|:------------------|-----------:|---------------:|:-----------|:-----------|:-----------|----------:|
| infection date    |       2322 |           0.65 | 2012-04-09 | 2015-04-27 | 2014-10-04 |       538 |
| hosp date         |          7 |           1.00 | 2012-04-20 | 2015-04-30 | 2014-10-15 |       570 |
| date\_of\_outcome |       1068 |           0.84 | 2012-05-14 | 2015-06-04 | 2014-10-26 |       575 |

``` r
#Look at column names
names(linelist_raw)
```

    ##  [1] "case_id"         "generation"      "infection date"  "date onset"     
    ##  [5] "hosp date"       "date_of_outcome" "outcome"         "gender"         
    ##  [9] "hospital"        "lon"             "lat"             "infector"       
    ## [13] "source"          "age"             "age_unit"        "row_num"        
    ## [17] "wt_kg"           "ht_cm"           "ct_blood"        "fever"          
    ## [21] "chills"          "cough"           "aches"           "vomit"          
    ## [25] "temp"            "time_admission"  "merged_header"   "...28"

``` r
#Can reference column name with spaces using back-ticks ex: linelist$`x60\infection date\x60`
```

Manipulate Columns

``` r
# select
linelist %>%
  select(case_id, date_onset, date_hospitalization, fever) %>%
  names()
```

    ## [1] "case_id"              "date_onset"           "date_hospitalization"
    ## [4] "fever"

``` r
# everything- move onset and hospitaliation to front
linelist %>%
  select(date_onset, date_hospitalization, everything()) %>%
  names()
```

    ##  [1] "date_onset"           "date_hospitalization" "case_id"             
    ##  [4] "generation"           "date_infection"       "date_outcome"        
    ##  [7] "outcome"              "gender"               "hospital"            
    ## [10] "lon"                  "lat"                  "infector"            
    ## [13] "source"               "age"                  "age_unit"            
    ## [16] "row_num"              "wt_kg"                "ht_cm"               
    ## [19] "ct_blood"             "fever"                "chills"              
    ## [22] "cough"                "aches"                "vomit"               
    ## [25] "temp"                 "time_admission"       "merged_header"       
    ## [28] "x28"

``` r
# select columns that are class numeric
linelist %>%
  select(where(is.numeric)) %>%
  names()
```

    ## [1] "generation" "lon"        "lat"        "row_num"    "wt_kg"     
    ## [6] "ht_cm"      "ct_blood"   "temp"

``` r
# select columns containing certain characters
linelist %>%
  select(contains("date")) %>%
  names()
```

    ## [1] "date_infection"       "date_onset"           "date_hospitalization"
    ## [4] "date_outcome"

``` r
# searched for multiple character matches- needs to be exact or will generate an error
linelist %>%
  select(matches("onset|hosp|fev")) %>%
  names()
```

    ## [1] "date_onset"           "date_hospitalization" "hospital"            
    ## [4] "fever"

``` r
# consider using any_of to search for columns that may or may not exist
linelist %>%
  select(any_of(c("date_onset","village_origin","village_detection","village_residence","village_travel"))) %>%
  names()
```

    ## [1] "date_onset"

``` r
# remove columns
linelist %>%
  select(-c(date_onset, fever:vomit)) %>%
  names()
```

    ##  [1] "case_id"              "generation"           "date_infection"      
    ##  [4] "date_hospitalization" "date_outcome"         "outcome"             
    ##  [7] "gender"               "hospital"             "lon"                 
    ## [10] "lat"                  "infector"             "source"              
    ## [13] "age"                  "age_unit"             "row_num"             
    ## [16] "wt_kg"                "ht_cm"                "ct_blood"            
    ## [19] "temp"                 "time_admission"       "merged_header"       
    ## [22] "x28"

``` r
# create new linelist with id and age-realted columns
linelist_age <- select(linelist, case_id, contains("age"))

names(linelist_age)
```

    ## [1] "case_id"  "age"      "age_unit"

Addition to pipe chain

``` r
linelist <- linelist_raw %>%
  janitor::clean_names() %>%
  rename(date_infection       = infection_date,
           date_hospitalisation = hosp_date,
           date_outcome         = date_of_outcome) %>%
  select(-c(row_num, merged_header, x28)) %>%
  distinct()
```

New Columns

``` r
new_col_demo <- linelist %>%                       
  mutate(
    new_var_dup    = case_id,             # new column = duplicate/copy another existing column
    new_var_static = 7,                   # new column = all values the same
    new_var_static = new_var_static + 5,  # you can overwrite a column, and it can be a calculation using other variables
    new_var_paste  = stringr::str_glue("{hospital} on ({date_hospitalisation})") # new column = pasting together values from other columns
    ) %>% 
  select(case_id, hospital, date_hospitalisation, contains("new"))        # show only new columns, for demonstration purposes

linelist <- linelist %>%
  select(-contains("new_var"))
```
