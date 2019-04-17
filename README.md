
<!-- README.md is generated from README.Rmd. Please edit that file -->

# secuTrialR ![travis](https://api.travis-ci.com/SwissClinicalTrialOrganisation/secuTrialR.svg?branch=master)

An R package to handle data from the clinical data management system
(CDMS) [secuTrial](https://www.secutrial.com/en/).

## Installing from github with devtools

``` r
devtools::install_github("SwissClinicalTrialOrganisation/secuTrialR")
```

## Basic usage

``` r
library(secuTrialR)
```

``` r
# prepare path to example export
export_location <- system.file("extdata", "s_export_CSV-xls_BMD.zip",
                               package = "secuTrialR")
# load all export data
sT_export <- load_secuTrial_export(data_dir = export_location)
```

For creating tables, it is often useful to have access to variable
labels. This is simple in secuTrialR.

``` r
labs <- labels_secuTrial(sT_export)
labs[["age"]]
```

    ## [1] "Age"

### Prepare factors

It is often useful to have categorical variables as factors (R knows how
to handle factors). SecuTrialR can prepare factors
    easily.

``` r
factors <- factorize_secuTrial(sT_export)
```

    ## Error in factorize_secuTrial.secuTrialdata(sT_export): Categorical variables are probably factors already (options()$stringsAsFactors == TRUE)
    ## Recommend saving reference values to seperate table in export

This functions loops through each table of the dataset, creating new
factor variables where necessary. The new variables are the same as the
original but with `.factor` appended (i.e. a new variable called
`sex.factor` would be added to the relevant form).
<!-- REFERENCE VALUES NOT EXPORTED FROM THE DATASET -->

For this function to work, reference values must be saved to a seperate
form. An error will be returned with a message suggesting to change the
export. There is an option for this in the secuTrial export. See
[here](https://swissclinicaltrialorganisation.github.io/secuTrial_recipes/export_data/)
for info.

## For contributors

### Testing with devtools

``` r
# run tests
devtools::test("secuTrialR")
# spell check -> will contain some R and secuTrial specific words which is fine
devtools::spell_check("secuTrialR")
```

### Linting with lintr

``` r
# lint the package -> should be clean
library(lintr)
lint_package("secuTrialR", linters = with_defaults(camel_case_linter = NULL,
                                                   object_usage_linter = NULL,
                                                   line_length_linter(125)))
```

### Generating the README file

The README file contains both standard text and interpreted R code. It
must therefore be compiled. Changes should be made in the README.Rmd
file and the file “knited” with R. This is easiest with RStudio, but
other methods are available.

### Guidelines for contributers

In order to contribute to this R package you should fork the main
repository. After you have made your changes please run the
[tests](README.md#testing-with-devtools) and
[lint](README.md#linting-with-lintr) your code as indicated above. If
all tests pass and linting confirms that your coding style conforms you
can send a pull request (PR).  
The PR should have a description to help the reviewer understand what
has been added/changed. New functionalities must be thoroughly
documented, have examples and should be accompanied by at least one
[test](tests/testthat/) to ensure longterm robustness. The PR will only
be reviewed if all travis checks are successful. The person sending the
PR should not be the one merging it.

A depiction of the core functionalities for loading can be found
[here](inst/extdata/secuTrialR.png).
