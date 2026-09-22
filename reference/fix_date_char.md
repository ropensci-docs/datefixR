# Convert non-standardized dates to R's `Date` class

Converts a character vector (or single character object) from
inconsistently formatted dates to R's `Date` class. Supports numerous
separators including /, -, ., or space. Supports numeric, abbreviation,
or long-hand month notation in multiple languages (English, French,
German, Spanish, Portuguese, Russian, Czech, Slovak, Indonesian). Where
day of the month has not been supplied, the first day of the month is
imputed by default. Either DMY or YMD is assumed by default. However,
the US system of MDY is supported via the `format` argument.

## Usage

``` r
fix_date_char(
  dates,
  day.impute = 1,
  month.impute = 7,
  format = "dmy",
  excel = FALSE,
  roman.numeral = FALSE
)
```

## Arguments

- dates:

  Character vector to be converted to R's date class.

- day.impute:

  Integer between 1 and 31, or NA, or NULL. Day of the month to be
  imputed when missing. Defaults to 1. If `day.impute = NA`, then `NA`
  will be imputed for the date and a warning will be raised. If
  `day.impute = NULL`, the function will fail with an error when day is
  missing.

- month.impute:

  Integer between 1 and 12, or NA, or NULL. Month to be imputed when
  missing. Defaults to 7 (July). If `month.impute = NA`, then `NA` will
  be imputed for the entire date and a warning will be raised. If
  `month.impute = NULL`, the function will fail with an error when month
  is missing.

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

## Value

A vector of elements belonging to R's built in `Date` class with the
following format `yyyy-mm-dd`.

## Details

This function intelligently parses dates by:

- Handling mixed separators within the same dataset

- Recognizing month names in multiple languages

- Converting Roman numeral months (experimental)

- Processing Excel serial date numbers

- Automatically detecting YMD format when year appears first

- Smart imputation of missing date components with user control

For comprehensive examples and advanced usage, see
`browseVignettes("datefixR")` or the package README at
<https://docs.ropensci.org/datefixR/>.

## See also

[`fix_date_df`](https://docs.ropensci.org/datefixR/reference/fix_date_df.md)
for data frame columns with date data.

For detailed examples and usage patterns, see:

- Package vignette: `browseVignettes("datefixR")`

- Online documentation:
  <https://docs.ropensci.org/datefixR/articles/datefixR.html>

- Package README: <https://docs.ropensci.org/datefixR/>

## Examples

``` r
# Basic usage
bad.date <- "02 03 2021"
fix_date_char(bad.date)
#> [1] "2021-03-02"

# Multiple formats with different separators
mixed_dates <- c(
  "02/05/92", # slash separator, 2-digit year
  "2020-may-01", # hyphen separator, text month
  "1996.05.01", # dot separator
  "02 04 96", # space separator
  "le 3 mars 2013" # French format
)
fix_date_char(mixed_dates)
#> [1] "1992-05-02" "2020-05-01" "1996-05-01" "1996-04-02" "2013-03-03"

# Text months in different languages
text_months <- c(
  "15 January 2020", # English
  "15 janvier 2020", # French
  "15 Januar 2020", # German
  "15 enero 2020", # Spanish
  "15 de janeiro de 2020" # Portuguese
)
fix_date_char(text_months)
#> [1] "2020-01-15" "2020-01-15" "2020-01-15" "2020-01-15" "2020-01-15"

# Roman numeral months (experimental)
roman_dates <- c("15.VII.2023", "3.XII.1999", "1.I.2000")
fix_date_char(roman_dates, roman.numeral = TRUE)
#> [1] "2023-07-15" "1999-12-03" "2000-01-01"

# Excel serial numbers
excel_serials <- c("44197", "44927") # Excel dates
fix_date_char(excel_serials, excel = TRUE)
#> [1] "2021-01-01" "2023-01-01"

# Two-digit years (automatic century detection)
two_digit_years <- c("15/03/99", "15/03/25", "15/03/50")
fix_date_char(two_digit_years) # 1999, 2025, 1950
#> [1] "1999-03-15" "2025-03-15" "1950-03-15"

# MDY format (US style)
us_dates <- c("12/25/2023", "07/04/1776", "02/29/2020")
fix_date_char(us_dates, format = "mdy")
#> [1] "2023-12-25" "1776-07-04" "2020-02-29"

# Incomplete dates with custom imputation
incomplete <- c("2023", "March 2022", "June 2021")
fix_date_char(incomplete, day.impute = 15, month.impute = 6)
#> [1] "2023-06-15" "2022-03-15" "2021-06-15"
```
