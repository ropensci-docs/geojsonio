# **I/O for GeoJSON**

Convert various data formats to/from GeoJSON or TopoJSON. This package
focuses mostly on converting lists, data.frame's, numeric,
SpatialPolygons, SpatialPolygonsDataFrame, and more to GeoJSON with the
help of sf. You can currently read TopoJSON - writing TopoJSON will come
in a future version of this package.

## Package organization

The core functions in this package are organized first around what
you're working with or want to get, GeoJSON or TopoJSON, then convert to
or read from various formats:

- [`geojson_list()`](https://docs.ropensci.org/geojsonio/reference/geojson_list.md)
  /
  [`topojson_list()`](https://docs.ropensci.org/geojsonio/reference/topojson_list.md) -
  convert to GeoJSON or TopoJSON as R list format

- [`geojson_json()`](https://docs.ropensci.org/geojsonio/reference/geojson_json.md)
  /
  [`topojson_json()`](https://docs.ropensci.org/geojsonio/reference/topojson_json.md) -
  convert to GeoJSON or TopoJSON as JSON

- [`geojson_sp()`](https://docs.ropensci.org/geojsonio/reference/geojson_sp.md) -
  convert to a spatial object from `geojson_list` or `geojson_json`

- [`geojson_sf()`](https://docs.ropensci.org/geojsonio/reference/geojson_sf.md) -
  convert to an sf object from `geojson_list` or `geojson_json`

- [`geojson_read()`](https://docs.ropensci.org/geojsonio/reference/geojson_read.md)
  /
  [`topojson_read()`](https://docs.ropensci.org/geojsonio/reference/topojson_read.md) -
  read a GeoJSON/TopoJSON file from file path or URL

- [`geojson_write()`](https://docs.ropensci.org/geojsonio/reference/geojson_write.md)
  /
  [`topojson_write()`](https://docs.ropensci.org/geojsonio/reference/topojson_write.md) -
  write a GeoJSON file locally (TopoJSON coming later)

Other interesting functions:

- [`map_gist()`](https://docs.ropensci.org/geojsonio/reference/map_gist.md) -
  Create a GitHub gist (renders as an interactive map)

- [`map_leaf()`](https://docs.ropensci.org/geojsonio/reference/map_leaf.md) -
  Create a local interactive map using the `leaflet` package

- [`geo2topo()`](https://docs.ropensci.org/geojsonio/reference/geo2topo.md) -
  Convert GeoJSON to TopoJSON

- [`topo2geo()`](https://docs.ropensci.org/geojsonio/reference/geo2topo.md) -
  Convert TopoJSON to GeoJSON

All of the above functions have methods for various classes, including
`numeric` vectors, `data.frame`, `list`, `SpatialPolygons`,
`SpatialLines`, `SpatialPoints`, and many more - which will try to do
the right thing based on the data you give as input.

## See also

Useful links:

- <https://github.com/ropensci/geojsonio>

- <https://docs.ropensci.org/geojsonio/>

- Report bugs at <https://github.com/ropensci/geojsonio/issues>

## Author

Scott Chamberlain

Andy Teucher <andy.teucher@gmail.com>

Michael Mahoney <mike.mahoney.218@gmail.com>
