Data Manipulation
================
ruby
2025-09-18

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.2     ✔ tibble    3.3.0
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.1.0     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

let’s import the datasets.

``` r
litters_df = 
  read_csv("data/FAS_litters.csv", na = c("NA", ".", ""))
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
  janitor::clean_names(litters_df)

pups_df =
    read.csv("data/FAS_pups.csv", skip = 3, na = c("NA", ".", ""))

pups_df = 
  janitor::clean_names(pups_df)
```

## `select`

`select` selects variables from my dataframe

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

select a range

``` r
select(litters_df, group:gd_of_birth)
```

    ## # A tibble: 49 × 5
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>
    ##  1 Con7  #85                   19.7        34.7          20
    ##  2 Con7  #1/2/95/2             27          42            19
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19
    ##  5 Con7  #4/2/95/3-3           NA          NA            20
    ##  6 Con7  #2/2/95/3-2           NA          NA            20
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20
    ##  8 Con8  #3/83/3-3             NA          NA            20
    ##  9 Con8  #2/95/3               NA          NA            20
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20
    ## # ℹ 39 more rows

select by removal

``` r
select(litters_df, -group)
```

    ## # A tibble: 49 × 7
    ##    litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 #85                   19.7        34.7          20               3
    ##  2 #1/2/95/2             27          42            19               8
    ##  3 #5/5/3/83/3-3         26          41.4          19               6
    ##  4 #5/4/2/95/2           28.5        44.1          19               5
    ##  5 #4/2/95/3-3           NA          NA            20               6
    ##  6 #2/2/95/3-2           NA          NA            20               6
    ##  7 #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 #3/83/3-3             NA          NA            20               9
    ##  9 #2/95/3               NA          NA            20               8
    ## 10 #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
select(litters_df, -(group:gd_of_birth))
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

select by prefix

``` r
select(litters_df, group, starts_with("gd"))
```

    ## # A tibble: 49 × 4
    ##    group gd0_weight gd18_weight gd_of_birth
    ##    <chr>      <dbl>       <dbl>       <dbl>
    ##  1 Con7        19.7        34.7          20
    ##  2 Con7        27          42            19
    ##  3 Con7        26          41.4          19
    ##  4 Con7        28.5        44.1          19
    ##  5 Con7        NA          NA            20
    ##  6 Con7        NA          NA            20
    ##  7 Con7        NA          NA            20
    ##  8 Con8        NA          NA            20
    ##  9 Con8        NA          NA            20
    ## 10 Con8        28.5        NA            20
    ## # ℹ 39 more rows

rearrange by selecting -\> however you write it is how select will
present it to you

``` r
select(litters_df, starts_with("gd"), group)
```

    ## # A tibble: 49 × 4
    ##    gd0_weight gd18_weight gd_of_birth group
    ##         <dbl>       <dbl>       <dbl> <chr>
    ##  1       19.7        34.7          20 Con7 
    ##  2       27          42            19 Con7 
    ##  3       26          41.4          19 Con7 
    ##  4       28.5        44.1          19 Con7 
    ##  5       NA          NA            20 Con7 
    ##  6       NA          NA            20 Con7 
    ##  7       NA          NA            20 Con7 
    ##  8       NA          NA            20 Con8 
    ##  9       NA          NA            20 Con8 
    ## 10       28.5        NA            20 Con8 
    ## # ℹ 39 more rows

``` r
select(litters_df, litter_number, everything())
```

    ## # A tibble: 49 × 8
    ##    litter_number   group gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr>           <chr>      <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 #85             Con7        19.7        34.7          20               3
    ##  2 #1/2/95/2       Con7        27          42            19               8
    ##  3 #5/5/3/83/3-3   Con7        26          41.4          19               6
    ##  4 #5/4/2/95/2     Con7        28.5        44.1          19               5
    ##  5 #4/2/95/3-3     Con7        NA          NA            20               6
    ##  6 #2/2/95/3-2     Con7        NA          NA            20               6
    ##  7 #1/5/3/83/3-3/2 Con7        NA          NA            20               9
    ##  8 #3/83/3-3       Con8        NA          NA            20               9
    ##  9 #2/95/3         Con8        NA          NA            20               8
    ## 10 #3/5/2/2/95     Con8        28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
relocate(litters_df, litter_number)
```

    ## # A tibble: 49 × 8
    ##    litter_number   group gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr>           <chr>      <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 #85             Con7        19.7        34.7          20               3
    ##  2 #1/2/95/2       Con7        27          42            19               8
    ##  3 #5/5/3/83/3-3   Con7        26          41.4          19               6
    ##  4 #5/4/2/95/2     Con7        28.5        44.1          19               5
    ##  5 #4/2/95/3-3     Con7        NA          NA            20               6
    ##  6 #2/2/95/3-2     Con7        NA          NA            20               6
    ##  7 #1/5/3/83/3-3/2 Con7        NA          NA            20               9
    ##  8 #3/83/3-3       Con8        NA          NA            20               9
    ##  9 #2/95/3         Con8        NA          NA            20               8
    ## 10 #3/5/2/2/95     Con8        28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

relocate does the same thing as the select

rename by selecting

``` r
select(litters_df, GROUP = group, everything())
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

what if i only select on variable select gives you a dataframe, pull
gives you a vector

``` r
select(litters_df, group)
```

    ## # A tibble: 49 × 1
    ##    group
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
pull(litters_df,group)
```

    ##  [1] "Con7" "Con7" "Con7" "Con7" "Con7" "Con7" "Con7" "Con8" "Con8" "Con8"
    ## [11] "Con8" "Con8" "Con8" "Con8" "Con8" "Mod7" "Mod7" "Mod7" "Mod7" "Mod7"
    ## [21] "Mod7" "Mod7" "Mod7" "Mod7" "Mod7" "Mod7" "Mod7" "Low7" "Low7" "Low7"
    ## [31] "Low7" "Low7" "Low7" "Low7" "Low7" "Mod8" "Mod8" "Mod8" "Mod8" "Mod8"
    ## [41] "Mod8" "Mod8" "Low8" "Low8" "Low8" "Low8" "Low8" "Low8" "Low8"

learning assessment: in the pups dataset, select the columns containing
litter number, sex, and pd ears

``` r
select(pups_df, litter_number, sex, pd_ears)
```

    ##       litter_number sex pd_ears
    ## 1               #85   1       4
    ## 2               #85   1       4
    ## 3         #1/2/95/2   1       5
    ## 4         #1/2/95/2   1       5
    ## 5     #5/5/3/83/3-3   1       5
    ## 6     #5/5/3/83/3-3   1       5
    ## 7       #5/4/2/95/2   1      NA
    ## 8       #4/2/95/3-3   1       4
    ## 9       #4/2/95/3-3   1       4
    ## 10      #2/2/95/3-2   1       4
    ## 11  #1/5/3/83/3-3/2   1       4
    ## 12  #1/5/3/83/3-3/2   1       4
    ## 13  #1/5/3/83/3-3/2   1       4
    ## 14  #1/5/3/83/3-3/2   1       4
    ## 15  #1/5/3/83/3-3/2   1       4
    ## 16              #85   2       4
    ## 17        #1/2/95/2   2       4
    ## 18        #1/2/95/2   2       4
    ## 19        #1/2/95/2   2       5
    ## 20        #1/2/95/2   2       5
    ## 21        #1/2/95/2   2       5
    ## 22    #5/5/3/83/3-3   2       5
    ## 23    #5/5/3/83/3-3   2       5
    ## 24    #5/5/3/83/3-3   2       5
    ## 25      #5/4/2/95/2   2      NA
    ## 26      #5/4/2/95/2   2      NA
    ## 27      #5/4/2/95/2   2      NA
    ## 28      #4/2/95/3-3   2       4
    ## 29      #4/2/95/3-3   2       4
    ## 30      #4/2/95/3-3   2       4
    ## 31      #4/2/95/3-3   2       4
    ## 32      #2/2/95/3-2   2       4
    ## 33      #2/2/95/3-2   2       4
    ## 34      #2/2/95/3-2   2       4
    ## 35  #1/5/3/83/3-3/2   2       4
    ## 36  #1/5/3/83/3-3/2   2       4
    ## 37  #1/5/3/83/3-3/2   2       4
    ## 38  #1/5/3/83/3-3/2   2       4
    ## 39        #3/83/3-3   1       3
    ## 40        #3/83/3-3   1       3
    ## 41        #3/83/3-3   1       3
    ## 42        #3/83/3-3   1       3
    ## 43        #3/83/3-3   1       4
    ## 44          #2/95/3   1       4
    ## 45          #2/95/3   1       3
    ## 46          #2/95/3   1       3
    ## 47          #2/95/3   1       3
    ## 48      #3/5/2/2/95   1       4
    ## 49      #3/5/2/2/95   1       4
    ## 50      #3/5/2/2/95   1       4
    ## 51      #3/5/2/2/95   1       4
    ## 52      #5/4/3/83/3   1       4
    ## 53      #5/4/3/83/3   1       4
    ## 54      #5/4/3/83/3   1       4
    ## 55      #5/4/3/83/3   1       4
    ## 56      #5/4/3/83/3   1       4
    ## 57    #1/6/2/2/95-2   1       3
    ## 58    #1/6/2/2/95-2   1       4
    ## 59  #3/5/3/83/3-3-2   1       4
    ## 60  #3/5/3/83/3-3-2   1       4
    ## 61  #3/5/3/83/3-3-2   1       4
    ## 62  #3/5/3/83/3-3-2   1       4
    ## 63        #2/2/95/2   1       5
    ## 64        #2/2/95/2   1       4
    ## 65    #3/6/2/2/95-3   1       3
    ## 66    #3/6/2/2/95-3   1       3
    ## 67    #3/6/2/2/95-3   1       3
    ## 68    #3/6/2/2/95-3   1       3
    ## 69    #3/6/2/2/95-3   1       3
    ## 70        #3/83/3-3   2       3
    ## 71        #3/83/3-3   2       3
    ## 72        #3/83/3-3   2       3
    ## 73          #2/95/3   2       3
    ## 74          #2/95/3   2       3
    ## 75          #2/95/3   2       4
    ## 76          #2/95/3   2       3
    ## 77      #3/5/2/2/95   2       4
    ## 78      #3/5/2/2/95   2       4
    ## 79      #3/5/2/2/95   2       3
    ## 80      #3/5/2/2/95   2       4
    ## 81      #5/4/3/83/3   2       4
    ## 82      #5/4/3/83/3   2       4
    ## 83      #5/4/3/83/3   2       4
    ## 84    #1/6/2/2/95-2   2       3
    ## 85    #1/6/2/2/95-2   2       3
    ## 86    #1/6/2/2/95-2   2       4
    ## 87    #1/6/2/2/95-2   2       4
    ## 88  #3/5/3/83/3-3-2   2       4
    ## 89  #3/5/3/83/3-3-2   2       4
    ## 90  #3/5/3/83/3-3-2   2       4
    ## 91  #3/5/3/83/3-3-2   2       4
    ## 92        #2/2/95/2   2       4
    ## 93        #2/2/95/2   2       4
    ## 94    #3/6/2/2/95-3   2       3
    ## 95    #3/6/2/2/95-3   2       3
    ## 96            #84/2   1       3
    ## 97            #84/2   1       3
    ## 98            #84/2   1       3
    ## 99             #107   1       4
    ## 100            #107   1       4
    ## 101            #107   1       4
    ## 102            #107   1       4
    ## 103            #107   1       4
    ## 104            #107   1       4
    ## 105            #107   1       4
    ## 106           #85/2   1       4
    ## 107           #85/2   1       4
    ## 108             #98   1       3
    ## 109             #98   1       4
    ## 110             #98   1       4
    ## 111             #98   1       4
    ## 112             #98   1       3
    ## 113            #102   1       4
    ## 114            #102   1       4
    ## 115            #101   1       3
    ## 116            #101   1       3
    ## 117            #101   1       4
    ## 118            #101   1       4
    ## 119            #111   1       4
    ## 120           #84/2   2       3
    ## 121           #84/2   2       3
    ## 122           #84/2   2       3
    ## 123           #84/2   2       3
    ## 124           #84/2   2       3
    ## 125            #107   2       4
    ## 126           #85/2   2       4
    ## 127           #85/2   2       4
    ## 128           #85/2   2       4
    ## 129           #85/2   2       4
    ## 130             #98   2       2
    ## 131             #98   2       4
    ## 132             #98   2       4
    ## 133             #98   2       3
    ## 134            #102   2       4
    ## 135            #102   2       3
    ## 136            #102   2       3
    ## 137            #102   2       4
    ## 138            #102   2       3
    ## 139            #101   2       3
    ## 140            #101   2       3
    ## 141            #101   2       3
    ## 142            #101   2       4
    ## 143            #101   2       4
    ## 144            #111   2       4
    ## 145            #111   2       4
    ## 146             #59   1       4
    ## 147             #59   1       4
    ## 148             #59   1       4
    ## 149            #103   1       4
    ## 150            #103   1       4
    ## 151            #103   1       3
    ## 152       #1/82/3-2   1       4
    ## 153       #1/82/3-2   1       4
    ## 154       #3/83/3-2   1       4
    ## 155       #3/83/3-2   1       4
    ## 156       #3/83/3-2   1       4
    ## 157       #3/83/3-2   1       4
    ## 158       #3/83/3-2   1       4
    ## 159       #3/83/3-2   1       4
    ## 160       #2/95/2-2   1       4
    ## 161       #2/95/2-2   1       4
    ## 162       #2/95/2-2   1       4
    ## 163       #2/95/2-2   1       4
    ## 164       #3/82/3-2   1       3
    ## 165       #3/82/3-2   1       4
    ## 166       #4/2/95/2   1       4
    ## 167       #4/2/95/2   1       4
    ## 168       #4/2/95/2   1       4
    ## 169       #4/2/95/2   1       4
    ## 170     #5/3/83/5-2   1       4
    ## 171     #5/3/83/5-2   1       4
    ## 172     #5/3/83/5-2   1       4
    ## 173      #8/110/3-2   1       3
    ## 174      #8/110/3-2   1       3
    ## 175      #8/110/3-2   1       3
    ## 176      #8/110/3-2   1       4
    ## 177            #106   1       3
    ## 178           #94/2   1      NA
    ## 179             #62   1       5
    ## 180             #62   1       5
    ## 181             #62   1       5
    ## 182             #59   2       4
    ## 183             #59   2       4
    ## 184            #103   2       3
    ## 185            #103   2       3
    ## 186            #103   2       3
    ## 187            #103   2       3
    ## 188            #103   2       3
    ## 189            #103   2       4
    ## 190       #1/82/3-2   2       4
    ## 191       #1/82/3-2   2       4
    ## 192       #1/82/3-2   2       5
    ## 193       #1/82/3-2   2       4
    ## 194       #3/83/3-2   2       4
    ## 195       #3/83/3-2   2       4
    ## 196       #2/95/2-2   2       4
    ## 197       #2/95/2-2   2       4
    ## 198       #2/95/2-2   2       4
    ## 199       #3/82/3-2   2       4
    ## 200       #3/82/3-2   2       3
    ## 201       #3/82/3-2   2       3
    ## 202       #4/2/95/2   2       4
    ## 203       #4/2/95/2   2       4
    ## 204       #4/2/95/2   2       4
    ## 205     #5/3/83/3-2   2       3
    ## 206     #5/3/83/3-2   2       3
    ## 207      #8/110/3-2   2       3
    ## 208      #8/110/3-2   2       3
    ## 209      #8/110/3-2   2       4
    ## 210      #8/110/3-2   2       4
    ## 211      #8/110/3-2   2       4
    ## 212            #106   2       3
    ## 213           #94/2   2      NA
    ## 214           #94/2   2      NA
    ## 215             #62   2       5
    ## 216             #53   1       4
    ## 217             #53   1       3
    ## 218             #53   1       4
    ## 219             #53   1       3
    ## 220             #53   1       4
    ## 221             #79   1       4
    ## 222             #79   1       4
    ## 223             #79   1       4
    ## 224             #79   1       4
    ## 225             #79   1       4
    ## 226            #100   1       3
    ## 227            #100   1       3
    ## 228           #4/84   1       3
    ## 229           #4/84   1       3
    ## 230           #4/84   1       4
    ## 231            #108   1       3
    ## 232            #108   1       3
    ## 233            #108   1       3
    ## 234            #108   1       3
    ## 235             #99   1      NA
    ## 236             #99   1      NA
    ## 237             #99   1      NA
    ## 238             #99   1      NA
    ## 239            #110   1      NA
    ## 240             #53   2       3
    ## 241             #53   2       4
    ## 242             #79   2       4
    ## 243             #79   2       4
    ## 244            #100   2       4
    ## 245            #100   2       3
    ## 246            #100   2       3
    ## 247            #100   2       3
    ## 248            #100   2       4
    ## 249           #4/84   2       3
    ## 250            #108   2       3
    ## 251            #108   2       3
    ## 252            #108   2       3
    ## 253             #99   2      NA
    ## 254            #110   2      NA
    ## 255            #110   2      NA
    ## 256            #110   2      NA
    ## 257            #110   2      NA
    ## 258            #110   2      NA
    ## 259             #97   1       3
    ## 260             #97   1       3
    ## 261             #97   1       3
    ## 262             #97   1       3
    ## 263             #97   1       3
    ## 264             #97   1       3
    ## 265           #5/93   1       3
    ## 266           #5/93   1       3
    ## 267           #5/93   1       3
    ## 268         #5/93/2   1       4
    ## 269       #7/82/3-2   1       3
    ## 270       #7/82/3-2   1       4
    ## 271       #7/82/3-2   1       3
    ## 272      #7/110/3-2   1       3
    ## 273      #7/110/3-2   1       3
    ## 274      #7/110/3-2   1       4
    ## 275      #7/110/3-2   1       3
    ## 276      #7/110/3-2   1       3
    ## 277      #7/110/3-2   1       3
    ## 278         #2/95/2   1       4
    ## 279         #2/95/2   1       4
    ## 280         #2/95/2   1       4
    ## 281           #82/4   1       4
    ## 282           #82/4   1       3
    ## 283           #82/4   1       4
    ## 284             #97   2       3
    ## 285             #97   2       3
    ## 286           #5/93   2       4
    ## 287           #5/93   2       3
    ## 288           #5/93   2       4
    ## 289           #5/93   2       3
    ## 290           #5/93   2       3
    ## 291           #5/93   2       3
    ## 292         #5/93/2   2       4
    ## 293         #5/93/2   2       5
    ## 294         #5/93/2   2       4
    ## 295         #5/93/2   2       5
    ## 296         #5/93/2   2       4
    ## 297         #5/93/2   2       4
    ## 298         #5/93/2   2       5
    ## 299       #7/82/3-2   2       3
    ## 300       #7/82/3-2   2       3
    ## 301       #7/82/3-2   2       3
    ## 302       #7/82/3-2   2       3
    ## 303      #7/110/3-2   2       4
    ## 304      #7/110/3-2   2       4
    ## 305         #2/95/2   2       4
    ## 306         #2/95/2   2       4
    ## 307         #2/95/2   2       4
    ## 308         #2/95/2   2       4
    ## 309         #2/95/2   2       3
    ## 310         #2/95/2   2       3
    ## 311           #82/4   2       4
    ## 312           #82/4   2       3
    ## 313           #82/4   2       3

## `filter`

use `filter()` to filter out rows.

``` r
filter(litters_df, gd_of_birth == 20, pups_born_alive == 6)
```

    ## # A tibble: 3 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #4/2/95/3-3         NA            NA          20               6
    ## 2 Con7  #2/2/95/3-2         NA            NA          20               6
    ## 3 Low8  #99                 23.5          39          20               6
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

look for more than two pups alive

``` r
filter(litters_df, pups_born_alive > 5)
```

    ## # A tibble: 41 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #1/2/95/2             27          42            19               8
    ##  2 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  3 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  4 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  5 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  6 Con8  #3/83/3-3             NA          NA            20               9
    ##  7 Con8  #2/95/3               NA          NA            20               8
    ##  8 Con8  #3/5/2/2/95           28.5        NA            20               8
    ##  9 Con8  #5/4/3/83/3           28          NA            19               9
    ## 10 Con8  #1/6/2/2/95-2         NA          NA            20               7
    ## # ℹ 31 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, pups_born_alive >= 5)
```

    ## # A tibble: 46 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #1/2/95/2             27          42            19               8
    ##  2 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  3 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  4 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  5 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  6 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  7 Con8  #3/83/3-3             NA          NA            20               9
    ##  8 Con8  #2/95/3               NA          NA            20               8
    ##  9 Con8  #3/5/2/2/95           28.5        NA            20               8
    ## 10 Con8  #5/4/3/83/3           28          NA            19               9
    ## # ℹ 36 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, pups_born_alive <= 5)
```

    ## # A tibble: 8 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ## 3 Con8  #2/2/95/2           NA          NA            19               5
    ## 4 Mod7  #3/82/3-2           28          45.9          20               5
    ## 5 Mod7  #5/3/83/5-2         22.6        37            19               5
    ## 6 Mod7  #106                21.7        37.8          20               5
    ## 7 Low7  #111                25.5        44.6          20               3
    ## 8 Low8  #4/84               21.8        35.2          20               4
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

look for non-matches

``` r
filter(litters_df, pups_born_alive != 6)
```

    ## # A tibble: 43 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2             27          42            19               8
    ##  3 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  4 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  5 Con8  #3/83/3-3             NA          NA            20               9
    ##  6 Con8  #2/95/3               NA          NA            20               8
    ##  7 Con8  #3/5/2/2/95           28.5        NA            20               8
    ##  8 Con8  #5/4/3/83/3           28          NA            19               9
    ##  9 Con8  #1/6/2/2/95-2         NA          NA            20               7
    ## 10 Con8  #3/5/3/83/3-3-2       NA          NA            20               8
    ## # ℹ 33 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

match to a set

``` r
filter(litters_df, group %in% c("Con7", "Con8"))
```

    ## # A tibble: 15 × 8
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
    ## 11 Con8  #5/4/3/83/3           28          NA            19               9
    ## 12 Con8  #1/6/2/2/95-2         NA          NA            20               7
    ## 13 Con8  #3/5/3/83/3-3-2       NA          NA            20               8
    ## 14 Con8  #2/2/95/2             NA          NA            19               5
    ## 15 Con8  #3/6/2/2/95-3         NA          NA            20               7
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

filter out missing rows with `drop_na()`

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

learning assessment:

``` r
filter(pups_df, sex == 1)
```

    ##       litter_number sex pd_ears pd_eyes pd_pivot pd_walk
    ## 1               #85   1       4      13        7      11
    ## 2               #85   1       4      13        7      12
    ## 3         #1/2/95/2   1       5      13        7       9
    ## 4         #1/2/95/2   1       5      13        8      10
    ## 5     #5/5/3/83/3-3   1       5      13        8      10
    ## 6     #5/5/3/83/3-3   1       5      14        6       9
    ## 7       #5/4/2/95/2   1      NA      14        5       9
    ## 8       #4/2/95/3-3   1       4      13        6       8
    ## 9       #4/2/95/3-3   1       4      13        7       9
    ## 10      #2/2/95/3-2   1       4      NA        8      10
    ## 11  #1/5/3/83/3-3/2   1       4      NA       NA       9
    ## 12  #1/5/3/83/3-3/2   1       4      NA        7       9
    ## 13  #1/5/3/83/3-3/2   1       4      NA        7       9
    ## 14  #1/5/3/83/3-3/2   1       4      NA        7       9
    ## 15  #1/5/3/83/3-3/2   1       4      NA        7       9
    ## 16        #3/83/3-3   1       3      13        4       7
    ## 17        #3/83/3-3   1       3      13       NA       7
    ## 18        #3/83/3-3   1       3      13        5       7
    ## 19        #3/83/3-3   1       3      12        5       8
    ## 20        #3/83/3-3   1       4      13        7       9
    ## 21          #2/95/3   1       4      13        6       9
    ## 22          #2/95/3   1       3      13        4       8
    ## 23          #2/95/3   1       3      13        4       8
    ## 24          #2/95/3   1       3      12        6       8
    ## 25      #3/5/2/2/95   1       4      13        6       8
    ## 26      #3/5/2/2/95   1       4      13        5       8
    ## 27      #3/5/2/2/95   1       4      13        5       8
    ## 28      #3/5/2/2/95   1       4      13        5       8
    ## 29      #5/4/3/83/3   1       4      13        7      10
    ## 30      #5/4/3/83/3   1       4      14        7       9
    ## 31      #5/4/3/83/3   1       4      14        6       9
    ## 32      #5/4/3/83/3   1       4      14        6      10
    ## 33      #5/4/3/83/3   1       4      14        7       9
    ## 34    #1/6/2/2/95-2   1       3      13        7       9
    ## 35    #1/6/2/2/95-2   1       4      13        7       9
    ## 36  #3/5/3/83/3-3-2   1       4      13        8      10
    ## 37  #3/5/3/83/3-3-2   1       4      13        7       9
    ## 38  #3/5/3/83/3-3-2   1       4      13        8      10
    ## 39  #3/5/3/83/3-3-2   1       4      13        8      10
    ## 40        #2/2/95/2   1       5      14        7       9
    ## 41        #2/2/95/2   1       4      14        8      11
    ## 42    #3/6/2/2/95-3   1       3      13        7       9
    ## 43    #3/6/2/2/95-3   1       3      13        7       9
    ## 44    #3/6/2/2/95-3   1       3      13        6       8
    ## 45    #3/6/2/2/95-3   1       3      12        6       8
    ## 46    #3/6/2/2/95-3   1       3      14        6       8
    ## 47            #84/2   1       3      13        5       8
    ## 48            #84/2   1       3      13        7      10
    ## 49            #84/2   1       3      13        4       7
    ## 50             #107   1       4      13        9      11
    ## 51             #107   1       4      13        9      11
    ## 52             #107   1       4      13       10      12
    ## 53             #107   1       4      13        9      11
    ## 54             #107   1       4      13        9      11
    ## 55             #107   1       4      13        9      11
    ## 56             #107   1       4      13       10      12
    ## 57            #85/2   1       4      13        9      11
    ## 58            #85/2   1       4      13       10      12
    ## 59              #98   1       3      13        7      10
    ## 60              #98   1       4      13        9      11
    ## 61              #98   1       4      13        9      11
    ## 62              #98   1       4      13       NA      10
    ## 63              #98   1       3      13        9      11
    ## 64             #102   1       4      13        7      11
    ## 65             #102   1       4      13        9      11
    ## 66             #101   1       3      12       10      12
    ## 67             #101   1       3      13        7       9
    ## 68             #101   1       4      12        6       8
    ## 69             #101   1       4      12        6      11
    ## 70             #111   1       4      13        5      10
    ## 71              #59   1       4      14       10      12
    ## 72              #59   1       4      14        8      11
    ## 73              #59   1       4      13       12      12
    ## 74             #103   1       4      13        8      10
    ## 75             #103   1       4      14        7       9
    ## 76             #103   1       3      13        8      10
    ## 77        #1/82/3-2   1       4      13        5      10
    ## 78        #1/82/3-2   1       4      13       NA       8
    ## 79        #3/83/3-2   1       4      13        6       9
    ## 80        #3/83/3-2   1       4      13        6       9
    ## 81        #3/83/3-2   1       4      13        6       8
    ## 82        #3/83/3-2   1       4      13        6       8
    ## 83        #3/83/3-2   1       4      13        6       8
    ## 84        #3/83/3-2   1       4      12        6       9
    ## 85        #2/95/2-2   1       4      13        6       8
    ## 86        #2/95/2-2   1       4      13        6       8
    ## 87        #2/95/2-2   1       4      13        6       8
    ## 88        #2/95/2-2   1       4      13        7       9
    ## 89        #3/82/3-2   1       3      13        6       8
    ## 90        #3/82/3-2   1       4      13        6       8
    ## 91        #4/2/95/2   1       4      14        8      10
    ## 92        #4/2/95/2   1       4      14       NA      11
    ## 93        #4/2/95/2   1       4      14       NA      11
    ## 94        #4/2/95/2   1       4      14        7       9
    ## 95      #5/3/83/5-2   1       4      13        6      10
    ## 96      #5/3/83/5-2   1       4      14       NA      10
    ## 97      #5/3/83/5-2   1       4      14        4      10
    ## 98       #8/110/3-2   1       3      13        6       8
    ## 99       #8/110/3-2   1       3      13        6       8
    ## 100      #8/110/3-2   1       3      13        6       8
    ## 101      #8/110/3-2   1       4      13        6       8
    ## 102            #106   1       3      13       10      12
    ## 103           #94/2   1      NA      13       NA       9
    ## 104             #62   1       5      14       11      13
    ## 105             #62   1       5      15       10      12
    ## 106             #62   1       5      15       11      13
    ## 107             #53   1       4      13       10      12
    ## 108             #53   1       3      13        9      12
    ## 109             #53   1       4      13        8      12
    ## 110             #53   1       3      13       10      12
    ## 111             #53   1       4      13        9      11
    ## 112             #79   1       4      14        9      11
    ## 113             #79   1       4      14       12      14
    ## 114             #79   1       4      14        8      10
    ## 115             #79   1       4      14        6       9
    ## 116             #79   1       4      14       10      13
    ## 117            #100   1       3      13        7       9
    ## 118            #100   1       3      13        8      10
    ## 119           #4/84   1       3      13        7       9
    ## 120           #4/84   1       3      13        6      10
    ## 121           #4/84   1       4      13        7      10
    ## 122            #108   1       3      13        5       7
    ## 123            #108   1       3      12        6       8
    ## 124            #108   1       3      13        6       8
    ## 125            #108   1       3      13        6       8
    ## 126             #99   1      NA      12        8      10
    ## 127             #99   1      NA      13        7       9
    ## 128             #99   1      NA      13        5       9
    ## 129             #99   1      NA      12        6       9
    ## 130            #110   1      NA      12        6       8
    ## 131             #97   1       3      12        7       9
    ## 132             #97   1       3      12        6       8
    ## 133             #97   1       3      12        6       9
    ## 134             #97   1       3      12        7       9
    ## 135             #97   1       3      12        7       9
    ## 136             #97   1       3      12        7       9
    ## 137           #5/93   1       3      12        8      10
    ## 138           #5/93   1       3      13        7       9
    ## 139           #5/93   1       3      13        7       9
    ## 140         #5/93/2   1       4      13        7       9
    ## 141       #7/82/3-2   1       3      12        6       8
    ## 142       #7/82/3-2   1       4      13        5       8
    ## 143       #7/82/3-2   1       3      13        6       8
    ## 144      #7/110/3-2   1       3      14        8      10
    ## 145      #7/110/3-2   1       3      14        8      10
    ## 146      #7/110/3-2   1       4      14        8      10
    ## 147      #7/110/3-2   1       3      14        7      10
    ## 148      #7/110/3-2   1       3      14        8      10
    ## 149      #7/110/3-2   1       3      14        8      10
    ## 150         #2/95/2   1       4      13        7       9
    ## 151         #2/95/2   1       4      13        7       9
    ## 152         #2/95/2   1       4      13        7       9
    ## 153           #82/4   1       4      13        8      10
    ## 154           #82/4   1       3      13        7       9
    ## 155           #82/4   1       4      13        7       9

``` r
filter(pups_df, pd_walk < 11, sex == 2)
```

    ##       litter_number sex pd_ears pd_eyes pd_pivot pd_walk
    ## 1         #1/2/95/2   2       4      13        7       9
    ## 2         #1/2/95/2   2       4      13        7      10
    ## 3         #1/2/95/2   2       5      13        8      10
    ## 4         #1/2/95/2   2       5      13        8      10
    ## 5         #1/2/95/2   2       5      13        6      10
    ## 6     #5/5/3/83/3-3   2       5      13        8      10
    ## 7     #5/5/3/83/3-3   2       5      14        7      10
    ## 8     #5/5/3/83/3-3   2       5      14        8      10
    ## 9       #5/4/2/95/2   2      NA      14        7      10
    ## 10      #5/4/2/95/2   2      NA      14        7      10
    ## 11      #5/4/2/95/2   2      NA      14        7      10
    ## 12      #4/2/95/3-3   2       4      13        5       7
    ## 13      #4/2/95/3-3   2       4      13        7       9
    ## 14      #4/2/95/3-3   2       4      13        6       8
    ## 15      #4/2/95/3-3   2       4      13        7       9
    ## 16      #2/2/95/3-2   2       4      NA        7      10
    ## 17      #2/2/95/3-2   2       4      NA        8      10
    ## 18  #1/5/3/83/3-3/2   2       4      NA        7       9
    ## 19  #1/5/3/83/3-3/2   2       4      NA        7       9
    ## 20  #1/5/3/83/3-3/2   2       4      NA        7       9
    ## 21  #1/5/3/83/3-3/2   2       4      NA        7       9
    ## 22        #3/83/3-3   2       3      13       NA       8
    ## 23        #3/83/3-3   2       3      13        4       7
    ## 24        #3/83/3-3   2       3      12        6       8
    ## 25          #2/95/3   2       3      12        6       9
    ## 26          #2/95/3   2       3      13        6       8
    ## 27          #2/95/3   2       4      12        6       9
    ## 28          #2/95/3   2       3      13        6       8
    ## 29      #3/5/2/2/95   2       4      13        6       9
    ## 30      #3/5/2/2/95   2       4      12        5       7
    ## 31      #3/5/2/2/95   2       3      13        5       9
    ## 32      #3/5/2/2/95   2       4      13        5       9
    ## 33      #5/4/3/83/3   2       4      13        7       9
    ## 34      #5/4/3/83/3   2       4      13        7      10
    ## 35      #5/4/3/83/3   2       4      13        7       9
    ## 36    #1/6/2/2/95-2   2       3      13        7       9
    ## 37    #1/6/2/2/95-2   2       3      13        5       8
    ## 38    #1/6/2/2/95-2   2       4      13        7       9
    ## 39    #1/6/2/2/95-2   2       4      13        8      10
    ## 40  #3/5/3/83/3-3-2   2       4      13        4       9
    ## 41  #3/5/3/83/3-3-2   2       4      13        7       9
    ## 42  #3/5/3/83/3-3-2   2       4      13        7      10
    ## 43  #3/5/3/83/3-3-2   2       4      13        7       9
    ## 44        #2/2/95/2   2       4      13        6       8
    ## 45    #3/6/2/2/95-3   2       3      12        6       8
    ## 46    #3/6/2/2/95-3   2       3      12        7       9
    ## 47            #84/2   2       3      13        6       8
    ## 48            #84/2   2       3      12        8      10
    ## 49            #84/2   2       3      13        5       8
    ## 50             #107   2       4      13        8      10
    ## 51              #98   2       2      13        7      10
    ## 52              #98   2       4      13        7      10
    ## 53             #102   2       4      13        8      10
    ## 54             #101   2       3      12        6      10
    ## 55             #111   2       4      13        5      10
    ## 56             #111   2       4      13        5      10
    ## 57              #59   2       4      13        8      10
    ## 58             #103   2       3      13        7       9
    ## 59             #103   2       3      12        7       9
    ## 60             #103   2       3      13        7       9
    ## 61             #103   2       3      12        7       9
    ## 62             #103   2       3      13        8      10
    ## 63             #103   2       4      13        6       9
    ## 64        #1/82/3-2   2       4      13        8      10
    ## 65        #1/82/3-2   2       4      13        5      10
    ## 66        #1/82/3-2   2       5      13        6      10
    ## 67        #1/82/3-2   2       4      13        8      10
    ## 68        #3/83/3-2   2       4      12       NA       8
    ## 69        #3/83/3-2   2       4      12        6       8
    ## 70        #2/95/2-2   2       4      13        6       9
    ## 71        #2/95/2-2   2       4      13        6       8
    ## 72        #2/95/2-2   2       4      13        5       8
    ## 73        #3/82/3-2   2       4      13        5       8
    ## 74        #3/82/3-2   2       3      12        4       8
    ## 75        #3/82/3-2   2       3      12        6       8
    ## 76        #4/2/95/2   2       4      14        7       9
    ## 77        #4/2/95/2   2       4      14        7      10
    ## 78      #5/3/83/3-2   2       3      12       NA       8
    ## 79      #5/3/83/3-2   2       3      13       NA      10
    ## 80       #8/110/3-2   2       3      12        4       9
    ## 81       #8/110/3-2   2       3      13        6       8
    ## 82       #8/110/3-2   2       4      14        6       9
    ## 83       #8/110/3-2   2       4      13        6       8
    ## 84       #8/110/3-2   2       4      13        6       8
    ## 85             #106   2       3      14        8      10
    ## 86            #94/2   2      NA      13       NA       9
    ## 87             #100   2       3      13        8      10
    ## 88            #4/84   2       3      13        6      10
    ## 89             #108   2       3      13        6       8
    ## 90             #108   2       3      14        6       8
    ## 91             #108   2       3      13        6      10
    ## 92              #99   2      NA      13        7       9
    ## 93             #110   2      NA      12        7       9
    ## 94             #110   2      NA      12        6       8
    ## 95             #110   2      NA      12        7       9
    ## 96             #110   2      NA      12        7       9
    ## 97             #110   2      NA      12        7       9
    ## 98              #97   2       3      12        7       9
    ## 99              #97   2       3      12        6       8
    ## 100           #5/93   2       4      13        7       9
    ## 101           #5/93   2       3      12        7       9
    ## 102           #5/93   2       4      13        7       9
    ## 103           #5/93   2       3      13        7       9
    ## 104           #5/93   2       3      12        7       9
    ## 105           #5/93   2       3      12        7       9
    ## 106         #5/93/2   2       4      14        7       9
    ## 107         #5/93/2   2       5      14        7       9
    ## 108         #5/93/2   2       4      13        7       9
    ## 109         #5/93/2   2       5      14        7       9
    ## 110         #5/93/2   2       4      13        7       9
    ## 111         #5/93/2   2       4      14        6       9
    ## 112         #5/93/2   2       5      13        7       9
    ## 113       #7/82/3-2   2       3      13        6       8
    ## 114       #7/82/3-2   2       3      12        6       8
    ## 115       #7/82/3-2   2       3      12        6       8
    ## 116       #7/82/3-2   2       3      12        6       8
    ## 117      #7/110/3-2   2       4      14        8      10
    ## 118      #7/110/3-2   2       4      14        7       9
    ## 119         #2/95/2   2       4      12        7       9
    ## 120         #2/95/2   2       4      12        6       8
    ## 121         #2/95/2   2       4      13        7       9
    ## 122         #2/95/2   2       4      12        7       9
    ## 123         #2/95/2   2       3      13        6       8
    ## 124         #2/95/2   2       3      13        7       9
    ## 125           #82/4   2       4      13        7       9
    ## 126           #82/4   2       3      13        7       9
    ## 127           #82/4   2       3      13        7       9

## `mutate`

use `mutate()` to create or modify variables.

``` r
mutate(litters_df, 
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

## `arrange`

`arrange()` arranges things

``` r
arrange(litters_df, desc(pups_born_alive))
```

    ## # A tibble: 49 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Low7  #102                  22.6        43.3          20              11
    ##  2 Mod8  #5/93                 NA          41.1          20              11
    ##  3 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  4 Con8  #3/83/3-3             NA          NA            20               9
    ##  5 Con8  #5/4/3/83/3           28          NA            19               9
    ##  6 Mod7  #103                  21.4        42.1          19               9
    ##  7 Mod7  #4/2/95/2             23.5        NA            19               9
    ##  8 Mod7  #8/110/3-2            NA          NA            20               9
    ##  9 Low7  #107                  22.6        42.4          20               9
    ## 10 Low7  #98                   23.8        43.8          20               9
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
arrange(litters_df, desc(group), pups_born_alive)
```

    ## # A tibble: 49 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Mod8  #7/82-3-2           26.9        43.2          20               7
    ##  2 Mod8  #97                 24.5        42.8          20               8
    ##  3 Mod8  #5/93/2             NA          NA            19               8
    ##  4 Mod8  #7/110/3-2          27.5        46            19               8
    ##  5 Mod8  #82/4               33.4        52.7          20               8
    ##  6 Mod8  #2/95/2             28.5        44.5          20               9
    ##  7 Mod8  #5/93               NA          41.1          20              11
    ##  8 Mod7  #3/82/3-2           28          45.9          20               5
    ##  9 Mod7  #5/3/83/5-2         22.6        37            19               5
    ## 10 Mod7  #106                21.7        37.8          20               5
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

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

## PIPES

let’s see a bad way to do things (very clunky, & messy)

``` r
litters_df = 
  read_csv("data/FAS_litters.csv", na = c("NA", ".",""))
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
  janitor::clean_names(litters_df)

litters_df = 
  select(litters_df, group, litter_number, starts_with("gd"))

litters_df = 
  drop_na(litters_df)

litters_df = 
  mutate(
    litters_df,
    wt_gain = gd18_weight - gd0_weight
  )
```

Use pipes to avoid all this!! (cmd-shift-m for pipe operator)

``` r
litters_df = 
  read_csv("data/FAS_litters.csv", na = c("NA", ".", "")) |> 
  janitor::clean_names() |> 
  select(group, litter_number, starts_with("gd")) |> 
  drop_na() |> 
  mutate(
    wt_gain = gd18_weight - gd0_weight
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

learning assessment: import and tidy pups data

``` r
pups_df = 
  read_csv("data/FAS_pups.csv", skip = 3, na = c("NA", ".", "")) |> 
  janitor::clean_names() |> 
  filter(sex == 1) |> 
  select(-pd_ears) |> 
  mutate(
    pd_pivot_ge7 = pd_pivot >= 7
  )
```

    ## Rows: 313 Columns: 6
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Litter Number
    ## dbl (5): Sex, PD ears, PD eyes, PD pivot, PD walk
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
