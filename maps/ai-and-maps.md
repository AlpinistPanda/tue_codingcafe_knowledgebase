---
title: Making maps with AI
subtitle: What an AI assistant gets wrong about maps, and how to check its work
short_title: Maps with AI
kernelspec:
  name: python3
  display_name: Python 3
---

The 2026/27 theme of the Coding Café is AI in research software development, and the
#30DayMapChallenge is a good place to try it: a small, self-contained task, with a result you can
see. AI assistants and coding agents are genuinely good at map code. They know the GeoPandas and
`sf` APIs better than most of us, and they will happily write the forty lines of matplotlib
styling you did not want to look up.

They are also confidently wrong in ways that are specific to maps, and a wrong map looks just as
convincing as a right one. This page is about getting the first without the second.

:::{note} From the session
At the [kick-off](anatomy.md) we watched an AI agent build a map live, then went through what it
got wrong. The challenge's [starter repo](https://github.com/TUe-BE-ISBE-Coding-Cafe/30daymapchallenge-2026)
has an `AGENTS.md` file with the rules on this page, which coding agents read automatically.
:::

## What goes wrong

These are the failures we see again and again, roughly in order of how much damage they do.

| Failure | What it looks like | Why it happens |
|---|---|---|
| **Invented data** | Plausible coordinates, counts or place names that do not exist. A "sample dataset" that the code quietly uses instead of the real one. | When a download fails or no data is given, the model fills the gap rather than stopping. |
| **Wrong or missing CRS** | Distances in degrees, a squashed map, buffers that are ellipses, layers that do not line up | Models often skip the reprojection step, or reproject only one of two layers. |
| **Lat/lon swapped** | Points in the Indian Ocean off Somalia instead of Eindhoven | Training data mixes `(lat, lon)` and `(x, y)` conventions. |
| **Wrong OSM tags** | `amenity=coffee_shop` (no such value), or missing ways and relations so half the features are gone | The model guesses tags instead of checking the wiki. |
| **Stale APIs** | Code that calls functions renamed or removed several versions ago (`geometries_from_place` in old `osmnx`, `unary_union` warnings) | Training data lags behind the libraries. |
| **No attribution** | A map of OSM data with no "© OpenStreetMap contributors" | Credits are not part of "make a map" unless you ask. |
| **Misleading design** | Raw counts on a choropleth of areas of different sizes; a rainbow palette; a legend that does not match the classes | The model optimises for a map that looks finished. |

None of these produce an error. The script runs, the PNG looks good, and the map is wrong.

## Prompting so it does not happen

Give the assistant the same information you would give a new colleague. A prompt that works well
for map code has five parts:

1. **The data**, with its path, format and CRS: *"`data/eindhoven_cafes.geojson`, points,
   EPSG:4326"*. If it needs to download something, name the source.
2. **The CRS to work in**: *"Reproject everything to EPSG:28992 before plotting or measuring."*
3. **What the map should show**, as a statement: *"Show that cafés with a terrace cluster in the
   city centre."*
4. **What must be on the image**: title, legend with units, credit line with
   "© OpenStreetMap contributors", the CRS.
5. **What to do when stuck**: *"If the data is missing or a download fails, stop and tell me. Do
   not invent data, not even as a placeholder."*

An example:

```text
Using Python with geopandas and matplotlib, make a map from data/eindhoven_cafes.geojson
(points, EPSG:4326) and data/eindhoven_boundary.geojson (polygon, EPSG:4326).
Reproject both to EPSG:28992 first.

Show the cafés that have outdoor_seating=yes in red and all others in grey, on the boundary
in light grey. Title: "Where to sit outside with a coffee in Eindhoven". Add a legend and a
credit line: "Data: © OpenStreetMap contributors (ODbL) | EPSG:28992".

Save to maps/day01-<me>/map.png at 7x7 inches, 200 dpi, white background.
Use only the data in these files. If something is missing, stop and ask me.
```

For a coding agent working in a repository, put the rules in an instruction file
(`AGENTS.md`, `CLAUDE.md` or `.github/copilot-instructions.md`) so you do not have to repeat them
in every prompt. The [starter repo's AGENTS.md](https://github.com/TUe-BE-ISBE-Coding-Cafe/30daymapchallenge-2026/blob/main/AGENTS.md)
is a ready-made example to copy.

## Checking the result

Never accept a map because it looks right. These checks take a minute and catch every failure in
the table above. The code below runs them on the Day 1 data; put the same lines at the end of any
script an AI wrote for you.

```{code-cell} python
import geopandas as gpd

cafes = gpd.read_file("data/eindhoven_cafes.geojson")
boundary = gpd.read_file("data/eindhoven_boundary.geojson")

# 1. Is the data what you think it is? Count and look at a few rows.
print(len(cafes), "features;", cafes.geom_type.unique().tolist())

# 2. Is it where you think it is? The Netherlands is roughly 3-7.3° E, 50.7-53.6° N.
xmin, ymin, xmax, ymax = cafes.to_crs(4326).total_bounds
assert 3 < xmin and xmax < 7.3 and 50.7 < ymin and ymax < 53.6, "not in NL: lat/lon swapped?"

# 3. Are all layers in the same, projected CRS before measuring?
cafes, boundary = cafes.to_crs(28992), boundary.to_crs(28992)
assert cafes.crs == boundary.crs and cafes.crs.is_projected

# 4. Do the numbers make sense? Eindhoven is about 89 km².
print(round(boundary.area.iloc[0] / 1e6, 1), "km²")

# 5. Is every point inside the area it is supposed to be in?
print(cafes.within(boundary.union_all()).mean() * 100, "% of cafés inside the boundary")
```

Then, by eye:

- **Compare with a real map.** Open the same area at <https://www.openstreetmap.org> or in
  [Overpass Turbo](https://overpass-turbo.eu). Are the points where the cafés actually are?
- **Read the code for any hard-coded data.** A list of coordinates or names typed into the script
  is a red flag unless you put it there.
- **Check the legend against the data.** Do the class edges match `describe()`? Does the colour
  bar run the right way?
- **Check the credits.** Every source named, with its licence.

If the assistant made a mistake, tell it what was wrong rather than asking it to "fix the map".
"The points are in the Indian Ocean: you swapped latitude and longitude in `points_from_xy`" gets a correct
answer; "it looks wrong" gets a different guess.

## Being open about it

The challenge has one rule about AI: every map's README says, in one line, what the AI did and
what you did.

> **AI disclosure:** AI wrote the plotting code, I chose the data and the projection.

> **AI disclosure:** I wrote the code; an assistant helped me debug a CRS error.

> **AI disclosure:** No AI used.

This is not a confession. It makes AI use something the group can learn from: which tasks it
helped with, and which it made harder. Whatever the AI produced, you are responsible for checking
that the data and the map are real and correct.

### Day 10: Prompting only

Day 10 asks for a map made *entirely* through an AI chat or agent: you may not edit the code
yourself. It is a good test of everything on this page. Save your prompts (for example in
`prompts.md` next to the map), and note how many rounds it took and what you had to correct. The
data still has to be real: "prompting only" means the AI writes the code, not the facts.

### Day 27: New tool

A good day to try an AI coding agent if you have only used chat so far, or the other way round.
Compare the result with a map you made yourself earlier in the month.

:::{seealso} Further reading
- The [AI-assisted coding](https://lumc-dcc.github.io/research_software_knowledgebase/) section of
  the LUMC Research Software Knowledge Base, a sister book to this one, covers prompting,
  assistants in the editor, and verification in more depth
- CodeRefinery, *Responsible use of generative AI in assisted coding* (CC BY 4.0):
  <https://coderefinery.github.io/>
:::
