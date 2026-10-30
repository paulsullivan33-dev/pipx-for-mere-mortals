# Running Without Installing

**Outcome: you can run a tool once —
for a single command, on a machine you
may never touch again — and leave
nothing behind but a cache.** Not every
tool deserves a permanent world. Some
you need for one afternoon, one
tutorial, one experiment. `pipx run`
is for them. Transcript:
`pipx-lab/transcripts/t03-run.txt`.

## The one-shot

```bash
pipx run pycowsay "run once, keep nothing"
```

The first time, the full ceremony
plays out:

```text
creating virtual environment...
creating shared libraries...
upgrading shared libraries...
installing pycowsay...

  ----------------------
< run once, keep nothing >
  ----------------------
   \   ^__^
    \  (oo)\_______
       (__)\       )\/\
           ||----w |
           ||     ||
```

pipx built a world, installed the tool
into it, ran the command with the rest
of your words as its arguments — and
did not plant a shim. The evidence,
checked immediately after:

```bash
pipx list
```

```text
nothing has been installed with pipx 😴
```

And there is no `pycowsay` link in
`~/.local/bin`. The tool ran; the tool
is not installed. Both halves of that
sentence are the feature.

## The cache

Run the same one-shot again:

```bash
pipx run pycowsay "cached and quick"
```

This time there is no ceremony at all —
the cow speaks immediately. The
throwaway world was not thrown all the
way away: pipx keeps run-worlds in a
cache (`~/.cache/pipx`, the
`PIPX_VENV_CACHEDIR` from chapter 2's
map), keyed by what you asked for, and
reuses a matching one. A repeat run is
a warm start; a run asking for a
different version builds a different
world. The cache is a cache — pipx
ages entries out, and deleting the
folder costs nothing but the next
run's few seconds.

## Run versus install

The rule of thumb the lab used:

- **Install** the tools that are part
  of your working life. They get a
  receipt, a notebook entry, upgrades
  in chapter 9, and a clean removal in
  chapter 10.
- **Run** the tools you are trying,
  following along with, or using once.
  If a run-tool earns a permanent
  place, installing it afterwards is
  one command — the two paths do not
  conflict.

Two more run shapes, told from pipx's
documentation rather than the lab's
transcripts. If the command's name
differs from the package's name, or you
want a particular version for the run,
`--spec` separates the two:
`pipx run --spec some-package==1.2
some-command`. And `pipx run` can fetch
a script from a URL and run it in a
world built for it — convenient, and
squarely under chapter 1's trust rule:
a URL run is running a stranger's
program as you, in one step, so read
the script first or do not run it.

## The check

Try a tool you have never installed,
then prove its absence:

```bash
pipx run pycowsay "here and gone"
pipx list
```

Installed tools list themselves; the
one-shot does not. That silence is the
receipt.
