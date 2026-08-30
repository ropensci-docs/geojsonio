# Add together geo_list or json objects

Add together geo_list or json objects

## Usage

``` r
# S3 method for class 'geo_list'
x1 + x2

# S3 method for class 'json'
x1 + x2
```

## Arguments

- x1:

  An object of class `geo_list` or `json`

- x2:

  A component to add to `x1`, of class `geo_list` or `json`

## Details

If the first object is an object of class `geo_list`, you can add
another object of class `geo_list` or of class `json`, and will result
in a `geo_list` object.

If the first object is an object of class `json`, you can add another
object of class `json` or of class `geo_list`, and will result in a
`json` object.

## See also

[`geojson_list()`](https://docs.ropensci.org/geojsonio/reference/geojson_list.md),
[`geojson_json()`](https://docs.ropensci.org/geojsonio/reference/geojson_json.md)

## Examples

``` r
if (FALSE) { # \dontrun{
# geo_list + geo_list
## Note: geo_list is the output type from geojson_list, it's just a list with
## a class attached so we know it's geojson :)
vec <- c(-99.74, 32.45)
a <- geojson_list(vec)
vecs <- list(
  c(100.0, 0.0), c(101.0, 0.0), c(101.0, 1.0),
  c(100.0, 1.0), c(100.0, 0.0)
)
b <- geojson_list(vecs, geometry = "polygon")
a + b

# json + json
c <- geojson_json(c(-99.74, 32.45))
vecs <- list(
  c(100.0, 0.0), c(101.0, 0.0), c(101.0, 1.0),
  c(100.0, 1.0), c(100.0, 0.0)
)
d <- geojson_json(vecs, geometry = "polygon")
c + d
(c + d) %>% pretty()
} # }
```
