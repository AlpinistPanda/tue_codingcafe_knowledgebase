---
title: Build once, run anywhere
subtitle: Making your research reproducible with Docker
short_title: Docker
---

:::{note} From the session
**Docker & Reproducibility** — 2 July 2025, Vertigo 9.06, presented by Özgün Balaban.
Slides are archived on Zenodo: [10.5281/zenodo.15802618](https://doi.org/10.5281/zenodo.15802618).
The exercises below are the ones from the session, in the
[Coding Café repository](https://github.com/ISBE-TUe/coding-cafe/tree/main/static/events/2025-07-02_docker-and-reproducibility).
:::

## The problem: dependency hell

Every researcher who has tried to install someone else's scientific software
recognises at least one of these:

- The software isn't available for your operating system.
- You fall into an endless install loop — each dependency needs another one.
- Only certain combinations of versions actually work together.
- You are not sure which version you are even running.
- The same software gives different results on different systems.
- It installs locally but fails on a colleague's machine or on a server.
- Other people cannot install your tool at all.
- You cannot reproduce an old project, because the environment it needed is gone.

This has a name: **dependency hell**. It is not a sign that you are bad at
computers. It is the normal condition of scientific software.

The consequences are not just annoying, they are research problems. You cannot
reproduce your own results when you are unsure what tools you actually used. You
cannot move to newer resources when your setup isn't portable. And nobody else
can validate or build on your work if it only runs on your machine.

:::{iframe} https://www.youtube.com/embed/HelrQnm3v4g
:width: 100%
"How can software containers help your research?" — a three-minute introduction
from the Australian Research Data Commons, shown during the session.
:::

## What is a container?

Start from what you actually want. You want to install research software without
breaking your system, and without buying another computer. Imagine you could spin
up a separate filesystem and operating system *inside* your current computer, kept
isolated from your main system, containing everything the software needs to run.

That is a container. Docker's own definition:

> A container is a standard unit of software that packages up code and all its
> dependencies so the application runs reliably from one computing environment to
> another.

### The shipping container analogy

Before shipping containers, cargo was mixed, fragile, and chaotic — every load was
a bespoke problem. With standard containers, everything is grouped, portable, and
predictable, and the crane does not care what is inside.

Software containers do the same thing: you ship a complete working setup to any
computer that has Docker installed, and it just works. What that buys you is
**reproducibility**, **portability**, **isolation**, and **consistency**.

Docker is a tool to build and run containers. It is not the only one, but it is
the most widely used.

### Images and containers are not the same thing

This is the distinction that trips people up first:

- A **container** is a running environment — a temporary mini-system.
- A **container image** is the recipe or template that defines what software is
  inside, what dependencies are needed, and how to run it.

The cookie-cutter analogy from the session:

:::{mermaid}
flowchart LR
    D[Dockerfile<br/>the written recipe] -->|docker build| I[Image<br/>the cookie cutter]
    I -->|docker run| C1[Container]
    I -->|docker run| C2[Container]
    I -->|docker run| C3[Container]
:::

The image is the cutter, a container is a cookie. One cutter makes as many
identical cookies as you like. Containers come and go; the image stays the same.

## Getting Docker

Two options, and for a first look the first one is enough:

- [labs.play-with-docker.com](https://labs.play-with-docker.com) — an online
  playground, nothing to install.
- A local installation of Docker Desktop.

Check it is working:

```bash
docker --version
```

## The commands you actually need

Everything below is from the live walkthrough in the session.

**Finding your way around.** Docker's built-in help is genuinely good, and worth
using before searching online:

```bash
docker --help
docker container --help
```

**Getting an image.** Images come from [Docker Hub](https://hub.docker.com) unless
you say otherwise:

```bash
docker image ls
docker image pull hello-world
```

**Running a container.** Each of these starts a container from an image, runs one
thing, and exits:

```bash
docker container run hello-world
docker container run alpine
docker container run alpine cat /etc/os-release
docker container run alpine echo 'Hello World'
```

That third command is worth pausing on — `cat /etc/os-release` reports the
operating system *inside* the container, which is Alpine Linux, whatever your own
machine happens to be running.

**Going inside.** The `-it` flags give you an interactive shell in the container:

```bash
docker container run -it alpine sh
```

Try `ls` and `whoami` once you are in. You are root, in a filesystem that is not
yours, and nothing you do there touches your own machine.

**Seeing what is running, and cleaning up:**

```bash
docker container ls          # running containers
docker container ls --all    # including stopped ones
docker image ls
docker image rm <imageid>
```

:::{warning} `prune` is not reversible
`docker container prune` removes **all** stopped containers, not just the one you
were thinking of. The same applies to the image and volume equivalents. Read what
it asks before confirming.
:::

## Writing a Dockerfile

A Dockerfile is a text file with the instructions to build an image. The shape of
one:

```docker
FROM python:3.9
COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
CMD ["python", "app.py"]
```

Each line is a step: start from an existing image, copy your code in, set the
working directory, install what you need, and define what runs by default.

## Exercise 1: hello-docker

The smallest complete example. Two files.

`app.py`:

```python
print("Hello from inside a Docker container!")
```

`Dockerfile`:

```docker
# Use official Python image as base
FROM python:3.10-slim

# Set working directory in container
WORKDIR /app

# Copy script into container
COPY app.py .

# Set the default command to run
CMD ["python", "app.py"]
```

Build the image, then run a container from it:

```bash
docker build -t hello-docker .
docker run hello-docker
```

Output:

```
Hello from inside a Docker container!
```

Note what you did *not* do: install Python. The `python:3.10-slim` base image
brought its own, at a version you named and can name again next year.

## Exercise 2: getting results back out

A container is disposable — when it exits, anything it wrote inside is gone. That
is a problem the moment your code produces output you care about. The answer is a
**volume**: a directory on your machine mounted into the container.

`writer.py`:

```python
with open("/data/output.txt", "w") as f:
    f.write("Saved from inside the container!\n")

print("File written to /data/output.txt")
```

`Dockerfile`:

```docker
FROM python:3.10-slim

WORKDIR /app

COPY writer.py .

CMD ["python", "writer.py"]
```

Build it, then run it with your local `data/` folder mounted at `/data` inside the
container:

```bash
docker build -t volume-demo .
docker run -v $(pwd)/data:/data volume-demo
```

The terminal reports `File written to /data/output.txt`, and — this is the point —
`output.txt` is now in your own `data/` folder on your own machine, containing
`Saved from inside the container!`.

The general form is `-v <host path>:<container path>`. The same mechanism works in
reverse: mount your code into a container so you can edit it normally and rerun
without rebuilding the image each time.

## Networking: a Jupyter server in one command

By default containers are isolated from the network. Publishing a port with `-p`
connects a port inside the container to one on your machine:

```bash
docker run -p 8888:8888 -e JUPYTER_TOKEN='mypassword' jupyter/base-notebook
```

Then open `http://localhost:8888/?token=mypassword`. That is a complete, working
Jupyter environment that you did not install and cannot break, and deleting it
afterwards leaves nothing behind.

## Why this is worth the effort

Pulling the session's wrap-up together — what containers give a researcher:

- **Documentation.** A Dockerfile is a clear, repeatable record of the tools and
  dependencies your work needs. Unlike a written description, it cannot drift out
  of date without breaking, which means you find out.
- **Portability.** It runs on any system with Docker: Mac, Windows, Linux.
- **Reproducibility.** The same software and the same environment, everywhere.
- **Configurability.** You can allocate CPU and memory to suit a cluster or a
  small machine.

:::{seealso} Where to go next
- [Docker overview](https://docs.docker.com/get-started/docker-overview/)
- [Dockerfile reference](https://docs.docker.com/reference/dockerfile/)
- [Volumes in Docker](https://docs.docker.com/engine/storage/volumes/)

Once your work runs in a container, running it on a bigger machine stops being a
separate problem — see [Cloud computing](cloud-computing.md).
:::
