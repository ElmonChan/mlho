
<!-- README.md is generated from README.Rmd. Please edit that file -->

# Hello MLHO\!

<!-- badges: start -->

<!-- badges: end -->

MLHO (pronounced as melo) is a thinkin’ Machine Learning framework that
implements iterative sequential representation mining, and feature and
model selection to predict health outcomes.

## Installation

You can install the released version of mlho from
[Github](https://https://github.com/hestiri/mlho) with:

``` r
devtools::install_github("hestiri/mlho")
```

## Data model

To implement MLHO you’ll need 2 tables, which can be extracted from any
clinical CMD. The current examples are based on the i2b2 star schema.

1- a table with outcome labels (called `labeldt`) and patient numbers

|              |        |
| :----------- | :----- |
| patient\_num | label  |
| character    | factor |

2- a patient clinical data table (called `dbmart`) with 3 columns.
Concepts are used as features by MLHO.

|              |             |           |
| :----------- | :---------- | :-------- |
| patient\_num | start\_date | phenx     |
| character    | date        | character |

The column `phenx` contains the entire feature space. In an `i2b2` data
model, for instance, this column is the equivalent of `concept_cd`.

3- a demographic table is optional, but recommended.

|              |           |           |           |
| :----------- | :-------- | :-------- | :-------- |
| patient\_num | age       | gender    | …         |
| character    | character | character | character |

you’ll need to feed the column names of the demographics table (`dems`
table) to the functions.

## MLHO Vanilla implementation

Here we go through the vanilla implementation of mlho using the
synthetic data provided in the package.

The synthetic data `syntheticmass` downloaded and prepared according to
mlho input data model from
[SyntheticMass](https://synthea.mitre.org/downloads), generated by
[SyntheaTM](https://synthetichealth.github.io/synthea/), an open-source
patient population simulation made available by [The MITRE
Corporation](https://health.mitre.org/).

``` r
library(mlho)
data("syntheticmass")
```

here’s how `dbmart` table looks:

``` r
head(dbmart)
#>                            patient_num                        encounter_num
#> 1 8d4c4326-e9de-4f45-9a4c-f8c36bff89ae 6aa37300-d1b4-48e7-a2f8-5e0f70f48f38
#> 2 10339b10-3cd1-4ac3-ac13-ec26728cb592 dae2b7cb-1316-4b78-954f-fa610a6c6d0e
#> 3 f5dcd418-09fe-4a2f-baa0-3da800bd8c3a 7ff86631-0378-4bfc-92ce-1edd697eb18e
#> 4 f5dcd418-09fe-4a2f-baa0-3da800bd8c3a b8f76eba-7795-4dcd-a544-f27ac2ef3d46
#> 5 f5dcd418-09fe-4a2f-baa0-3da800bd8c3a 640837d9-845a-433c-9fad-47426664a69d
#> 6 f5dcd418-09fe-4a2f-baa0-3da800bd8c3a 1923c698-accd-4d70-ba09-e1938f6e96d1
#>       phenx                             DESCRIPTION          start_date
#> 1 169553002 Insertion of subcutaneous contraceptive 2011-04-30 00:26:23
#> 2 430193006   Medication Reconciliation (procedure) 2010-07-27 12:58:08
#> 3 430193006   Medication Reconciliation (procedure) 2010-11-20 03:04:34
#> 4 117015009              Throat culture (procedure) 2011-02-07 03:04:34
#> 5 117015009              Throat culture (procedure) 2011-04-19 03:04:34
#> 6 430193006   Medication Reconciliation (procedure) 2011-11-26 03:04:34
```
