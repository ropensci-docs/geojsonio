# Convert json input to pretty printed output

Convert json input to pretty printed output

## Usage

``` r
pretty(x, indent = 4)
```

## Arguments

- x:

  Input, character string

- indent:

  (integer) Number of spaces to indent

## Details

Only works with json class input. This is a simple wrapper around
[`jsonlite::prettify()`](https://jeroen.r-universe.dev/jsonlite/reference/prettify.html),
so you can easily use that yourself.
