# Removing Tools

**Outcome: tools leave as cleanly as
they arrived — shim, world and guests
together — and you have verified the
leaving from every side, because an
uninstall you do not check is a claim,
not a fact.** Transcript:
`pipx-lab/transcripts/t09-uninstall.txt`.

## One tool

```bash
pipx uninstall pycowsay
```

```text
uninstalled pycowsay! ✨ 🌟 ✨
```

One line, but several things happened:
the shim in `~/.local/bin` was
removed, the links to its manual pages
were removed, the world at
`~/.local/share/pipx/venvs/pycowsay`
was deleted whole, and the notebook
entry went with it. Nothing was left
orphaned on anyone's shelf — the old
shared-shelf mess from chapter 1,
where dependencies outlived their
tool, cannot happen here, because the
dependencies lived in the world and
the world is gone.

## Verify the leaving

Four checks, all from the lab's
transcript, and each answers a
different question:

```bash
ls -l ~/.local/bin/pycowsay
ls ~/.local/share/pipx/venvs
which pycowsay
pipx list
```

```text
ls: cannot access '/home/hatch/.local/bin/pycowsay': No such file or directory
httpie
(pipx list now shows httpie alone)
```

The link is gone, so the PATH has no
door. The world folder is gone — only
httpie's remains. `which` finds
nothing, which is your shell agreeing.
And the ledger shows the one tool that
is still a resident. An uninstall is
believed when all four agree.

## The guest leaves with the host

pycowsay's world held chapter 8's
guest, `cowsay`. There was no separate
step for it, and that is the design: a
guest is an entry in its host's
notebook and a folder on its host's
shelf, so the host's removal is the
guest's removal. If you ever need a
guest gone but its host kept, that is
a rebuild — uninstall the host, install
it again, inject only what you still
want. Guests are cheap to re-add;
silent leftovers are the thing pipx
exists to prevent.

## Everything at once

```bash
pipx uninstall-all
```

```text
uninstalled httpie! ✨ 🌟 ✨
```

The lab finished here: `pipx list`
back to *nothing has been installed*,
the venvs folder empty, `http` and
`https` gone from `~/.local/bin`. What
survives an uninstall-all is pipx
itself and its furniture — the shared
scaffolding, the cache of chapter 7's
run-worlds, the line `ensurepath`
added to your shell's startup file. If
you are removing pipx from a machine
entirely, those are the traces to walk
back by hand: `sudo apt remove pipx`
(or your package manager's equivalent),
then delete `~/.local/share/pipx` and
the startup-file line. Nothing else on
the machine was ever touched — which,
eleven chapters in, is the least
surprising sentence in this guide.

## The check

The removal habit, worth keeping for
every tool you will ever uninstall
anywhere: name check (`which`), door
check (the shim), room check (the
folder), ledger check (`pipx list`).
Four agreements, then believe it.
