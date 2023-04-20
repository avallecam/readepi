
<!-- README.md is generated from README.Rmd. Please edit that file -->

# readepi

*readepi* provides functions for importing data into **R** from files
and common *health information systems*.

<!-- badges: start -->

[![License:
MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![R-CMD-check](https://github.com/epiverse-trace/readepi/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/epiverse-trace/readepi/actions/workflows/R-CMD-check.yaml)
[![Codecov test
coverage](https://codecov.io/gh/epiverse-trace/readepi/branch/main/graph/badge.svg)](https://app.codecov.io/gh/epiverse-trace/readepi?branch=main)
[![lifecycle-concept](https://raw.githubusercontent.com/reconverse/reconverse.github.io/master/images/badge-maturing.svg)](https://www.reconverse.org/lifecycle.html#concept)
<!-- badges: end -->

## Installation

You can install the development version of readepi from
[GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
# devtools::install_github("epiverse-trace/readepi@develop", build_vignettes = TRUE)
suppressPackageStartupMessages(library(readepi))
```

## Importing data in R

The `readepi()` function allows importing data from several file types
and database management systems. These include:

- all file formats in the
  [rio](https://cran.r-project.org/web/packages/rio/vignettes/rio.html)
  package.  
- file formats that are not accounted for by `rio`  
- relational database management systems (RDBMS) such as **REDCap**,
  **MS SQL server**, **DHIS2**  
- Fingertips (repository of public health indicators in England)

### Importing data from files

``` r
# READING DATA FROM JSON file
file <- system.file("extdata", "test.json", package = "readepi")
data <- readepi(file.path = file)

# IMPORTING DATA FROM THE SECOND EXCEL SHEET
file <- system.file("extdata", "test.xlsx", package = "readepi")
data <- readepi(file.path = file, which = "Sheet2")

# IMPORTING DATA FROM THE FIRST AND SECOND EXCEL SHEETS
file <- system.file("extdata", "test.xlsx", package = "readepi")
data <- readepi(file.path = file, which = c("Sheet2", "Sheet1"))
```

### Importing data from several files in a directory

``` r
# READING ALL FILES IN A GIVEN DIRECTORY
dir.path <- system.file("extdata", package = "readepi")
data <- readepi(file.path = dir.path)

# READING ONLY '.txt' FILES
data <- readepi(file.path = dir.path, pattern = ".txt")

# READING '.txt' and '.xlsx' FILES
data <- readepi(file.path = dir.path, pattern = c(".txt", ".xlsx"))
```

### Importing data from DBMS

This requires the users to:

1.  install the MS SQL driver that is compatible with your SQL server
    version. Details about this installation process can be found in the
    **vignette**.  
2.  create a credentials file where the user credential details will be
    stored. Use the `show_example_file()` to see a template of this
    file.

``` r
# DISPLAY THE STRUCTURE OF THE CREDENTIALS FILE
show_example_file()

# DEFINING THE CREDENTIALS FILE
credentials.file <- system.file("extdata", "test.ini", package = "readepi")

# READING ALL FIELDS AND RECORDS FROM A REDCap PROJECT
data <- readepi(
  credentials.file = credentials.file,
  project.id = "TEST_REDCap"
)
project.data <- data$data
project.metadeta <- data$metadata

# DISPLAY THE LIST OF ALL TABLES IN A DATABASE HOSTED IN A MySQL SERVER
# for the test MySQL server, the driver name does not need to be specified
show_tables(
  credentials.file = credentials.file,
  project.id = "Rfam",
  driver.name = ""
)

# READING ALL FIELDS AND ALL RECORDS FROM A DATABASE HOSTED BY A MS SQL SERVER
data <- readepi(
  credentials.file = credentials.file,
  project.id = "Rfam", # this is the database name
  driver.name = "",
  table.name = "author"
)

# READING DATA FROM DHIS2
data <- readepi(
  credentials.file = credentials.file,
  project.id = "DHIS2_DEMO",
  dataset = "pBOMPrpg1QX",
  organisation.unit = "DiszpKrYNg8",
  data.element.group = NULL,
  start.date = "2014",
  end.date = "2023"
)

# READING FROM FINFERTIPS
data = readepi(indicator_id=90362, 
               area_type_id=202, 
               parent_area_type_id=6  # optional
               )
```

## Vignette

The vignette of the **readepi** contains detailed illustrations about
the used of each function. This can be accessed by typing the command
below:

``` r
browseVignettes("readepi")
```

## Development

### Lifecycle

This package is currently a *concept*, as defined by the [RECON software
lifecycle](https://www.reconverse.org/lifecycle.html). This means that
essential features and mechanisms are still being developed, and the
package is not ready for use outside of the development team.

### Contributions

Contributions are welcome via [pull
requests](https://github.com/epiverse-trace/readepi/pulls).

Contributors to the project include:

- Karim Mané (author)
- Thibaut Jombart (author)

### Code of Conduct

Please note that the linelist project is released with a [Contributor
Code of
Conduct](https://contributor-covenant.org/version/2/0/CODE_OF_CONDUCT.html).
By contributing to this project, you agree to abide by its terms.
