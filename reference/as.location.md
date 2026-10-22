# Convert a path or URL to a location object.

Convert a path or URL to a location object.

## Usage

``` r
as.location(x, ...)
```

## Arguments

- x:

  Input.

- ...:

  Ignored.

## Examples

``` r
if (FALSE) { # \dontrun{
# A file
file <- system.file("examples", "zillow_or.geojson", package = "geojsonio")
as.location(file)

# A URL
url <- "https://raw.githubusercontent.com/glynnbird/usstatesgeojson/master/california.geojson"
as.location(url)
} # }
```
