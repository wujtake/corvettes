Corvettes
================
Alexis Jeter
2022-06-11

## Corvettes

## Corvettes Dataset

Importing the dataset. NOTE: This assumes that your Corvette.xlsx is in
the same directory as the RMD file.

``` r
corvettes <- read_excel("Corvette.xlsx")
str(corvettes)
```

    ## tibble [100 × 9] (S3: tbl_df/tbl/data.frame)
    ##  $ Year        : num [1:100] 1980 1984 1985 1994 1984 ...
    ##  $ Color       : chr [1:100] "Blue" "Red" "Black" "Black" ...
    ##  $ Interior    : chr [1:100] "Oyster" "Black" "Silver" "Black" ...
    ##  $ Engine      : chr [1:100] "350 CI" "350/205 HP" "350 CI" "350/300 HP" ...
    ##  $ Transmission: chr [1:100] "A" "A" "A" "S" ...
    ##  $ Convertible : chr [1:100] "N" "N" "N" "N" ...
    ##  $ Miles       : num [1:100] NA NA 31000 NA 79000 ...
    ##  $ Price       : num [1:100] 6050 6050 6600 6600 6600 ...
    ##  $ Auction     : chr [1:100] "Dallas" "Louisville" "Louisville" "Kansas City" ...

## Finding Missing Data

Displaying the column names which are missing data and the count of
missing data.

|              | Missing Values |
|:-------------|---------------:|
| Interior     |              3 |
| Engine       |              4 |
| Transmission |              5 |
| Miles        |              7 |

### Engines

I’m interested in the unique engines in this list for question 2.

``` r
unique(corvettes$Engine)
```

    ##  [1] "350 CI"      "350/205 HP"  "350/300 HP"  "5.7L"        NA           
    ##  [6] "350/210 HP"  "383 CI"      "LS1"         "502 CI"      "350/195 HP" 
    ## [11] "350/375 HP"  "5.7/330 HP"  "350/190 HP"  "350/180 HP"  "LS1/350 HP" 
    ## [16] "327/300 HP"  "5.7/350 HP"  "5.7/300 HP"  "5.7L/345 HP" "350/220 HP" 
    ## [21] "L82"         "5.7/345 HP"  "6.0/400 HP"  "LS2"         "6.0L"       
    ## [26] "6.0L/400 HP" "327/350 HP"  "5.7/405 HP"  "427 CI"      "LT5/375 HP" 
    ## [31] "427/390 HP"  "LS7/505 HP"  "454 CI"      "350/330 HP"  "6.2L/460 HP"
    ## [36] "327 CI"      "6.2L"        "350/435 HP"  "427/425 HP"  "327/360 HP" 
    ## [41] "427/435 HP"  "454/425 HP"

### Color & Interior

#### Color

| Colors       |
|:-------------|
| Blue         |
| Red          |
| Black        |
| Brown        |
| Yellow       |
| Maroon       |
| Silver       |
| Buckskin     |
| White        |
| Gold         |
| Teal         |
| Green        |
| Silver Beige |
| Burgundy     |
| Black Rose   |
| Ruby Red     |
| Purple       |
| Bronze       |
| Gray         |
| Tan          |
| Silver Pearl |
| Orange       |

|  id | colorfam_1 | colorfam_2 | colorfam_3 | colorfam_4   | colorfam_5 | colorfam_6   | colorfam_7 |
|----:|:-----------|:-----------|:-----------|:-------------|:-----------|:-------------|:-----------|
|   1 | Blue       | Red        | Black      | Brown        | Yellow     | Silver       | Green      |
|   2 | Teal       | Maroon     | Black Rose | Buckskin     | Orange     | Gray         | \-         |
|   3 | Purple     | Burgundy   | \-         | Silver Beige | \-         | Silver Pearl | \-         |
|   4 | \-         | Ruby Red   | \-         | Tan          | \-         | White        | \-         |
|   5 | \-         | \-         | \-         | Gold         | \-         | \-           | \-         |
|   6 | \-         | \-         | \-         | Bronze       | \-         | \-           | \-         |

#### Interior

| Interiors    |
|:-------------|
| Oyster       |
| Black        |
| Silver       |
| Brown        |
| NA           |
| Red          |
| Gray         |
| Grey         |
| Tan          |
| Blue         |
| Camel        |
| Blue/White   |
| White        |
| Silver Beige |
| Ruby Red     |
| Saddle       |
| Yellow/Black |
| Green        |
| Red/Black    |
| Tobacco      |

|  id | intfam_1 | intfam_2 | intfam_3     | intfam_4 | intfam_5 | intfam_6     | intfam_7 |
|----:|:---------|:---------|:-------------|:---------|:---------|:-------------|:---------|
|   1 | Oyster   | Black    | Brown        | Red      | Blue     | Blue/White   | Green    |
|   2 | Silver   | \-       | Tan          | Ruby Red | \-       | Yellow/Black | \-       |
|   3 | Gray     | \-       | Camel        | \-       | \-       | Red/Black    | \-       |
|   4 | Grey     | \-       | Tobacco      | \-       | \-       | \-           | \-       |
|   5 | White    | \-       | Silver Beige | \-       | \-       | \-           | \-       |
|   6 | \-       | \-       | Saddle       | \-       | \-       | \-           | \-       |

## Blank Rows

### Transmissions

| Year | Color | Interior | Engine     | Transmission | Convertible | Miles | Price | Auction     |
|-----:|:------|:---------|:-----------|:-------------|:------------|------:|------:|:------------|
| 1990 | Red   | Camel    | 350 CI     | NA           | Y           | 11000 |  9350 | Monterey    |
| 1964 | White | White    | 327/300 HP | NA           | Y           |    NA | 50000 | Kansas City |

| Standard | Automatic | Total | Standard Percent |
|---------:|----------:|------:|:-----------------|
|       43 |        55 |    98 | 44%              |

### Convertible

| Convertible | Regular | Total | Convertible Percent |
|------------:|--------:|------:|:--------------------|
|          33 |      67 |   100 | 33%                 |

### Miles

Using mean imputation in this case. Using simple rounding since the
existing dataset does not have decimals.

    ## [1] 42986

## Categorical Variables

Will create a new table for this so that I can substitute without
altering the original table (and losing track). New table with all the
changes for the remaining questions will be at the bottom of the page.

``` r
## figure out what auction locations exist
auction_loc <- corvettes$Auction
unique(auction_loc, rm.na=TRUE)
```

    ## [1] "Dallas"      "Louisville"  "Kansas City" "Monterey"

``` r
## find the most frequent
names(which.max(table(auction_loc)))
```

    ## [1] "Dallas"

    ## Warning: `funs()` was deprecated in dplyr 0.8.0.
    ## Please use a list of either functions or lambdas: 
    ## 
    ##   # Simple named list: 
    ##   list(mean = mean, median = median)
    ## 
    ##   # Auto named with `tibble::lst()`: 
    ##   tibble::lst(mean, median)
    ## 
    ##   # Using lambdas
    ##   list(~ mean(., trim = .2), ~ median(., na.rm = TRUE))
    ## This warning is displayed once every 8 hours.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was generated.

| Year | Color        | Interior     | Engine      | Transmission | Convertible | Miles |  Price | Auction     | auc_louisville | auc_kc | auc_monterey |
|-----:|:-------------|:-------------|:------------|-------------:|------------:|------:|-------:|:------------|---------------:|-------:|-------------:|
| 1980 | Blue         | Oyster       | 350 CI      |            0 |           0 | 42986 |   6050 | Dallas      |              0 |      0 |            0 |
| 1984 | Brown        | Brown        | 5.7L        |            0 |           0 | 79000 |   6600 | Dallas      |              0 |      0 |            0 |
| 1978 | Yellow       | Brown        | 350 CI      |            0 |           0 | 65000 |   7150 | Dallas      |              0 |      0 |            0 |
| 1989 | Red          | Red          | 5.7L        |            1 |           1 | 42986 |   7150 | Dallas      |              0 |      0 |            0 |
| 1978 | Silver       | Tan          | 350 CI      |            0 |           0 | 42986 |   8800 | Dallas      |              0 |      0 |            0 |
| 1977 | Blue         | Blue         | 350/210 HP  |            0 |           0 | 42986 |   8800 | Dallas      |              0 |      0 |            0 |
| 2001 | White        | Black        | 5.7L        |            0 |           0 | 42986 |   9900 | Dallas      |              0 |      0 |            0 |
| 1999 | Gold         | Tan          | 5.7L        |            1 |           0 | 42986 |  11000 | Dallas      |              0 |      0 |            0 |
| 1991 | Teal         | Black        | 5.7L        |            0 |           1 | 42986 |  11000 | Dallas      |              0 |      0 |            0 |
| 1996 | Green        | Tan          | 5.7/330 HP  |            1 |           0 | 42986 |  12100 | Dallas      |              0 |      0 |            0 |
| 1988 | White        | White        | 5.7L        |            0 |           0 | 57566 |  12100 | Dallas      |              0 |      0 |            0 |
| 1996 | Silver       | Black        | 5.7L        |            1 |           0 | 34800 |  12650 | Dallas      |              0 |      0 |            0 |
| 1982 | Silver Beige | Silver Beige | 350 CI      |            0 |           0 | 66275 |  13200 | Dallas      |              0 |      0 |            0 |
| 1977 | Burgundy     | Red          | 350/180 HP  |            0 |           0 | 42986 |  13200 | Dallas      |              0 |      0 |            0 |
| 1978 | Black        | Tan          | 350 CI      |            1 |           0 | 42986 |  13750 | Dallas      |              0 |      0 |            0 |
| 1968 | Blue         | Blue         | 327/300 HP  |            0 |           0 | 42986 |  14300 | Dallas      |              0 |      0 |            0 |
| 1990 | White        | Red          | 350/210 HP  |            0 |           0 | 60000 |  14300 | Dallas      |              0 |      0 |            0 |
| 2002 | Blue         | Tan          | 5.7/350 HP  |            1 |           1 | 54000 |  15400 | Dallas      |              0 |      0 |            0 |
| 1980 | White        | Black        | 350 CI      |            0 |           0 | 42986 |  16500 | Dallas      |              0 |      0 |            0 |
| 1993 | Ruby Red     | Ruby Red     | 350/300 HP  |            1 |           0 | 32000 |  16500 | Dallas      |              0 |      0 |            0 |
| 2000 | Yellow       | Black        | 5.7L/345 HP |            0 |           1 |  9109 |  16500 | Dallas      |              0 |      0 |            0 |
| 1978 | Silver       | White        | 350/220 HP  |            1 |           0 | 35000 |  17050 | Dallas      |              0 |      0 |            0 |
| 1995 | White        | Black        | 350/300 HP  |            0 |           0 | 45000 |  17600 | Dallas      |              0 |      0 |            0 |
| 2006 | Black        | Black        | 6.0/400 HP  |            0 |           1 | 42986 |  20900 | Dallas      |              0 |      0 |            0 |
| 2005 | Black        | Black        | LS2         |            1 |           0 | 12754 |  20900 | Dallas      |              0 |      0 |            0 |
| 2002 | Blue         | Black        | 5.7L        |            1 |           0 | 13000 |  24200 | Dallas      |              0 |      0 |            0 |
| 1971 | Green        | Green        | LS1         |            0 |           1 | 42986 |  25300 | Dallas      |              0 |      0 |            0 |
| 1968 | Blue         | Blue         | 327/350 HP  |            1 |           1 | 34124 |  26400 | Dallas      |              0 |      0 |            0 |
| 2003 | Red          | Red/Black    | 5.7/405 HP  |            1 |           0 | 22000 |  26400 | Dallas      |              0 |      0 |            0 |
| 1982 | Silver Beige | Silver Beige | 350 CI      |            0 |           0 | 42986 |  27500 | Dallas      |              0 |      0 |            0 |
| 1968 | Yellow       | Black        | 427 CI      |            1 |           0 | 42986 |  27500 | Dallas      |              0 |      0 |            0 |
| 2002 | Blue         | Black        | 5.7L        |            1 |           0 |  8574 |  27500 | Dallas      |              0 |      0 |            0 |
| 1970 | Green        | Green        | 454 CI      |            0 |           0 | 25123 |  35200 | Dallas      |              0 |      0 |            0 |
| 1996 | Blue         | Black        | 350 CI      |            1 |           1 |  4447 |  36300 | Dallas      |              0 |      0 |            0 |
| 1972 | Green        | Black        | 350 CI      |            0 |           0 | 42986 |  38500 | Dallas      |              0 |      0 |            0 |
| 1968 | Blue         | Black        | 427/390 HP  |            1 |           1 | 42986 |  42900 | Dallas      |              0 |      0 |            0 |
| 1965 | Red          | White        | 327 CI      |            0 |           0 | 42986 |  48400 | Dallas      |              0 |      0 |            0 |
| 2016 | Gray         | Tan          | 6.2L        |            1 |           0 | 11000 |  48400 | Dallas      |              0 |      0 |            0 |
| 1966 | Silver       | Blue         | 350/435 HP  |            1 |           0 | 42986 |  49500 | Dallas      |              0 |      0 |            0 |
| 1966 | Yellow       | Black        | 327/350 HP  |            1 |           1 | 42986 |  68200 | Dallas      |              0 |      0 |            0 |
| 1966 | White        | Red          | 327 CI      |            0 |           0 | 64668 |  74800 | Dallas      |              0 |      0 |            0 |
| 1966 | Blue         | Black        | 427/425 HP  |            1 |           1 | 42986 |  96800 | Dallas      |              0 |      0 |            0 |
| 1966 | Blue         | White        | 327/350 HP  |            1 |           1 | 42986 | 107800 | Dallas      |              0 |      0 |            0 |
| 1963 | Tan          | Tan          | 327/300 HP  |            1 |           0 | 29435 | 126500 | Dallas      |              0 |      0 |            0 |
| 1994 | Black        | Black        | 350/300 HP  |            1 |           0 | 42986 |   6600 | Kansas City |              0 |      1 |            0 |
| 1993 | Maroon       | Red          | 5.7L        |            0 |           0 | 54530 |   7425 | Kansas City |              0 |      1 |            0 |
| 1979 | Black        | Black        | 350 CI      |            0 |           0 | 42986 |   8250 | Kansas City |              0 |      1 |            0 |
| 1976 | Buckskin     | Brown        | 350 CI      |            0 |           0 | 81000 |   8250 | Kansas City |              0 |      1 |            0 |
| 1996 | Red          | Grey         | 350/300 HP  |            0 |           0 | 42986 |   8525 | Kansas City |              0 |      1 |            0 |
| 1981 | Blue         | Black        | 383 CI      |            0 |           0 | 42986 |  10000 | Kansas City |              0 |      1 |            0 |
| 2000 | Blue         | Black        | 5.7L        |            0 |           0 | 37000 |  10450 | Kansas City |              0 |      1 |            0 |
| 2001 | Green        | Tan          | LS1         |            0 |           0 | 86000 |  11000 | Kansas City |              0 |      1 |            0 |
| 1979 | Black        | Black        | 350/195 HP  |            0 |           0 | 42986 |  12100 | Kansas City |              0 |      1 |            0 |
| 2001 | Silver       | Black        | LS1/350 HP  |            1 |           0 | 69000 |  13200 | Kansas City |              0 |      1 |            0 |
| 2002 | Black        | Black        | 5.7L        |            0 |           1 | 54000 |  15400 | Kansas City |              0 |      1 |            0 |
| 1993 | Red          | Red          | 5.7/300 HP  |            0 |           1 | 50000 |  16500 | Kansas City |              0 |      1 |            0 |
| 2000 | Red          | Black        | 5.7L/345 HP |            1 |           0 | 69000 |  17600 | Kansas City |              0 |      1 |            0 |
| 1979 | Red          | Black        | L82         |            1 |           0 | 42986 |  17600 | Kansas City |              0 |      1 |            0 |
| 1975 | White        | Saddle       | 350 CI      |            0 |           1 | 67585 |  18150 | Kansas City |              0 |      1 |            0 |
| 1998 | Purple       | Yellow/Black | 5.7/345 HP  |            0 |           1 | 53000 |  19800 | Kansas City |              0 |      1 |            0 |
| 2005 | Yellow       | Black        | 6.0L/400 HP |            0 |           1 | 57000 |  25300 | Kansas City |              0 |      1 |            0 |
| 1991 | Red          | Red          | LT5/375 HP  |            1 |           0 |  6300 |  28600 | Kansas City |              0 |      1 |            0 |
| 2006 | Black        | Black        | LS7/505 HP  |            1 |           0 | 20090 |  29150 | Kansas City |              0 |      1 |            0 |
| 1972 | Red          | Red          | 454 CI      |            0 |           1 | 54000 |  35750 | Kansas City |              0 |      1 |            0 |
| 2014 | Silver       | White        | 6.2L/460 HP |            1 |           0 | 23000 |  42900 | Kansas City |              0 |      1 |            0 |
| 1964 | Red          | Red          | 327 CI      |            1 |           1 | 42986 |  47300 | Kansas City |              0 |      1 |            0 |
| 1964 | White        | White        | 327/300 HP  |            1 |           1 | 42986 |  50000 | Kansas City |              0 |      1 |            0 |
| 1967 | Silver Pearl | Black        | 427/435 HP  |            1 |           1 | 29000 | 181500 | Kansas City |              0 |      1 |            0 |
| 1984 | Red          | Black        | 350/205 HP  |            0 |           0 | 42986 |   6050 | Louisville  |              1 |      0 |            0 |
| 1985 | Black        | Silver       | 350 CI      |            0 |           0 | 31000 |   6600 | Louisville  |              1 |      0 |            0 |
| 1978 | Silver       | Gray         | 350 CI      |            0 |           0 | 82024 |   8250 | Louisville  |              1 |      0 |            0 |
| 1993 | White        | Red          | 5.7L        |            0 |           0 | 48490 |   8250 | Louisville  |              1 |      0 |            0 |
| 1996 | Silver       | Gray         | 5.7L        |            0 |           0 | 42986 |   8800 | Louisville  |              1 |      0 |            0 |
| 1976 | Black        | Tan          | 350 CI      |            0 |           0 | 55184 |   9075 | Louisville  |              1 |      0 |            0 |
| 1977 | Red          | Black        | 350 CI      |            0 |           0 | 53487 |   9350 | Louisville  |              1 |      0 |            0 |
| 1994 | Red          | Red          | 5.7L        |            0 |           0 | 73094 |   9350 | Louisville  |              1 |      0 |            0 |
| 1984 | Red          | Red          | 5.7L        |            1 |           0 | 42986 |   9350 | Louisville  |              1 |      0 |            0 |
| 1993 | Black        | Black        | 350/300 HP  |            0 |           0 | 43000 |  10450 | Louisville  |              1 |      0 |            0 |
| 1976 | Blue         | Blue/White   | 502 CI      |            0 |           0 | 42986 |  11500 | Louisville  |              1 |      0 |            0 |
| 1981 | Silver       | Red          | 350/190 HP  |            0 |           0 | 62000 |  12650 | Louisville  |              1 |      0 |            0 |
| 1994 | Black Rose   | Black        | 350/300 HP  |            0 |           1 | 29940 |  14300 | Louisville  |              1 |      0 |            0 |
| 1996 | Red          | Black        | 350/300 HP  |            0 |           1 | 57000 |  14850 | Louisville  |              1 |      0 |            0 |
| 1998 | Silver       | Black        | 350 CI      |            0 |           1 | 55772 |  15950 | Louisville  |              1 |      0 |            0 |
| 1993 | Red          | Red          | 350/300 HP  |            0 |           0 | 14689 |  16500 | Louisville  |              1 |      0 |            0 |
| 2002 | Black        | Black        | 5.7L        |            1 |           0 | 26029 |  16500 | Louisville  |              1 |      0 |            0 |
| 1998 | Silver       | Black        | 5.7L        |            0 |           0 | 52879 |  17050 | Louisville  |              1 |      0 |            0 |
| 2004 | Blue         | Tan          | 5.7L        |            1 |           1 | 71152 |  17600 | Louisville  |              1 |      0 |            0 |
| 2005 | Silver       | Black        | 6.0L        |            1 |           1 | 45456 |  23100 | Louisville  |              1 |      0 |            0 |
| 1968 | Blue         | Black        | 427/390 HP  |            1 |           0 | 65191 |  29150 | Louisville  |              1 |      0 |            0 |
| 1968 | Bronze       | Black        | 350/330 HP  |            1 |           1 | 42986 |  38500 | Louisville  |              1 |      0 |            0 |
| 1990 | Red          | Camel        | 350 CI      |            1 |           1 | 11000 |   9350 | Monterey    |              0 |      0 |            1 |
| 1996 | Silver       | Red          | 5.7L        |            1 |           1 | 44000 |   9350 | Monterey    |              0 |      0 |            1 |
| 1991 | Black        | Black        | 350/375 HP  |            1 |           0 | 42986 |  12100 | Monterey    |              0 |      0 |            1 |
| 1962 | White        | Black        | 327/360 HP  |            1 |           1 | 11225 | 101750 | Monterey    |              0 |      0 |            1 |
| 1968 | Bronze       | Tobacco      | 427/435 HP  |            1 |           0 |  5300 | 129250 | Monterey    |              0 |      0 |            1 |
| 1971 | Orange       | Black        | 454/425 HP  |            1 |           1 | 42986 | 368500 | Monterey    |              0 |      0 |            1 |
