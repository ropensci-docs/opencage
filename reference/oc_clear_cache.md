# Clear the opencage cache

Forget past results and reset the opencage cache.

## Usage

``` r
oc_clear_cache()
```

## Examples

``` r
if (FALSE) { # oc_key_present() && oc_api_ok()

system.time(oc_reverse(latitude = 10, longitude = 10))
system.time(oc_reverse(latitude = 10, longitude = 10))
oc_clear_cache()
system.time(oc_reverse(latitude = 10, longitude = 10))
}
```
