Video Game Sales Data Cleaning in R
================
Theresa Gyamfi Allotey
2025-05-10

## Introduction

This document outlines the data cleaning process for the `vgsales.csv`
dataset, sourced from Kaggle. The dataset contains 16,598 records of
video game sales across 11 columns: `Rank`, `Name`, `Platform`, `Year`,
`Genre`, `Publisher`, `NA_Sales`, `EU_Sales`, `JP_Sales`, `Other_Sales`,
and `Global_Sales`. The goal is to clean the data by addressing missing
values, duplicates, and data types, producing `vgsales_cleaned.csv` for
downstream analysis or visualization (e.g., in Tableau).

This process showcases data cleaning skills essential for data
analytics, including handling missing data, transforming variables, and
ensuring data quality, aligned with industry-standard practices.

## Load Required Libraries

Load R packages for data manipulation, summary statistics, and
exploratory data analysis (EDA). Install if not already available.

``` r
# Install packages if not present
install.packages("tidyverse")
install.packages("DataExplorer")
install.packages("here")
```

``` r
if (!requireNamespace("skimr", quietly = TRUE)) {
  install.packages("skimr")
}
library(tidyverse)     # For data manipulation and visualization
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.2     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.4     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(skimr)         # For summary statistics
library(DataExplorer)  # For automated EDA
```

## Read the Dataset

Load the dataset, treating “N/A” and empty strings as missing values.

``` r
df <- read.csv("vgsales.csv", na.strings = c("N/A", ""))
```

## Initial Data Inspection

Inspect the dataset to identify structure, dimensions, and issues like
missing values or incorrect data types.

### Check Dimensions

Verify the number of rows and columns.

``` r
dim(df)
```

    ## [1] 16598    11

### Inspect Structure

Examine data types and sample values.

``` r
str(df)
```

    ## 'data.frame':    16598 obs. of  11 variables:
    ##  $ Rank        : int  1 2 3 4 5 6 7 8 9 10 ...
    ##  $ Name        : chr  "Wii Sports" "Super Mario Bros." "Mario Kart Wii" "Wii Sports Resort" ...
    ##  $ Platform    : chr  "Wii" "NES" "Wii" "Wii" ...
    ##  $ Year        : int  2006 1985 2008 2009 1996 1989 2006 2006 2009 1984 ...
    ##  $ Genre       : chr  "Sports" "Platform" "Racing" "Sports" ...
    ##  $ Publisher   : chr  "Nintendo" "Nintendo" "Nintendo" "Nintendo" ...
    ##  $ NA_Sales    : num  41.5 29.1 15.8 15.8 11.3 ...
    ##  $ EU_Sales    : num  29.02 3.58 12.88 11.01 8.89 ...
    ##  $ JP_Sales    : num  3.77 6.81 3.79 3.28 10.22 ...
    ##  $ Other_Sales : num  8.46 0.77 3.31 2.96 1 0.58 2.9 2.85 2.26 0.47 ...
    ##  $ Global_Sales: num  82.7 40.2 35.8 33 31.4 ...

### View First Few Rows

Preview the data to confirm content.

``` r
head(df)
```

    ##   Rank                     Name Platform Year        Genre Publisher NA_Sales
    ## 1    1               Wii Sports      Wii 2006       Sports  Nintendo    41.49
    ## 2    2        Super Mario Bros.      NES 1985     Platform  Nintendo    29.08
    ## 3    3           Mario Kart Wii      Wii 2008       Racing  Nintendo    15.85
    ## 4    4        Wii Sports Resort      Wii 2009       Sports  Nintendo    15.75
    ## 5    5 Pokemon Red/Pokemon Blue       GB 1996 Role-Playing  Nintendo    11.27
    ## 6    6                   Tetris       GB 1989       Puzzle  Nintendo    23.20
    ##   EU_Sales JP_Sales Other_Sales Global_Sales
    ## 1    29.02     3.77        8.46        82.74
    ## 2     3.58     6.81        0.77        40.24
    ## 3    12.88     3.79        3.31        35.82
    ## 4    11.01     3.28        2.96        33.00
    ## 5     8.89    10.22        1.00        31.37
    ## 6     2.26     4.22        0.58        30.26

### Summary Statistics

Summarize missing values and distributions using `skimr`.

``` r
skim(df)
```

|                                                  |       |
|:-------------------------------------------------|:------|
| Name                                             | df    |
| Number of rows                                   | 16598 |
| Number of columns                                | 11    |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |       |
| Column type frequency:                           |       |
| character                                        | 4     |
| numeric                                          | 7     |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |       |
| Group variables                                  | None  |

Data summary

**Variable type: character**

| skim_variable | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:--------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| Name          |         0 |             1 |   1 | 132 |     0 |    11493 |          0 |
| Platform      |         0 |             1 |   2 |   4 |     0 |       31 |          0 |
| Genre         |         0 |             1 |   4 |  12 |     0 |       12 |          0 |
| Publisher     |        58 |             1 |   3 |  38 |     0 |      578 |          0 |

**Variable type: numeric**

| skim_variable | n_missing | complete_rate | mean | sd | p0 | p25 | p50 | p75 | p100 | hist |
|:---|---:|---:|---:|---:|---:|---:|---:|---:|---:|:---|
| Rank | 0 | 1.00 | 8300.61 | 4791.85 | 1.00 | 4151.25 | 8300.50 | 12449.75 | 16600.00 | ▇▇▇▇▇ |
| Year | 271 | 0.98 | 2006.41 | 5.83 | 1980.00 | 2003.00 | 2007.00 | 2010.00 | 2020.00 | ▁▁▃▇▂ |
| NA_Sales | 0 | 1.00 | 0.26 | 0.82 | 0.00 | 0.00 | 0.08 | 0.24 | 41.49 | ▇▁▁▁▁ |
| EU_Sales | 0 | 1.00 | 0.15 | 0.51 | 0.00 | 0.00 | 0.02 | 0.11 | 29.02 | ▇▁▁▁▁ |
| JP_Sales | 0 | 1.00 | 0.08 | 0.31 | 0.00 | 0.00 | 0.00 | 0.04 | 10.22 | ▇▁▁▁▁ |
| Other_Sales | 0 | 1.00 | 0.05 | 0.19 | 0.00 | 0.00 | 0.01 | 0.04 | 10.57 | ▇▁▁▁▁ |
| Global_Sales | 0 | 1.00 | 0.54 | 1.56 | 0.01 | 0.06 | 0.17 | 0.47 | 82.74 | ▇▁▁▁▁ |

### Automated EDA

Visualize missing values and distributions with `DataExplorer`.

``` r
plot_missing(df)   # Missing data visualization
```

![](video-game-sales-data-cleaning-in-R_files/figure-gfm/eda-1.png)<!-- -->

``` r
plot_histogram(df) # Histograms for numeric columns
```

![](video-game-sales-data-cleaning-in-R_files/figure-gfm/eda-2.png)<!-- -->

## Data Cleaning Steps

Based on inspection, address:

- Missing `Year` values: Impute with median.
- Unknown publishers: Replace with `NA`, impute with mode.
- Duplicates: Remove exact duplicates.
- Skewed sales: Add `Global_Sales_log` for visualizations.
- Categorical variables: Convert `Platform`, `Genre`, `Publisher` to
  factors.
- Missing sales: Impute with 0.

### Step 1: Handle Missing Year Values

Impute missing `Year` values with the median to support time-based
analysis.

``` r
# Check missing Year values
sum(is.na(df$Year))
```

    ## [1] 271

``` r
# Convert Year to numeric
df$Year <- as.numeric(df$Year)

# Impute with median
df$Year[is.na(df$Year)] <- median(df$Year, na.rm = TRUE)

# Verify
sum(is.na(df$Year))
```

    ## [1] 0

### Step 2: Handle Unknown Publishers

Replace “Unknown” in `Publisher` with `NA` and impute with the mode
(most frequent publisher).

``` r
# Check Unknown publishers
sum(df$Publisher == "Unknown", na.rm = TRUE)
```

    ## [1] 203

``` r
# Replace Unknown with NA
df$Publisher[df$Publisher == "Unknown"] <- NA

# Calculate mode publisher
mode_publisher <- names(sort(table(df$Publisher), decreasing = TRUE))[1]

# Impute NA with mode
df$Publisher[is.na(df$Publisher)] <- mode_publisher

# Verify
sum(df$Publisher == "Unknown", na.rm = TRUE)
```

    ## [1] 0

``` r
sum(is.na(df$Publisher))
```

    ## [1] 0

### Step 3: Remove Duplicates

Remove exact duplicates to avoid overcounting.

``` r
# Check duplicates
sum(duplicated(df))
```

    ## [1] 0

``` r
# Remove duplicates
df <- df[!duplicated(df), ]

# Verify
sum(duplicated(df))
```

    ## [1] 0

``` r
dim(df)
```

    ## [1] 16598    11

### Step 4: Add Log-Transformed Global Sales

Add `Global_Sales_log` using `log1p` to handle skewness for
visualizations.

``` r
# Add log-transformed Global_Sales
df$Global_Sales_log <- log1p(df$Global_Sales)

# Preview
summary(df$Global_Sales_log)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ## 0.00995 0.05827 0.15700 0.30651 0.38526 4.42772

### Step 5: Convert Categorical Columns to Factors

Convert `Platform`, `Genre`, and `Publisher` to factors for grouping in
analysis.

``` r
# Convert to factors
df$Platform <- as.factor(df$Platform)
df$Genre <- as.factor(df$Genre)
df$Publisher <- as.factor(df$Publisher)

# Verify
str(df)
```

    ## 'data.frame':    16598 obs. of  12 variables:
    ##  $ Rank            : int  1 2 3 4 5 6 7 8 9 10 ...
    ##  $ Name            : chr  "Wii Sports" "Super Mario Bros." "Mario Kart Wii" "Wii Sports Resort" ...
    ##  $ Platform        : Factor w/ 31 levels "2600","3DO","3DS",..: 26 12 26 26 6 6 5 26 26 12 ...
    ##  $ Year            : num  2006 1985 2008 2009 1996 ...
    ##  $ Genre           : Factor w/ 12 levels "Action","Adventure",..: 11 5 7 11 8 6 5 4 5 9 ...
    ##  $ Publisher       : Factor w/ 577 levels "10TACLE Studios",..: 368 368 368 368 368 368 368 368 368 368 ...
    ##  $ NA_Sales        : num  41.5 29.1 15.8 15.8 11.3 ...
    ##  $ EU_Sales        : num  29.02 3.58 12.88 11.01 8.89 ...
    ##  $ JP_Sales        : num  3.77 6.81 3.79 3.28 10.22 ...
    ##  $ Other_Sales     : num  8.46 0.77 3.31 2.96 1 0.58 2.9 2.85 2.26 0.47 ...
    ##  $ Global_Sales    : num  82.7 40.2 35.8 33 31.4 ...
    ##  $ Global_Sales_log: num  4.43 3.72 3.61 3.53 3.48 ...

### Step 6: Handle Missing Sales Values

Impute missing sales values with 0, assuming no sales recorded.

``` r
# Check missing sales
colSums(is.na(df[, c("NA_Sales", "EU_Sales", "JP_Sales", "Other_Sales", "Global_Sales")]))
```

    ##     NA_Sales     EU_Sales     JP_Sales  Other_Sales Global_Sales 
    ##            0            0            0            0            0

``` r
# Impute with 0
df$NA_Sales[is.na(df$NA_Sales)] <- 0
df$EU_Sales[is.na(df$EU_Sales)] <- 0
df$JP_Sales[is.na(df$JP_Sales)] <- 0
df$Other_Sales[is.na(df$Other_Sales)] <- 0
df$Global_Sales[is.na(df$Global_Sales)] <- 0

# Verify
colSums(is.na(df[, c("NA_Sales", "EU_Sales", "JP_Sales", "Other_Sales", "Global_Sales")]))
```

    ##     NA_Sales     EU_Sales     JP_Sales  Other_Sales Global_Sales 
    ##            0            0            0            0            0

### Step 7: Save Cleaned Dataset

Save the cleaned dataset as `vgsales_cleaned.csv`.

``` r
# Save cleaned dataset
write.csv(df, "vgsales_cleaned.csv", row.names = FALSE)

# Confirm file creation
file.exists("vgsales_cleaned.csv")
```

    ## [1] TRUE

## Final Data Inspection

Verify the cleaned dataset’s structure and completeness.

``` r
# Dimensions
dim(df)
```

    ## [1] 16598    12

``` r
# Structure
str(df)
```

    ## 'data.frame':    16598 obs. of  12 variables:
    ##  $ Rank            : int  1 2 3 4 5 6 7 8 9 10 ...
    ##  $ Name            : chr  "Wii Sports" "Super Mario Bros." "Mario Kart Wii" "Wii Sports Resort" ...
    ##  $ Platform        : Factor w/ 31 levels "2600","3DO","3DS",..: 26 12 26 26 6 6 5 26 26 12 ...
    ##  $ Year            : num  2006 1985 2008 2009 1996 ...
    ##  $ Genre           : Factor w/ 12 levels "Action","Adventure",..: 11 5 7 11 8 6 5 4 5 9 ...
    ##  $ Publisher       : Factor w/ 577 levels "10TACLE Studios",..: 368 368 368 368 368 368 368 368 368 368 ...
    ##  $ NA_Sales        : num  41.5 29.1 15.8 15.8 11.3 ...
    ##  $ EU_Sales        : num  29.02 3.58 12.88 11.01 8.89 ...
    ##  $ JP_Sales        : num  3.77 6.81 3.79 3.28 10.22 ...
    ##  $ Other_Sales     : num  8.46 0.77 3.31 2.96 1 0.58 2.9 2.85 2.26 0.47 ...
    ##  $ Global_Sales    : num  82.7 40.2 35.8 33 31.4 ...
    ##  $ Global_Sales_log: num  4.43 3.72 3.61 3.53 3.48 ...

``` r
# Missing values
plot_missing(df)
```

![](video-game-sales-data-cleaning-in-R_files/figure-gfm/final-inspection-1.png)<!-- -->
