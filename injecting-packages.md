# Injecting Packages

**Outcome: one tool's world gains a
guest package that no other Python on
the machine can see — and you
understand why that pattern, called
injecting, is how Python tools take
plugins.** Transcript:
`pipx-lab/transcripts/t07-inject.txt`.

## Why a world would need a guest

Some tools are built to be extended.
Their extra abilities live in separate
packages: a plugin that adds a feature,
a library the tool can use if it finds
it. Under the old shared-shelf way,
extending a tool was natural — just
`pip install` the plugin next to it.
Under pipx, the shelf is private, so
the question is fair: how does anything
else get *in*?

Deliberately, through the door marked
`inject`:

```bash
pipx inject <tool-package> <guest-package>
```

## The lab's guest

The lab gave pycowsay a guest: the
`cowsay` package, a different cow
implementation with its own ideas.
Watch the wording of the receipt —
*injected into*, not installed:

```bash
pipx inject pycowsay cowsay
```

```text
installing cowsay...
done! ✨ 🌟 ✨
  injected package cowsay into venv pycowsay
```

pipx opened pycowsay's world, put
`cowsay` and anything it needed onto
that world's shelf, recorded the guest
in the world's `pipx_metadata.json`,
and closed the door.

## Proving the guest's address

`import` is the test, as in chapter 6.
Inside the host's world, the guest
answers — and performs:

```bash
~/.local/share/pipx/venvs/pycowsay/bin/python -c "import cowsay; cowsay.cow('a guest in the pycowsay venv')"
```

```text
  ____________________________
| a guest in the pycowsay venv |
  ============================
                            \
                             \
                               ^__^
                               (oo)\_______
                               (__)\\       )\/\\
                                   ||----w |
                                   ||     ||
```

Everywhere else, the guest does not
exist:

```bash
~/.local/share/pipx/venvs/httpie/bin/python -c "import cowsay"
python3 -c "import cowsay"
```

```text
ModuleNotFoundError: No module named 'cowsay'
ModuleNotFoundError: No module named 'cowsay'
```

The host's neighbour cannot see it. The
system cannot see it. The guest lives
at exactly one address — which is what
a plugin needs: present where the host
looks, absent everywhere it would be
clutter.

## The honest fine print

Two things the lab found while doing
this. First, plain `pipx list` in
pipx 1.4.3 does not itemise guests —
it lists host packages only. The record
of a guest lives in the host's
`pipx_metadata.json`:

```bash
python3 -c "import json; m=json.load(open('$HOME/.local/share/pipx/venvs/pycowsay/pipx_metadata.json')); print(list(m['injected_packages']))"
```

```text
['cowsay']
```

(Newer pipx releases surface guests in
`pipx list` directly; the metadata file
is the record either way.)

Second, the lab's guest was a library,
proven at the import line. Guests can
also bring **commands** of their own,
and pipx can expose those as shims
too, with `--include-apps` — and
`--include-deps` pulls in the
dependencies' commands as well. Those
two flags are documented here rather
than run; the shape is the same, with
more doors planted in `~/.local/bin`.

## The rule for guests

A guest shares its host's fate.
Upgrade the host and pipx keeps the
guest list in mind; remove the host —
chapter 10 — and the guest leaves with
it, unlisted and unmissed, because a
guest has no life of its own. Choose
guests the way you chose the host:
chapter 1's trust rule applies to
everything that runs as you, guests
included.
