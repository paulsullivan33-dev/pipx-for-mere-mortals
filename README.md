# pipx for Mere Mortals

Python's command-line tools — the
formatters, downloaders, linters and
little utilities you run by name — are
programs with appetites. Each one needs
its own set of supporting libraries, and
the traditional way to install one is
`pip install`, which puts the tool and
all its libraries on **one shared
shelf**: your system Python's shelf, or
your user shelf. Two tools that want
different versions of the same library
cannot both win. One install quietly
breaks another, and the breakage turns
up weeks later, in a tool you were not
even touching. Modern Linux systems have
stopped allowing it at all: they mark
their Python **externally managed** and
refuse the install outright.

**pipx** gives every tool **its own
little world** instead. It builds a
private **virtual environment** — a
sealed-off Python with its own library
shelf — for each tool you install, and
puts one small link, a **shim**, in a
folder on your **PATH**. You type the
tool's name as always. The shim leads
into the tool's own world, where its
libraries are exactly the ones it asked
for, and no other tool can disturb them.
Removing the tool removes the whole
world, cleanly, and your system Python
never knew it was there.

This guide explains how pipx works,
installs it, and then installs two real
tools with it — a tiny one and a
many-dependency one — and takes the
isolation apart to prove it is real:
import tests run from the system Python,
from each tool's own Python, and across
the fence between the two tools. It
runs a tool without installing it,
tucks a guest package into one tool's
world and proves the guest exists there
and nowhere else, upgrades one tool
across a real version step, and removes
everything again, checking at each stage
that what should be gone is gone.

One line is drawn early, in chapter 1,
and it matters: a pipx tool runs as
*you*, with your full rights on your
machine. The little worlds separate
libraries, not permissions. pipx is a
tidiness tool, not a safety cage, and
the guide never pretends otherwise.

The lab behind this guide ran the real
thing: **pipx 1.4.3**, the Ubuntu 24.04
package, on a small Linux machine with
Python 3.12.3. Against it the lab hit
the externally-managed refusal in full,
installed **pycowsay** and **httpie**
(one of them pinned to an older version
on purpose), made a real HTTP request
with the result, injected the `cowsay`
package into pycowsay's world, upgraded
httpie from 3.2.2 to 3.2.4, and then
uninstalled the lot. One honesty note:
the lab machine is shared, and its
package lock was held by another user
every time the lab tried, so the lab
downloaded the same two `.deb` files
`apt` would have used and unpacked them
in place — the identical pipx, run from
a scratch folder, with every other step
unchanged. `criteria.md` keeps the full
ledger of what was run and what was
only read.

What the lab could not do, it did not
fake. Installing pipx on Windows or a
Mac, the newest pipx release direct from
PyPI, and the neighbour tools of
chapter 12 are taught from their
documentation and labelled as such
wherever they appear.

## What this guide covers

- [**What pipx is**](what-pipx-is.md) —
  the shared-shelf problem, the little
  worlds that solve it, and the trust
  rule that comes before everything
- [**The moving parts**](the-moving-parts.md) —
  the world, the shim, the shared tools
  pipx lends out, the notebook it keeps,
  and the map `pipx environment` draws
- [**Installing pipx**](installing-pipx.md) —
  the package route, the pip route and
  the refusal it now meets, and
  `ensurepath`, the step everyone skips
- [**Your first tool**](your-first-tool.md) —
  pycowsay moves in, and the shim is
  taken apart link by link
- [**A bigger tool**](a-bigger-tool.md) —
  httpie with its twenty-odd libraries,
  three commands from one package, and
  choosing a version on purpose
- [**Proving the isolation**](proving-the-isolation.md) —
  import tests from three Pythons, one
  tool's private library shelf, and the
  size of two worlds
- [**Running without installing**](pipx-run.md) —
  one-shot tools in throwaway worlds,
  and the cache that makes the second
  run quick
- [**Injecting packages**](injecting-packages.md) —
  a guest in one tool's world, proven
  present there and absent everywhere
  else — the plugin pattern
- [**Keeping tools fresh**](keeping-tools-fresh.md) —
  one upgrade across a real version
  step, the whole shelf at once, and
  "already at latest" as a normal answer
- [**Removing tools**](removing-tools.md) —
  uninstall, verified from every side,
  and why a guest leaves with its host
- [**Recipes**](recipes.md) — try before
  you install, pin a known-good version,
  give a tool a plugin, spring-clean
  the shelf, and read a tool's shelf
- [**The neighbours**](the-neighbours.md) —
  pip and its blocked door, the
  hand-rolled world, uv's tool mode, and
  the system package manager — what each
  does that pipx doesn't, and vice versa
- [**Limits**](limits.md) — tidiness not
  safety, a Python-shaped fence, disk
  spent per tool, and worlds that age
  with their Python
- [**Troubleshooting**](troubleshooting.md) —
  the command that is not found, the
  externally-managed wall, the tool that
  waits for input, and the package whose
  name is not its command
- [**Glossary**](glossary.md) — the
  terms, collected in one place

## What it doesn't cover

Writing Python programs or managing a
project's libraries — that is project
work, with its own tools and its own
guides, and pipx is deliberately not
one of them: it installs finished tools
for your shell, and a pipx world is not
a place to develop in. Publishing your
own tool to PyPI is likewise a different
story told elsewhere. And pipx is not a
security boundary: chapter 13 is plain
about what the isolation does and does
not protect.

## How to use this guide

Read chapter 1 for the model and
chapter 2 before installing anything —
knowing which part lives where turns
later confusion into a named problem.
Chapters 3 and 4 get you pipx and a
first tool in one sitting. Chapter 6 is
the one to read slowly: its import
tests are the evidence that the whole
idea works, and they double as the
checks to run whenever a tool behaves
as if the fence had a hole in it. The
lab's transcripts live in
`pipx-lab/transcripts/`, numbered in
the order the chapters use them.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [Python for Mere Mortals](https://paulsullivan33-dev.github.io/python-for-mere-mortals/)
- [uv for Mere Mortals](https://paulsullivan33-dev.github.io/uv-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/pipx-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/pipx-for-mere-mortals/blob/main/LICENSE-CODE).
