# Recipes

**Outcome: five jobs people actually
do with pipx, each assembled from
commands the earlier chapters ran and
verified.** Nothing in this chapter is
a new mechanism; it is the old ones,
aimed.

## Try a tool before it moves in

You meet a tool in a tutorial and want
to know if it deserves shelf space:

```bash
pipx run some-tool --help
pipx run some-tool --version
```

Run it properly on a small job. If it
earns its keep, `pipx install
some-tool` gives it a world and a shim;
if not, `pipx list` proves nothing was
ever installed. Trying costs a cache
entry, never a commitment (chapter 7).

## Pin a known-good version

Your notes, your team's runbook or
your course all describe one version:

```bash
pipx install "httpie==3.2.2"
pipx list
```

The pin is in the receipt and the
ledger (chapter 5). Remember chapter
9's finding: `pipx upgrade` will walk
the tool forward to PyPI's latest when
you ask it to, pin notwithstanding —
so a pinned tool's discipline is
simply to leave it out of your upgrade
rounds until you mean to move it.

## Give a tool its plugin

A tool's manual says an ability lives
in a companion package. The host is
installed; the companion goes through
the door marked inject:

```bash
pipx install host-tool
pipx inject host-tool companion-package
```

Then prove the guest's address the
chapter 8 way — importable from the
host's Python, invisible to the system
Python:

```bash
~/.local/share/pipx/venvs/host-tool/bin/python -c "import companion_package"
python3 -c "import companion_package"
```

The first succeeds; the second must
fail. Both results are the recipe
working.

## Spring-clean the shelf

Once in a while, walk the whole shelf:

```bash
pipx list
pipx upgrade-all
pipx list
```

The first list is your inventory —
you will find tools you forgot, which
is information too. Upgrade-all
restocks every world (chapter 9). The
second list is the after-picture.
Anything on it that you no longer use
gets chapter 10's clean exit:

```bash
pipx uninstall forgotten-tool
which forgotten-tool
```

Name, door, room, ledger: the four
checks take ten seconds and turn
"uninstalled, probably" into
"uninstalled".

## Read a tool's shelf

A tool misbehaves and you suspect its
libraries. Inventory the world before
touching anything:

```bash
ls ~/.local/share/pipx/venvs/httpie/lib/python3.12/site-packages
```

The `.dist-info` folders are the
shelf's catalogue cards — each names a
library and its exact version (chapter
5). Quote them in a bug report and you
have told the tool's authors more than
a page of description would. And the
chapter 6 import tests will tell you
whether the suspect library loads at
all:

```bash
~/.local/share/pipx/venvs/httpie/bin/python -c "import requests; print(requests.__version__)"
```

If the shelf is right and the tool
still misbehaves, the fault is not the
fence — which is itself a finding
worth having before you change
anything.
