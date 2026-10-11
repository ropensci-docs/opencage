# Reverse geocoding

**\[deprecated\]**

Deprecated: use `oc_reverse` or `oc_reverse_df` for reverse geocoding.

## Usage

``` r
opencage_reverse(
  latitude,
  longitude,
  key = opencage_key(),
  bounds = NULL,
  countrycode = NULL,
  language = NULL,
  limit = 10,
  min_confidence = NULL,
  no_annotations = FALSE,
  no_dedupe = FALSE,
  no_record = FALSE,
  abbrv = FALSE,
  add_request = TRUE
)
```

## Arguments

- latitude, longitude:

  Numeric vectors of latitude and longitude values.

- key:

  Your OpenCage API key as a character vector of length one. By default,
  [`opencage_key()`](https://docs.ropensci.org/opencage/reference/opencage_key.md)
  will attempt to retrieve the key from the environment variable
  `OPENCAGE_KEY`.

- bounds:

  Bounding box, ignored for reverse geocoding.

- countrycode:

  Country code, ignored for reverse geocoding.

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

- limit:

  How many results should be returned (1-100), ignored for reverse
  geocoding.

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

- no_dedupe:

  Logical vector (default `FALSE`), when `TRUE` the results will not be
  deduplicated.

- no_record:

  Logical vector of length one (default `FALSE`), when `TRUE` no log
  entry of the query is created, and the geocoding request is not cached
  by OpenCage.

- abbrv:

  Logical vector (default `FALSE`), when `TRUE` addresses in the
  `formatted` field of the results are abbreviated (e.g. "Main St."
  instead of "Main Street").

- add_request:

  Logical vector (default `FALSE`) indicating whether the request is
  returned again with the results. If the `return` value is a `df_list`,
  the query text is added as a column to the results. `json_list`
  results will contain all request parameters, including the API key
  used! This is currently ignored by OpenCage if return value is
  `geojson_list`.

## Value

A list with

- results as a tibble with one line per result,

- the number of results as an integer,

- the timestamp as a POSIXct object,

- rate_info tibble/data.frame with the maximal number of API calls per
  day for the used key, the number of remaining calls for the day and
  the time at which the number of remaining calls will be reset.

## Examples

``` r
if (FALSE) { # oc_key_present() && oc_api_ok()

opencage_reverse(
  latitude = 0, longitude = 0,
  limit = 2
)
}
```
