---
title: Making maps in code
short_title: Overview
---

:::{tip} What you will learn
- The five steps every map in code goes through, from a data file to a PNG you can share
- How spatial data is stored (points, lines, polygons, rasters) and how to work with it
- What a coordinate reference system is, and why Dutch maps should almost always be in EPSG:28992
- Where to find Dutch open data, and how to credit it
- How to make a map that is clear, honest, and good to look at
- How to use an AI assistant to make maps without publishing something that is wrong
:::

Almost everything in the Built Environment happens somewhere. Buildings, streets, energy use,
flood risk, heat, noise, who lives where: sooner or later your data has coordinates, and sooner or
later someone asks you for a map. This section is the foundation for making that map in code,
reproducibly, rather than by hand.

It grew out of the October 2026 special edition of the Coding Café, the
[#30DayMapChallenge kick-off](https://github.com/TUe-BE-ISBE-Coding-Cafe/30daymapchallenge-2026),
and it is written so that you can work through the challenge with it: one theme a day, 1 to
30 November. The [day-by-day guide](challenge-guide.md) points every theme at the page that
covers it.

:::{note} The examples run on real data
Every Python example on these pages is executed when the book is built, on the same small Eindhoven
dataset as the challenge's starter repo: 91 cafés and the municipal boundary, from OpenStreetMap
(see [`data/README.md`](https://github.com/AlpinistPanda/tue_codingcafe_knowledgebase/tree/main/maps/data)).
The maps you see are the output of the code above them. R versions are shown alongside in tabs.
:::

## How a map gets made

Whatever tool you use, a map in code goes through the same five steps. Most of the problems people
hit are a missing or skipped step, usually the second one.

:::{mermaid}
flowchart LR
    D[1. Data<br/>read a file or<br/>download it] --> P[2. Projection<br/>pick a CRS and<br/>reproject]
    P --> G[3. Geometry<br/>filter, buffer,<br/>join, aggregate]
    G --> S[4. Styling<br/>colour, size,<br/>labels, legend]
    S --> E[5. Export<br/>PNG at a known<br/>size and DPI]
:::

## The pages

::::{grid} 1 1 2 2

:::{card} Anatomy of a map
:link: anatomy.md

**Start here.** One complete map, step by step, in Python and R. Everything else in the section
is a deeper look at one of the five steps.
:::

:::{card} Vector data
:link: vector-data.md

**Points, lines and polygons.** What a GeoDataFrame is, and the handful of operations
(filter, buffer, distance, spatial join, dissolve) behind most maps.
:::

:::{card} Projections
:link: projections.md

**Why your map looks squashed.** Coordinate reference systems, EPSG:4326 vs 28992 vs 3857, and the
mistakes that silently give wrong distances and areas.
:::

:::{card} Finding data
:link: finding-data.md

**OpenStreetMap, PDOK, CBS and Eindhoven open data.** How to download from each, and what you
must put on the map to credit it.
:::

:::{card} Map design
:link: design.md

**From a plot to a map.** Visual hierarchy, colour, classification, legends, titles and credits,
and a checklist to run before you post.
:::

:::{card} Grids, rasters and web maps
:link: beyond-vectors.md

**Beyond points on a plot.** Hexagon grids, density surfaces, elevation rasters, basemaps and
interactive maps.
:::

:::{card} Maps with AI
:link: ai-and-maps.md

**What an AI assistant gets wrong about maps**, how to prompt so it does not, and how to check its
work. Fits the 2026/27 Coding Café theme.
:::

:::{card} #30DayMapChallenge guide
:link: challenge-guide.md

**All 30 themes**, each with a technique, a page to read, and a data idea for Eindhoven or the
Netherlands.
:::

::::

## If you have one hour

1. Read [Anatomy of a map](anatomy.md) and run its code on your own laptop.
2. Read the first half of [Projections](projections.md), up to *The three you will meet*.
3. Make the Day 1 *Points* map from the
   [starter repo](https://github.com/TUe-BE-ISBE-Coding-Cafe/30daymapchallenge-2026), then change
   one thing: the colour, the data, or the question it answers.
4. Before you share it, run the [checklist](design.md#before-you-post) at the end of Map design.

## Setting up

You need either Python or R. You do not need both.

::::{tab-set}
:::{tab-item} Python
```bash
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install geopandas matplotlib mapclassify
```

Conda users: `conda install -c conda-forge geopandas matplotlib mapclassify`.
`geopandas` brings `shapely` (geometry), `pyproj` (projections) and `pyogrio` (reading files)
with it.
:::
:::{tab-item} R
```r
install.packages(c("sf", "ggplot2"))
# optional, used on some pages:
install.packages(c("tmap", "terra", "osmdata", "classInt"))
```

On Linux, `sf` needs the GDAL, GEOS and PROJ system libraries first
(`sudo apt install libgdal-dev libgeos-dev libproj-dev` on Ubuntu). On Windows and macOS the
CRAN binaries include them.
:::
:::{tab-item} No code
[QGIS](https://qgis.org) is free and reads every file on these pages. The ideas (projections,
classification, design) apply unchanged; only the buttons differ. See
[Grids, rasters and web maps](beyond-vectors.md#qgis) for the QGIS route.
:::
::::

## Further reading

- *Geocomputation with R*, Lovelace, Nowosad and Muenchow, free online at
  <https://r.geocompx.org>, and its Python sibling *Geocomputation with Python* at
  <https://py.geocompx.org>. The best next step after this section.
- [GeoPandas user guide](https://geopandas.org/en/stable/docs/user_guide.html) and the
  [sf documentation](https://r-spatial.github.io/sf/)
- [Rbanism](https://rbanism.org), TU Delft's community for R in urbanism, who run their own
  R edition of the #30DayMapChallenge every November
