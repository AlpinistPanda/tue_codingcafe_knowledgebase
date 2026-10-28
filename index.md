---
title: Start here
subtitle: A field guide for researchers working in the Built Environment
---

You are doing a PhD in the Built Environment, not in computer science. Nobody
handed you a course on how to keep your files straight, how to make your analysis
run on a machine that isn't your laptop, or what to do when your data outgrows a
spreadsheet. You picked it up as you went, and mostly it works.

This book is the written companion to the [BE Coding Café](https://www.becodingcafe.com)
at TU/e. Sessions are an hour and a half — things get demonstrated, questions get
asked, and then everyone goes back to their own work and forgets the details.
This is where those details live.

:::{card}
:link: https://www.becodingcafe.com

```{image} _static/be-coding-cafe-logo-tile.png
:alt: BE Coding Café logo
:width: 72px
:align: left
```

**BE Coding Café** · [becodingcafe.com](https://www.becodingcafe.com)

The schedule of upcoming sessions, slides and recordings from past ones, and how to propose a
topic or give a talk.
:::

## Do you recognise any of these?

You do not need to know the name of a tool to know that something hurts. Find your
complaint below, and it will point you at the session that deals with it.

:::{mermaid}
flowchart TD
    Q{What is annoying you<br/>right now?}
    Q -->|"I have five versions of<br/>one file and no idea which is real"| VC[Version control]
    Q -->|"It works on my laptop<br/>and nowhere else"| DK[Docker]
    Q -->|"My laptop takes three days<br/>to run this"| CC[Cloud computing]
    Q -->|"My data is forty spreadsheets<br/>held together by hope"| DB[Databases]
    Q -->|"I need my data to connect<br/>to someone else's data"| RDF[RDF]
    Q -->|"Someone asked me<br/>for a map"| MAP[Maps in code]

    click VC "./version-control/version-control.md"
    click DK "./running-elsewhere/docker.md"
    click CC "./running-elsewhere/cloud-computing.md"
    click DB "./structuring-data/databases.md"
    click RDF "./structuring-data/rdf.md"
    click MAP "./maps/index.md"
:::

Nothing here is a prerequisite for anything else. Read the one that matches your
current problem and ignore the rest until it becomes your problem too.

---

## Keeping track of your own work

The problems that start small and quietly get worse. If you have ever emailed
yourself a zip file as a backup, this cluster is for you.

::::{grid} 1 1 1 1

:::{card} Version control
:link: ./version-control/version-control.md

**"Which one is the final version?"**

`thesis_v3_FINAL_revised_ACTUALLYfinal.docx`. Everyone does this, and it works
right up until it doesn't — you overwrite something that mattered, or your
supervisor asks what changed since March and you cannot say.

Git gives you a labelled history of every state your work has been in, and a way
back to any of them. It also happens to be how you collaborate without emailing
files around.
:::

::::

## Making it run somewhere that isn't your laptop

Two related problems: *it only runs here*, and *here is too slow*. They have
different solutions and they combine well.

::::{grid} 1 1 2 2

:::{card} Docker
:link: ./running-elsewhere/docker.md

**"It works on my machine"**

You send your analysis to a colleague and it fails immediately. Wrong Python
version, a missing library, a path that only exists on your computer.

A container packages your code together with the exact environment it needs, so
it runs identically anywhere. It is also the honest answer to "can someone
reproduce this in five years" — a container will still run when your prose
description of the setup has rotted.
:::

:::{card} Cloud computing
:link: ./running-elsewhere/cloud-computing.md

**"This will take three days on my laptop"**

Simulations, large datasets, or training runs that turn your machine into a
space heater you cannot use for anything else.

Renting a bigger machine for an afternoon is cheaper and easier than most people
expect, and TU/e provides options before you reach for a credit card. The hard
part is not the computing — it is getting your work onto it and back, which is
exactly what containers solve.
:::

::::

## Giving your data a structure

When a spreadsheet stops being the right tool, and what to reach for instead.

::::{grid} 1 1 2 2

:::{card} Databases
:link: ./structuring-data/databases.md

**"My data is forty spreadsheets"**

The same building appears in six files under three different names. You change a
value in one place and forget the other five. Every question means another round
of copy-paste.

A database stores things once, enforces that the pieces agree with each other,
and answers questions you did not plan for in advance. You do not need to be a
developer to use one.
:::

:::{card} RDF and linked data
:link: ./structuring-data/rdf.md

**"How do I connect my data to theirs?"**

Your building data needs to talk to an energy dataset, a GIS layer, and a
national registry — none of which use your column names.

RDF is a way of describing data so that its *meaning* travels with it, which is
what makes combining datasets from different sources possible at all. It is the
backbone of a lot of Built Environment data work, from BIM to city-scale models.
:::

::::

## Putting it on a map

Almost everything in the Built Environment happens somewhere. Sooner or later your data has
coordinates, and someone asks for a map.

::::{grid} 1 1 1 1

:::{card} Making maps in code
:link: ./maps/index.md

**"Can you make a map of this?"**

You can export a screenshot from Google Maps, or you can make a map that is reproducible,
correctly projected, and credits its data. This section takes you from a GeoJSON file to a
finished map in Python or R: projections, Dutch open data, map design, rasters, and how to use an
AI assistant without publishing something that is wrong. It doubles as a guide to the
#30DayMapChallenge, with a technique for every one of the 30 themes.
:::

::::

---

## How to use this book

Every page with code is executable. You can **read it** — each chapter stands on
its own — or **run it**, by cloning the repo and serving it locally. Corrections
and additions from people who attended a session are the whole point; see
[Contributing](CONTRIBUTING.md).
