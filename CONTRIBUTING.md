---
title: Contributing
short_title: Contributing
---

If you presented at a session, or attended one and took decent notes, your write-up belongs here.
It does not need to be polished and it does not need to be long: a page that saves someone an
afternoon is worth more than a perfect one that never gets written.

You do not need to be an expert, and you do not need permission to start.

## What is worth contributing

- **Notes from a session you ran or attended**: the walkthrough, the worked example, the thing
  that tripped everyone up
- **A fix**: a broken link, a command that no longer works, a step that is missing
- **A clarification**: if a page confused you, it will confuse the next person
- **A new topic**: fill in a page marked *still being written*, or propose something else
- **A map**: if you made a #30DayMapChallenge map with a technique the
  [maps section](maps/index.md) does not cover yet, a short worked example is very welcome

Small corrections are as valuable as new chapters. A typo fix is a real contribution.

## How to propose a change

**For a small fix**, use the edit button at the top of any page on the published site, which takes
you straight to the file on GitHub.

**For anything larger:**

1. Open an issue first if it is a new topic, so we do not duplicate work
2. Fork the repository and create a branch: `git switch -c add-my-topic`
3. Make your change and preview it locally (see below)
4. Commit, push, and open a pull request describing what you added and why

Do not worry about getting the formatting perfect. It is easier to tidy up a draft that exists
than to review one that does not.

## Previewing your change

```bash
git clone https://github.com/AlpinistPanda/tue_codingcafe_knowledgebase.git
cd tue_codingcafe_knowledgebase
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
npm install -g mystmd
myst start --execute
```

This serves a live preview at <http://localhost:3000> and reloads as you edit.

Before opening a pull request, check that a full build is clean:

```bash
BASE_URL=/tue_codingcafe_knowledgebase myst build --html --execute
```

Look for lines beginning `⛔️`: those are real errors, usually a missing image, a broken
cross-reference, or a code cell that failed. Warnings beginning `⚠️` are mostly pre-existing and
are not something your pull request needs to fix.

## Adding a page

1. Create the file inside the relevant section folder, for example `maps/my-topic.md`
2. Give it frontmatter with a title:

   ```markdown
   ---
   title: My topic
   short_title: My topic
   ---
   ```

3. Add it to the `toc:` in [`myst.yml`](myst.yml), under the right section
4. Link it from that section's `index.md`

A new section is a new folder with its own `index.md`, added to the `toc:` the same way.

## Things you can use in a page

Executable code, which runs when the site is built. The page needs a `kernelspec` in its
frontmatter (see [](reproducibility/reproducible-ai-experiments.md) or any page in `maps/`), and any
package it imports must be pinned in `requirements.txt`:

````
```{code-cell} python
print("this runs")
```
````

Code in another language, shown but not run, side by side with the Python version:

`````
::::{tab-set}
:::{tab-item} Python
```python
gdf = gpd.read_file("data.geojson")
```
:::
:::{tab-item} R
```r
gdf <- st_read("data.geojson")
```
:::
::::
`````

A session recording, or any other embeddable page:

```
:::{iframe} https://www.youtube.com/embed/VIDEO_ID
:width: 100%
A caption for the video.
:::
```

Diagrams, written as text rather than drawn:

````
:::{mermaid}
flowchart LR
  A[Raw data] --> B[Clean] --> C[Analyse]
:::
````

Callouts for things worth interrupting the reader for: `{note}`, `{tip}`, `{warning}`,
`{important}`, and `{seealso}`.

## Conventions

These keep the book feeling like one document rather than a pile of pages.

- **Write for a researcher who is new to the topic.** Assume intelligence, not background.
- **Prefer a worked example over an abstract explanation.** Show the code and its output.
- **One idea per section**, with a heading that says what the section is about.
- **Start each section's `index.md` with a "What you will learn" tip.**
- **Put figures in `_static/`.** SVG is preferred over PNG where possible.
- **Keep data small.** Anything over a few MB should be downloaded by the code, not committed.
  Record the source, download date and licence next to any data you add, as in
  [`maps/data/README.md`](maps/data/README.md).
- **British spelling**, matching the rest of the text.

## Citing your sources

If you adapt material from elsewhere, say so on the page itself, and give the licence. Much of the
best research-software training material is CC BY licensed, which means you are free to reuse it
**provided you attribute it**. Carry the attribution across rather than dropping it: a source note
at the foot of the section is enough.

Please do not paste in material whose licence you have not checked.

## What you are agreeing to

By contributing, you agree that your contribution is published under the same licences as the rest
of the repository: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) for prose and figures,
and MIT for code samples. See [LICENSE](LICENSE).

Contributors are credited. If you add a substantial chapter, add yourself to
[`CITATION.cff`](CITATION.cff) in the same pull request.

## Questions

Open an issue, or ask at the next Coding Café. If you are not sure whether something belongs here,
ask: the answer is usually yes.
