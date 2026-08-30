# Convert inputs to JSON

Convert inputs to JSON

## Usage

``` r
as.json(x, ...)
```

## Arguments

- x:

  Input

- ...:

  Further args passed on to
  [`jsonlite::toJSON()`](https://jeroen.r-universe.dev/jsonlite/reference/fromJSON.html)

## Details

when the output of
[`topojson_list()`](https://docs.ropensci.org/geojsonio/reference/topojson_list.md)
is given to this function we use a special internal fxn `astjl()` to
parse the object - see that fxn and let us know if any problems you run
in to

## Examples

``` r
if (FALSE) { # \dontrun{
(res <- geojson_list(us_cities[1:2, ], lat = "lat", lon = "long"))
as.json(res)
as.json(res, pretty = TRUE)

vec <- c(-99.74, 32.45)
as.json(geojson_list(vec))
as.json(geojson_list(vec), pretty = TRUE)
} # }
```
