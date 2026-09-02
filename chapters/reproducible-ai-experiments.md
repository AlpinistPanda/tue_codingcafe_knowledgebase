---
title: Reproducible AI experiments
subtitle: Why your model gives a different answer every time, and what to pin
short_title: Reproducible AI
kernelspec:
  name: python3
  display_name: Python 3
---

You run a training script twice and get two different accuracies. Nothing changed.
This is the single most common reproducibility complaint with machine learning
code, and it usually comes down to three sources of variation that are easy to
control once you know where they are.

:::{note} What you'll get out of this
A short checklist you can apply to any experiment, and a demonstration of why
"I set the seed" is often not enough.
:::

## The problem, demonstrated

Here is an experiment with no seed set. Run this page twice and the numbers change.

```{code-cell} python
import numpy as np

def experiment():
    """A stand-in for a training run: draw some data, report a summary."""
    data = np.random.normal(loc=0.0, scale=1.0, size=1000)
    return data.mean()

[round(experiment(), 4) for _ in range(3)]
```

Three runs, three answers. That is expected — but it means that if you report one
number in a paper, nobody, including you, can get it back.

## Source 1: the global random seed

The obvious fix is to seed the generator:

```{code-cell} python
np.random.seed(42)
first = [round(experiment(), 4) for _ in range(3)]

np.random.seed(42)
second = [round(experiment(), 4) for _ in range(3)]

first, second
```

Identical. But notice what actually happened: the *sequence* is reproducible, not
each individual call. The three values within a run still differ from each other,
because they consume the same stream in order. If you later insert another random
call anywhere earlier in your script, every number after it shifts.

:::{warning} Global seeds are fragile
`np.random.seed()` sets state on a single global generator that every library
sharing it will consume from. Adding one shuffle at the top of your script
silently changes every result below it.
:::

## Source 2: generator state that isn't global

The modern fix is an explicit generator you pass around, so each component draws
from its own stream:

```{code-cell} python
def experiment_rng(rng):
    data = rng.normal(loc=0.0, scale=1.0, size=1000)
    return data.mean()

# Two independent, individually reproducible streams.
rng_a = np.random.default_rng(seed=42)
rng_b = np.random.default_rng(seed=42)

round(experiment_rng(rng_a), 4) == round(experiment_rng(rng_b), 4)
```

Now adding a random call elsewhere in the program cannot perturb this one. For
anything with more than one stochastic component — data splitting, augmentation,
initialisation, dropout — give each its own seeded generator.

## Source 3: everything that isn't your code

Seeds do not control:

- **Library versions.** A change in a default argument between minor versions will
  move your numbers. Pin them: `numpy==2.1.3`, not `numpy`.
- **Hardware and thread count.** Floating-point addition is not associative, so
  summing in a different order gives a different last decimal. Parallel reductions
  on GPU reorder freely, and results vary between GPU models.
- **Non-deterministic kernels.** Several common deep-learning operations have
  non-deterministic implementations by default. PyTorch exposes
  `torch.use_deterministic_algorithms(True)` to force the slower deterministic
  path; frameworks differ in how completely they honour this.

```{important}
Bit-for-bit reproducibility across different machines is usually not achievable
for GPU training, and chasing it is rarely worth the cost. Aim for *the same
machine and environment gives the same answer*, and report the variation across
seeds rather than a single number.
```

## Report variation, not a point estimate

If your result changes across seeds, that spread is information about your method,
not noise to be hidden. Run several seeds and report the distribution:

```{code-cell} python
scores = [experiment_rng(np.random.default_rng(seed=s)) for s in range(10)]

print(f"mean of run means : {np.mean(scores): .4f}")
print(f"std  of run means : {np.std(scores): .4f}")
print(f"range             : [{min(scores): .4f}, {max(scores): .4f}]")
```

A method that beats a baseline by less than its own seed-to-seed spread has not
been shown to beat it.

## Checklist

Before an experiment counts as reproducible:

- [ ] Every stochastic component has an explicit, passed-in seeded generator
- [ ] Dependencies pinned to exact versions in `requirements.txt` or a lockfile
- [ ] The seed(s) used are recorded alongside the results, not just in the code
- [ ] Results reported across several seeds, with the spread
- [ ] The environment is captured — a container or lockfile, not a prose list
- [ ] Hardware noted where it matters (GPU model, thread count)

:::{seealso}
Environment capture is its own topic — containers, lockfiles, and what to do when
someone hands you a folder of scripts and no dependency list. That belongs in a
future chapter.
:::
