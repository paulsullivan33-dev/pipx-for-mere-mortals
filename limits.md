# Limits

**Outcome: you can state what pipx
does not do — plainly enough that none
of it surprises you later.** Every
limit here follows from the design,
which is the honest kind of limit to
have.

## Tidiness is not safety

The fence separates libraries. It does
not separate the tool from *you*. A
pipx-installed program runs with your
account's full rights — your files,
your network, your keys — exactly as
if you had run it any other way. pipx
is not a sandbox, not a container, not
a permission system, and a malicious
or careless tool inside a venv is a
malicious or careless tool with a tidy
library shelf. Chapter 1's trust rule
is not decoration; it is the whole of
the security model.

## The fence is Python-shaped

pipx installs tools that PyPI
distributes as Python packages. Tools
written in other languages, tools
distributed as bare binaries, and
graphical applications with system
libraries to satisfy are outside its
shape — those belong to your system
package manager, or to their own
installers. A Python tool that needs a
non-Python system library can still
struggle inside a world: the venv
isolates Python's shelf, and the
system library was never on it.

## A world per tool costs disk

Separation is paid in copies. The lab
measured 108 KB for a toy and 28 MB
for httpie — and httpie's shelf held
libraries, like `requests`, that other
tools on a shared shelf would each
have borrowed from one copy. Under
pipx, every tool that uses `requests`
carries its own. Twenty tools can mean
twenty copies. The shared scaffolding
(chapter 2) softens the overhead per
world, not the duplication of the
tools' own dependencies. On modern
disks this is usually a fair trade —
disk is cheaper than breakage — but it
is a real line in the ledger, and on a
small machine it adds up visibly.

## Worlds age with their Python

A world is built from one Python —
`PIPX_DEFAULT_PYTHON` on chapter 2's
map — and its inner paths name that
Python's version. When your system
moves from Python 3.12 to 3.13, the
old worlds do not follow by
themselves; their interpreter links
point where they pointed. pipx's
answer is the rebuild —
`reinstall-all`, chapter 9 — which is
quick because the notebook remembers
everything. But it is a step, and
forgetting it is a known way to meet
chapter 14's stranger errors after a
system upgrade.

## pipx versions vary by shelf

The lab's pipx was 1.4.3, Ubuntu
24.04's snapshot; PyPI's pipx is newer,
and small behaviours differ between
them — chapter 8 met one, where
`pipx list` did not itemise guests.
Nothing in this guide depends on a
behaviour the lab did not run, but a
reader on a different pipx may find a
receipt worded differently or a list
slightly fuller. The worlds, shims
and verbs are stable across versions;
the polish is not.

## One user, one machine

pipx is a per-user tool. Worlds live
in your home folder, shims on your
PATH; nothing is installed for other
accounts on the machine, and nothing
synchronises between machines. A team
that wants identical tools everywhere
needs a list — the specs, kept in a
note or a script of `pipx install`
lines — and each machine builds its
own worlds from it. The notebook makes
that list easy to write; pipx does not
write it for you.
