# Forward geocoding with data frames

Forward geocoding from a column or vector of location names to latitude
and longitude tuples.

## Usage

``` r
oc_forward_df(...)

# S3 method for class 'data.frame'
oc_forward_df(
  data,
  placename,
  bind_cols = TRUE,
  output = c("short", "all"),
  bounds = NULL,
  proximity = NULL,
  countrycode = NULL,
  language = NULL,
  limit = 1L,
  min_confidence = NULL,
  no_annotations = TRUE,
  roadinfo = FALSE,
  no_dedupe = FALSE,
  abbrv = FALSE,
  address_only = FALSE,
  ...
)

# S3 method for class 'character'
oc_forward_df(
  placename,
  output = c("short", "all"),
  bounds = NULL,
  proximity = NULL,
  countrycode = NULL,
  language = NULL,
  limit = 1L,
  min_confidence = NULL,
  no_annotations = TRUE,
  roadinfo = FALSE,
  no_dedupe = FALSE,
  abbrv = FALSE,
  address_only = FALSE,
  ...
)
```

## Arguments

- ...:

  Ignored.

- data:

  A data frame.

- placename:

  An unquoted variable name of a character column or vector with the
  location names or addresses to be geocoded.

  If the locations are addresses, see [OpenCage's
  instructions](https://github.com/OpenCageData/opencagedata-misc-docs/blob/master/query-formatting.md)
  on how to format addresses for best forward geocoding results.

- bind_cols:

  When `bind_col = TRUE`, the default, the results are column bound to
  `data`. When `FALSE`, the results are returned as a new tibble.

- output:

  A character vector of length one indicating whether only latitude,
  longitude, and formatted address variables (`"short"`, the default),
  or all variables (`"all"`) variables should be returned.

- bounds:

  A list of length one, or an unquoted variable name of a list column of
  bounding boxes. Bounding boxes are named numeric vectors, each with 4
  coordinates forming the south-west and north-east corners of the
  bounding box: `list(c(xmin, ymin, xmax, ymax))`. `bounds` restricts
  the possible results to the supplied region. It can be specified with
  the
  [`oc_bbox()`](https://docs.ropensci.org/opencage/reference/oc_bbox.md)
  helper. For example:
  `bounds = oc_bbox(-0.563160, 51.280430, 0.278970, 51.683979)`. Default
  is `NULL`.

- proximity:

  A list of length one, or an unquoted variable name of a list column of
  points. Points are named numeric vectors with latitude, longitude
  coordinate pairs in decimal format. `proximity` provides OpenCage with
  a hint to bias results in favour of those closer to the specified
  location. It can be specified with the
  [`oc_points()`](https://docs.ropensci.org/opencage/reference/oc_points.md)
  helper. For example: `proximity = oc_points(41.40139, 2.12870)`.
  Default is `NULL`.

- countrycode:

  Character vector, or an unquoted variable name of such a vector, of
  two-letter codes as defined by the [ISO 3166-1 Alpha
  2](https://www.iso.org/obp/ui/#search/code) standard that restricts
  the results to the given country or countries. E.g. "AR" for
  Argentina, "FR" for France, "NZ" for the New Zealand. Multiple
  countrycodes per `placename` must be wrapped in a list. Default is
  `NULL`.

- language:

  Character vector, or an unquoted variable name of such a vector, of
  [IETF BCP 47 language
  tags](https://en.wikipedia.org/wiki/IETF_language_tag) (such as "es"
  for Spanish or "pt-BR" for Brazilian Portuguese). OpenCage will
  attempt to return results in that language. Alternatively you can
  specify the "native" tag, in which case OpenCage will attempt to
  return the response in the "official" language(s). In case the
  `language` parameter is set to `NULL` (which is the default), the tag
  is not recognized, or OpenCage does not have a record in that
  language, the results will be returned in English.

- limit:

  Numeric vector of integer values, or an unquoted variable name of such
  a vector, to determine the maximum number of results returned for each
  `placename`. Integer values between 1 and 100 are allowed. Default is
  1.

- min_confidence:

  Numeric vector of integer values, or an unquoted variable name of such
  a vector, between 0 and 10 indicating the precision of the returned
  result as defined by its geographical extent, (i.e. by the extent of
  the result's bounding box). See the [API
  documentation](https://opencagedata.com/api#confidence) for details.
  Only results with at least the requested confidence will be returned.
  Default is `NULL`).

- no_annotations:

  Logical vector, or an unquoted variable name of such a vector,
  indicating whether additional information about the result location
  should be returned. `TRUE` by default, which means that the results
  will not contain annotations.

- roadinfo:

  Logical vector, or an unquoted variable name of such a vector,
  indicating whether the geocoder should attempt to match the nearest
  road (rather than an address) and provide additional road and driving
  information. Default is `FALSE`.

- no_dedupe:

  Logical vector, or an unquoted variable name of such a vector. Default
  is `FALSE`. When `TRUE` the results will not be deduplicated.

- abbrv:

  Logical vector, or an unquoted variable name of such a vector. Default
  is `FALSE`. When `TRUE` addresses in the `oc_formatted` variable of
  the results are abbreviated (e.g. "Main St." instead of "Main
  Street").

- address_only:

  Logical vector, or an unquoted variable name of such a vector. Default
  is `FALSE`. When `TRUE` only the address details are returned in the
  `oc_formatted` variable of the results, not the name of a
  point-of-interest should there be one at this address.

## Value

A tibble. Column names coming from the OpenCage API are prefixed with
`"oc_"`.

## See also

[`oc_forward()`](https://docs.ropensci.org/opencage/reference/oc_forward.md)
for inputs as vectors, or
[`oc_reverse()`](https://docs.ropensci.org/opencage/reference/oc_reverse.md)
and
[`oc_reverse_df()`](https://docs.ropensci.org/opencage/reference/oc_reverse_df.md)
for reverse geocoding. For more information about the API and the
various parameters, see the [OpenCage API
documentation](https://opencagedata.com/api).

## Examples

``` r
if (FALSE) { # oc_key_present() && oc_api_ok()

library(tibble)
df <- tibble(
  id = 1:3,
  locations = c("Nantes", "Hamburg", "Los Angeles")
)

# Return lat, lng, and formatted address
oc_forward_df(df, placename = locations)

# Return more detailed information about the locations
oc_forward_df(df, placename = locations, output = "all")

# Do not column bind results to input data frame
oc_forward_df(df, placename = locations, bind_cols = FALSE)

# Add more results by changing the limit from the default of 1.
oc_forward_df(df, placename = locations, limit = 5)

# Restrict results to a given bounding box
oc_forward_df(df,
  placename = locations,
  bounds = oc_bbox(-5, 45, 15, 55)
)

# oc_forward_df accepts unquoted column names for all
# arguments except bind_cols and output.
# This makes it possible to build up more detailed queries
# through the data frame passed to the data argument.

df2 <- add_column(df,
  bounds = oc_bbox(
    xmin = c(-2, 9, -119),
    ymin = c(47, 53, 34),
    xmax = c(0, 10, -117),
    ymax = c(48, 54, 35)
  ),
  limit = 1:3,
  countrycode = c("ca", "us", "co"),
  language = c("fr", "de", "en")
)

# Use the bounds column to help return accurate results and
# language column to specify preferred language of results
oc_forward_df(df2,
  placename = locations,
  bounds = bounds,
  language = language
)

# Different limit of results for each placename
oc_forward_df(df2,
  placename = locations,
  limit = limit
)

# Specify the desired results by the countrycode column
oc_forward_df(df2,
  placename = locations,
  countrycode = countrycode
)
}
```
