# Installing pipx

**Outcome: pipx itself is installed,
states its version, and its shim
folder is on your PATH — the step
every other chapter quietly assumes.**
The lab's transcript for this chapter
is `pipx-lab/transcripts/t01-install-pipx.txt`,
with the PATH step in `t02-ensurepath.txt`.

## First, watch the old road close

pipx is a Python program, so the
instinct is to install it the Python
way. On a modern Ubuntu, try it and you
meet PEP 668 in person:

```bash
python3 -m pip install --user pipx
```

```text
error: externally-managed-environment

× This environment is externally managed
╰─> To install Python packages system-wide, try apt install
    python3-xyz, where xyz is the package you are trying to
    install.
    ...
    If you wish to install a non-Debian packaged Python application,
    it may be easiest to use pipx install xyz, which will manage a
    virtual environment for you. Make sure you have pipx installed.

note: If you believe this is a mistake, please contact your Python
installation or OS distribution provider. You can override this, at
the risk of breaking your Python installation or OS, by passing
--break-system-packages.
```

Read it once and it teaches the whole
guide: the system protects its shelf,
the error itself prescribes pipx as
the way to install Python applications,
and the override it names —
`--break-system-packages` — is a risk
it warns you about in the same breath.
This guide does not use the override.
There is a cleaner door.

## The package route

On Ubuntu and Debian, pipx is a system
package like any other:

```bash
sudo apt install pipx
```

Fedora, Arch and macOS (through
Homebrew) carry it the same way. The
lab ran **pipx 1.4.3**, Ubuntu 24.04's
package. One honest wrinkle from the
lab bench: the machine this guide was
written on is shared, and its package
lock was held by another user's `apt`
every time the lab tried, so the lab
downloaded the same two `.deb` files
apt would have used — `pipx` and its
one missing companion, `python3-userpath`
— and unpacked them into a scratch
folder, running the identical pipx from
there. Nothing else in this guide
changed because of it. On your own
machine the one line above is the
whole step.

The check:

```bash
pipx --version
```

```text
1.4.3
```

A word about versions. A distribution
package is a snapshot: Ubuntu 24.04's
pipx is 1.4.3, and PyPI offers newer
releases of pipx itself. Every command
in this guide works in 1.4.3, because
1.4.3 is what ran them. Newer pipx
releases add polish — richer `pipx
list` output among it — but no chapter
here depends on the difference, and
taking pipx from your distribution
means it is upgraded with everything
else. Prefer the package.

## The step everyone skips: ensurepath

pipx plants its shims in
`~/.local/bin`. If that folder is not
on your **PATH**, your shell will
report every installed tool as *command
not found* — the single most common
pipx confusion, and it is a PATH
problem, not a pipx problem. Fix it
once, now:

```bash
pipx ensurepath
```

```text
Success! Added /home/hatch/.local/bin to the PATH environment variable.
...
You will need to open a new terminal or re-login for the PATH changes to take
effect.
```

`ensurepath` edits your shell's startup
file for you — the lab's `~/.bashrc`
gained exactly two lines:

```text
# Created by `pipx` on 2026-10-30 06:01:37
export PATH="$PATH:/home/hatch/.local/bin"
```

Open a new terminal afterwards, as the
message says; a running shell does not
reread its startup file. If
`~/.local/bin` was already on your
PATH, `ensurepath` says so and changes
nothing — running it is always safe.

While you are here, meet the map from
chapter 2:

```bash
pipx environment
```

If `PIPX_DEFAULT_PYTHON` names the
Python you expected, you are done.
Chapter 4 puts a first tool in its
world.
