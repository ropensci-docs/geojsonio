# Convert many input types with spatial data to a geojson file

Convert many input types with spatial data to a geojson file

## Usage

``` r
geojson_write(
  input,
  lat = NULL,
  lon = NULL,
  geometry = "point",
  group = NULL,
  file = "myfile.geojson",
  overwrite = TRUE,
  precision = NULL,
  convert_wgs84 = FALSE,
  crs = NULL,
  ...
)
```

## Arguments

- input:

  Input list, data.frame, spatial class, or sf class. Inputs can also be
  dplyr `tbl_df` class since it inherits from `data.frame`

- lat:

  (character) Latitude name. The default is `NULL`, and we attempt to
  guess.

- lon:

  (character) Longitude name. The default is `NULL`, and we attempt to
  guess.

- geometry:

  (character) One of point (Default) or polygon.

- group:

  (character) A grouping variable to perform grouping for polygons -
  doesn't apply for points

- file:

  (character) A path and file name (e.g., myfile), with the `.geojson`
  file extension. Default writes to current working directory.

- overwrite:

  (logical) Overwrite the file given in `file` with `input`. Default:
  `TRUE`. If this param is `FALSE` and the file already exists, we stop
  with error message.

- precision:

  desired number of decimal places for the coordinates in the geojson
  file. Using fewer decimal places can decrease file sizes (at the cost
  of precision).

- convert_wgs84:

  Should the input be converted to the standard CRS for GeoJSON
  (https://tools.ietf.org/html/rfc7946) (geographic coordinate reference
  system, using the WGS84 datum, with longitude and latitude units of
  decimal degrees; EPSG: 4326). Default is `FALSE` though this may
  change in a future package version. This will only work for `sf` or
  `Spatial` objects with a CRS already defined. If one is not defined
  but you know what it is, you may define it in the `crs` argument
  below.

- crs:

  The CRS of the input if it is not already defined. This can be an epsg
  code as a four or five digit integer or a valid proj4 string. This
  argument will be ignored if `convert_wgs84` is `FALSE` or the object
  already has a CRS.

- ...:

  Further args passed on to internal functions. For Spatial\* classes,
  data.frames, regular lists, and numerics, it is passed through to
  [`sf::st_write()`](https://r-spatial.github.io/sf/reference/st_write.html).
  For sf classes, geo_lists and json classes, it is passed through to
  [`jsonlite::toJSON()`](https://jeroen.r-universe.dev/jsonlite/reference/fromJSON.html).

## Value

A `geojson_write` class, with two elements:

- path: path to the file with the GeoJSON

- type: type of object the GeoJSON came from, e.g., SpatialPoints

## See also

[`geojson_list()`](https://docs.ropensci.org/geojsonio/reference/geojson_list.md),
[`geojson_json()`](https://docs.ropensci.org/geojsonio/reference/geojson_json.md),
[`topojson_write()`](https://docs.ropensci.org/geojsonio/reference/topojson_write.md)

## Examples

``` r
if (FALSE) { # \dontrun{
# From a data.frame
## to points
geojson_write(us_cities[1:2, ], lat = "lat", lon = "long")

## to polygons
head(states)
geojson_write(
  input = states, lat = "lat", lon = "long",
  geometry = "polygon", group = "group"
)

## partial states dataset to points (defaults to points)
geojson_write(input = states, lat = "lat", lon = "long")

## Lists
### list of numeric pairs
poly <- list(
  c(-114.345703125, 39.436192999314095),
  c(-114.345703125, 43.45291889355468),
  c(-106.61132812499999, 43.45291889355468),
  c(-106.61132812499999, 39.436192999314095),
  c(-114.345703125, 39.436192999314095)
)
geojson_write(poly, geometry = "polygon")

### named list
mylist <- list(
  list(latitude = 30, longitude = 120, marker = "red"),
  list(latitude = 30, longitude = 130, marker = "blue")
)
geojson_write(mylist)

# From a numeric vector of length 2
## Expected order is lon, lat
vec <- c(-99.74, 32.45)
geojson_write(vec)

## polygon from a series of numeric pairs
### this requires numeric class input, so inputting a list will
### dispatch on the list method
poly <- c(
  c(-114.345703125, 39.436192999314095),
  c(-114.345703125, 43.45291889355468),
  c(-106.61132812499999, 43.45291889355468),
  c(-106.61132812499999, 39.436192999314095),
  c(-114.345703125, 39.436192999314095)
)
geojson_write(poly, geometry = "polygon")

# Write output of geojson_list to file
res <- geojson_list(us_cities[1:2, ], lat = "lat", lon = "long")
class(res)
geojson_write(res)

# Write output of geojson_json to file
res <- geojson_json(us_cities[1:2, ], lat = "lat", lon = "long")
class(res)
geojson_write(res)

# From SpatialPolygons class
library("sp")
poly1 <- Polygons(list(Polygon(cbind(
  c(-100, -90, -85, -100),
  c(40, 50, 45, 40)
))), "1")
poly2 <- Polygons(list(Polygon(cbind(
  c(-90, -80, -75, -90),
  c(30, 40, 35, 30)
))), "2")
sp_poly <- SpatialPolygons(list(poly1, poly2), 1:2)
geojson_write(sp_poly)

# From SpatialPolygonsDataFrame class
sp_polydf <- as(sp_poly, "SpatialPolygonsDataFrame")
geojson_write(input = sp_polydf)

# From SpatialGrid
x <- GridTopology(c(0, 0), c(1, 1), c(5, 5))
y <- SpatialGrid(x)
geojson_write(y)

# From SpatialGridDataFrame
sgdim <- c(3, 4)
sg <- SpatialGrid(GridTopology(rep(0, 2), rep(10, 2), sgdim))
sgdf <- SpatialGridDataFrame(sg, data.frame(val = 1:12))
geojson_write(sgdf)


# From SpatialPixels
library("sp")
pixels <- suppressWarnings(SpatialPixels(SpatialPoints(us_cities[c("long", "lat")])))
summary(pixels)
geojson_write(pixels)

# From SpatialPixelsDataFrame
library("sp")
pixelsdf <- suppressWarnings(
  SpatialPixelsDataFrame(points = canada_cities[c("long", "lat")], data = canada_cities)
)
geojson_write(pixelsdf)


# From sf classes:
if (require(sf)) {
  file <- system.file("examples", "feature_collection.geojson", package = "geojsonio")
  sf_fc <- st_read(file, quiet = TRUE)
  geojson_write(sf_fc)
}
} # }
```
