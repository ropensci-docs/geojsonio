# Read topojson from a local file or a URL

Read topojson from a local file or a URL

## Usage

``` r
topojson_read(x, ...)
```

## Arguments

- x:

  Path to a local file or a URL.

- ...:

  Further args passed on to
  [`sf::st_read()`](https://r-spatial.github.io/sf/reference/st_read.html).
  Can use any args from
  [`sf::st_read()`](https://r-spatial.github.io/sf/reference/st_read.html)
  except `quiet`, which we have set as `quiet = TRUE` internally already

## Value

an object of class `sf`/`data.frame`

## Details

Returns a `sf` class, but you can easily and quickly get this to
geojson, see examples.

Note that this does not give you Topojson, but gives you a `sf` class -
which you can use then to turn it into geojson as a list or json

## See also

[`geojson_read()`](https://docs.ropensci.org/geojsonio/reference/geojson_read.md),
[`topojson_write()`](https://docs.ropensci.org/geojsonio/reference/topojson_write.md)

## Examples

``` r
if (FALSE) { # \dontrun{
# From a file
file <- system.file("examples", "us_states.topojson", package = "geojsonio")
topojson_read(file)

# From a URL
url <- "https://raw.githubusercontent.com/shawnbot/d3-cartogram/master/data/us-states.topojson"
topojson_read(url)

# Use as.location first if you want
topojson_read(as.location(file))

# quickly convert to geojson as a list
file <- system.file("examples", "us_states.topojson", package = "geojsonio")
tmp <- topojson_read(file)
geojson_list(tmp)
geojson_json(tmp)

# pass on args
topojson_read(file, quiet = TRUE)
topojson_read(file, stringsAsFactors = FALSE)
} # }
```
