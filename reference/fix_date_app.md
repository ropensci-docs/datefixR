# Shiny application standardizing date data in csv or excel files

A shiny application which allows users to standardize dates using a
graphical user interface (GUI). Most features of `datefixR` are
supported including imputing missing date data. Data can be provided as
CSV (comma-separated value) or XLSX (Excel) files. Processed datasets
can be downloaded as CSV files. Please note, the dependencies for this
app (`DT`, `htmltools`, `readxl`, and `shiny`) are not installed
alongside `datefixR`. This allows `datefixR` to be installed on secure
systems where these packages may not be allowed. If one of these
dependencies is not installed on the system when this function is
called, then the user will be given the option of installing them.

## Usage

``` r
fix_date_app(theme = "datefixR")
```

## Arguments

- theme:

  Color theme for shiny app. Either `"datefixR"` (`datefixR` colors) or
  `"none"`(default shiny app styling).

## Value

A shiny app.

## See also

The [`shiny`](https://rdrr.io/pkg/shiny/man/shiny-package.html) package.

## Examples

``` r
if (FALSE) { # \dontrun{
fix_date_app()
} # }
```
