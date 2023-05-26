# timeAndPlate

This [R](https://www.R-project.org/) package simplifies reformatting excel files with multiple plate readouts into a format where each column corresponds to the measurements from a well. The wells in the output file can be grouped (sorted) by plate-row (`A1, A2, A3, ..., B1, B2, ...`), or by plate-column (`A1, B1, C1, ..., A2, B2, ...`). This can be useful when the plate reader measures the same plate many times, and each readout is provided in the layout of the plate (rather than grouped by well).

Here we present a function (`reformat.plates`) to reformat an excel file containing several plate readouts. The excel file can contain multiple worksheets and/or multiple plates per worksheet. Each plate should have its own row- and column-names, with row-names `A`, `B`, ..., and column-names `1`, `2`, .... 

**Each readout is assumed to be part of a series, with a numeric identifier in the top-left corner of the plate (same row as colunm-names and same column as row-names).**

The measurements from each plate are extracted, and combined per well. Only wells which have at least one data point are included.

The columns of the output data can be ordered by the row or column of the plate. This can be convenient when replicates are placed in columns or in rows.

The collected measurements are returned both as a `data.frame` (invisibly) and written to a new file (either `xlsx` or `csv`).


## Installation

There is only a development version available, which depends on the packages [openxlsx](https://CRAN.R-project.org/package=openxlsx) and [data.table](https://cran.r-project.org/package=data.table). This package can be installed directly from GitHub by

```R
remotes::install_github("bramburger/timeAndPlate")
```

After installation, load the library in the usual way:
```R
library("timeAndPlate")
```

## Usage

There is only a single function: `reformat.plates`, which takes an excel file as described above.

In the simplest form the only input is the filename of the excel file containing the plate readouts, e.g. `reformat.plates("readouts.xlsx")`. This creates a new file `readouts.reformat.xlsx` (i.e. appending `.reformat` to the original filename), where the first column contains the identifier of the plate, and subsequent column is a well with at least one measurement.

The wells of the plate can be ordered by row or by column in the output file. The default is grouping the wells by column (`A1, B1, C1, ..., A2, B2, ...`). Alternatively, wells can be grouped by row (`A1, A2, A3, ..., B1, B2, ...`) by using the `order.by` argument: `reformat.plates("readouts.xlsx", order.by="rows")`.

The name of the output file can also be specified, e.g. `reformat.plates("readouts.xlsx", outfile = "readouts.grouped.xlsx")`.

To output a `csv` file instead of an excel file, you have to specify the output file name with a `.csv` extension, e.g. `reformat.plates("readouts.xlsx", outfile = "readouts.csv")`.

Please beware, if the output file already exists it is overwritten without warning.
