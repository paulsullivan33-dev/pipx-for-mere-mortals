# Writing criteria for this guide

Every "for Mere Mortals" guide is written to the same nine-point
standard, so a reader who trusts one can trust the rest. Here it
is, with the verification notes for this particular guide below.

1. **Fix it first.** Reader outcomes lead, not action items: each
   chapter opens by naming the outcome it delivers, shows a check
   the reader can run to confirm they got it, and a reasonable
   reader finishes able to state what was done, what it means,
   and what to try next.
2. **Plain language.** Short sentences, everyday vocabulary,
   technical terms defined on first use. No slang, and no
   idioms that only make sense in one country — the reader may
   be translating as they go.
3. **No dead ends.** A guide never tells the reader to do
   something it cannot show them how to verify. Every claim a
   reader relies on — an install, a command, an output — is
   something this guide demonstrates or clearly labels
   documented-only below.
4. **Live-verified where it matters.** Procedures are run for
   real in the guide's lab wherever the build machine allows it,
   and the transcripts kept in the repository are the evidence.
   What could not be run is listed honestly below, never
   presented as if it had been.
5. **General audiences.** Nothing in this guide — examples,
   fixtures, names, addresses — is modelled on any particular
   reader or author's real life, network, or organisations.
   The lab's tools are two well-known public packages and a
   third public package as a guest.
6. **Standalone.** A reader needs this guide, its lab, and
   nothing else. Where another guide would genuinely help, it
   is pointed to once, by name, and the chapter still completes
   without it.
7. **Honest about limits.** Chapter 13 names what the tool
   cannot do: the isolation covers libraries and not
   permissions, the fence is Python-shaped, each world costs
   disk in duplicated libraries, worlds age with the Python
   that built them, and pipx behaviours polish-differ between
   a distribution's snapshot and PyPI's latest.
8. **The ethics rule is in the text, early.** pipx's version of
   the rule is about trust, and chapter 1 states it before any
   install command appears: a pipx tool runs as you, with your
   full rights, so install only what you would run anyway, and
   treat a `pipx run` of a fetched script as running a
   stranger's program — because it is.
9. **Builds clean.** The site builds with MkDocs in strict mode
   with no warnings, and a fresh reader following the chapters
   in order reaches the stated outcome of each.

## Verification notes for this guide

The build machine is a 2-CPU, 7.7 GB Ubuntu 24.04 sandbox,
shared with other work while the lab ran (timings are therefore
ranges, and the guide says so). Python is 3.12.3. pipx **1.4.3**
is Ubuntu 24.04's own package (candidate 1.4.3-1, shown in
transcript t01). One deviation, disclosed wherever it matters:
the sandbox's dpkg frontend lock was held by another user's
apt process on every attempt across the lab session, so
`sudo apt install pipx` could not complete; the lab instead
downloaded the identical `.deb` files apt would have used —
`pipx_1.4.3-1_all.deb` and `python3-userpath_1.9.1-1_all.deb`,
the one runtime companion not already present — and unpacked
them into a scratch tree, running the same pipx 1.4.3 bytes
from there. Every subsequent step used pipx's own defaults
(`~/.local/share/pipx`, `~/.local/bin`), and all transcripts
record exactly what ran. All transcripts live in
`pipx-lab/transcripts/`.

Run for real, with transcripts as evidence:

- The PEP 668 refusal in full: `python3 -m pip install --user
  pipx` failing with `externally-managed-environment`, the
  system Python shown to carry `/usr/lib/python3.12/EXTERNALLY-MANAGED`
  (t00).
- pipx 1.4.3 stating its version; `pipx ensurepath` adding
  `~/.local/bin` to PATH via two lines in `~/.bashrc`; the full
  `pipx environment` map (t01, t02).
- `pipx run pycowsay` with nothing installed: the full build
  ceremony, the cow, `pipx list` reporting nothing installed,
  no shim planted, and a second run served instantly from the
  cache (t03).
- `pipx install pycowsay`: the receipt (0.0.0.2, Python
  3.12.3, app and man page exposed), the shim shown by
  `ls -l` and `readlink` to be a symlink into the venv, the
  target script's shebang naming the venv's own Python, the
  tool run from PATH, and the venv's top-level layout (t04).
- `pipx install "httpie==3.2.2"`: three apps (`http`,
  `httpie`, `https`) and three man pages from one package;
  `http --version` and `https --version` both 3.2.2; a real
  request — `http --ignore-stdin --headers GET` against a
  local `python3 -m http.server` — returning its 200 OK
  headers (t05).
- The isolation proof (t06): `pipx list` for both tools; the
  system Python failing to import either package; httpie's
  venv Python importing httpie 3.2.2 and requests 2.34.2;
  pycowsay's venv Python importing pycowsay; pycowsay's venv
  failing to import requests and httpie's venv failing to
  import pycowsay; httpie's full site-packages inventory; venv
  sizes 108K (pycowsay) and 28M (httpie).
- `pipx inject pycowsay cowsay` (t07): the injected receipt;
  the guest imported and run inside pycowsay's venv only,
  failing to import from httpie's venv and the system Python;
  plain `pipx list` shown not to itemise the guest, whose
  record was read from the venv's `pipx_metadata.json`.
- Upgrades (t08): `pipx upgrade httpie` carrying 3.2.2 to
  3.2.4, bracketed by `http --version` on both sides;
  `pipx upgrade pycowsay` answering "already at latest version
  0.0.0.2"; `pipx upgrade-all` walking both worlds and
  reporting no changes.
- Removals (t09): `pipx uninstall pycowsay` verified by the
  missing shim, the venvs listing, `which`, and `pipx list`;
  the injected guest leaving with its host with no separate
  step; `pipx uninstall-all` returning the machine to "nothing
  has been installed", venvs folder empty.

Documented only — described from the projects' documentation,
not run, and the text does not claim otherwise:

- Installing pipx on Windows and macOS, and pipx releases
  newer than the distribution's 1.4.3 (including their richer
  `pipx list` output for injected packages).
- `pipx run --spec` with a differing package and command, and
  `pipx run` of a script fetched from a URL.
- `pipx inject --include-apps` and `--include-deps`.
- `pipx reinstall-all`, `pipx install --python`, and the
  Python-upgrade repair in chapter 14 that uses them; the
  marriage they repair is itself visible in the lab's
  transcripts (each venv's Python is named in its receipts and
  shebangs).
- Moving pipx's home with PIPX_HOME and friends; the variables
  and defaults were printed by `pipx environment` (t02) but
  no relocation was performed.
- The neighbour tools of chapter 12 (uv's tool mode, Homebrew,
  system-packaged Python tools, the hand-rolled venv), except
  pip itself, whose PEP 668 refusal is transcript t00.
- Named real-world plugins (such as httpie's) as inject
  targets; the lab's guest was a library package, proven at
  the import line.

Fixtures are this guide's own and deliberately generic: two
stock tool packages, one stock guest package, demonstration
strings spoken by cows, and a throwaway file server that
served the lab's own scratch folder for one request. No
names, organisations or real events appear.
