# A Bigger Tool

**Outcome: a real working tool with
twenty-odd dependencies is installed,
has made a real HTTP request, and was
deliberately installed at an older
version — because choosing a version
is a skill this chapter teaches on
purpose.** Transcript:
`pipx-lab/transcripts/t05-install-httpie.txt`.

## The tool: httpie

httpie is a command-line HTTP client —
a friendlier way to talk to web servers
than curl for most everyday purposes.
It is a good second tool for three
reasons: it is genuinely useful, its
dependency pile is big enough to prove
chapter 6's point at scale, and one
package provides three commands, which
revises any idea that package and
command are the same thing.

## Choosing a version on purpose

A plain `pipx install httpie` takes
the newest release PyPI offers. Often
that is right. But the string you hand
to `pipx install` is a **spec**, and it
can carry a version condition, exactly
as pip's can. The lab pinned one:

```bash
pipx install "httpie==3.2.2"
```

```text
creating virtual environment...
installing httpie from spec 'httpie==3.2.2'...
done! ✨ 🌟 ✨
  installed package httpie 3.2.2, installed using Python 3.12.3
  These apps are now globally available
    - http
    - httpie
    - https
  These manual pages are now globally available
    - man1/http.1
    - man1/httpie.1
    - man1/https.1
```

Why would anyone do this? Because
"newest" is a moving target and
sometimes you need the target to hold
still: the version your team's notes
describe, the one a tutorial matches,
the last one before a change you are
avoiding. The spec is written into the
tool's `pipx_metadata.json`, so pipx
remembers the choice — chapter 9 shows
what `pipx upgrade` then does with it.
(The lab had a second motive: an older
version gives chapter 9 a genuine
upgrade to perform.)

Note the receipt's app list: **http**,
**httpie** and **https** — three doors
into one world, three shims in
`~/.local/bin`. The command you will
actually type, `http`, is not the
package's name. pipx tells you the
names at install time; that receipt is
the only sure list.

## The check: a real request

The lab's machine had no website of
its own to test against, so it borrowed
one: Python's own throwaway file
server, started in a scratch folder
for a minute and then stopped.

```bash
python3 -m http.server 8901 --directory ~/workspace/pipx-labwork &
http --ignore-stdin --headers GET http://127.0.0.1:8901/t01.sh
```

```text
HTTP/1.0 200 OK
Server: SimpleHTTP/0.6 Python/3.12.3
Date: Fri, 30 Oct 2026 06:05:23 GMT
Content-type: text/x-sh
Content-Length: 805
Last-Modified: Fri, 30 Oct 2026 06:01:33 GMT
```

A real server answered a real request
and httpie printed its half of the
conversation. `--headers` asks for just
the response headers; `--ignore-stdin`
tells httpie not to wait for a request
body on standard input — in a script
or a pipeline it otherwise pauses,
watching an empty stdin, and chapter 14
tells the story of the lab learning
that the slow way. Both version
commands agree about the install:

```bash
http --version
https --version
```

```text
3.2.2
3.2.2
```

`https` is httpie wearing its other
hat: the same program, offering the
HTTPS-first behaviour under its own
name. One world, three doors, and the
world's shelf now carries requests,
rich, pygments and their friends —
exactly the pile chapter 6 inventories.
Where pycowsay's world was a closet,
this one is a workshop. Both were
built by the same four steps.
