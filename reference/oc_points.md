# List of points for OpenCage queries

Create a list of points (latitude/longitude coordinate pairs) for
OpenCage queries.

## Usage

``` r
oc_points(...)

# S3 method for class 'numeric'
oc_points(latitude, longitude, ...)

# S3 method for class 'data.frame'
oc_points(data, latitude, longitude, ...)
```

## Arguments

- ...:

  Ignored.

- latitude, longitude:

  Numeric vectors of latitude and longitude values.

- data:

  A `data.frame` containing at least 2 columns with `latitude` and
  `longitude` values.

## Value

A list of points. Each point is a named vector of length 2 containing a
latitude/longitude coordinate pair.

## Examples

``` r
oc_points(-21.01404, 55.26077)
#> [[1]]
#>  latitude longitude 
#> -21.01404  55.26077 
#> 

xdf <-
  data.frame(
    place = c("Hamburg", "Los Angeles"),
    lat = c(53.5503, 34.0536),
    lon = c(10.0006, -118.2427)
  )
oc_points(
  data = xdf,
  latitude = lat,
  longitude = lon
)
#> [[1]]
#>  latitude longitude 
#>   53.5503   10.0006 
#> 
#> [[2]]
#>  latitude longitude 
#>   34.0536 -118.2427 
#> 

# create a list column with points with dplyr
library(dplyr)
xdf %>%
  mutate(
    points =
      oc_points(
        lat,
        lon
      )
  )
#>         place     lat       lon             points
#> 1     Hamburg 53.5503   10.0006   53.5503, 10.0006
#> 2 Los Angeles 34.0536 -118.2427 34.0536, -118.2427
```
