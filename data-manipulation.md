Data Manipulation
================

This document will show how to *manipulate* data

Import the two datasets that we’re going to manipulation

``` r
litters_df = 
  read_csv("data_import_examples/FAS_litters.csv", na = c("NA", ",", "."))
```

    ## Warning: One or more parsing issues, call `problems()` on your data frame for details,
    ## e.g.:
    ##   dat <- vroom(...)
    ##   problems(dat)

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_df = janitor::clean_names(litters_df)

pups_df = 
  read_csv("data_import_examples/FAS_pups.csv", na = c("NA", ",", "."))
```

    ## Rows: 313 Columns: 6
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Litter Number
    ## dbl (5): Sex, PD ears, PD eyes, PD pivot, PD walk
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
pups_df = 
    janitor::clean_names(pups_df)
```

## `Select`

Use `select()` to select variables

``` r
select(litters_df, group, litter_number, gd0_weight)
```

    ## # A tibble: 49 × 3
    ##    group litter_number   gd0_weight
    ##    <chr> <chr>                <dbl>
    ##  1 Con7  #85                   19.7
    ##  2 Con7  #1/2/95/2             27  
    ##  3 Con7  #5/5/3/83/3-3         26  
    ##  4 Con7  #5/4/2/95/2           28.5
    ##  5 Con7  #4/2/95/3-3           NA  
    ##  6 Con7  #2/2/95/3-2           NA  
    ##  7 Con7  #1/5/3/83/3-3/2       NA  
    ##  8 Con8  #3/83/3-3             NA  
    ##  9 Con8  #2/95/3               NA  
    ## 10 Con8  #3/5/2/2/95           28.5
    ## # ℹ 39 more rows

selecting everything from group to gd18_weight

``` r
select(litters_df, group:gd18_weight)
```

    ## # A tibble: 49 × 4
    ##    group litter_number   gd0_weight gd18_weight
    ##    <chr> <chr>                <dbl>       <dbl>
    ##  1 Con7  #85                   19.7        34.7
    ##  2 Con7  #1/2/95/2             27          42  
    ##  3 Con7  #5/5/3/83/3-3         26          41.4
    ##  4 Con7  #5/4/2/95/2           28.5        44.1
    ##  5 Con7  #4/2/95/3-3           NA          NA  
    ##  6 Con7  #2/2/95/3-2           NA          NA  
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA  
    ##  8 Con8  #3/83/3-3             NA          NA  
    ##  9 Con8  #2/95/3               NA          NA  
    ## 10 Con8  #3/5/2/2/95           28.5        NA  
    ## # ℹ 39 more rows

can select to remove things that you don’t want

``` r
select(litters_df, -pups_survive)
```

    ## # A tibble: 49 × 7
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2             27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 Con8  #3/83/3-3             NA          NA            20               9
    ##  9 Con8  #2/95/3               NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 1 more variable: pups_dead_birth <dbl>

can combine

``` r
select(litters_df, -(group:gd18_weight))
```

    ## # A tibble: 49 × 4
    ##    gd_of_birth pups_born_alive pups_dead_birth pups_survive
    ##          <dbl>           <dbl>           <dbl>        <dbl>
    ##  1          20               3               4            3
    ##  2          19               8               0            7
    ##  3          19               6               0            5
    ##  4          19               5               1            4
    ##  5          20               6               0            6
    ##  6          20               6               0            4
    ##  7          20               9               0            9
    ##  8          20               9               1            8
    ##  9          20               8               0            8
    ## 10          20               8               0            8
    ## # ℹ 39 more rows

can select for suffixes

``` r
select(litters_df, starts_with("gd"))
```

    ## # A tibble: 49 × 3
    ##    gd0_weight gd18_weight gd_of_birth
    ##         <dbl>       <dbl>       <dbl>
    ##  1       19.7        34.7          20
    ##  2       27          42            19
    ##  3       26          41.4          19
    ##  4       28.5        44.1          19
    ##  5       NA          NA            20
    ##  6       NA          NA            20
    ##  7       NA          NA            20
    ##  8       NA          NA            20
    ##  9       NA          NA            20
    ## 10       28.5        NA            20
    ## # ℹ 39 more rows

``` r
select(litters_df, contains ("pups"))
```

    ## # A tibble: 49 × 3
    ##    pups_born_alive pups_dead_birth pups_survive
    ##              <dbl>           <dbl>        <dbl>
    ##  1               3               4            3
    ##  2               8               0            7
    ##  3               6               0            5
    ##  4               5               1            4
    ##  5               6               0            6
    ##  6               6               0            4
    ##  7               9               0            9
    ##  8               9               1            8
    ##  9               8               0            8
    ## 10               8               0            8
    ## # ℹ 39 more rows

can rename as I go (select() selects out a group and renames it)
rename() keeps the entire dataframe but renames the column

``` r
select(litters_df, GROUP = group)
```

    ## # A tibble: 49 × 1
    ##    GROUP
    ##    <chr>
    ##  1 Con7 
    ##  2 Con7 
    ##  3 Con7 
    ##  4 Con7 
    ##  5 Con7 
    ##  6 Con7 
    ##  7 Con7 
    ##  8 Con8 
    ##  9 Con8 
    ## 10 Con8 
    ## # ℹ 39 more rows

``` r
rename(litters_df, GROUP = group)
```

    ## # A tibble: 49 × 8
    ##    GROUP litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2             27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 Con8  #3/83/3-3             NA          NA            20               9
    ##  9 Con8  #2/95/3               NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

these two do the same thing to relocate a column, just need to indicate
that keeping everything in select

``` r
select(litters_df, litter_number, gd0_weight, everything())
```

    ## # A tibble: 49 × 8
    ##    litter_number   gd0_weight group gd18_weight gd_of_birth pups_born_alive
    ##    <chr>                <dbl> <chr>       <dbl>       <dbl>           <dbl>
    ##  1 #85                   19.7 Con7         34.7          20               3
    ##  2 #1/2/95/2             27   Con7         42            19               8
    ##  3 #5/5/3/83/3-3         26   Con7         41.4          19               6
    ##  4 #5/4/2/95/2           28.5 Con7         44.1          19               5
    ##  5 #4/2/95/3-3           NA   Con7         NA            20               6
    ##  6 #2/2/95/3-2           NA   Con7         NA            20               6
    ##  7 #1/5/3/83/3-3/2       NA   Con7         NA            20               9
    ##  8 #3/83/3-3             NA   Con8         NA            20               9
    ##  9 #2/95/3               NA   Con8         NA            20               8
    ## 10 #3/5/2/2/95           28.5 Con8         NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
relocate(litters_df, litter_number, gd0_weight)
```

    ## # A tibble: 49 × 8
    ##    litter_number   gd0_weight group gd18_weight gd_of_birth pups_born_alive
    ##    <chr>                <dbl> <chr>       <dbl>       <dbl>           <dbl>
    ##  1 #85                   19.7 Con7         34.7          20               3
    ##  2 #1/2/95/2             27   Con7         42            19               8
    ##  3 #5/5/3/83/3-3         26   Con7         41.4          19               6
    ##  4 #5/4/2/95/2           28.5 Con7         44.1          19               5
    ##  5 #4/2/95/3-3           NA   Con7         NA            20               6
    ##  6 #2/2/95/3-2           NA   Con7         NA            20               6
    ##  7 #1/5/3/83/3-3/2       NA   Con7         NA            20               9
    ##  8 #3/83/3-3             NA   Con8         NA            20               9
    ##  9 #2/95/3               NA   Con8         NA            20               8
    ## 10 #3/5/2/2/95           28.5 Con8         NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

Learning Assessment

``` r
select(pups_df, litter_number, sex, pd_ears)
```

    ## # A tibble: 313 × 3
    ##    litter_number   sex pd_ears
    ##    <chr>         <dbl>   <dbl>
    ##  1 #85               1       4
    ##  2 #85               1       4
    ##  3 #1/2/95/2         1       5
    ##  4 #1/2/95/2         1       5
    ##  5 #5/5/3/83/3-3     1       5
    ##  6 #5/5/3/83/3-3     1       5
    ##  7 #5/4/2/95/2       1      NA
    ##  8 #4/2/95/3-3       1       4
    ##  9 #4/2/95/3-3       1       4
    ## 10 #2/2/95/3-2       1       4
    ## # ℹ 303 more rows

\##`filter`

Select works on columns, filter() filters out rows Taking a variable
inside the dataset, asking if it is true or false, if it is true then
keep if false then do not

``` r
filter(litters_df, gd_of_birth == 20)
```

    ## # A tibble: 32 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  3 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  4 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  5 Con8  #3/83/3-3             NA          NA            20               9
    ##  6 Con8  #2/95/3               NA          NA            20               8
    ##  7 Con8  #3/5/2/2/95           28.5        NA            20               8
    ##  8 Con8  #1/6/2/2/95-2         NA          NA            20               7
    ##  9 Con8  #3/5/3/83/3-3-2       NA          NA            20               8
    ## 10 Con8  #3/6/2/2/95-3         NA          NA            20               7
    ## # ℹ 22 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

double equal == ask whether this equal to that, asks a question (ask
whether something is true about a variable)

greater than, greater than equal to, not equal to (numeric variables)

``` r
filter(litters_df, pups_born_alive > 8)
```

    ## # A tibble: 12 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  2 Con8  #3/83/3-3             NA          NA            20               9
    ##  3 Con8  #5/4/3/83/3           28          NA            19               9
    ##  4 Mod7  #103                  21.4        42.1          19               9
    ##  5 Mod7  #4/2/95/2             23.5        NA            19               9
    ##  6 Mod7  #8/110/3-2            NA          NA            20               9
    ##  7 Low7  #107                  22.6        42.4          20               9
    ##  8 Low7  #98                   23.8        43.8          20               9
    ##  9 Low7  #102                  22.6        43.3          20              11
    ## 10 Low7  #101                  23.8        42.7          20               9
    ## 11 Mod8  #5/93                 NA          41.1          20              11
    ## 12 Mod8  #2/95/2               28.5        44.5          20               9
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, pups_born_alive >= 8)
```

    ## # A tibble: 28 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #1/2/95/2             27          42            19               8
    ##  2 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  3 Con8  #3/83/3-3             NA          NA            20               9
    ##  4 Con8  #2/95/3               NA          NA            20               8
    ##  5 Con8  #3/5/2/2/95           28.5        NA            20               8
    ##  6 Con8  #5/4/3/83/3           28          NA            19               9
    ##  7 Con8  #3/5/3/83/3-3-2       NA          NA            20               8
    ##  8 Mod7  #59                   17          33.4          19               8
    ##  9 Mod7  #103                  21.4        42.1          19               9
    ## 10 Mod7  #3/83/3-2             NA          NA            19               8
    ## # ℹ 18 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, pups_born_alive != 9)
```

    ## # A tibble: 39 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2             27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 Con8  #2/95/3               NA          NA            20               8
    ##  8 Con8  #3/5/2/2/95           28.5        NA            20               8
    ##  9 Con8  #1/6/2/2/95-2         NA          NA            20               7
    ## 10 Con8  #3/5/3/83/3-3-2       NA          NA            20               8
    ## # ℹ 29 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

looking at character variables

``` r
filter(litters_df, group == "Low8")
```

    ## # A tibble: 7 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Low8  #53                 21.8        37.2          20               8
    ## 2 Low8  #79                 25.4        43.8          19               8
    ## 3 Low8  #100                20          39.2          20               8
    ## 4 Low8  #4/84               21.8        35.2          20               4
    ## 5 Low8  #108                25.6        47.5          20               8
    ## 6 Low8  #99                 23.5        39            20               6
    ## 7 Low8  #110                25.5        42.7          20               7
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, group %in% c("Low7", "Low8"))
```

    ## # A tibble: 15 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Low7  #84/2               24.3        40.8          20               8
    ##  2 Low7  #107                22.6        42.4          20               9
    ##  3 Low7  #85/2               22.2        38.5          20               8
    ##  4 Low7  #98                 23.8        43.8          20               9
    ##  5 Low7  #102                22.6        43.3          20              11
    ##  6 Low7  #101                23.8        42.7          20               9
    ##  7 Low7  #111                25.5        44.6          20               3
    ##  8 Low7  #112                23.9        40.5          19               6
    ##  9 Low8  #53                 21.8        37.2          20               8
    ## 10 Low8  #79                 25.4        43.8          19               8
    ## 11 Low8  #100                20          39.2          20               8
    ## 12 Low8  #4/84               21.8        35.2          20               4
    ## 13 Low8  #108                25.6        47.5          20               8
    ## 14 Low8  #99                 23.5        39            20               6
    ## 15 Low8  #110                25.5        42.7          20               7
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

not using the == since asking whether it is in Low7 or Low8 (question is
or)

``` r
filter(litters_df, group %in% c("Low7", "Low8"), pups_born_alive == 8)
```

    ## # A tibble: 6 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Low7  #84/2               24.3        40.8          20               8
    ## 2 Low7  #85/2               22.2        38.5          20               8
    ## 3 Low8  #53                 21.8        37.2          20               8
    ## 4 Low8  #79                 25.4        43.8          19               8
    ## 5 Low8  #100                20          39.2          20               8
    ## 6 Low8  #108                25.6        47.5          20               8
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

here, two things true at the same time (either Low7 or Low8) AND pups
born alive equals to 8

This is case sensitive, need to be specific about lower and upper case

to get rid of rows with missing data doing it on everything will drop
all rows with missing variables

``` r
drop_na(litters_df)
```

    ## # A tibble: 31 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2           27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 Mod7  #59                 17          33.4          19               8
    ##  6 Mod7  #103                21.4        42.1          19               9
    ##  7 Mod7  #3/82/3-2           28          45.9          20               5
    ##  8 Mod7  #5/3/83/5-2         22.6        37            19               5
    ##  9 Mod7  #106                21.7        37.8          20               5
    ## 10 Mod7  #94/2               24.4        42.9          19               7
    ## # ℹ 21 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

or could specify

``` r
drop_na(litters_df, gd0_weight)
```

    ## # A tibble: 34 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2           27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 Con8  #3/5/2/2/95         28.5        NA            20               8
    ##  6 Con8  #5/4/3/83/3         28          NA            19               9
    ##  7 Mod7  #59                 17          33.4          19               8
    ##  8 Mod7  #103                21.4        42.1          19               9
    ##  9 Mod7  #3/82/3-2           28          45.9          20               5
    ## 10 Mod7  #4/2/95/2           23.5        NA            19               9
    ## # ℹ 24 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

Notice that we are not saving any of the new data frame (we are always
going back to the original litters_df) normally could do new_litters_df
= drop_na(litters_df, gd0_weight) to create a new data frame

Learning assessment

``` r
filter(pups_df, sex == 1)
```

    ## # A tibble: 155 × 6
    ##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
    ##    <chr>         <dbl>   <dbl>   <dbl>    <dbl>   <dbl>
    ##  1 #85               1       4      13        7      11
    ##  2 #85               1       4      13        7      12
    ##  3 #1/2/95/2         1       5      13        7       9
    ##  4 #1/2/95/2         1       5      13        8      10
    ##  5 #5/5/3/83/3-3     1       5      13        8      10
    ##  6 #5/5/3/83/3-3     1       5      14        6       9
    ##  7 #5/4/2/95/2       1      NA      14        5       9
    ##  8 #4/2/95/3-3       1       4      13        6       8
    ##  9 #4/2/95/3-3       1       4      13        7       9
    ## 10 #2/2/95/3-2       1       4      NA        8      10
    ## # ℹ 145 more rows

``` r
filter(pups_df, pd_walk < 11, sex == 2)
```

    ## # A tibble: 127 × 6
    ##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
    ##    <chr>         <dbl>   <dbl>   <dbl>    <dbl>   <dbl>
    ##  1 #1/2/95/2         2       4      13        7       9
    ##  2 #1/2/95/2         2       4      13        7      10
    ##  3 #1/2/95/2         2       5      13        8      10
    ##  4 #1/2/95/2         2       5      13        8      10
    ##  5 #1/2/95/2         2       5      13        6      10
    ##  6 #5/5/3/83/3-3     2       5      13        8      10
    ##  7 #5/5/3/83/3-3     2       5      14        7      10
    ##  8 #5/5/3/83/3-3     2       5      14        8      10
    ##  9 #5/4/2/95/2       2      NA      14        7      10
    ## 10 #5/4/2/95/2       2      NA      14        7      10
    ## # ℹ 117 more rows

\##`mutate`

``` r
mutate(litters_df, wt_gain = gd18_weight - gd0_weight)
```

    ## # A tibble: 49 × 9
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2             27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 Con8  #3/83/3-3             NA          NA            20               9
    ##  9 Con8  #2/95/3               NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 3 more variables: pups_dead_birth <dbl>, pups_survive <dbl>, wt_gain <dbl>

defined a new weight gain variable (will go to the end of the data
frame)

``` r
mutate(litters_df, sq_pups = pups_born_alive^2)
```

    ## # A tibble: 49 × 9
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2             27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 Con8  #3/83/3-3             NA          NA            20               9
    ##  9 Con8  #2/95/3               NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 3 more variables: pups_dead_birth <dbl>, pups_survive <dbl>, sq_pups <dbl>

``` r
mutate(litters_df, group = str_to_lower(group))
```

    ## # A tibble: 49 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 con7  #85                   19.7        34.7          20               3
    ##  2 con7  #1/2/95/2             27          42            19               8
    ##  3 con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 con8  #3/83/3-3             NA          NA            20               9
    ##  9 con8  #2/95/3               NA          NA            20               8
    ## 10 con8  #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

can overwrite variables in the data frame (from upper case group Con7 to
con7)

can manipulate multiple things

``` r
mutate(
  litters_df,
  wt_gain = gd18_weight - gd0_weight,
  group = str_to_lower(group)
)
```

    ## # A tibble: 49 × 9
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 con7  #85                   19.7        34.7          20               3
    ##  2 con7  #1/2/95/2             27          42            19               8
    ##  3 con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 con8  #3/83/3-3             NA          NA            20               9
    ##  9 con8  #2/95/3               NA          NA            20               8
    ## 10 con8  #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 3 more variables: pups_dead_birth <dbl>, pups_survive <dbl>, wt_gain <dbl>

\##`arrange`

goes from lightest gd0_weight to heaviest

``` r
arrange(litters_df, gd0_weight)
```

    ## # A tibble: 49 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Mod7  #59                 17          33.4          19               8
    ##  2 Mod7  #62                 19.5        35.9          19               7
    ##  3 Con7  #85                 19.7        34.7          20               3
    ##  4 Low8  #100                20          39.2          20               8
    ##  5 Mod7  #103                21.4        42.1          19               9
    ##  6 Mod7  #106                21.7        37.8          20               5
    ##  7 Low8  #53                 21.8        37.2          20               8
    ##  8 Low8  #4/84               21.8        35.2          20               4
    ##  9 Low7  #85/2               22.2        38.5          20               8
    ## 10 Mod7  #5/3/83/5-2         22.6        37            19               5
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

or go from heaviest to lightest

``` r
arrange(litters_df, desc(gd0_weight))
```

    ## # A tibble: 49 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Mod8  #82/4               33.4        52.7          20               8
    ##  2 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  3 Con8  #3/5/2/2/95         28.5        NA            20               8
    ##  4 Mod8  #2/95/2             28.5        44.5          20               9
    ##  5 Con8  #5/4/3/83/3         28          NA            19               9
    ##  6 Mod7  #3/82/3-2           28          45.9          20               5
    ##  7 Mod8  #7/110/3-2          27.5        46            19               8
    ##  8 Con7  #1/2/95/2           27          42            19               8
    ##  9 Mod8  #7/82-3-2           26.9        43.2          20               7
    ## 10 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

arrange by multiple things

``` r
arrange(litters_df, pups_born_alive, gd0_weight)
```

    ## # A tibble: 49 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Low7  #111                25.5        44.6          20               3
    ##  3 Low8  #4/84               21.8        35.2          20               4
    ##  4 Mod7  #106                21.7        37.8          20               5
    ##  5 Mod7  #5/3/83/5-2         22.6        37            19               5
    ##  6 Mod7  #3/82/3-2           28          45.9          20               5
    ##  7 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  8 Con8  #2/2/95/2           NA          NA            19               5
    ##  9 Low8  #99                 23.5        39            20               6
    ## 10 Low7  #112                23.9        40.5          19               6
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

handy to have if need to arrange participant numbers etc.

\##`PIPING`

If i know that when taking a dataframe I want to do multiple things to
it

``` r
litters_df = 
  read_csv("data_import_examples/FAS_litters.csv", na = c("NA", ".", "")) %>%   janitor::clean_names() %>% 
  select(-pups_born_alive) %>% 
  filter(group == "Con7") %>% 
  mutate(
    wt_gain = 
      gd18_weight - gd0_weight,
    group = str_to_lower(group)
  )
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_df = 
  read_csv("data_import_examples/FAS_litters.csv", na = c("NA", ".", "")) %>%   janitor::clean_names() %>% 
  mutate(wt_gain = gd18_weight - gd0_weight) %>% 
  lm(wt_gain ~ pups_born_alive, data =.)
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

The reason why this part is not running is being this pipe property %\>%
is not exactly synonymous to the other one that he was using in class
Answer: instead of using data = \_ use data = .

\#Data Export

``` r
litters_df = 
  read_csv("data_import_examples/FAS_litters.csv", na = c("NA", ".", "")) %>%   janitor::clean_names() %>% 
  select(-pups_born_alive) %>% 
  filter(group == "Con7") %>% 
  mutate(
    wt_gain = 
      gd18_weight - gd0_weight,
    group = str_to_lower(group)
  )
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
write_csv(litters_df, "data_import_examples/cleaned_fas_litters.csv")
```
