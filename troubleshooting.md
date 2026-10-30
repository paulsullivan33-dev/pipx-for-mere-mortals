# Troubleshooting

**Outcome: the five confusions that
account for nearly all pipx trouble,
each with the check that names it.**
None of them is a broken pipx. All of
them happened to somebody; three of
them happened in this guide's own lab.

## "Command not found" right after installing

The install receipt said the app is
*globally available*, and your shell
says it has never heard of it. Nine
times in ten the shim exists and your
PATH does not include its folder —
chapter 3's skipped step. Name the
parts separately:

```bash
ls -l ~/.local/bin
echo $PATH
pipx ensurepath
```

If the command is in the first listing
but `~/.local/bin` is not in the
second, run `ensurepath` and open a
**new terminal** — a running shell
does not reread the startup file
ensurepath just edited. If the command
is missing from the first listing too,
the install planted a differently
named app: `pipx list` shows the true
names.

## The externally-managed wall

You (or a tutorial) tried `pip install`
for a tool and met:

```text
error: externally-managed-environment
```

That is chapter 3's wall, and it is
the system working correctly. Do not
reach for `--break-system-packages`;
the error's own text points at the
intended road. The translation is
mechanical: wherever a tutorial says
`pip install <tool>`, read `pipx
install <tool>` — for tools, that is.
Libraries for a project you are
writing are a different job, done
inside that project's own venv.

## The tool that waits, silent, for ten seconds

The lab installed httpie, pointed it
at a local server, and watched it
pause before answering — with this
note, once it finally moved:

```text
warning: no stdin data read in 10.0s (perhaps you want to --ignore-stdin)
```

httpie, and tools like it, read a
request body from **standard input**
when stdin is not a terminal — and in
a script, a pipe or a captured lab
session, stdin is never a terminal, so
the tool politely waits on an empty
pipe. The lesson generalises beyond
httpie: a command that hangs in a
script but not at your keyboard is
often reading, or waiting to read,
stdin. The fixes are the tool's flag
(`--ignore-stdin` here) or giving the
command an explicit stdin: `< /dev/null`
at the end of the line.

## The package name is not the command name

`pipx install httpie` gave you `http`.
Other packages are wider of the mark
still. This is not pipx hiding things:
a package declares its commands, and
pipx prints them in the install
receipt — *These apps are now globally
available* — and again in `pipx list`.
When a name you expected is missing,
believe the receipt over your
expectation. The mirror trap belongs
to `pipx run`: chapter 7's `--spec`
exists for exactly the case where the
command and the package part company.

## A tool broke after the system Python upgraded

The world was built by Python 3.12;
the machine now runs 3.13, and the
tool fails in ways that mention its
own guts. Chapter 13 named the cause:
worlds are married to their Python.
The remedy is the rebuild, which works
because the notebook remembers the
specs:

```bash
pipx reinstall-all
```

One tool at a time, `pipx install
--force --python python3.13 <package>`
remarries a single world. (This
remedy is documented rather than
lab-run — the lab's machine kept one
Python throughout — but the marriage
it repairs is visible in every world's
`pyvenv.cfg`, which names its Python
in plain text.)

## The check that sorts most of it

Before changing anything, ask the
three chapter 6 questions: does
`pipx list` show the tool, at what
version, with which apps? Does the
shim exist in `~/.local/bin`? Does the
tool's own Python import the tool?
The pattern of yes and no among those
three names the fault — PATH, install,
or world — and most fixes in this
chapter are just the named fault,
repaired.
