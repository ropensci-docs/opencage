# Configure settings

Configure session settings for opencage.

## Usage

``` r
oc_config(
  key = Sys.getenv("OPENCAGE_KEY"),
  rate_sec = getOption("oc_rate_sec", default = 1L),
  no_record = getOption("oc_no_record", default = TRUE),
  show_key = getOption("oc_show_key", default = FALSE),
  ...
)
```

## Arguments

- key:

  Your OpenCage API key as a character vector of length one. Do not pass
  the key directly as a parameter, though. See details.

- rate_sec:

  Numeric vector of length one. Sets the maximum number of requests sent
  to the OpenCage API per second. Defaults to the value set in the
  `oc_rate_sec` option, or, in case that does not exist, to 1L.

- no_record:

  Logical vector of length one. When `TRUE`, OpenCage will not create
  log entries of the queries and will not cache the geocoding requests.
  Defaults to the value set in the `oc_no_record` option, or, in case
  that does not exist, to `TRUE`.

- show_key:

  Logical vector of length one. This is only relevant when debugging
  [`oc_forward()`](https://docs.ropensci.org/opencage/reference/oc_forward.md)
  or
  [`oc_reverse()`](https://docs.ropensci.org/opencage/reference/oc_reverse.md)
  calls with the `return = "url_only"` argument. When `TRUE`, the result
  will show your OpenCage API key in the URL as stored in the
  `OPENCAGE_KEY` environment variable. When not `TRUE`, the API key will
  be replaced with the string `OPENCAGE_KEY`. `show_key` defaults to the
  value set in the `oc_show_key` option, or, in case that does not
  exist, to `FALSE`.

- ...:

  Ignored.

## Set your OpenCage API key

opencage will conveniently retrieve your API key if it is saved in the
environment variable `"OPENCAGE_KEY"`. `oc_config()` will help to set
that environment variable. Do not pass the key directly as a parameter
to the function, though, as you risk exposing it via your script or your
history. There are three safer ways to set your API key instead:

1.  Save your API key as an environment variable in
    [`.Renviron`](https://rdrr.io/r/base/Startup.html) as described in
    [What They Forgot to Teach You About
    R](https://rstats.wtf/r-startup.html#renviron) or [Efficient R
    Programming](https://csgillespie.github.io/efficientR/set-up.html#renviron).
    From there it will be fetched by all functions that call the
    OpenCage API. You do not even have to call `oc_config()` to set your
    key; you can start geocoding right away. If you have the usethis
    package installed, you can edit your
    [`.Renviron`](https://rdrr.io/r/base/Startup.html) with
    [`usethis::edit_r_environ()`](https://usethis.r-lib.org/reference/edit.html).
    We strongly recommend storing your API key in the user-level
    .Renviron, as opposed to the project-level. This makes it less
    likely you will share sensitive information by mistake.

2.  If you use a package like keyring to store your credentials, you can
    safely pass your key in a script with a function call like this
    `oc_config(key = keyring::key_get("opencage"))`.

3.  If you call `oc_config()` in an
    [`base::interactive()`](https://rdrr.io/r/base/interactive.html)
    session and the `OPENCAGE_KEY` environment variable is not set, it
    will prompt you to enter the key in the console.

## Set your OpenCage API rate limit

The rate limit allowed by the API depends on the OpenCage plan you
purchased and ranges from 1 request/sec for the "Free Trial" plan to 15
requests/sec for the "Medium" or "Large" plans, see
<https://opencagedata.com/pricing> for details and up-to-date
information. You can set the rate limit persistently across sessions by
setting an `oc_rate_sec` [option](https://rdrr.io/r/base/options.html)
in your [`.Rprofile`](https://rdrr.io/r/base/Startup.html). If you have
the usethis package installed, you can edit your
[`.Rprofile`](https://rdrr.io/r/base/Startup.html) with
[`usethis::edit_r_profile()`](https://usethis.r-lib.org/reference/edit.html).

## Prevent query logging and caching

By default, OpenCage will store your queries in its server logs and will
cache the forward geocoding requests on their side. They do this in
order to speed up response times and to be able to debug errors and
improve their service. Logs are automatically deleted after six months
according to OpenCage's [page on data protection and
GDPR](https://opencagedata.com/gdpr).

If you set `no_record` to `TRUE`, the query contents are not logged nor
cached. OpenCage still records that you made a request, but not the
specific query you made. `oc_config(no_record = TRUE)` sets the
`oc_no_record` [option](https://rdrr.io/r/base/options.html) for the
active R session, so it will be used for all subsequent OpenCage
queries. You can set the `oc_no_record`
[option](https://rdrr.io/r/base/options.html) persistently across
sessions in your [`.Rprofile`](https://rdrr.io/r/base/Startup.html).

For increased privacy opencage sets `no_record` to `TRUE`, by default.
Please note, however, that opencage always caches the data it receives
from the OpenCage API locally, but only for as long as your R session is
alive.

For more information on OpenCage's policies on privacy and data
protection see [their FAQs](https://opencagedata.com/faq#legal), their
[GDPR page](https://opencagedata.com/gdpr), and, for the `no_record`
parameter, see the relevant [blog
post](https://blog.opencagedata.com/post/145602604628/more-privacy-with-norecord-parameter).
