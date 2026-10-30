# What pipx Is

**Outcome: you can say what problem
pipx solves, what it builds to solve
it, and the one trust rule that governs
everything after this chapter.** The
check is at the end: three Python
programs on one machine, and you will
know which of them can see which
libraries, and why.

## The shared shelf

A Python program rarely travels alone.
Underneath the part you see sits a pile
of **dependencies** — libraries other
people wrote, which the program borrows
to do its work. A small tool might need
two. A large one can need thirty.

The classic way to install a Python tool
is:

```bash
python3 -m pip install some-tool
```

pip fetches the tool *and* its
dependencies and puts them all in one
place: the **site-packages** folder of
whichever Python you pointed it at.
Every tool installed that way shares
that one shelf. That sharing is the
problem, in three flavours:

- **The fight.** Tool A needs library X
  at version 2. Tool B needs X at
  version 1. The shelf holds one copy.
  The second install wins, and the first
  tool breaks — silently, later, when
  you are doing something else.
- **The mess.** Uninstalling was always
  the weak half of the bargain. pip
  removes the package you name; the
  dependencies it dragged in stay
  behind, ownerless, forever.
- **The danger.** That shelf belongs to
  your *system* Python — the one your
  operating system's own tools are
  written against. Break its libraries
  and you can break the machine's own
  housekeeping.

The third flavour got serious enough
that the Python world acted. **PEP
668** lets a Linux distribution mark
its Python **externally managed**, and
pip then refuses to touch it:

```text
error: externally-managed-environment
```

Chapter 3 shows that refusal in full —
the lab walked into it on purpose. It
is not a bug or a locked door to pick.
It is the system saying: *do not stock
my shelves; build your tools their own
rooms instead.*

## The little worlds

pipx builds those rooms. When you run:

```bash
pipx install some-tool
```

pipx does four things. It creates a
**virtual environment** — a private
Python installation, with its own
site-packages shelf, in a folder pipx
owns. It installs the tool and its
dependencies into that room, and
nowhere else. It finds the commands the
tool provides and plants a **shim** for
each one — a small link — in
`~/.local/bin`, a folder meant to be on
your **PATH**. And it writes down what
it did, so it can upgrade or remove the
whole room later.

From then on you type the tool's name
and the shim leads into the room. The
tool runs with its own Python and its
own libraries, at the versions it
asked for. A second tool lives in a
second room with a second shelf. The
fight cannot happen, because there is
no shared shelf to fight over. Your
system Python is never touched at all.

That is the whole idea. The rest of
this guide is the idea made concrete,
and then taken apart to prove it.

## The trust rule

One thing pipx is **not**: a safety
cage. When a pipx tool runs, it runs as
you, with your account's full rights —
it can read your files, use your
network, and do anything you can do.
The little worlds keep *libraries*
apart. They do not keep *the tool*
away from you.

So the rule, stated before any install
command in this guide: **install with
pipx only what you would run anyway.**
Tools from PyPI — the Python Package
Index, the public shelf pipx fetches
from — are published under names anyone
can register. Check that the name is
the tool you mean, that the project
behind it is the real one, and that you
would trust its authors with a program
running as you. That was always the
rule for installing software; pipx
changes where the libraries live, not
who you are trusting.

## The check

By the end of chapter 5 the lab has
two tools installed: pycowsay and httpie.
Ask the system Python about either:

```bash
python3 -c "import httpie"
```

```text
ModuleNotFoundError: No module named 'httpie'
```

That failure is the success. The tool
is installed, its command runs — and
the system Python has never heard of
it, because none of it is on the
system's shelf. Chapter 6 runs this
test from every side. For now, hold
the shape of it: *installed* and *on
the system* are different things now,
and that difference is the point.
