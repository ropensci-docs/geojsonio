# Read geojson or other formats from a local file or a URL

Read geojson or other formats from a local file or a URL

## Usage

``` r
geojson_read(
  x,
  parse = FALSE,
  what = "list",
  stringsAsFactors = FALSE,
  query = NULL,
  ...
)
```

## Arguments

- x:

  (character) Path to a local file or a URL.

- parse:

  (logical) To parse geojson to data.frame like structures if possible.
  Default: `FALSE`

- what:

  (character) What to return. One of "list", "sp" (for Spatial class),
  or "json". Default: "list". "list" "and" sp run through package sf. if
  "json", returns json as character class

- stringsAsFactors:

  Convert strings to Factors? Default `FALSE`.

- query:

  (character) A SQL query, see also
  [postgis](https://docs.ropensci.org/geojsonio/reference/postgis.md)

- ...:

  Further args passed on to
  [`sf::st_read()`](https://r-spatial.github.io/sf/reference/st_read.html)

## Value

various, depending on what's chosen in `what` parameter

- list: geojson as a list using
  [`jsonlite::fromJSON()`](https://jeroen.r-universe.dev/jsonlite/reference/fromJSON.html)

- sp: geojson as an sp class object using
  [`sf::st_read()`](https://r-spatial.github.io/sf/reference/st_read.html)

- json: geojson as character string, to parse downstream as you wish

## Details

This function supports various geospatial file formats from a URL, as
well as local kml, shp, and geojson file formats.

## File size

We previously used
[`file_to_geojson()`](https://docs.ropensci.org/geojsonio/reference/file_to_geojson.md)
in this function, leading to file size problems; this should no longer
be a concern, but let us know if you run into file size problems

## See also

[`topojson_read()`](https://docs.ropensci.org/geojsonio/reference/topojson_read.md),
[`geojson_write()`](https://docs.ropensci.org/geojsonio/reference/geojson_write.md)
[postgis](https://docs.ropensci.org/geojsonio/reference/postgis.md)

## Examples

``` r
if (FALSE) { # \dontrun{
# From a file
file <- system.file("examples", "california.geojson", package = "geojsonio")
(out <- geojson_read(file))
geojson_read(file)

# From a URL
url <- "https://raw.githubusercontent.com/glynnbird/usstatesgeojson/master/california.geojson"
geojson_read(url)
geojson_read(url, parse = TRUE)

# Use as.location first if you want
geojson_read(as.location(file))

# output a SpatialClass object
## read kml
file <- system.file("examples", "norway_maple.kml", package = "geojsonio")
geojson_read(as.location(file), what = "sp")
## read geojson
file <- system.file("examples", "california.geojson", package = "geojsonio")
geojson_read(as.location(file), what = "sp")
## read geojson from a url
url <- "https://raw.githubusercontent.com/glynnbird/usstatesgeojson/master/california.geojson"
geojson_read(url, what = "sp")
## read from a shape file
file <- system.file("examples", "bison.zip", package = "geojsonio")
dir <- tempdir()
unzip(file, exdir = dir)
shpfile <- list.files(dir, pattern = ".shp", full.names = TRUE)
geojson_read(shpfile, what = "sp")

x <- "https://raw.githubusercontent.com/johan/world.geo.json/master/countries.geo.json"
geojson_read(x, what = "sp")
geojson_read(x, what = "list")

utils::download.file(x, destfile = basename(x))
geojson_read(basename(x), what = "sp")

# from a Postgres database - your Postgres instance must be running
## MAKE SURE to run the setup in the postgis manual file first!
if (requireNamespace("DBI") && requireNamespace("RPostgres")) {
  library(DBI)
  conn <- tryCatch(dbConnect(RPostgres::Postgres(), dbname = "postgistest"),
    error = function(e) e
  )
  if (inherits(conn, "PqConnection")) {
    state <- "SELECT row_to_json(fc)
   FROM (SELECT 'FeatureCollection' As type, array_to_json(array_agg(f)) As features
   FROM (SELECT 'Feature' As type
     , ST_AsGeoJSON(lg.geog)::json As geometry
     , row_to_json((SELECT l FROM (SELECT loc_id, loc_name) As l
       )) As properties
    FROM locations As lg   ) As f )  As fc;"
    json <- geojson_read(conn, query = state, what = "json")
    map_leaf(json)
  }
}
} # }
```
