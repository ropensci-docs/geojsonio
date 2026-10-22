# GeoJSON to TopoJSON and back

GeoJSON to TopoJSON and back

## Usage

``` r
geo2topo(x, object_name = "foo", quantization = 0, ...)

topo2geo(x, ...)
```

## Arguments

- x:

  GeoJSON or TopoJSON as a character string, json, a file path, or url

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

  for `geo2topo` args passed on to
  [`jsonlite::fromJSON()`](https://jeroen.r-universe.dev/jsonlite/reference/fromJSON.html),
  and for `topo2geo` args passed on to
  [`sf::st_read()`](https://r-spatial.github.io/sf/reference/st_read.html)

## Value

An object of class `json`, of either GeoJSON or TopoJSON

## See also

[`topojson_write()`](https://docs.ropensci.org/geojsonio/reference/topojson_write.md),
[`topojson_read()`](https://docs.ropensci.org/geojsonio/reference/topojson_read.md)

## Examples

``` r
# geojson to topojson
x <- '{"type": "LineString", "coordinates": [ [100.0, 0.0], [101.0, 1.0] ]}'
z <- geo2topo(x)
jsonlite::prettify(z)
#> {
#>     "type": "Topology",
#>     "objects": {
#>         "foo": {
#>             "type": "LineString",
#>             "arcs": [
#>                 0
#>             ]
#>         }
#>     },
#>     "arcs": [
#>         [
#>             [
#>                 100,
#>                 0
#>             ],
#>             [
#>                 101,
#>                 1
#>             ]
#>         ]
#>     ],
#>     "bbox": [
#>         100,
#>         0,
#>         101,
#>         1
#>     ]
#> }
#>  
if (FALSE) { # \dontrun{
library(leaflet)
leaflet() %>%
  addProviderTiles(provider = "Stamen.Terrain") %>%
  addTopoJSON(z)
} # }

# geojson to topojson as a list
x <- list(
  '{"type": "LineString", "coordinates": [ [100, 0], [101, 1] ]}',
  '{"type": "LineString", "coordinates": [ [110, 0], [110, 1] ]}',
  '{"type": "LineString", "coordinates": [ [120, 0], [121, 1] ]}'
)
geo2topo(x)
#> [[1]]
#> {"type":"Topology","objects":{"foo":{"type":"LineString","arcs":[0]}},"arcs":[[[100,0],[101,1]]],"bbox":[100,0,101,1]} 
#> 
#> [[2]]
#> {"type":"Topology","objects":{"foo":{"type":"LineString","arcs":[0]}},"arcs":[[[110,0],[110,1]]],"bbox":[110,0,110,1]} 
#> 
#> [[3]]
#> {"type":"Topology","objects":{"foo":{"type":"LineString","arcs":[0]}},"arcs":[[[120,0],[121,1]]],"bbox":[120,0,121,1]} 
#> 

# change the object name created
x <- '{"type": "LineString", "coordinates": [ [100.0, 0.0], [101.0, 1.0] ]}'
geo2topo(x, object_name = "HelloWorld")
#> {"type":"Topology","objects":{"HelloWorld":{"type":"LineString","arcs":[0]}},"arcs":[[[100,0],[101,1]]],"bbox":[100,0,101,1]} 
geo2topo(x, object_name = "4")
#> {"type":"Topology","objects":{"4":{"type":"LineString","arcs":[0]}},"arcs":[[[100,0],[101,1]]],"bbox":[100,0,101,1]} 

x <- list(
  '{"type": "LineString", "coordinates": [ [100, 0], [101, 1] ]}',
  '{"type": "LineString", "coordinates": [ [110, 0], [110, 1] ]}',
  '{"type": "LineString", "coordinates": [ [120, 0], [121, 1] ]}'
)
geo2topo(x, "HelloWorld")
#> [[1]]
#> {"type":"Topology","objects":{"HelloWorld":{"type":"LineString","arcs":[0]}},"arcs":[[[100,0],[101,1]]],"bbox":[100,0,101,1]} 
#> 
#> [[2]]
#> {"type":"Topology","objects":{"HelloWorld":{"type":"LineString","arcs":[0]}},"arcs":[[[110,0],[110,1]]],"bbox":[110,0,110,1]} 
#> 
#> [[3]]
#> {"type":"Topology","objects":{"HelloWorld":{"type":"LineString","arcs":[0]}},"arcs":[[[120,0],[121,1]]],"bbox":[120,0,121,1]} 
#> 
geo2topo(x, c("A", "B", "C"))
#> [[1]]
#> {"type":"Topology","objects":{"A":{"type":"LineString","arcs":[0]}},"arcs":[[[100,0],[101,1]]],"bbox":[100,0,101,1]} 
#> 
#> [[2]]
#> {"type":"Topology","objects":{"B":{"type":"LineString","arcs":[0]}},"arcs":[[[110,0],[110,1]]],"bbox":[110,0,110,1]} 
#> 
#> [[3]]
#> {"type":"Topology","objects":{"C":{"type":"LineString","arcs":[0]}},"arcs":[[[120,0],[121,1]]],"bbox":[120,0,121,1]} 
#> 


# topojson to geojson
w <- topo2geo(z)
#> Loading required namespace: stringi
jsonlite::prettify(w)
#> {
#>     "type": "FeatureCollection",
#>     "features": [
#>         {
#>             "type": "Feature",
#>             "properties": {
#>                 "id": "foo"
#>             },
#>             "geometry": {
#>                 "type": "LineString",
#>                 "coordinates": [
#>                     [
#>                         100,
#>                         0
#>                     ],
#>                     [
#>                         101,
#>                         1
#>                     ]
#>                 ]
#>             }
#>         }
#>     ]
#> }
#>  

## larger examples
file <- system.file("examples", "us_states.topojson", package = "geojsonio")
topo2geo(file)
#> <FeatureCollection> 
#>   type:  FeatureCollection 
#>   no. features:  51 
#>   features (1st 5):  MultiPolygon, MultiPolygon, MultiPolygon, MultiPolygon, MultiPolygon 
```
