# List of bounding boxes for OpenCage queries

Create a list of bounding boxes for OpenCage queries.

## Usage

``` r
oc_bbox(...)

# S3 method for class 'numeric'
oc_bbox(xmin, ymin, xmax, ymax, ...)

# S3 method for class 'data.frame'
oc_bbox(data, xmin, ymin, xmax, ymax, ...)

# S3 method for class 'bbox'
oc_bbox(bbox, ...)
```

## Arguments

- ...:

  Ignored.

- xmin:

  Minimum longitude (also known as `min_lon`, `southwest_lng`, `west`,
  or `left`).

- ymin:

  Minimum latitude (also known as `min_lat`, `southwest_lat`, `south`,
  or `bottom`).

- xmax:

  Maximum longitude (also known as `max_lon`, `northeast_lng`, `east`,
  or `right`).

- ymax:

  Maximum latitude (also known as `max_lat`, `northeast_lat`, `north`,
  or `top`).

- data:

  A `data.frame` containing at least 4 columns with `xmin`, `ymin`,
  `xmax`, and `ymax` values, respectively.

- bbox:

  A `bbox` object, see
  [`sf::st_bbox`](https://r-spatial.github.io/sf/reference/st_bbox.html).

## Value

A list of bounding boxes, each of class `bbox`.

## Examples

``` r
oc_bbox(-5.63160, 51.280430, 0.278970, 51.683979)
#> [[1]]
#>     xmin     ymin     xmax     ymax 
#> -5.63160 51.28043  0.27897 51.68398 
#> attr(,"crs")
#> $epsg
#> [1] 4326
#> 
#> $proj4string
#> [1] "+proj=longlat +datum=WGS84 +no_defs"
#> 
#> attr(,"class")
#> [1] "crs"
#> attr(,"class")
#> [1] "bbox"
#> 

xdf <-
  data.frame(
    place = c("Hamburg", "Hamburg"),
    northeast_lat = c(54.0276817, 42.7397729),
    northeast_lng = c(10.3252805, -78.812825),
    southwest_lat = c(53.3951118, 42.7091669),
    southwest_lng = c(8.1053284, -78.860521)
  )
oc_bbox(
  xdf,
  southwest_lng,
  southwest_lat,
  northeast_lng,
  northeast_lat
)
#> [[1]]
#>      xmin      ymin      xmax      ymax 
#>  8.105328 53.395112 10.325280 54.027682 
#> attr(,"crs")
#> $epsg
#> [1] 4326
#> 
#> $proj4string
#> [1] "+proj=longlat +datum=WGS84 +no_defs"
#> 
#> attr(,"class")
#> [1] "crs"
#> attr(,"class")
#> [1] "bbox"
#> 
#> [[2]]
#>      xmin      ymin      xmax      ymax 
#> -78.86052  42.70917 -78.81283  42.73977 
#> attr(,"crs")
#> $epsg
#> [1] 4326
#> 
#> $proj4string
#> [1] "+proj=longlat +datum=WGS84 +no_defs"
#> 
#> attr(,"class")
#> [1] "crs"
#> attr(,"class")
#> [1] "bbox"
#> 

# create bbox list column with dplyr
library(dplyr)
#> 
#> Attaching package: ‘dplyr’
#> The following objects are masked from ‘package:stats’:
#> 
#>     filter, lag
#> The following objects are masked from ‘package:base’:
#> 
#>     intersect, setdiff, setequal, union
xdf %>%
  mutate(
    bbox =
      oc_bbox(
        southwest_lng,
        southwest_lat,
        northeast_lng,
        northeast_lat
      )
  )
#>     place northeast_lat northeast_lng southwest_lat southwest_lng
#> 1 Hamburg      54.02768      10.32528      53.39511      8.105328
#> 2 Hamburg      42.73977     -78.81283      42.70917    -78.860521
#>                                        bbox
#> 1 8.105328, 53.395112, 10.325280, 54.027682
#> 2  -78.86052, 42.70917, -78.81283, 42.73977

# create bbox list from a simple features bbox
if (requireNamespace("sf", quietly = TRUE)) {
  library(sf)
  bbox <- st_bbox(c(xmin = 16.1, xmax = 16.6, ymax = 48.6, ymin = 47.9),
    crs = 4326
  )
  oc_bbox(bbox)
}
#> Linking to GEOS 3.12.1, GDAL 3.8.4, PROJ 9.4.0; sf_use_s2() is TRUE
#> [[1]]
#> xmin ymin xmax ymax 
#> 16.1 47.9 16.6 48.6 
#> 
```
