# Reverse geocoding

Reverse geocoding from numeric vectors of latitude and longitude pairs
to the names and addresses of a location.

## Usage

``` r
oc_reverse(
  latitude,
  longitude,
  return = c("df_list", "json_list", "geojson_list", "url_only"),
  language = NULL,
  min_confidence = NULL,
  no_annotations = TRUE,
  roadinfo = FALSE,
  no_dedupe = FALSE,
  abbrv = FALSE,
  address_only = FALSE,
  add_request = FALSE,
  ...
)
```

## Arguments

- latitude, longitude:

  Numeric vectors of latitude and longitude values.

- return:

  A character vector of length one indicating the return value of the
  function, either a list of tibbles (`df_list`, the default), a JSON
  list (`json_list`), a GeoJSON list (`geojson_list`), or the URL with
  which the API would be called (`url_only`).

- language:

  An [IETF BCP 47 language
  tag](https://en.wikipedia.org/wiki/IETF_language_tag) (such as "es"
  for Spanish or "pt-BR" for Brazilian Portuguese). OpenCage will
  attempt to return results in that language. Alternatively you can
  specify the "native" tag, in which case OpenCage will attempt to
  return the response in the "official" language(s). In case the
  `language` parameter is set to `NULL` (which is the default), the tag
  is not recognized, or OpenCage does not have a record in that
  language, the results will be returned in English.

- min_confidence:

  Numeric vector of integer values between 0 and 10 indicating the
  precision of the returned result as defined by its geographical
  extent, (i.e. by the extent of the result's bounding box). See the
  [API documentation](https://opencagedata.com/api#confidence) for
  details. Only results with at least the requested confidence will be
  returned. Default is `NULL`.

- no_annotations:

  Logical vector indicating whether additional information about the
  result location should be returned. `TRUE` by default, which means
  that the results will not contain annotations.

- roadinfo:

  Logical vector indicating whether the geocoder should attempt to match
  the nearest road (rather than an address) and provide additional road
  and driving information. Default is `FALSE`.

- no_dedupe:

  Logical vector (default `FALSE`), when `TRUE` the results will not be
  deduplicated.

- abbrv:

  Logical vector (default `FALSE`), when `TRUE` addresses in the
  `formatted` field of the results are abbreviated (e.g. "Main St."
  instead of "Main Street").

- address_only:

  Logical vector (default `FALSE`), when `TRUE` only the address details
  are returned in the `formatted` field of the results, not the name of
  a point-of-interest should there be one at this address.

- add_request:

  Logical vector (default `FALSE`) indicating whether the request is
  returned again with the results. If the `return` value is a `df_list`,
  the query text is added as a column to the results. `json_list`
  results will contain all request parameters, including the API key
  used! This is currently ignored by OpenCage if return value is
  `geojson_list`.

- ...:

  Ignored.

## Value

Depending on the `return` argument, `oc_reverse` returns a list with
either

- the results as tibbles (`"df_list"`, the default),

- the results as JSON specified as a list (`"json_list"`),

- the results as GeoJSON specified as a list (`"geojson_list"`), or

- the URL of the OpenCage API call for debugging purposes
  (`"url_only"`).

When the results are returned as (a list of) tibbles, the column names
coming from the OpenCage API are prefixed with `"oc_"`.

## See also

[`oc_reverse_df()`](https://docs.ropensci.org/opencage/reference/oc_reverse_df.md)
for inputs as a data frame, or
[`oc_forward()`](https://docs.ropensci.org/opencage/reference/oc_forward.md)
and
[`oc_forward()`](https://docs.ropensci.org/opencage/reference/oc_forward.md)
for forward geocoding. For more information about the API and the
various parameters, see the [OpenCage API
documentation](https://opencagedata.com/api).

## Examples

``` r
if (FALSE) { # oc_key_present() && oc_api_ok()

# Reverse geocode a single location
oc_reverse(latitude = -36.85007, longitude = 174.7706)

# Reverse geocode multiple locations
lat <- c(47.21864, 53.55034, 34.05369)
lng <- c(-1.554136, 10.000654, -118.242767)

oc_reverse(latitude = lat, longitude = lng)

# Return results in a preferred language if possible
oc_reverse(
  latitude = lat, longitude = lng,
  language = "fr"
)

# Return results as a json list
oc_reverse(
  latitude = lat, longitude = lng,
  return = "json_list"
)
}
```
