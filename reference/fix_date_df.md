# Clean up messy date columns

Tidies a `dataframe` or `tibble` object with date columns entered via a
free-text interface, addressing non-standardized formats. Supports
diverse separators including /, -, ., and spaces. Handles all-numeric,
abbreviated, or full-length month names in languages such as English,
French, German, Spanish, Portuguese, Russian, Czech, Slovak, and
Indonesian. Imputes missing day data by default, with flexibility for
custom imputation strategies.

## Usage

``` r
fix_date_df(
  df,
  col.names,
  day.impute = 1,
  month.impute = 7,
  id = NULL,
  format = "dmy",
  excel = FALSE,
  roman.numeral = FALSE,
  cores = getOption("Ncpus", 1)
)
```

## Arguments

- df:

  A `dataframe` or `tibble` object containing messy date column(s).

- col.names:

  Character vector specifying column names of date data to be cleaned.

- day.impute:

  Integer between 1 and 31, or NA, or NULL. Day of the month to be
  imputed when missing. Defaults to 1. If `day.impute` is greater than
  the number of days in a given month, the last day of that month will
  be imputed (accounting for leap years). If `day.impute = NA`, then
  `NA` will be imputed for the entire date and a warning will be raised.
  If `day.impute = NULL`, the function will fail with an error when day
  is missing.

- month.impute:

  Integer between 1 and 12, or NA, or NULL. Month to be imputed when
  missing. Defaults to 7 (July). If `month.impute = NA`, then `NA` will
  be imputed for the entire date and a warning will be raised. If
  `month.impute = NULL`, the function will fail with an error when month
  is missing.

- id:

  Optional parameter specifying the name of the column containing row
  IDs. Defaults to using the first column for IDs.

- format:

  Character string specifying date interpretation preference. Either
  `"dmy"` (day-month-year, default) or `"mdy"` (month-day-year, US
  format). This setting only affects ambiguous numeric dates like
  "01/02/2023". When month names are present or year appears first, the
  format is auto-detected regardless of this parameter. Note that
  unambiguous dates (e.g., "25/12/2023") are parsed correctly regardless
  of the format setting.

- excel:

  Logical: Assumes `FALSE` by default. If `TRUE`, treats numeric-only
  dates with more than four digits as Excel serial dates with 1900-01-01
  origin, correcting for known Excel date discrepancies.

- roman.numeral:

  **\[experimental\]** Logical: Defaults to `FALSE`. When `TRUE`,
  attempts to interpret Roman numeral month indications within datasets.
  This feature may not handle all cases correctly.

- cores:

  Integer: Number of CPU cores to use for parallel processing. Defaults
  to `getOption("Ncpus", 1)`. When `cores > 1`, processes multiple date
  columns in parallel using the `future` framework. Requires the
  `future` and `future.apply` packages to be installed. The actual
  number of workers used will be the minimum of `cores` and the number
  of columns to process.

## Value

A revised `dataframe` or `tibble` structure, maintaining input type.
Date columns will be formatted with `Date` class and display as
`yyyy-mm-dd`.

## Details

This function processes messy date data by:

- Supporting mixed format data entries

- Recognizing multilingual month names and Roman numeral inputs

- Interpreting Excel-style serial date numbers if specified

- Providing warnings and controls for missing day/month imputation

For further details and advanced usage, refer to the vignette via
`browseVignettes("datefixR")` or visit the online documentation at
<https://docs.ropensci.org/datefixR/>.

## See also

[`fix_date_char`](https://docs.ropensci.org/datefixR/reference/fix_date_char.md)
for similar functionality on character vectors.

For comprehensive examples and usage practices, consult:

- Vignette: `browseVignettes("datefixR")`

- Documentation:
  <https://docs.ropensci.org/datefixR/articles/datefixR.html>

- README Overview: <https://docs.ropensci.org/datefixR/>

## Examples

``` r
# Basic cleanup
data(exampledates)
fix_date_df(exampledates, c("some.dates", "some.more.dates"))
#>   id some.dates some.more.dates
#> 1  1 1992-05-02      2015-07-01
#> 2  2 2020-04-01      2000-05-02
#> 3  3 1996-05-01      1990-05-01
#> 4  4 2020-05-01      2012-08-01
#> 5  5 1996-04-02      2020-01-01
#> 6  6 2013-03-03      1977-07-22
#> 7  7 2014-09-07      2007-11-04

# Usage with metadata
messy_dates_df <- data.frame(
  id = seq(1, 3),
  dates = c("1992", "April 1990", "Mar 19")
)
fix_date_df(messy_dates_df, "dates", day.impute = 15, month.impute = 12)
#>   id      dates
#> 1  1 1992-12-15
#> 2  2 1990-04-15
#> 3  3 2019-03-15

# Diverse format normalization
df_formats <- data.frame(
  mixed.dates = c("02/05/92", "2020-may-01", "1996.05.01", "October 2022"),
  european.dates = c("22.07.1977", "05.06.2023")
)
fix_date_df(df_formats, c("mixed.dates", "european.dates"))
#>   mixed.dates european.dates
#> 1  1992-05-02     1977-07-22
#> 2  2020-05-01     2023-06-05
#> 3  1996-05-01     1977-07-22
#> 4  2022-10-01     2023-06-05

# Excel serial examples
serial_df <- data.frame(serial.dates = c("44197", "44927"))
fix_date_df(serial_df, "serial.dates", excel = TRUE)
#>   serial.dates
#> 1   2021-01-01
#> 2   2023-01-01

# Handling Roman numerals
roman_df <- data.frame(roman.dates = c("15.I.2023", "03.XII.2019"))
fix_date_df(roman_df, "roman.dates", roman.numeral = TRUE)
#>   roman.dates
#> 1  2023-01-15
#> 2  2019-12-03

# Parallel processing (requires 'future' and 'future.apply' packages)
if (FALSE) { # \dontrun{
large_df <- data.frame(
  dates1 = c("01/02/2020", "15/03/2021", "22/12/2019"),
  dates2 = c("2020-01-01", "March 2021", "Dec 2019"),
  dates3 = c("01.01.20", "15.03.21", "22.12.19")
)
# Use 4 cores for parallel processing
fix_date_df(large_df, c("dates1", "dates2", "dates3"), cores = 4)

# Use all available cores (respects getOption("Ncpus"))
options(Ncpus = parallel::detectCores())
fix_date_df(large_df, c("dates1", "dates2", "dates3"))
} # }
```
