# Your First Tool

**Outcome: one small tool is installed
the pipx way, runs from your PATH, and
you have taken its shim apart and seen
exactly how the trick is done.** The
lab's transcript is
`pipx-lab/transcripts/t04-install-pycowsay.txt`.

## The tool: pycowsay

pycowsay is a toy — it prints a cow
saying your words, after a program from
computing prehistory. Toys are the
right first install: no configuration,
no accounts, and if anything went
wrong, nothing matters. It is also
genuinely tiny, which makes its world
easy to inspect.

## The install

```bash
pipx install pycowsay
```

```text
creating virtual environment...
installing pycowsay...
done! ✨ 🌟 ✨
  installed package pycowsay 0.0.0.2, installed using Python 3.12.3
  These apps are now globally available
    - pycowsay
  These manual pages are now globally available
    - man6/pycowsay.6
```

Read the receipt; pipx always writes
one. It names the package and the
version it chose, the Python the world
was built with, and — most usefully —
the **apps now globally available**:
the command names this package
provides. Those names are what you
type. They are not always the package
name (chapter 14 has the trap), so the
receipt is worth reading every time.
Even pycowsay brought a manual page,
which pipx linked into
`~/.local/share/man` alongside the
shim.

Behind that output, the four steps
from chapter 1 happened: a world was
created at
`~/.local/share/pipx/venvs/pycowsay`,
the package and its dependencies went
onto that world's shelf, a shim was
planted, and the notebook was written.

## Run it

In a terminal where `~/.local/bin` is
on the PATH (chapter 3's `ensurepath`,
plus a fresh terminal):

```bash
pycowsay "installed, and on the PATH"
```

```text
  --------------------------
< installed, and on the PATH >
  --------------------------
   \   ^__^
    \  (oo)\_______
       (__)\       )\/\
           ||----w |
           ||     ||
```

That is the whole user experience of
pipx, and it is meant to be: the tool
behaves like any command on your
machine. Everything interesting is in
how it got there.

## Take the shim apart

Three checks, run once, teach the
mechanism better than any diagram:

```bash
ls -l ~/.local/bin/pycowsay
readlink ~/.local/bin/pycowsay
head -1 ~/.local/share/pipx/venvs/pycowsay/bin/pycowsay
```

```text
lrwxrwxrwx ... /home/hatch/.local/bin/pycowsay -> /home/hatch/.local/share/pipx/venvs/pycowsay/bin/pycowsay
/home/hatch/.local/share/pipx/venvs/pycowsay/bin/pycowsay
#!/home/hatch/.local/share/pipx/venvs/pycowsay/bin/python
```

The `l` at the start of the first line
and the `->` in the middle say the shim
is a **symbolic link**, not a copy.
`readlink` names its target: a script
inside the world's `bin/`. And the
script's first line — its shebang —
names the world's own Python as its
interpreter. So the chain, when you
type `pycowsay`, is: shell finds the
link on your PATH, kernel follows it,
script runs under the world's Python,
and that Python can only see the
world's shelf.

The world itself, listed:

```bash
ls ~/.local/share/pipx/venvs/pycowsay
```

```text
bin  include  lib  lib64  pipx_metadata.json  pyvenv.cfg  share
```

Chapter 2 named each of those. The
next chapter installs a tool big
enough to make the proof interesting,
and the chapter after opens `lib/`
and proves the fence around it holds —
from both sides.
