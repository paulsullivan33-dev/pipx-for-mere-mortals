# Glossary

**Dependency** — a library a program
needs in order to run, written by
someone else and fetched alongside it.
The thing tools used to fight over.

**Entry point** — a command a package
declares it provides. pipx reads these
to know which shims to plant.

**Environment variable** — a named
value your shell passes to programs.
pipx's folders are steered by them;
`pipx environment` prints the map.

**Externally managed** — a Python
installation its operating system has
marked as not pip's to touch, under
PEP 668. pip refuses, out loud.

**Inject** — pipx's verb for adding a
guest package to an existing tool's
world, onto its private shelf only.

**PATH** — the list of folders your
shell searches, in order, when you
type a command name. A command whose
folder is not on it is "not found",
however installed it may be.

**PEP 668** — the Python proposal that
lets a system mark its Python
externally managed. The wall in
chapter 3, with a number.

**PyPI** — the Python Package Index,
the public repository pip and pipx
fetch packages from. Names there are
registered by their owners; chapter
1's trust rule applies.

**Shared libraries (pipx's)** — the
one set of install-time tools (pip and
companions) that pipx keeps centrally
and lends to every world it builds.
Scaffolding, not shelving.

**Shebang** — the first line of a
script, beginning `#!`, naming the
interpreter that should run it. In a
pipx shim's target, it names the
world's own Python.

**Shim** — a small link in
`~/.local/bin`, named after a command,
pointing into a tool's world. The only
part of an installed tool your PATH
ever touches.

**Site-packages** — the folder where a
Python keeps its installed libraries:
that Python's shelf. Every venv has
its own, which is the whole idea.

**Spec** — the string you hand an
install command naming a package and
optionally its version, as in
`httpie==3.2.2`. pipx records it in the
notebook.

**Venv (virtual environment)** — a
private Python installation in a
folder: its own interpreter link, its
own site-packages, its own commands.
One per tool, under pipx.

**World** — this guide's plain word
for a tool's venv and everything in
it: shelf, commands, notebook entry.
