---
title: Start here
subtitle: A field guide for researchers who did not sign up to be software engineers
---

You are doing a PhD in the Built Environment, not in computer science. Nobody
handed you a course on how to keep your files straight, how to make your analysis
run on a machine that isn't your laptop, or what to do when your data outgrows a
spreadsheet. You picked it up as you went, and mostly it works.

This book is the written companion to the [BE Coding Café](https://github.com/TUe-BE-ISBE-Coding-Cafe/coding-cafe)
at TU/e. Sessions are an hour and a half — things get demonstrated, questions get
asked, and then everyone goes back to their own work and forgets the details.
This is where those details live.

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

    click VC "./chapters/version-control.md"
    click DK "./chapters/docker.md"
    click CC "./chapters/cloud-computing.md"
    click DB "./chapters/databases.md"
    click RDF "./chapters/rdf.md"
:::

Nothing here is a prerequisite for anything else. Read the one that matches your
current problem and ignore the rest until it becomes your problem too.

---

## Keeping track of your own work

The problems that start small and quietly get worse. If you have ever emailed
yourself a zip file as a backup, this cluster is for you.

::::{grid} 1 1 1 1

:::{card} Version control
:link: ./chapters/version-control.md

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
:link: ./chapters/docker.md

**"It works on my machine"**

You send your analysis to a colleague and it fails immediately. Wrong Python
version, a missing library, a path that only exists on your computer.

A container packages your code together with the exact environment it needs, so
it runs identically anywhere. It is also the honest answer to "can someone
reproduce this in five years" — a container will still run when your prose
description of the setup has rotted.
:::

:::{card} Cloud computing
:link: ./chapters/cloud-computing.md

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
:link: ./chapters/databases.md

**"My data is forty spreadsheets"**

The same building appears in six files under three different names. You change a
value in one place and forget the other five. Every question means another round
of copy-paste.

A database stores things once, enforces that the pieces agree with each other,
and answers questions you did not plan for in advance. You do not need to be a
developer to use one.
:::

:::{card} RDF and linked data
:link: ./chapters/rdf.md

**"How do I connect my data to theirs?"**

Your building data needs to talk to an energy dataset, a GIS layer, and a
national registry — none of which use your column names.

RDF is a way of describing data so that its *meaning* travels with it, which is
what makes combining datasets from different sources possible at all. It is the
backbone of a lot of Built Environment data work, from BIM to city-scale models.
:::

::::

---

## How to use this book

Every page with code is executable. You can **read it** — each chapter stands on
its own — or **run it**, by cloning the repo and serving it locally. Corrections
and additions from people who attended a session are the whole point; see
[Contributing](contributing.md).
