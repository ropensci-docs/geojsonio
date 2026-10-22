# Get centroid for a geo_list

Get centroid for a geo_list

## Usage

``` r
centroid(x, ...)
```

## Arguments

- x:

  An object of class geo_list

- ...:

  Ignored

## Value

A vector of the form longitude, latitude

## Examples

``` r
# numeric
vec <- c(-99.74, 32.45)
x <- geojson_list(vec)
centroid(x)
#> [1] -99.74  32.45

# list
mylist <- list(
  list(latitude = 30, longitude = 120, marker = "red"),
  list(latitude = 30, longitude = 130, marker = "blue")
)
x <- geojson_list(mylist)
#> Assuming 'longitude' and 'latitude' are longitude and latitude, respectively
centroid(x)
#> [1] 125  30

# data.frame
x <- geojson_list(states[1:20, ])
#> Assuming 'long' and 'lat' are longitude and latitude, respectively
centroid(x)
#> [1] -87.76539  30.29342
```
