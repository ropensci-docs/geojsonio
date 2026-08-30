# Changelog

## geojsonio (development version)

## geojsonio 0.11.3

CRAN release: 2023-09-06

- Removed an unused V8 call to geojsonhint

- Removed all remaining references to rgeos

## geojsonio 0.11.2

CRAN release: 2023-08-21

- This is a tiny patch release with no user-facing changes.

- Added `\alias{geojsonio-package}` to `man/geojsonio.rd`.

## geojsonio 0.11.1

CRAN release: 2023-05-16

- Removed references to geojsonlint as that package nears retirement.

## geojsonio 0.11.0

CRAN release: 2023-03-08

### Breaking changes

- SpatialPolygonsDataFrame inputs are now cast to sf before being
  written out to fix ring ordering. This was previously handled by
  maptools, which is being retired. Outputs are visually identical, but
  underlying representations may have changed.
- Functions relying on rgeos (such as those writing rgeos objects to
  file) are defunct. PRs to replace these using the newer geos package
  are welcomed.

### New features

- [`topojson_write()`](https://docs.ropensci.org/geojsonio/reference/topojson_write.md)
  has been restored, and now supports conversion to topoJSON formats.
  User reports indicate output files might be larger than anticipated;
  PRs to address this are welcomed. Huge thanks to
  [@Shaunson26](https://github.com/Shaunson26) for this PR!

### Other changes

- The rgeos and maptools packages have been removed from Imports.
  (Thanks to Roger Bivand and Mike Sumner on Mastodon!)

## geojsonio 0.10.0

CRAN release: 2022-10-07

- Deprecated (with a warning) functions relying on rgeos. These will
  stop working in 2023.
- Tests have been updated to use testthat 3e, plus a number of other
  improvements around isolating tests and improving test quality. HUGE
  thanks to [@czeildi](https://github.com/czeildi) for tackling this in
  two massive PRs
  ([\#187](https://github.com/ropensci/geojsonio/issues/187),
  [\#186](https://github.com/ropensci/geojsonio/issues/186),
  [\#183](https://github.com/ropensci/geojsonio/issues/183)).

## geojsonio 0.9.5

CRAN release: 2022-09-13

## geojsonio 0.9.4

CRAN release: 2021-01-13

#### BUG FIXES

- fix for [`sprintf()`](https://rdrr.io/r/base/sprintf.html) usage
  within the
  [`projections()`](https://docs.ropensci.org/geojsonio/reference/projections.md)
  function; only run sprintf on a particular string if it has length \>
  0 ([\#172](https://github.com/ropensci/geojsonio/issues/172))
- fix for
  [`as.json()`](https://docs.ropensci.org/geojsonio/reference/as.json.md)
  when the input is the output of
  [`topojson_list()`](https://docs.ropensci.org/geojsonio/reference/topojson_list.md) -
  we weren’t constructing the TopoJSON arcs correctly
  ([\#160](https://github.com/ropensci/geojsonio/issues/160))
- fix to
  [`geojson_read()`](https://docs.ropensci.org/geojsonio/reference/geojson_read.md):
  now using package `geojsonsf` to read geojson
  ([\#163](https://github.com/ropensci/geojsonio/issues/163))

## geojsonio 0.9.2

CRAN release: 2020-04-07

#### BUG FIXES

- fix a test for change in `stringsAsFactors` behavior in R v4
  ([\#166](https://github.com/ropensci/geojsonio/issues/166))
  ([\#167](https://github.com/ropensci/geojsonio/issues/167))
- temporarily make
  [`topojson_write()`](https://docs.ropensci.org/geojsonio/reference/topojson_write.md)
  defunct until we can sort out issues with new sf version
  ([\#168](https://github.com/ropensci/geojsonio/issues/168))

## geojsonio 0.9.0

CRAN release: 2020-02-13

#### NEW FEATURES

- [`geojson_sf()`](https://docs.ropensci.org/geojsonio/reference/geojson_sf.md)
  and
  [`geojson_sp()`](https://docs.ropensci.org/geojsonio/reference/geojson_sp.md)
  now accept strings in addition to `json`, `geoson_list` and
  `geojson_json` types
  ([\#164](https://github.com/ropensci/geojsonio/issues/164))

#### MINOR IMPROVEMENTS

- [`topojson_json()`](https://docs.ropensci.org/geojsonio/reference/topojson_json.md)
  and
  [`topojson_list()`](https://docs.ropensci.org/geojsonio/reference/topojson_list.md)
  gain params `object_name` and `quantization` to pass through to
  [`geojson_json()`](https://docs.ropensci.org/geojsonio/reference/geojson_json.md)
  ([\#158](https://github.com/ropensci/geojsonio/issues/158))
- replace httr with crul
  ([\#105](https://github.com/ropensci/geojsonio/issues/105))
- rgdal replaced with sf throughout the package; all `writeOGR` replaced
  with `st_write` and `readOGR` with `st_read`; this should not create
  any user facing changes, but please let us know if you have problems
  with this version
  ([\#41](https://github.com/ropensci/geojsonio/issues/41))
  ([\#150](https://github.com/ropensci/geojsonio/issues/150))
  ([\#157](https://github.com/ropensci/geojsonio/issues/157))

## geojsonio 0.8.0

CRAN release: 2019-10-29

### NEW FEATURES

- [`geojson_read()`](https://docs.ropensci.org/geojsonio/reference/geojson_read.md)
  gains new S3 method `geojson_read.PqConnection` for connecting to a
  PostgreSQL database set up with PostGIS. See also
  [`?postgis`](https://docs.ropensci.org/geojsonio/reference/postgis.md)
  for notes on Postgis installation, and setting up some simple data in
  Postgis from this package
  ([\#61](https://github.com/ropensci/geojsonio/issues/61))
  ([\#155](https://github.com/ropensci/geojsonio/issues/155)) thanks to
  [@fxi](https://github.com/fxi)

#### MINOR IMPROVEMENTS

- [`geojson_read()`](https://docs.ropensci.org/geojsonio/reference/geojson_read.md)
  instead of going through package `sp` now goes through package `sf`
  for a significant speed up, see
  <https://github.com/ropensci/geojsonio/issues/136#issuecomment-546123078>
  ([\#136](https://github.com/ropensci/geojsonio/issues/136))
- [`geojson_list()`](https://docs.ropensci.org/geojsonio/reference/geojson_list.md)
  gains parameter `precision` to adjust number of decimal places used.
  only applies to classes from packages sp and rgeos
  ([\#152](https://github.com/ropensci/geojsonio/issues/152)) (related
  to [\#141](https://github.com/ropensci/geojsonio/issues/141)) thanks
  to [@ChrisJones687](https://github.com/ChrisJones687)
- improve dependency installation notes in README
  ([\#149](https://github.com/ropensci/geojsonio/issues/149))
  ([\#151](https://github.com/ropensci/geojsonio/issues/151)) thanks
  [@setgree](https://github.com/setgree) and
  [@nickto](https://github.com/nickto)
- move to using markdown docs
- [`file_to_geojson()`](https://docs.ropensci.org/geojsonio/reference/file_to_geojson.md)
  now using https protocol instead of http for the online ogre service
  called when using `method = "web"`

#### BUG FIXES

- fix
  [`geojson_read()`](https://docs.ropensci.org/geojsonio/reference/geojson_read.md)
  to fail better when using `method="web"`; and update docs to note that
  `method="web"` can result if file size issues, but `method="local"`
  should not have such issues
  ([\#153](https://github.com/ropensci/geojsonio/issues/153))
- change name of `print.location` method to not conflict with `dplyr`
  ([\#154](https://github.com/ropensci/geojsonio/issues/154))

## geojsonio 0.7.0

CRAN release: 2019-04-25

### NEW FEATURES

- [`geo2topo()`](https://docs.ropensci.org/geojsonio/reference/geo2topo.md)
  gains a new parameter `quantization` to quantize geometry prior to
  computing topology. because
  [`topojson_write()`](https://docs.ropensci.org/geojsonio/reference/topojson_write.md)
  uses
  [`geo2topo()`](https://docs.ropensci.org/geojsonio/reference/geo2topo.md)
  internally,
  [`topojson_write()`](https://docs.ropensci.org/geojsonio/reference/topojson_write.md)
  also gains a `quantization` parameter that’s passed to
  [`geo2topo()`](https://docs.ropensci.org/geojsonio/reference/geo2topo.md)
  internally ([\#138](https://github.com/ropensci/geojsonio/issues/138))
  thanks [@pvictor](https://github.com/pvictor)

#### MINOR IMPROVEMENTS

- use package `sf` instead of `sp` in
  [`topojson_read()`](https://docs.ropensci.org/geojsonio/reference/topojson_read.md).
  note that the return object is now class sf instead of classes from
  the sp package
  ([\#144](https://github.com/ropensci/geojsonio/issues/144))
  ([\#145](https://github.com/ropensci/geojsonio/issues/145))
- the `type` parameter in
  [`topojson_json()`](https://docs.ropensci.org/geojsonio/reference/topojson_json.md)
  now set to `type="auto"` if the input is an sf/sfc/sfg class object
  ([\#139](https://github.com/ropensci/geojsonio/issues/139))
  ([\#146](https://github.com/ropensci/geojsonio/issues/146))
- fix to `geojson_list.sfc()` for changes in sf \>= v0.7, which names
  geometries, but that’s not valid geojson
  ([\#142](https://github.com/ropensci/geojsonio/issues/142))

#### DEPRECATED AND DEFUNCT

- The two linting functions in this package, `lint()` and `validate()`
  are now defunct. They have been marked as deprecated since `v0.2`. See
  the package `geojsonlint` on CRAN for linting geojson functionality
  ([\#135](https://github.com/ropensci/geojsonio/issues/135))
  ([\#147](https://github.com/ropensci/geojsonio/issues/147))

## geojsonio 0.6.0

CRAN release: 2018-03-30

### NEW FEATURES

- [`topojson_write()`](https://docs.ropensci.org/geojsonio/reference/topojson_write.md)
  gains a new parameter `object_name`. With it you can set the name for
  the resulting TopoJSON object name created. As part of this
  [`geo2topo()`](https://docs.ropensci.org/geojsonio/reference/geo2topo.md)
  also gains a new parameter, similarly called `object_name`, that does
  the same thing as for
  [`topojson_write()`](https://docs.ropensci.org/geojsonio/reference/topojson_write.md).
  ([\#129](https://github.com/ropensci/geojsonio/issues/129)) (thanks
  [@josiekre](https://github.com/josiekre)) PR
  ([\#131](https://github.com/ropensci/geojsonio/issues/131))
- As part of PR
  ([\#132](https://github.com/ropensci/geojsonio/issues/132)) we added a
  new function
  [`geojson_sf()`](https://docs.ropensci.org/geojsonio/reference/geojson_sf.md)
  to convert output of
  [`geojson_list()`](https://docs.ropensci.org/geojsonio/reference/geojson_list.md)
  or
  [`geojson_json()`](https://docs.ropensci.org/geojsonio/reference/geojson_json.md)
  to `sf` package classes - as an analog to
  [`geojson_sp()`](https://docs.ropensci.org/geojsonio/reference/geojson_sp.md)

#### MINOR IMPROVEMENTS

- [`geojson_json()`](https://docs.ropensci.org/geojsonio/reference/geojson_json.md)
  gains option with the `type` parameter to skip a coercion to the
  `geojson` package class `geoclass`. Using `type = "skip"` you can skip
  the `geoclass` class coercion, which in some cases with large datasets
  should have performance improvements
  ([\#128](https://github.com/ropensci/geojsonio/issues/128)) PR
  ([\#133](https://github.com/ropensci/geojsonio/issues/133))

#### BUG FIXES

- A bug arose in
  [`geojson_sp()`](https://docs.ropensci.org/geojsonio/reference/geojson_sp.md)
  with the newest version of `rgdal`. This was resolved by using the
  `sf` package instead to read GeoJSON. This had a knock-on benefit of
  speeding up reading GeoJSON. In addition, `sf` is now in `Imports`
  instead of `Suggests`
  ([\#130](https://github.com/ropensci/geojsonio/issues/130)) PR
  ([\#132](https://github.com/ropensci/geojsonio/issues/132))

## geojsonio 0.5.0

CRAN release: 2017-11-10

#### NEW FEATURES

- gains new function `geojson_atomize` to “atomize” a FeatureCollection
  into its features, or a GeometryCollection into its geometries
  ([\#120](https://github.com/ropensci/geojsonio/issues/120)) via
  ([\#119](https://github.com/ropensci/geojsonio/issues/119)) thx
  [@SymbolixAU](https://github.com/SymbolixAU)
- gains new functions `topojson_list` and `topojson_json` for converting
  many input types with spatial data to TopoJSON, both as lists and as
  JSON ([\#117](https://github.com/ropensci/geojsonio/issues/117))
- `geojson_json` uses brief output provided by the `geojson` package -
  this makes it less frustrating when you have an especially large
  geojson string that prints to console - this instead prints a brief
  summary of the GeoJSON object
  ([\#86](https://github.com/ropensci/geojsonio/issues/86))
  ([\#124](https://github.com/ropensci/geojsonio/issues/124))

#### MINOR IMPROVEMENTS

- doing a much more thorough job of cleaning up temp files that are
  necessarily generated due to having to go to disk sometimes
  ([\#122](https://github.com/ropensci/geojsonio/issues/122))
- @ateucher made improvements to `geojson_json` to make `type` parameter
  more flexible
  ([\#125](https://github.com/ropensci/geojsonio/issues/125))

#### BUG FIXES

- Fixe bug in `topojson_write` - we were writing topojson file, but also
  a geojson file - we now cleanup the geojson file
  ([\#127](https://github.com/ropensci/geojsonio/issues/127))

## geojsonio 0.4.2

CRAN release: 2017-09-01

#### BUG FIXES

- Fix package so that we load `topojson-server.js` from within the
  package instead of from the web. This makes it so that the package
  doesn’t make any web requests on load, which prevented package from
  loading when no internet connection available.
  ([\#118](https://github.com/ropensci/geojsonio/issues/118))

## geojsonio 0.4.0

CRAN release: 2017-08-20

#### NEW FEATURES

- Gains new functions `geo2topo`, `topo2geo`, `topojson_write`, and
  `topojson_read` for working with TopoJSON data - associated with this,
  we now import `geojson` package
  ([\#24](https://github.com/ropensci/geojsonio/issues/24))
  ([\#100](https://github.com/ropensci/geojsonio/issues/100))

#### MINOR IMPROVEMENTS

- Updated vignette with details on the GeoJSON specification to the new
  specification at <https://www.rfc-editor.org/rfc/rfc7946>
  ([\#114](https://github.com/ropensci/geojsonio/issues/114))

## geojsonio 0.3.8

CRAN release: 2017-07-24

#### MINOR IMPROVEMENTS

- `geojson_write` and `geojson_json` now pass `...` argument through to
  `rgdal::writeOGR` or
  [`jsonlite::toJSON`](https://jeroen.r-universe.dev/jsonlite/reference/fromJSON.html)
  depending on the class/method. For those methods that use the latter,
  this now allows setting of the `na` argument to control how `NA`
  values are represented in json, and the `pretty` argument to control
  whether or the resulting json is pretty-formated or compact
  ([\#109](https://github.com/ropensci/geojsonio/issues/109))
  ([\#111](https://github.com/ropensci/geojsonio/issues/111))
- Spelling/grammar fixes, thanks [@patperu](https://github.com/patperu)
  ! ([\#106](https://github.com/ropensci/geojsonio/issues/106))

#### BUG FIXES

- `geojson_json` and `geojson_write` now convert unsupported classes to
  their basic class before conversion and/or writing to geojson. This
  was most commonly occurring with fields in `sf` objects calculated by
  [`sf::st_area`](https://r-spatial.github.io/sf/reference/geos_measures.html)
  and
  [`sf::st_length`](https://r-spatial.github.io/sf/reference/geos_measures.html)
  which were of class `units`.
  ([\#107](https://github.com/ropensci/geojsonio/issues/107))
- Fixed a bug occurring with `GDAL` version \>= 2.2.0 where the layer
  name in a geojson file was not detected properly
  ([\#108](https://github.com/ropensci/geojsonio/issues/108))

## geojsonio 0.3.2

CRAN release: 2017-02-06

#### BUG FIXES

- Fix to tests for internal fxn `convert_wgs84` to do minimal test of
  output, and to conditionally test only if `sf` is available
  ([\#103](https://github.com/ropensci/geojsonio/issues/103))

## geojsonio 0.3.0

CRAN release: 2017-01-27

#### NEW FEATURES

- `geojson_json`, `geojson_list`, and `geojson_write` gain new S3
  methods: `sf`, `sfc`, and `sfg` - the three classes in the `sf`
  package ([\#95](https://github.com/ropensci/geojsonio/issues/95))
- `geojson_json`, `geojson_list`, and `geojson_write` gain two new
  parameters each: `convert_wgs84` (boolean) to convert to WGS84 or not
  (the projection assumed for GeoJSON) and `crs` to assign a CRS if
  known ([\#101](https://github.com/ropensci/geojsonio/issues/101))
  ([\#102](https://github.com/ropensci/geojsonio/issues/102))

#### MINOR IMPROVEMENTS

- [`geojson_json()`](https://docs.ropensci.org/geojsonio/reference/geojson_json.md)
  for non-sp classes now only keeps seven decimal places in the
  coordinates. This follows the default that GDAL uses.
- Now namespacing base package calls for `methods`/`stats`/`utils`
  instead of importing them
- Improved documentation for `method` parameter in `geojson_read`
  clarifying what the options are for
  ([\#93](https://github.com/ropensci/geojsonio/issues/93)) thanks
  [@bhaskarvk](https://github.com/bhaskarvk)
- Internal fxn `to_json` now defaults to 7 digits, which is used in
  `as.json` and `geojson_json`
  ([\#96](https://github.com/ropensci/geojsonio/issues/96))

#### BUG FIXES

- Fix to `geojson_read` to read correctly from a URL - in addition to
  file paths ([\#91](https://github.com/ropensci/geojsonio/issues/91))
  ([\#92](https://github.com/ropensci/geojsonio/issues/92)) thanks
  [@lecy](https://github.com/lecy)
- Fix to `geojson_read` to read non-`.geojson` extensions
  ([\#93](https://github.com/ropensci/geojsonio/issues/93)) thanks
  [@bhaskarvk](https://github.com/bhaskarvk)

## geojsonio 0.2.0

CRAN release: 2016-07-14

#### MINOR IMPROVEMENTS

- Major performance improvement for
  [`geojson_json()`](https://docs.ropensci.org/geojsonio/reference/geojson_json.md) -
  moved to reading in json with
  [`readr::read_file()`](https://readr.tidyverse.org/reference/read_file.html)
  ([\#85](https://github.com/ropensci/geojsonio/issues/85)) thanks
  [@javrucebo](https://github.com/javrucebo) !
- Now requiring explicit versions of some package dependencies
- Removed the startup message

#### BUG FIXES

- Changed
  [`file_to_geojson()`](https://docs.ropensci.org/geojsonio/reference/file_to_geojson.md)
  to use
  [`httr::write_disk()`](https://httr.r-lib.org/reference/write_disk.html)
  instead of
  [`download.file()`](https://rdrr.io/r/utils/download.file.html)
  ([\#83](https://github.com/ropensci/geojsonio/issues/83)) thanks
  [@patperu](https://github.com/patperu)

#### DEPRECATED AND DEFUNCT

- The two linting functions in this package, `lint()` and `validate()`
  are now deprecated, and will be defunct in the next version of this
  package. See the new package `geojsonlint` on CRAN for linting geojson
  functionality
  ([\#82](https://github.com/ropensci/geojsonio/issues/82))

## geojsonio 0.1.8

CRAN release: 2016-04-10

#### NEW FEATURES

- New method `geojson_sp.json()` added to
  [`geojson_sp()`](https://docs.ropensci.org/geojsonio/reference/geojson_sp.md)
  to handle json class inputs

#### MINOR IMPROVEMENTS

- Added `encodin="UTF-8"` to
  [`httr::content()`](https://httr.r-lib.org/reference/content.html)
  calls

#### BUG FIXES

- [`geojson_write()`](https://docs.ropensci.org/geojsonio/reference/geojson_write.md)
  didn’t overwrite existing files despite saying so. New parameter added
  to the function `overwrite` to specify whether to overwrite a function
  or not, which defaults to `TRUE`
  ([\#81](https://github.com/ropensci/geojsonio/issues/81)) thanks
  [@Robinlovelace](https://github.com/Robinlovelace) !

## geojsonio 0.1.6

CRAN release: 2016-01-15

#### NEW FEATURES

- New function
  [`geojson_sp()`](https://docs.ropensci.org/geojsonio/reference/geojson_sp.md)
  to convert output of
  [`geojson_list()`](https://docs.ropensci.org/geojsonio/reference/geojson_list.md)
  or
  [`geojson_json()`](https://docs.ropensci.org/geojsonio/reference/geojson_json.md)
  to spatial classes (e.g., `SpatialPointsDataFrame`)
  ([\#71](https://github.com/ropensci/geojsonio/issues/71))

#### MINOR IMPROVEMENTS

- Startup message added to notify users to ideally update to
  `rgdal > v1.1-1` given fix to make writing multipolygon objects to
  geojson correct
  ([\#69](https://github.com/ropensci/geojsonio/issues/69))
- Filled out test suite more
  ([\#46](https://github.com/ropensci/geojsonio/issues/46))

#### BUG FIXES

- Fix to `lint()` function, due to bug in passing data to the Javascript
  layer ([\#73](https://github.com/ropensci/geojsonio/issues/73))
- Fixes to
  [`as.json()`](https://docs.ropensci.org/geojsonio/reference/as.json.md)
  ([\#76](https://github.com/ropensci/geojsonio/issues/76))

## geojsonio 0.1.4

CRAN release: 2015-08-12

#### NEW FEATURES

- New function
  [`map_leaf()`](https://docs.ropensci.org/geojsonio/reference/map_leaf.md)
  uses the `leaflet` package to make maps, with S3 methods for most
  spatial classes as well as most R classes, including data.frame’s,
  lists, vectors, file inputs, and more
  ([\#48](https://github.com/ropensci/geojsonio/issues/48))
- [`geojson_read()`](https://docs.ropensci.org/geojsonio/reference/geojson_read.md)
  now optionally can give back a spatial class object, just a
  convenience in case you want to not get back geojson, but a spatial
  class ([\#60](https://github.com/ropensci/geojsonio/issues/60))

#### MINOR IMPROVEMENTS

- Now that `leaflet` R package is on CRAN, put back in examples using it
  to make maps ([\#49](https://github.com/ropensci/geojsonio/issues/49))
- Added a linter for list inputs comined with `geometry="polygon"` to
  all `geojson_*()` functions that have `.list` methods. This checks to
  make sure inputs have the same first and last coordinate pairs to
  close the polygon
  ([\#34](https://github.com/ropensci/geojsonio/issues/34))

#### BUG FIXES

- Importing all non-base R funtions, including from `methods`, `stats`
  and `utils` packages
  ([\#62](https://github.com/ropensci/geojsonio/issues/62))
- Fixed bug in
  [`geojson_write()`](https://docs.ropensci.org/geojsonio/reference/geojson_write.md)
  in which geojson style names were altered on accident
  ([\#56](https://github.com/ropensci/geojsonio/issues/56))

## geojsonio 0.1.0

CRAN release: 2015-04-30

#### NEW FEATURES

- released to CRAN
