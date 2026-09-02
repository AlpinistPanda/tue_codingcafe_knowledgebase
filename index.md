---
title: BE Coding Café Knowledge Base
subtitle: Notes and runnable examples from our sessions
---

This is the written companion to the [BE Coding Café](https://github.com/ISBE-TUe/coding-cafe)
at the Department of the Built Environment, TU/e.

Sessions are an hour and a half. Things get demonstrated, questions get asked, and
then everyone goes back to their own work and forgets the details. This book is
where those details live: short, practical write-ups you can come back to, with
code you can actually run.

:::{note} This year's theme is AI for research
Sessions in 2026/27 centre on using AI in research — the tools, the workflows,
and what actually holds up in practice. Chapters on that theme land here as we go.
:::

## How to use this book

Every page with code is executable. You can:

- **Read it** — each chapter stands on its own.
- **Run it** — click the rocket icon at the top of a page to launch it, or clone
  the repo and run it locally (see below).
- **Fix it** — spotted something wrong or out of date? Open an issue or a pull
  request. Corrections from people who attended are the whole point.

## Running it locally

```bash
git clone https://github.com/REPLACE-ME/tue_knowledgebase.git
cd tue_knowledgebase
pip install -r requirements.txt
npm install -g mystmd
myst start --execute
```

That serves the book at `http://localhost:3000` with live reload.

## Contributing a chapter

Chapters are Markdown files in `chapters/`, listed in the `toc:` section of
`myst.yml`. A new chapter is two steps:

1. Write `chapters/your-topic.md`.
2. Add it to the `toc:` in `myst.yml`.

If you presented at a session, your write-up belongs here — it does not need to be
polished, and it does not need to be long. See
[the reproducibility chapter](chapters/reproducible-ai-experiments.md) for the general shape of one.
