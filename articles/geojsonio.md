# geojsonio vignette

`geojsonio` converts geographic data to geojson and topojson formats.
Nothing else. We hope to do this one job very well, and handle all
reasonable use cases.

Functions in this package are organized first around what you’re working
with or want to get, geojson or topojson, then convert to or read from
various formats:

- [`geojson_list()`](https://docs.ropensci.org/geojsonio/reference/geojson_list.md)/[`topojson_list()`](https://docs.ropensci.org/geojsonio/reference/topojson_list.md) -
  convert to GeoJSON/TopoJSON as R list format
- [`geojson_json()`](https://docs.ropensci.org/geojsonio/reference/geojson_json.md)/[`topojson_json()`](https://docs.ropensci.org/geojsonio/reference/topojson_json.md) -
  convert to GeoJSON/TopoJSON as JSON
- [`geojson_sp()`](https://docs.ropensci.org/geojsonio/reference/geojson_sp.md) -
  convert output of
  [`geojson_list()`](https://docs.ropensci.org/geojsonio/reference/geojson_list.md)
  or
  [`geojson_json()`](https://docs.ropensci.org/geojsonio/reference/geojson_json.md)
  to spatial objects
- [`geojson_read()`](https://docs.ropensci.org/geojsonio/reference/geojson_read.md)/[`topojson_read()`](https://docs.ropensci.org/geojsonio/reference/topojson_read.md) -
  read a GeoJSON/TopoJSON file from file path or URL
- [`geojson_write()`](https://docs.ropensci.org/geojsonio/reference/geojson_write.md)/[`topojson_write()`](https://docs.ropensci.org/geojsonio/reference/topojson_write.md) -
  write a GeoJSON/TopoJSON file locally

Each of the above functions have methods for various objects/classes,
including `numeric`, `data.frame`, `list`, `SpatialPolygons`,
`SpatialLines`, `SpatialPoints`, etc.

Additional functions:

- [`map_gist()`](https://docs.ropensci.org/geojsonio/reference/map_gist.md) -
  push up a geojson or topojson file as a GitHub gist (renders as an
  interactive map) - See the *maps with geojsonio* vignette.
- [`map_leaf()`](https://docs.ropensci.org/geojsonio/reference/map_leaf.md) -
  create a local interactive map with the `leaflet` package - See the
  *maps with geojsonio* vignette.

## Install

Install rgdal - in case you can’t get it installed from binary , here’s
what works on a Mac (change to the version of `rgdal` and `GDAL` you
have).

``` r

install.packages("http://cran.r-project.org/src/contrib/rgdal_1.1-3.tar.gz", repos = NULL, type="source", configure.args = "--with-gdal-config=/Library/Frameworks/GDAL.framework/Versions/1.11/unix/bin/gdal-config --with-proj-include=/Library/Frameworks/PROJ.framework/unix/include --with-proj-lib=/Library/Frameworks/PROJ.framework/unix/lib")
```

Stable version from CRAN

``` r

install.packages("geojsonio")
```

Development version from GitHub

``` r

remotes::install_github("ropensci/geojsonio")
```

``` r

library("geojsonio")
```

## GeoJSON

### Convert various formats to geojson

From a `numeric` vector of length 2

as *json*

``` r

geojson_json(c(32.45, -99.74))
#> <FeatureCollection> 
#>   type:  FeatureCollection 
#>   no. features:  1 
#>   features (1st 5):  Point
```

as a **list**

``` r

geojson_list(c(32.45, -99.74))
#> $type
#> [1] "FeatureCollection"
#> 
#> $features
#> $features[[1]]
#> $features[[1]]$type
#> [1] "Feature"
#> 
#> $features[[1]]$geometry
#> $features[[1]]$geometry$type
...
```

From a `data.frame`

as **json**

``` r

library('maps')
data(us.cities)
geojson_json(us.cities[1:2, ], lat = 'lat', lon = 'long')
#> <FeatureCollection> 
#>   type:  FeatureCollection 
#>   no. features:  2 
#>   features (1st 5):  Point, Point
```

as a **list**

``` r

geojson_list(us.cities[1:2, ], lat = 'lat', lon = 'long')
#> $type
#> [1] "FeatureCollection"
#> 
#> $features
#> $features[[1]]
#> $features[[1]]$type
#> [1] "Feature"
#> 
#> $features[[1]]$geometry
#> $features[[1]]$geometry$type
...
```

From `SpatialPolygons` class

``` r

library('sp')
poly1 <- Polygons(list(Polygon(cbind(c(-100,-90,-85,-100),
  c(40,50,45,40)))), "1")
poly2 <- Polygons(list(Polygon(cbind(c(-90,-80,-75,-90),
  c(30,40,35,30)))), "2")
sp_poly <- SpatialPolygons(list(poly1, poly2), 1:2)
```

to **json**

``` r

geojson_json(sp_poly)
#> <FeatureCollection> 
#>   type:  FeatureCollection 
#>   no. features:  2 
#>   features (1st 5):  Polygon, Polygon
```

to a **list**

``` r

geojson_list(sp_poly)
#> $type
#> [1] "FeatureCollection"
#> 
#> $name
#> [1] "file8936407b162"
#> 
#> $features
#> $features[[1]]
#> $features[[1]]$type
#> [1] "Feature"
...
```

From `SpatialPoints` class

``` r

x <- c(1, 2, 3, 4, 5)
y <- c(3, 2, 5, 1, 4)
s <- SpatialPoints(cbind(x, y))
```

to **json**

``` r

geojson_json(s)
#> <FeatureCollection> 
#>   type:  FeatureCollection 
#>   no. features:  5 
#>   features (1st 5):  Point, Point, Point, Point, Point
```

to a **list**

``` r

geojson_list(s)
#> $type
#> [1] "FeatureCollection"
#> 
#> $name
#> [1] "file89333aaad07"
#> 
#> $features
#> $features[[1]]
#> $features[[1]]$type
#> [1] "Feature"
...
```

### Write geojson

``` r

library('maps')
data(us.cities)
geojson_write(us.cities[1:2, ], lat = 'lat', lon = 'long')
#> <geojson-file>
#>   Path:       myfile.geojson
#>   From class: data.frame
```

### Read geojson

``` r

library("sp")
file <- system.file("examples", "california.geojson", package = "geojsonio")
out <- geojson_read(file, what = "sp")
plot(out)
```

![](../reference/figures/unnamed-chunk-14-1.png)

## Topojson

To JSON

``` r

topojson_json(c(-99.74,32.45))
#> {"type":"Topology","objects":{"foo":{"type":"GeometryCollection","geometries":[{"type":"Point","coordinates":[-99.74,32.45]}]}},"arcs":[],"bbox":[-99.74,32.45,-99.74,32.45]}
```

To a list

``` r

library(sp)
x <- c(1,2,3,4,5)
y <- c(3,2,5,1,4)
s <- SpatialPoints(cbind(x,y))
topojson_list(s)
#> $type
#> [1] "Topology"
#> 
#> $objects
#> $objects$foo
#> $objects$foo$type
#> [1] "GeometryCollection"
#> 
#> $objects$foo$geometries
#> $objects$foo$geometries[[1]]
#> $objects$foo$geometries[[1]]$type
#> [1] "Point"
#> 
#> $objects$foo$geometries[[1]]$coordinates
#> [1] 1 3
#> 
#> $objects$foo$geometries[[1]]$properties
#> $objects$foo$geometries[[1]]$properties$dat
#> [1] 1
#> 
...
```

Read from a file

``` r

file <- system.file("examples", "us_states.topojson", package = "geojsonio")
out <- topojson_read(file)
summary(out)
#>          id             geometry 
#>  Length   :51   MULTIPOLYGON:51  
#>  N.unique :51   epsg:NA     : 0  
#>  N.blank  : 0                    
#>  Min.nchar: 4                    
#>  Max.nchar:20
```

Read from a URL

``` r

url <- "https://raw.githubusercontent.com/shawnbot/d3-cartogram/master/data/us-states.topojson"
out <- topojson_read(url)
```

Or use
[`as.location()`](https://docs.ropensci.org/geojsonio/reference/as.location.md)
first

``` r

(loc <- as.location(file))
#> <location> 
#>    Type:  file 
#>    Location:  /github/home/R/x86_64-pc-linux-gnu-library/4.6/geojsonio/examples/us_states.topojson
out <- topojson_read(loc)
```
