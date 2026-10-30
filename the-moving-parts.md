# The Moving Parts

**Outcome: given any file pipx has
made, you can say which part it belongs
to and what breaks if it vanishes.**
pipx has five parts. None of them is
complicated; the confusions in chapter
14 are nearly all two parts being
mistaken for each other.

## The world (the venv)

Each tool gets a **virtual
environment**, universally shortened to
**venv**: a folder holding a Python
interpreter (linked to a real one), a
`bin/` folder of commands, and a
`lib/` folder whose `site-packages` is
that tool's private library shelf.

The lab's two worlds sat here:

```text
~/.local/share/pipx/venvs/
  pycowsay/
    bin/  include/  lib/  lib64/
    share/  pyvenv.cfg  pipx_metadata.json
  httpie/
    ...the same shape...
```

The whole world is self-describing.
`pyvenv.cfg` is the venv's own
birth certificate — which Python it was
made from. `pipx_metadata.json` is
pipx's notebook entry for this tool
(chapter 2's fourth part, below). The
tool itself differs wildly in size —
the lab measured 108 KB for pycowsay's
world and 28 MB for httpie's, which
carried twenty-odd libraries — but the
shape never changes.

Delete a world folder by hand and the
tool is gone — though its shim stays
behind, pointing at nothing, until
pipx tidies it. Chapter 10 removes
tools the clean way instead.

## The shim

A **shim** is a link in
`~/.local/bin` — pipx's
**PIPX_BIN_DIR** — named after the
tool's command and pointing into the
world:

```text
$ ls -l ~/.local/bin/pycowsay
lrwxrwxrwx ... /home/hatch/.local/bin/pycowsay -> /home/hatch/.local/share/pipx/venvs/pycowsay/bin/pycowsay
```

Follow the link and the target is a
small script whose first line names the
world's own Python:

```text
#!/home/hatch/.local/share/pipx/venvs/pycowsay/bin/python
```

That first line — the **shebang** — is
the mechanism. The shell finds the shim
because `~/.local/bin` is on your PATH,
the kernel follows the link, the script
starts under the world's Python, and
that Python looks for libraries on the
world's shelf only. Nothing is copied
into `~/.local/bin` but the link.

A package can provide several commands.
httpie's package planted three shims —
`http`, `https` and `httpie` — plus
links to its manual pages under
`~/.local/share/man`. One package, one
world, however many doors.

## The shared tools

Watch pipx work and a line recurs:

```text
creating shared libraries...
upgrading shared libraries...
```

Every world needs pip itself, to
install with. Instead of giving each
world its own copy of pip and its
companions, pipx keeps **one shared
set** at `~/.local/share/pipx/shared`
and lends it to each world as the world
is built or upgraded. It is the one
deliberate exception to "nothing is
shared" — and it is safe to share
because no tool ever *runs* against it.
It is scaffolding, not shelving: the
tools a building crew passes around,
not the rooms the tenants live in.

## The notebook

pipx remembers each world in the
world's own `pipx_metadata.json`: the
package name, the exact **spec** it was
installed from (`httpie==3.2.2` in the
lab's case), the Python used, the
commands exposed, and any **injected**
packages (chapter 8's guests). This
file is how `pipx upgrade` knows what
"this tool, newer" means, and how
`pipx list` can describe worlds without
opening them. When chapter 8's guest
did not appear in `pipx list`, it was
this file that held the record:

```text
['cowsay']
```

## The map

All of pipx's folders are defaults, and
every one can be moved by an
**environment variable**. Rather than
memorising them, ask pipx to draw the
map:

```bash
pipx environment
```

The lab's map, trimmed to the lines
that matter:

```text
PIPX_HOME=/home/hatch/.local/share/pipx
PIPX_BIN_DIR=/home/hatch/.local/bin
PIPX_MAN_DIR=/home/hatch/.local/share/man
PIPX_SHARED_LIBS=/home/hatch/.local/share/pipx/shared
PIPX_LOCAL_VENVS=/home/hatch/.local/share/pipx/venvs
PIPX_LOG_DIR=/home/hatch/.local/state/pipx/log
PIPX_DEFAULT_PYTHON=/usr/bin/python3
```

Two warnings live in this map. First,
`PIPX_DEFAULT_PYTHON` is the Python new
worlds are built from — when a machine
has several Pythons, that line decides
which one a tool is married to
(chapter 13 picks up the consequences).
Second, if you set these variables
*after* installing tools, pipx will
look for its worlds in the new place
and find nothing — the tools are not
lost, just filed under the old map.
Decide the map first, or leave it
alone.

## The check

Name the parts on your own machine
after chapter 4's first install:

```bash
ls ~/.local/share/pipx/venvs
ls -l ~/.local/bin
pipx environment
```

A folder per tool in the first, a link
per command in the second, and the map
that ties them together in the third.
