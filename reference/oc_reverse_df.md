# Reverse geocoding with data frames

Reverse geocoding from latitude and longitude pairs to the names and
addresses of a location.

## Usage

``` r
oc_reverse_df(...)

# S3 method for class 'data.frame'
oc_reverse_df(
  data,
  latitude,
  longitude,
  bind_cols = TRUE,
  output = c("short", "all"),
  language = NULL,
  min_confidence = NULL,
  roadinfo = FALSE,
  no_annotations = TRUE,
  no_dedupe = FALSE,
  abbrv = FALSE,
  address_only = FALSE,
  ...
)

# S3 method for class 'numeric'
oc_reverse_df(
  latitude,
  longitude,
  output = c("short", "all"),
  language = NULL,
  min_confidence = NULL,
  no_annotations = TRUE,
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

- latitude, longitude:

  Unquoted variable names of numeric columns or vectors of latitude and
  longitude values.

- bind_cols:

  When `bind_col = TRUE`, the default, the results are column bound to
  `data`. When `FALSE`, the results are returned as a new tibble.

- output:

  A character vector of length one indicating whether only the formatted
  address (`"short"`, the default) or all variables (`"all"`) variables
  should be returned.

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

- min_confidence:

  Numeric vector of integer values, or an unquoted variable name of such
  a vector, between 0 and 10 indicating the precision of the returned
  result as defined by its geographical extent, (i.e. by the extent of
  the result's bounding box). See the [API
  documentation](https://opencagedata.com/api#confidence) for details.
  Only results with at least the requested confidence will be returned.
  Default is `NULL`).

- roadinfo:

  Logical vector, or an unquoted variable name of such a vector,
  indicating whether the geocoder should attempt to match the nearest
  road (rather than an address) and provide additional road and driving
  information. Default is `FALSE`.

- no_annotations:

  Logical vector, or an unquoted variable name of such a vector,
  indicating whether additional information about the result location
  should be returned. `TRUE` by default, which means that the results
  will not contain annotations.

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

[`oc_reverse()`](https://docs.ropensci.org/opencage/reference/oc_reverse.md)
for inputs as vectors, or
[`oc_forward()`](https://docs.ropensci.org/opencage/reference/oc_forward.md)
and
[`oc_forward()`](https://docs.ropensci.org/opencage/reference/oc_forward.md)
for forward geocoding. For more information about the API and the
various parameters, see the [OpenCage API
documentation](https://opencagedata.com/api).

## Examples

``` r
if (FALSE) { # oc_key_present() && oc_api_ok()

library(tibble)
df <- tibble(
  id = 1:4,
  lat = c(-36.85007, 47.21864, 53.55034, 34.05369),
  lng = c(174.7706, -1.554136, 10.000654, -118.242767)
)

# Return formatted address of lat/lng values
oc_reverse_df(df, latitude = lat, longitude = lng)

# Return more detailed information about the locations
oc_reverse_df(df,
  latitude = lat, longitude = lng,
  output = "all"
)

# Return results in a preferred language if possible
oc_reverse_df(df,
  latitude = lat, longitude = lng,
  language = "fr"
)

# oc_reverse_df accepts unquoted column names for all
# arguments except bind_cols and output.
# This makes it possible to build up more detailed queries
# through the data frame passed to the data argument.

df2 <- add_column(df,
  language = c("en", "fr", "de", "en"),
  confidence = c(8, 10, 10, 10)
)

# Use language column to specify preferred language of results
# and confidence column to allow different confidence levels
oc_reverse_df(df2,
  latitude = lat, longitude = lng,
  language = language,
  min_confidence = confidence
)
}
```
