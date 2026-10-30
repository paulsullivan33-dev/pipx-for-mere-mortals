# Proving the Isolation

**Outcome: you have tested the fence
from every side — system Python, each
tool's Python, and across the fence
between two tools — and seen it hold
four times out of four.** Believing
pipx works is optional after this
chapter; the tests are yours to rerun
whenever you doubt it. The transcript
is `pipx-lab/transcripts/t06-isolation.txt`,
with httpie installed in chapter 5.

## The claim, restated

pipx says: a tool's libraries live on
its world's shelf, and nowhere else —
not on the system's shelf, not in any
other tool's world. That is a strong
claim, and it has a precise test. Every
Python can be asked what it can
**import** — load as a library. If the
fence is real, each Python's answers
form a pattern with no exceptions.

## Side one: the system knows nothing

```bash
python3 -c "import httpie"
python3 -c "import pycowsay"
```

```text
ModuleNotFoundError: No module named 'httpie'
ModuleNotFoundError: No module named 'pycowsay'
```

Both tools are installed. Both commands
run. And the system Python cannot see
either one's code. Nothing leaked onto
the shared shelf — because pipx never
used it.

## Side two: each world knows its own

Each world's Python lives in its
world's `bin/`. Ask each about itself:

```bash
~/.local/share/pipx/venvs/httpie/bin/python -c "import httpie, requests; print('httpie venv sees httpie', httpie.__version__, 'and requests', requests.__version__)"
~/.local/share/pipx/venvs/pycowsay/bin/python -c "import pycowsay; print('pycowsay venv sees pycowsay')"
```

```text
httpie venv sees httpie 3.2.2 and requests 2.34.2
pycowsay venv sees pycowsay
```

Note the second import in the httpie
line: `requests`, httpie's most famous
dependency, is present on httpie's
shelf at the version httpie wanted.
That is the fight from chapter 1,
prevented — httpie's `requests` can be
any version at all without touching
any other program on the machine.

## Side three: across the fence

The sharpest test. If the worlds truly
are separate, then pycowsay's Python
should not be able to borrow httpie's
`requests`, and httpie's Python should
not know pycowsay exists:

```bash
~/.local/share/pipx/venvs/pycowsay/bin/python -c "import requests"
~/.local/share/pipx/venvs/httpie/bin/python -c "import pycowsay"
```

```text
ModuleNotFoundError: No module named 'requests'
ModuleNotFoundError: No module named 'pycowsay'
```

Four questions, four correct answers.
The shelf each Python sees is exactly
its own world's, no more and no less.

## What a shelf actually holds

Open the biggest world the lab built:

```bash
ls ~/.local/share/pipx/venvs/httpie/lib/python3.12/site-packages
```

Twenty-nine entries: `requests`,
`urllib3`, `certifi`, `idna`,
`charset_normalizer` (the HTTP stack),
`rich`, `pygments`, `markdown_it`,
`mdurl` (the pretty output), plus
`httpie` itself and a card catalogue of
`.dist-info` folders recording each
library's version. One entry is worth
knowing: `pipx_shared.pth`, the thread
back to chapter 2's shared scaffolding.

And the cost of all this separation:

```bash
du -sh ~/.local/share/pipx/venvs/pycowsay ~/.local/share/pipx/venvs/httpie
```

```text
108K    .../venvs/pycowsay
28M     .../venvs/httpie
```

A world costs what its tool weighs.
Chapter 13 weighs the consequences;
for now the point is that the price is
paid per tool, in disk, instead of by
every other tool, in breakage.

## The check

Whenever a pipx tool misbehaves in a
way that smells like a library problem,
these are the questions to ask, in
order: does the tool's own Python
import the tool? Does it import the
dependency in question? Does the system
Python import either (it should not)?
The answers localise the fault to one
world — or clear the fence entirely.
