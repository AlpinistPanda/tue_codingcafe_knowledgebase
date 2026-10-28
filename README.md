# TU/e BE Coding Café — Knowledge Base

Notes, worked examples, and runnable code from the BE Coding Café sessions at the Department of
the Built Environment, TU/e.

📖 **Published at <https://alpinistpanda.github.io/tue_codingcafe_knowledgebase/>**

## What this is

The Coding Café is a monthly lunchtime session for anyone at TU/e Built Environment who writes
code for research. Sessions are ninety minutes: things get demonstrated, questions get asked, and
then everyone goes back to their own work and forgets the details. This knowledge base is where
those details live.

Whenever a topic is worked through at a session (a walkthrough, a worked example, a set of notes,
a solution to a problem someone brought along), it is written up here so it stays available
afterwards, to the people who were there and to those who were not.

It is written for researchers in the Built Environment: PhD candidates, postdocs, engineers and
designers. No prior software engineering experience is assumed.

The companion website, with the schedule, slides and posters, is
[becodingcafe.com](https://www.becodingcafe.com). Its source is in a separate repo:
[TUe-BE-ISBE-Coding-Cafe/coding-cafe](https://github.com/TUe-BE-ISBE-Coding-Cafe/coding-cafe).

## What is inside

| Section | Covers |
|---|---|
| **Keeping track of your work** | Version control with Git |
| **Running it somewhere else** | Docker and containers, cloud computing |
| **Giving your data a structure** | Relational databases and SQL, RDF and linked data |
| **Reproducibility** | Reproducible AI and machine-learning experiments |
| **Making maps in code** | The anatomy of a map, vector data, projections, finding Dutch open data, cartographic design, rasters and web maps, making maps with AI, and a day-by-day guide to the #30DayMapChallenge |

Pages marked *still being written* have a short summary but no full session write-up yet.

## Building it locally

The site is built with [MyST](https://mystmd.org/). Pages with code are executed at build time,
so you need the Python packages too.

```bash
python -m venv .venv
source .venv/bin/activate            # Windows: .venv\Scripts\activate
pip install -r requirements.txt
npm install -g mystmd
myst start --execute                 # live preview at http://localhost:3000
```

To reproduce a production build, which needs the base path GitHub Pages serves from:

```bash
BASE_URL=/tue_codingcafe_knowledgebase myst build --html --execute
```

Output lands in `_build/html/`.

## How it is deployed

Every push to `main` rebuilds and redeploys through
[`.github/workflows/deploy.yml`](.github/workflows/deploy.yml). GitHub Pages must be set to
**Settings → Pages → Source: GitHub Actions**, not "Deploy from a branch".

The workflow sets `BASE_URL` from the repository name. If the repository is renamed or moved,
update the published URL in this README, [`CITATION.cff`](CITATION.cff) and
[`LICENSE`](LICENSE) at the same time.

Any package a code cell imports must be pinned in [`requirements.txt`](requirements.txt), or the
CI build will fail.

## Repository structure

```
├── index.md                 # landing page
├── myst.yml                 # site config and table of contents
├── _static/                 # figures and other assets
├── version-control/         # section: keeping track of your work
├── running-elsewhere/       # section: Docker, cloud computing
├── structuring-data/        # section: databases, RDF
├── reproducibility/         # section: reproducible AI experiments
├── maps/                    # section: making maps in code
│   └── data/                # small Eindhoven datasets used by the map pages (ODbL)
├── requirements.txt         # Python packages the executed pages need
└── .github/workflows/       # Pages deployment
```

Each section folder has an `index.md` overview and one file per page.

## Contributing

Contributions from the BE community are very welcome: corrections, new topics, or notes from a
session you ran. See [CONTRIBUTING.md](CONTRIBUTING.md) for how to propose a change and the
conventions to follow.

## Citing this work

If this knowledge base is useful in your teaching or your work, please cite it. Metadata is in
[`CITATION.cff`](CITATION.cff), and GitHub renders a *Cite this repository* button from it.

To mint a DOI, connect the repository to [Zenodo](https://zenodo.org/) and publish a release;
Zenodo reads `CITATION.cff` automatically. Session slides are already archived in the
[Coding Café Zenodo community](https://zenodo.org/communities/tue_be_isbe_codingcafe).

## Licence

| What | Licence |
|---|---|
| Prose, figures, and teaching material | [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) |
| Code samples and configuration | [MIT](https://opensource.org/licenses/MIT) |
| Data in `maps/data/` | [ODbL 1.0](https://opendatacommons.org/licenses/odbl/1-0/), © OpenStreetMap contributors |

See [LICENSE](LICENSE) for the details. In short: reuse, adapt, and teach from this freely,
including commercially, as long as you give credit.

## Contact

Maintained by Özgün Balaban for the BE Coding Café. For questions or to propose a session, open an
issue here or on the [website repo](https://github.com/TUe-BE-ISBE-Coding-Cafe/coding-cafe/issues),
or email [o.balaban@tue.nl](mailto:o.balaban@tue.nl).
