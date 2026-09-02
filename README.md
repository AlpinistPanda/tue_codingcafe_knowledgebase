# TU/e BE Coding Café — Knowledge Base

A living [MyST](https://mystmd.org) book collecting notes, worked examples, and
runnable code from the BE Coding Café sessions at the Department of the Built
Environment, TU/e.

The companion website (schedule, slides, posters) lives in a separate repo:
[TUe-BE-ISBE-Coding-Cafe/coding-cafe](https://github.com/TUe-BE-ISBE-Coding-Cafe/coding-cafe).

## Run it locally

```bash
pip install -r requirements.txt
npm install -g mystmd
myst start --execute
```

Then open <http://localhost:3000>. Pages rebuild as you edit.

To build the static site the way CI does:

```bash
myst build --html --execute
```

Output lands in `_build/html/`.

## Add a chapter

1. Write `chapters/your-topic.md`.
2. Add it to the `toc:` in `myst.yml`.

Code cells use the `{code-cell}` directive and need a `kernelspec` in the page's
front matter — see `chapters/reproducible-ai-experiments.md` for a full example.
Any package your cells import must be pinned in `requirements.txt`, or the CI
build will fail.

## Deployment

Pushing to `main` builds the book and publishes it to GitHub Pages via
`.github/workflows/deploy.yml`.

**First-time setup**, once this repo is on GitHub:

1. Settings → Pages → Source: **GitHub Actions**.
2. Push to `main`. The first run creates the site.
3. Replace the `REPLACE-ME` placeholders in `myst.yml` and this README with the
   real repo path.
4. In the website repo, set `knowledgeBaseUrl` in `src/lib/links.js` to the
   published URL and flip `knowledgeBaseReady` to `true`.

## Licence

Content is CC BY 4.0 unless a chapter states otherwise. Code samples are MIT.
