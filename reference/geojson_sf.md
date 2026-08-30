# Convert objects to an sf class

Convert objects to an sf class

## Usage

``` r
geojson_sf(x, stringsAsFactors = FALSE, ...)
```

## Arguments

- x:

  Object of class `geo_list`, `geo_json`, string, or json

- stringsAsFactors:

  Convert strings to Factors? Default `FALSE`.

- ...:

  Further args passed on to
  [`sf::st_read()`](https://r-spatial.github.io/sf/reference/st_read.html)

## Value

An sf class object, see Details.

## Details

The type of sf object returned will depend on the input GeoJSON.
Sometimes you will get back a `POINTS` class, and sometimes a `POLYGON`
class, etc., depending on what the structure of the GeoJSON.

The reading and writing of the CRS to/from geojson is inconsistent. You
can directly set the CRS by passing a valid PROJ4 string or epsg code to
the crs argument in
[`sf::st_read()`](https://r-spatial.github.io/sf/reference/st_read.html)

## Examples

``` r
if (FALSE) { # \dontrun{
library(sf)

# geo_list ------------------
## From a numeric vector of length 2 to a point
vec <- c(-99.74, 32.45)
geojson_list(vec) %>% geojson_sf()

## Lists
## From a list
mylist <- list(
  list(latitude = 30, longitude = 120, marker = "red"),
  list(latitude = 30, longitude = 130, marker = "blue")
)
geojson_list(mylist) %>% geojson_sf()
geojson_list(mylist) %>%
  geojson_sf() %>%
  plot()

## From a list of numeric vectors to a polygon
vecs <- list(c(100.0, 0.0), c(101.0, 0.0), c(101.0, 1.0), c(100.0, 1.0), c(100.0, 0.0))
geojson_list(vecs, geometry = "polygon") %>% geojson_sf()
geojson_list(vecs, geometry = "polygon") %>%
  geojson_sf() %>%
  plot()

# geo_json ------------------
## from point
geojson_json(c(-99.74, 32.45)) %>% geojson_sf()
geojson_json(c(-99.74, 32.45)) %>%
  geojson_sf() %>%
  plot()

# from featurecollectino of points
geojson_json(us_cities[1:2, ], lat = "lat", lon = "long") %>% geojson_sf()
geojson_json(us_cities[1:2, ], lat = "lat", lon = "long") %>%
  geojson_sf() %>%
  plot()

# Set the CRS via the crs argument
geojson_json(us_cities[1:2, ], lat = "lat", lon = "long") %>% geojson_sf(crs = "+init=epsg:4326")

# json ----------------------
x <- geojson_json(us_cities[1:2, ], lat = "lat", lon = "long")
geojson_sf(x)

# character string ----------------------
x <- unclass(geojson_json(c(-99.74, 32.45)))
geojson_sf(x)
} # }
```
