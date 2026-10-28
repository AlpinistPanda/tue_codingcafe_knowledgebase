# Data

Small, pre-downloaded datasets for Eindhoven, used by the pages in the maps section. They are the same
files as in the [#30DayMapChallenge starter repo](https://github.com/TUe-BE-ISBE-Coding-Cafe/30daymapchallenge-2026),
so code from the book runs there unchanged.
Both files are **GeoJSON in EPSG:4326 (WGS 84, lon/lat)**. Reproject to **EPSG:28992 (RD New)** for mapping.

| File | Geometry | Features | Size |
|---|---|---:|---:|
| `eindhoven_cafes.geojson` | Point | 91 | ~34 KB |
| `eindhoven_boundary.geojson` | Polygon | 1 | ~78 KB |

## Source

- **Provider:** [OpenStreetMap](https://www.openstreetmap.org), queried through the Overpass API
  (`https://overpass-api.de/api/interpreter`).
- **Download date:** 2026-10-21 (Overpass database timestamp `2026-10-21T10:00:56Z` for cafés,
  `2026-10-21T10:01:55Z` for the boundary).
- **Area:** municipality (gemeente) of Eindhoven, OSM relation
  [47504](https://www.openstreetmap.org/relation/47504)
  (`boundary=administrative`, `admin_level=8`, `ref:gemeentecode=0772`).

### Queries

Cafés (nodes and ways tagged `amenity=cafe` inside the municipality; ways are reduced to their centre point):

```
[out:json][timeout:60];
area["boundary"="administrative"]["admin_level"="8"]["name"="Eindhoven"]["ref:gemeentecode"="0772"]->.gem;
(
  node["amenity"="cafe"](area.gem);
  way["amenity"="cafe"](area.gem);
);
out center tags;
```

Municipal boundary:

```
[out:json][timeout:60];
relation["boundary"="administrative"]["admin_level"="8"]["name"="Eindhoven"]["ref:gemeentecode"="0772"];
out geom;
```

You can paste either query into <https://overpass-turbo.eu> to see or re-run it.

### Processing

The raw Overpass JSON was converted to GeoJSON with a small Python (standard library) script:

- **Cafés:** one point per element (`node` coordinates, or the `center` of a `way`). Kept attributes:
  `osm_id` (e.g. `node/123`), `name`, `amenity`, `cuisine`, `opening_hours`, `outdoor_seating`,
  `wheelchair`, `addr:street`, `addr:housenumber`, `addr:postcode`, `website` (`null` when not tagged).
- **Boundary:** the relation's outer ways joined into a single closed ring (no inner rings).
  Attributes: `osm_id`, `name`, `admin_level`, `ref:gemeentecode`, `wikidata`.
- Coordinates rounded to 6 decimals (~0.1 m). No other edits.

Note that OSM is volunteer-mapped: "91 cafés" means "91 things someone tagged `amenity=cafe`",
not an official count. Bars (`amenity=bar`) and pubs (`amenity=pub`) are not included.

## Licence and attribution

© OpenStreetMap contributors. Available under the
[Open Database License (ODbL) 1.0](https://opendatacommons.org/licenses/odbl/1-0/).
See <https://www.openstreetmap.org/copyright>.

Any map made with these files **must** show: **"© OpenStreetMap contributors"**.
These data files are *not* covered by the repository's CC BY 4.0 or MIT licences.
