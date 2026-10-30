# The Neighbours

**Outcome: you can name the other
roads to a Python tool on your PATH,
what each one shares with pipx, and
where each one stops.** These tools
are characterised, not benchmarked —
with one exception: the first
neighbour's blocked door was run for
real in chapter 3.

## pip itself, and its user shelf

`pip install --user` was the old road:
the tool and its libraries into
`~/.local/lib`, its commands into
`~/.local/bin`. One user shelf for
everything, which means every fight
from chapter 1, held in your home
folder instead of the system's. On a
PEP 668 machine the road is closed
outright — the lab's transcript t00 is
the barrier, photographed. pip remains
the right tool *inside* a project or a
venv, which is a different job: feeding
code you are writing, not stocking
tools you run. pipx is, literally, pip
plus the discipline of one venv per
tool, automated.

## The hand-rolled world

Everything pipx does can be done by
hand: `python3 -m venv` a folder,
install the tool into it with its pip,
and link the command into
`~/.local/bin` yourself. People did
exactly this for years; pipx began as
that habit, written down as a program.
What the hand-rolled version lacks is
the notebook and the verbs — no
recorded spec, no `upgrade`, no
`inject`, no clean `uninstall` that
remembers the man pages too. Doable,
honest, and tedious at five tools,
which is the whole reason this guide
exists.

## uv and uvx

uv is a fast Python package and project
manager written in Rust, and it has a
tool mode of its own: `uv tool install`
plants persistent tools in isolated
environments much as pipx does, and
`uvx` runs one-shot tools much as
`pipx run` does. If you already live in
uv for project work, its tool mode is
the same idea from inside the same
house, and the [uv guide in this
series](https://paulsullivan33-dev.github.io/uv-for-mere-mortals/)
covers it. pipx's ground is being the
one job, done by the tool the Python
community built for it, in every
distribution's packages. The concepts
transfer whole: worlds, shims, and the
trust rule are identical in both.

## The system package manager

Some Python tools are also system
packages — `apt install httpie` exists,
for instance. That road puts the tool
under the system's own care: upgraded
with the system, at the system's pace,
integrated with its manuals and
accounts. The price is the version
lag: a distribution freezes its shelf
at release time, so a system-packaged
tool is a snapshot, sometimes years
behind PyPI. pipx and apt coexist
without friction — they keep separate
shelves — and choosing per tool is
normal: the system package for the
boring infrastructure, pipx for the
tools you want current.

## Homebrew and friends

On macOS, Homebrew carries both pipx
itself and many Python tools as
formulae, and Linuxbrew mirrors that
on Linux. A brew-installed tool is the
system-package idea with a faster
shelf. The neighbours agree more than
they differ: every one of them is a
way of answering *whose shelf, whose
versions, whose cleanup*. pipx's
answers — the tool's own shelf, PyPI's
versions, pipx's cleanup — are simply
the ones tuned to Python tools in
particular.
