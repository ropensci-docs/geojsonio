# Get bounds for a list or geo_list

Get bounds for a list or geo_list

## Usage

``` r
bounds(x, ...)
```

## Arguments

- x:

  An object of class list or geo_list

- ...:

  Ignored

## Value

A vector of the form min longitude, min latitude, max longitude, max
latitude

## Examples

``` r
# numeric
vec <- c(-99.74, 32.45)
x <- geojson_list(vec)
bounds(x)
#> [1] -99.74  32.45 -99.74  32.45

# list
mylist <- list(
  list(latitude = 30, longitude = 120, marker = "red"),
  list(latitude = 30, longitude = 130, marker = "blue")
)
x <- geojson_list(mylist)
#> Assuming 'longitude' and 'latitude' are longitude and latitude, respectively
bounds(x)
#> [1] 120  30 130  30

# data.frame
x <- geojson_list(states[1:20, ])
#> Assuming 'long' and 'lat' are longitude and latitude, respectively
bounds(x)
#> [1] -88.01778  30.24071 -87.46201  30.38968
```
