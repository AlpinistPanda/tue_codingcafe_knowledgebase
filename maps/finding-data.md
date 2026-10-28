---
title: Finding data
subtitle: OpenStreetMap, PDOK, CBS and Eindhoven open data, and how to credit each
short_title: Finding data
---

The Netherlands is one of the best-mapped countries in the world, and almost all of it is open.
The hard part is knowing which source has what, and how to get it into your code without clicking
through a portal. This page covers the four sources that will get you through most of the
#30DayMapChallenge, with code that downloads directly.

The code on this page is shown but not run when the book is built, because it depends on outside
services. Every request was tested in October 2026.

## Which source for what

| You want | Go to | Licence |
|---|---|---|
| Amenities, shops, roads, paths, trees, anything people map | **OpenStreetMap** | ODbL 1.0, must credit |
| Building footprints with construction year (BAG) | **PDOK** | CC0 |
| Population, income, age, housing per neighbourhood | **CBS** *Wijken en buurten*, via PDOK | CC BY 4.0 |
| Elevation, at 0.5 m resolution (AHN) | **PDOK** | CC0 |
| Topographic base map layers (BRT, TOP10NL) | **PDOK** | CC BY 4.0 |
| Municipal data: trees, lamp posts, parking, events | **Eindhoven open data** | mostly CC0 or CC BY, per dataset |

Write every source and its licence in your map's README, and put the credit on the map itself.

## OpenStreetMap

OpenStreetMap (OSM) is a map of the world made by volunteers. Everything in it is a **feature**
with **tags**: key=value pairs such as `amenity=cafe`, `highway=cycleway` or `natural=tree`. To
find the tag for what you want, search the [OSM wiki](https://wiki.openstreetmap.org/wiki/Map_features)
or click on a similar object at <https://www.openstreetmap.org> with the query tool (the arrow
with a question mark).

### Overpass Turbo: look before you download

[Overpass Turbo](https://overpass-turbo.eu) runs a query in your browser and shows the result on
a map, so you can check you are getting what you think before writing any code. The *Wizard*
button turns plain text like `amenity=cafe in Eindhoven` into a query. *Export* saves the result
as GeoJSON.

This is the query used to make `data/eindhoven_cafes.geojson`:

```
[out:json][timeout:60];
area["boundary"="administrative"]["admin_level"="8"]["name"="Eindhoven"]["ref:gemeentecode"="0772"]->.gem;
(
  node["amenity"="cafe"](area.gem);
  way["amenity"="cafe"](area.gem);
);
out center tags;
```

Matching on the municipality code (`0772`) as well as the name avoids picking up anything else
called Eindhoven.

### From code

::::{tab-set}
:::{tab-item} Python
```python
# pip install osmnx
import osmnx as ox

trees = ox.features_from_place("Eindhoven, Netherlands", tags={"natural": "tree"})
parks = ox.features_from_place("Eindhoven, Netherlands", tags={"leisure": "park"})

# a street network as a graph, for routing and Day 24 (Network)
G = ox.graph_from_place("Eindhoven, Netherlands", network_type="bike")
nodes, edges = ox.graph_to_gdfs(G)

trees = trees.to_crs(28992)
```

`features_from_place` returns a GeoDataFrame with mixed geometry types (points, lines and
polygons) and one column per tag. Filter on `trees.geom_type` before plotting.
:::
:::{tab-item} R
```r
# install.packages("osmdata")
library(osmdata)
library(sf)

trees <- opq("Eindhoven, Netherlands") |>
  add_osm_feature(key = "natural", value = "tree") |>
  osmdata_sf()

trees_pts <- st_transform(trees$osm_points, 28992)
```

`osmdata_sf()` returns a list with `osm_points`, `osm_lines`, `osm_polygons` and the multi
versions. Pick the one that matches what you asked for.
:::
::::

:::{important} Credit OpenStreetMap on the map
OSM data is under the Open Database License. Any map made from it **must** show
**"© OpenStreetMap contributors"**, on the image itself, not only in the post. If you publish a
dataset derived from OSM, that dataset must stay under ODbL.
:::

:::{warning} What OSM can and cannot tell you
OSM is volunteer-mapped, so it is complete where people care and patchy elsewhere. City centres
are excellent; opening hours, wheelchair access and building heights are hit and miss. A missing
tag means *nobody recorded it*, not *no*. "91 cafés" means "91 things someone tagged
`amenity=cafe`", not an official count.
:::

## PDOK: Dutch government geodata

[PDOK](https://www.pdok.nl) publishes the national base registrations as web services that you
can read straight into GeoPandas or `sf`. The modern ones are **OGC API Features**: a URL that
returns GeoJSON, with filters as query parameters. Browse what is available in the
[PDOK dataset list](https://www.pdok.nl/datasets).

### CBS neighbourhoods with statistics

CBS publishes the boundaries of every municipality (*gemeente*), district (*wijk*) and
neighbourhood (*buurt*), with more than a hundred statistics attached: population, households,
age groups, income, housing, distance to the nearest school or park. It is the best source for
choropleth maps of the Netherlands.

::::{tab-set}
:::{tab-item} Python
```python
import geopandas as gpd

url = ("https://api.pdok.nl/cbs/wijken-en-buurten-2024/ogc/v1/collections/buurten/items"
       "?f=json&limit=1000&gemeentenaam=Eindhoven")
buurten = gpd.read_file(url).to_crs(28992)

len(buurten)                                     # 116 neighbourhoods
buurten[["buurtnaam", "aantal_inwoners", "bevolkingsdichtheid_inwoners_per_km2"]]
```
:::
:::{tab-item} R
```r
library(sf)

url <- paste0("https://api.pdok.nl/cbs/wijken-en-buurten-2024/ogc/v1/collections/buurten/items",
              "?f=json&limit=1000&gemeentenaam=Eindhoven")
buurten <- st_read(url) |> st_transform(28992)
```
:::
::::

Swap `buurten` for `wijken` or `gemeenten`, and the year in the URL for another edition
(2023, 2024 and 2025 are available). Any column can be used as a filter in the same way as
`gemeentenaam`.

:::{warning} CBS uses negative numbers for "unknown"
Where a figure is confidential or not available, CBS stores a negative placeholder such as
`-99995` instead of leaving it empty. Set these to missing before you colour a map by them, or a
handful of neighbourhoods will drag your whole colour scale down:

```python
num = buurten.select_dtypes("number").columns
buurten[num] = buurten[num].mask(buurten[num] < 0)
```

In R: `buurten <- mutate(buurten, across(where(is.numeric), ~ ifelse(.x < 0, NA, .x)))`.
:::

### BAG buildings

The BAG (*Basisregistratie Adressen en Gebouwen*) has every building (*pand*) in the country,
with its footprint, construction year (`bouwjaar`) and status. Colouring buildings by
construction year is a classic map, and a good one for Day 3, *Polygons*, or Day 6, *Vintage*.

```python
bbox = "5.470,51.435,5.490,51.448"          # lon/lat: min x, min y, max x, max y
url = ("https://api.pdok.nl/kadaster/bag/ogc/v2/collections/pand/items"
       f"?f=json&limit=1000&bbox={bbox}")
panden = gpd.read_file(url).to_crs(28992)
```

One request returns at most 1000 features. For a larger area, follow the `next` link in the
response, or download the whole BAG as a file from the
[BAG dataset page](https://www.pdok.nl/introductie/-/article/basisregistratie-adressen-en-gebouwen-ba-1)
(several GB for the country).

### AHN elevation

The AHN (*Actueel Hoogtebestand Nederland*) is a laser-scanned elevation model of the whole
country at 0.5 m resolution. `dtm_05m` is the ground surface; `dsm_05m` includes buildings and
trees. It is served as a WCS (a service for rasters), and you ask for a box in RD coordinates:

```python
import requests

url = ("https://service.pdok.nl/rws/ahn/wcs/v1_0?service=WCS&version=2.0.1&request=GetCoverage"
       "&coverageId=dtm_05m&subset=x(161000,161500)&subset=y(383500,384000)&format=image/tiff")
with open("ahn_dtm.tif", "wb") as f:
    f.write(requests.get(url, timeout=60).content)
```

That is a 500 × 500 m tile of central Eindhoven, 1000 × 1000 pixels, about 1.5 MB. Keep requests
small: a whole city at 0.5 m is gigabytes. See
[Grids, rasters and web maps](beyond-vectors.md#elevation-ahn) for how to plot it.

## Eindhoven open data

The municipality publishes its own datasets at <https://data.eindhoven.nl>: trees, lamp posts,
parking, playgrounds, waste containers, events, and more. Each dataset page has an *Export*
option; choose GeoJSON. The portal also has an API, and the export link can be read directly.
Every municipal tree, with its species and planting year:

```python
bomen = gpd.read_file("https://data.eindhoven.nl/api/explore/v2.1/catalog/datasets/"
                      "bomen/exports/geojson").to_crs(28992)
```

For another dataset, replace `bomen` with the identifier in that dataset's URL. This is a large
download (well over a hundred thousand trees), so save it locally after the first time. Check the
licence on each dataset page; they differ.

## Habits that save time

- **Download once, then read from disk.** Save what you downloaded (`gdf.to_file("data/trees.gpkg")`)
  and read the local copy while you work on the styling. Services are slow, rate-limited, and
  occasionally down on the day you need them.
- **Record where it came from** next to the file: source, URL or query, download date, licence.
  [`data/README.md`](https://github.com/AlpinistPanda/tue_codingcafe_knowledgebase/tree/main/maps/data)
  is a worked example. Your future self, and your supervisor, will ask.
- **Keep the repository small.** Commit data under a few MB; for anything bigger, commit the
  script that downloads it.
- **Check the date of the data, not only the download.** CBS figures for 2024 may describe
  1 January 2024; OSM changes every minute.

:::{seealso} More sources
- [Natural Earth](https://www.naturalearthdata.com): world countries, coastlines, rivers, at
  three scales. Public domain. In R, `rnaturalearth`.
- [Copernicus Land Monitoring](https://land.copernicus.eu): European land cover and imperviousness
- [Eurostat GISCO](https://ec.europa.eu/eurostat/web/gisco): European administrative boundaries
- [Nationaal Georegister](https://www.nationaalgeoregister.nl): the catalogue of all Dutch
  government geodata
- [Klimaateffectatlas](https://www.klimaateffectatlas.nl): heat, flooding and drought maps for the
  Netherlands
:::
