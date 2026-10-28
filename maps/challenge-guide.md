---
title: "#30DayMapChallenge: a day-by-day guide"
subtitle: All 30 themes of 2026, each with a technique, a page to read, and a data idea
short_title: "#30DayMapChallenge guide"
---

The [#30DayMapChallenge](https://30daymapchallenge.com) runs every day from **1 to 30 November**.
Each day has a theme, and you make a map for it and share it with **#30DayMapChallenge**. There is
no sign-up and no jury, any tool is welcome, and you do not need to do all 30: one map is a
success.

The TU/e BE Coding Café edition collects everyone's maps in one repository and a showcase site:

- **Starter repo:** <https://github.com/TUe-BE-ISBE-Coding-Cafe/30daymapchallenge-2026>, with
  R and Python templates, the Eindhoven data used throughout this section, and instructions for
  AI agents
- **All the maps:** <https://tue-be-isbe-coding-cafe.github.io/30daymapchallenge-2026/>
- **Sign-up form**, for updates and a possible show-and-tell at the end of November:
  <https://forms.cloud.microsoft/e/Qqk5yjSuVT>

## How to take part

1. Pick one or two days below, and put your name in `signup.md` in the starter repo.
2. Fork the repo and make your map, starting from `r/day01_points.R` or
   `python/day01_points.py`.
3. Add a folder `maps/dayNN-<github-handle>/` with `map.png`, your code, and a `README.md` copied
   from `maps/_TEMPLATE/`, **including the one-line AI disclosure**.
4. Open a pull request, and post the map with **#30DayMapChallenge**. Your map appears on the
   showcase site a few minutes after the pull request is merged.

## The themes

The *Technique* column suggests one way to approach the theme, not the only one. The best maps in
the challenge are usually the ones that interpret a theme sideways. Always check
<https://30daymapchallenge.com> for the official description of each day.

### Week 1: the basics

| Day | Theme | Technique | Read | Data idea |
|---:|---|---|---|---|
| 1 | **Points** | Plot points over a boundary; emphasise a subset | [Anatomy](anatomy.md) | Cafés in Eindhoven (in the starter repo) |
| 2 | **Lines** | Line data, or lines made from points (nearest neighbour, origin–destination) | [Vector data](vector-data.md#vd-nearest) | Cycle paths (`highway=cycleway`) from OSM; the Dommel |
| 3 | **Polygons** | Fill polygons by an attribute | [Finding data](finding-data.md#bag-buildings) | BAG buildings coloured by construction year |
| 4 | **Clusters** | Buffers and dissolve, DBSCAN, convex hulls | [Vector data](vector-data.md#vd-clusters) | Cafés, bars or shops in the centre |
| 5 | **Senses: Sight** | What can be seen from where: viewpoints, sightlines, light | [Rasters](beyond-vectors.md#elevation-ahn) | Street lights (`highway=street_lamp`), Eindhoven lamp posts, AHN for heights |
| 6 | **Vintage** | Old-map styling: sepia palette, serif fonts, hand-drawn texture; or old data | [Map design](design.md#typography-and-layout) | BAG buildings built before 1940; historical maps on [topotijdreis.nl](https://www.topotijdreis.nl) |
| 7 | **10 minute map** | Set a timer. Reuse yesterday's script with new data | [Anatomy](anatomy.md#the-whole-script) | Any OSM tag you have not tried |

### Week 2: ideas and places

| Day | Theme | Technique | Read | Data idea |
|---:|---|---|---|---|
| 8 | **Utopia** | Your ideal city: edit data, draw new features, or map a plan | [Vector data](vector-data.md) | A car-free centre; every building within 300 m of a park |
| 9 | **Urban-rural** | Classify areas by density; compare inside and outside the city | [Map design](design.md#classification-the-choice-that-changes-the-story) | CBS address density (`omgevingsadressendichtheid`) per neighbourhood |
| 10 | **Prompting only** | The whole map made through an AI chat or agent. Save your prompts | [Maps with AI](ai-and-maps.md#day-10-prompting-only) | Anything, as long as it is real |
| 11 | **Senses: Sound** | Noise from roads, rail and the airport; quiet places | [Finding data](finding-data.md#pdok-dutch-government-geodata) | Noise maps (*geluidkaarten*) on PDOK or the Atlas Leefomgeving |
| 12 | **Power** | Energy infrastructure, or who has power | [Finding data](finding-data.md#openstreetmap) | `power=line`, `power=substation`, solar panels (`generator:source=solar`) from OSM |
| 13 | **Interactions** | Flows and connections between places; or an interactive map | [Web maps](beyond-vectors.md#interactive-web-maps) | Commuting flows from CBS; a `folium` or `tmap` view map |
| 14 | **Borgesian map** | A map at 1:1, or a map of a map. Read Borges' *On Exactitude in Science* first | [Map design](design.md) | Your own desk, or the boundary of a boundary |

### Week 3: experiments

| Day | Theme | Technique | Read | Data idea |
|---:|---|---|---|---|
| 15 | **Inside out** | Invert the usual figure and ground: map the gaps, the outside, the negative space | [Vector data](vector-data.md#vd-dissolve) | Everywhere *more* than 500 m from a café (`overlay(..., how="difference")`) |
| 16 | **Collaborative map** | Made with **another person**: AI does not count | — | Pair up at the Coding Café; combine your two datasets |
| 17 | **Light & dark** | Dark basemap, glowing data; or day and night | [Rasters](beyond-vectors.md#a-raster-from-points-density) | Café density as a glow; street lamps |
| 18 | **NULL** | Map the missing: empty values, data gaps, places with nothing | [Finding data](finding-data.md#openstreetmap) | Cafés *without* opening hours in OSM; CBS negative placeholder values |
| 19 | **Senses: Smell** | Sources and spread: buffers, density, wind direction | [Vector data](vector-data.md#vd-buffer) | Bakeries, coffee roasters, the sewage works, flowering trees from Eindhoven's tree data |
| 20 | **Hexagons** | Aggregate to a hexagon grid | [Grids](beyond-vectors.md#hexagon-grids) | Cafés or trees per hexagon; H3 |
| 21 | **OpenStreetMap** | Anything from OSM, with the credit on the map | [Finding data](finding-data.md#openstreetmap) | Benches, bike parking, drinking water (`amenity=drinking_water`) |

### Week 4: craft

| Day | Theme | Technique | Read | Data idea |
|---:|---|---|---|---|
| 22 | **Projections** | Compare projections, or use an unusual one on purpose | [Projections](projections.md#seeing-the-distortion) | The world in five projections; the Netherlands in Web Mercator vs RD New |
| 23 | **Senses: Taste** | Food: where it is grown, sold, eaten | [Finding data](finding-data.md#openstreetmap) | Restaurants by `cuisine` (a qualitative palette); markets; allotments |
| 24 | **Network** | Street networks, routing, travel time | [Finding data](finding-data.md#from-code) | `osmnx` bike network of Eindhoven; distance from TU/e to every café along streets |
| 25 | **Is this a map?** | Push the definition: a diagram, a table, an abstraction | [Map design](design.md) | Cafés as a list sorted by distance to the station, drawn as a map |
| 26 | **Water** | Rivers, flood risk, rain, groundwater | [Rasters](beyond-vectors.md#elevation-ahn) | The Dommel and Gender (`waterway=*`); AHN low points; Klimaateffectatlas flooding |
| 27 | **New tool** | Use something you have never used: QGIS, R if you use Python, an AI agent | [QGIS](beyond-vectors.md#qgis), [Maps with AI](ai-and-maps.md#day-27-new-tool) | Remake your Day 1 map in the new tool |
| 28 | **Senses: Feeling** | Heat, wind, comfort, safety; or how a place feels | [Finding data](finding-data.md#habits-that-save-time) | Heat maps (*hittekaart*) from the Klimaateffectatlas; tree cover per neighbourhood |
| 29 | **Raster** | Elevation, hillshade, satellite imagery, a density surface | [Rasters](beyond-vectors.md#rasters) | AHN hillshade of the TU/e campus; building heights (DSM minus DTM) |
| 30 | **Pen & paper** | No code. Draw a map by hand, photograph it, and share it | — | Your walk to work |

## Planning your month

You will not do all 30, and nobody expects you to. A few ways to choose:

- **Follow a thread.** Pick one dataset (Eindhoven's trees, say) and map it on every day where it
  fits: points, hexagons, density, *NULL* for trees without a species, *Smell* for flowering ones.
- **Follow a technique.** Do the four basic days (1 Points, 2 Lines, 3 Polygons, 20 Hexagons) in
  both Python and R.
- **Follow a question from your research.** If you work on heat, energy, mobility or water, there
  is a day for it. A map made for the challenge often turns into a figure for a paper.
- **Do the AI days.** Day 10 and Day 27 are a direct way to take part in the Coding Café's
  2026/27 theme, and the comparison is worth sharing at a session.

:::{seealso} Elsewhere
- **Rbanism** at TU Delft runs an R edition of the challenge every November: <https://rbanism.org>
- Previous years' maps: search the hashtag on Bluesky, Mastodon or LinkedIn, or browse the
  official site's gallery
:::
