# Keeping Tools Fresh

**Outcome: a tool crosses a real
version step under your control, the
whole shelf can be refreshed in one
command, and "already at latest" reads
as the healthy answer it is.**
Transcript: `pipx-lab/transcripts/t08-upgrade.txt`.

## One tool, one step

Chapter 5 installed httpie at 3.2.2 on
purpose. Here is the payoff — first the
before, so the after means something:

```bash
http --version
pipx upgrade httpie
http --version
```

```text
3.2.2
upgrading shared libraries...
upgrading httpie...
upgraded package httpie from 3.2.2 to 3.2.4 (location:
/home/hatch/.local/share/pipx/venvs/httpie)
3.2.4
```

Under the cover, pipx consulted the
notebook: the world was built from the
spec `httpie==3.2.2`... wait — does a
pinned spec freeze the tool forever?
No, and this transcript is the proof.
`pipx upgrade` re-reads the spec as
*the package, at its newest allowed
version*: the pin recorded what was
installed, and the upgrade replaced
httpie **and its dependencies** inside
the same world with the newest release
PyPI offers, rewriting the notebook to
match. The shim never moved; the world
behind it was restocked. (A pin in the
spec still matters at *reinstall* time,
and chapter 13 is honest that upgrade
follows PyPI's latest rather than any
pin — if you truly need a tool frozen,
the discipline is to not upgrade it,
and `pipx list` will keep showing you
the version you froze.)

The version commands bracket the
upgrade because chapter 6's habit
applies to change as much as to
installs: claim, step, verify.

## Already current is an answer

```bash
pipx upgrade pycowsay
```

```text
upgrading shared libraries...
upgrading pycowsay...
pycowsay is already at latest version 0.0.0.2 (location:
/home/hatch/.local/share/pipx/venvs/pycowsay)
```

Not an error, not a shrug — a definite
statement with the version in it.
pycowsay's newest release *is* 0.0.0.2;
the tool is small, old and finished,
which is a respectable state for a
program to be in.

## The whole shelf at once

```bash
pipx upgrade-all
```

```text
upgrading shared libraries...
upgrading pycowsay...
upgrading httpie...
Versions did not change after running 'pipx upgrade' for each package 😴
```

One command walks every world in
`PIPX_LOCAL_VENVS`, upgrading each.
Run monthly, it is most of pipx
maintenance. The sleepy summary line
is the sound of a shelf in good order.

## The heavier hammers, documented

Two relatives of `upgrade` are told
here rather than run. **`pipx
reinstall-all`** rebuilds every world
from its notebook entry — same
versions, fresh rooms. Its hour comes
after a **Python upgrade**: a world is
married to the Python that built it
(chapter 13), and when your system
Python moves from 3.12 to 3.13, the
worlds still point at the old one.
Reinstall-all remarries them.
Individually, **`pipx install --python
python3.13`** names the Python a world
is built with, for machines carrying
several.

Both are the same pattern as
everything else in this guide: pipx
knows what it built, wrote it down,
and can therefore rebuild it. The
notebook is what makes a shelf of
little worlds manageable instead of
merely tidy.

## The check

```bash
pipx list
```

After any upgrade round, the list is
the ledger: each package, its current
version, its commands. If the ledger
and the tool's own `--version` agree,
the shelf is as you left it.
