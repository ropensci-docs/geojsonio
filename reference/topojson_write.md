# Write TopoJSON from various inputs

Write TopoJSON from various inputs

## Usage

``` r
topojson_write(
  input,
  lat = NULL,
  lon = NULL,
  geometry = "point",
  group = NULL,
  file = "myfile.topojson",
  overwrite = TRUE,
  precision = NULL,
  convert_wgs84 = FALSE,
  crs = NULL,
  object_name = "foo",
  quantization = 0,
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

- object_name:

  (character) name to give to the TopoJSON object created. Default:
  "foo"

- quantization:

  (numeric) quantization parameter, use this to quantize geometry prior
  to computing topology. Typical values are powers of ten (`1e4`, `1e5`,
  ...), default is `0` to not perform quantization. For more information
  about quantization, see this by Mike Bostock
  https://stackoverflow.com/questions/18900022/topojson-quantization-vs-simplification/18921214#18921214

- ...:

  Further args passed on to internal functions. For Spatial\* classes,
  data.frames, regular lists, and numerics, it is passed through to
  [`sf::st_write()`](https://r-spatial.github.io/sf/reference/st_write.html).
  For sf classes, geo_lists and json classes, it is passed through to
  [`jsonlite::toJSON()`](https://jeroen.r-universe.dev/jsonlite/reference/fromJSON.html).

## Value

A `topojson_write` class, with two elements:

- path: path to the file with the TopoJSON

- type: type of object the TopoJSON came from, e.g., SpatialPoints

## Details

Under the hood we simply wrap
[`geojson_write()`](https://docs.ropensci.org/geojsonio/reference/geojson_write.md),
then take the GeoJSON output of that operation, then convert to TopoJSON
with
[`geo2topo()`](https://docs.ropensci.org/geojsonio/reference/geo2topo.md),
then write to disk.

Unfortunately, this process requires a number of round trips to disk, so
speed ups will hopefully come soon.

Any intermediate geojson files are cleaned up (deleted).

## See also

[`geojson_write()`](https://docs.ropensci.org/geojsonio/reference/geojson_write.md),
[`topojson_read()`](https://docs.ropensci.org/geojsonio/reference/topojson_read.md)
