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

\##`filter`

\##`mutate`
