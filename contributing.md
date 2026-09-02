---
title: Contributing
short_title: Contributing
---

If you presented at a session, or attended one and took decent notes, your
write-up belongs here. It does not need to be polished and it does not need to be
long — a page that saves someone an afternoon is worth more than a perfect one
that never gets written.

## Adding or filling in a chapter

Chapters are Markdown files in `chapters/`, listed under `toc:` in `myst.yml`.

1. Write or edit `chapters/your-topic.md`.
2. Make sure it is listed in the `toc:` in `myst.yml`.
3. Open a pull request.

## Running the book locally

```bash
git clone https://github.com/AlpinistPanda/tue_codingcafe_knowledgebase.git
cd tue_codingcafe_knowledgebase
pip install -r requirements.txt
npm install -g mystmd
myst start --execute
```

That serves the book at `http://localhost:3000` with live reload, so you can see
your changes as you write.

## Things you can use in a chapter

Executable code, which runs when the site is built:

````
```{code-cell} python
print("this runs")
```
````

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

Callouts for things worth interrupting the reader for: `{note}`, `{warning}`,
`{important}`, `{seealso}`, and `{tip}`.
