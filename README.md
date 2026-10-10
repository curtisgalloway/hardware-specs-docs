<!--
SPDX-FileCopyrightText: 2026 contributors
SPDX-License-Identifier: CC-BY-4.0
-->

# hardware-specs-docs

Hardware specs, built only from public datasheets, technical reference manuals (TRMs) and
standards, licensed CC-BY-4.0. No spec here cites source code, so you may use them whatever your
own project's license is, with attribution. Each spec lists its documents with a URL and a
SHA-256 hash and cites them by name and page.

**Every fact is cited, and the checker runs in CI.** Each spec here is a YAML file in spec
format 2: every fact is a record whose `support` names the evidence it rests on, a listed
document with a page or section, or lines of a source tree at a pinned commit, and every spec
has a verification record holding each fact's verdict. Every push and pull request runs
driver-lab's `spec.py` over the whole repository: a malformed record, a citation that does not
resolve, a source whose license this repository does not accept, or a fact without a current
verdict fails the build.

**Terms.** A *spec* is a hardware description that driver authors, people or AI agents, read
instead of the original sources. A *fact* is one record in it: a claim with a stable id and its
*support*, the citations it rests on. A *repos entry* (under `resources.repos`) pins a source
tree: its URL, commit and the SPDX license of the cited files (SPDX is the standard
license-identifier language); an *anchor* cites lines of it. An *overlay* adds facts to a spec in
another file or repository. `specs/board-specs.yaml` is this repository's *root marker*: it
states the repository's license and its *accepts list*, the licenses a cited source may carry.
The *license gate* is the check that fails a spec citing a source outside that list. A
*verification record* (`specs/resources/<name>.verify.yaml`) holds a verdict per fact, with a
hash of what the verdict was based on, so a changed fact shows as stale. The *views* are
generated reading copies: a Markdown view and an HTML *viewer*. Full definitions: driver-lab's
[glossary](https://github.com/curtisgalloway/driver-lab/blob/main/GLOSSARY.md).

## The specs

The published site, https://curtisgalloway.github.io/hardware-specs-docs/, is rebuilt from `main` on every merge and every week; its
[index](https://curtisgalloway.github.io/hardware-specs-docs/) lists every page. Each view carries a banner naming the commits it was built
from. Nothing generated is committed here: read the views on the site, or download a pull
request's `spec-views` artifact to read the views of a proposed change.

| Spec | Published views |
|---|---|
| [`specs/bcm2711.spec.yaml`](specs/bcm2711.spec.yaml): soc spec `bcm2711` | [viewer](https://curtisgalloway.github.io/hardware-specs-docs/spec-f2774344b19519b9d11940fa4e83b601635289e9a79262b17be96a0e7f97fd22.html) · [Markdown](https://curtisgalloway.github.io/hardware-specs-docs/spec-f2774344b19519b9d11940fa4e83b601635289e9a79262b17be96a0e7f97fd22.md) |
| [`specs/rk3588.spec.yaml`](specs/rk3588.spec.yaml): soc spec `rk3588` | [viewer](https://curtisgalloway.github.io/hardware-specs-docs/spec-929cffebddf50cdb6e44949b113d03ef977dfcc018c0788864fcd737405fa053.html) · [Markdown](https://curtisgalloway.github.io/hardware-specs-docs/spec-929cffebddf50cdb6e44949b113d03ef977dfcc018c0788864fcd737405fa053.md) |
| [`specs/rock5t.spec.yaml`](specs/rock5t.spec.yaml): board spec `rock5t` | [viewer](https://curtisgalloway.github.io/hardware-specs-docs/spec-a736dde9c97dcaa944de1d4c6771885419ed2bd396ab3d54bdc971b97c9927e7.html) · [Markdown](https://curtisgalloway.github.io/hardware-specs-docs/spec-a736dde9c97dcaa944de1d4c6771885419ed2bd396ab3d54bdc971b97c9927e7.md) |

## Which repo does my spec go in?

**Placement rule:** a spec lives in the most restrictive repository among the sources it anchors
to. A spec may reference repositories with less restrictive licenses, never ones with more
restrictive licenses.

| Repo | License | Anchors allowed | Holds |
|---|---|---|---|
| `hardware-specs-gpl` | GPL-2.0-only | `[src:]` into any GPL-2.0-only or GPL-2.0-or-later tree, plus `[doc:]`, plus anything the permissive repo accepts | Linux-derived specs: references for Linux work, or for anyone who doesn't care about license. Easiest to verify. |
| `hardware-specs-docs` | CC-BY-4.0 (specs); per-file Apache-2.0 SPDX headers on CI files | `[doc:]` only | Specs built only from public datasheets, TRMs and standards |
| `hardware-specs-permissive` | Apache-2.0, plus a NOTICE file for the BSD/ISC/MIT sources | `[src:]` into BSD, ISC, 0BSD, MIT or Apache trees (and `GPL-2.0 OR MIT` files), plus `[doc:]` | TF-A, rpi-tools, Zephyr, FreeBSD, dual-licensed device trees. First material: the bcm2711 overlay (facts 2, 3, 6 below) |

The table is the license-split design's ([LICENSE-SPLIT.md](https://github.com/curtisgalloway/driver-lab/blob/main/docs/LICENSE-SPLIT.md#the-repos)),
verbatim; its "facts 2, 3, 6 below" are three boot-stub facts from BSD-licensed Raspberry Pi
tools, listed in that design's audit of the deleted specs.

This repository accepts no source license (`accepts: []` in [specs/board-specs.yaml](specs/board-specs.yaml)): its specs cite documents only.

In spec format 2, the table's `[src:]` is a `src` or `DT` support entry whose anchors point
into a `resources.repos` entry, and `[doc:]` a document support entry naming a
`resources.documents` entry. A spec here may cite documents only: every fact's support names a `resources.documents` entry of the same file and a locator (page, section, table or clause); no repos entry is accepted.

## What is here

- `specs/`: the spec root. `board-specs.yaml` is the root marker; each spec is `<name>.spec.yaml`, and its verification record is `resources/<name>.verify.yaml`.
- `LICENSE`: the license.
- `LICENSES/Apache-2.0.txt`: the license of the CI files.
- `.github/workflows/checks.yml` and `scripts/checks.sh`: the checks below.
- `.github/workflows/publish.yml`: builds the published views below.

## How a spec is written and checked

The method lives in [driver-lab](https://github.com/curtisgalloway/driver-lab). The contract for specs, the root marker and
verification records is the
[`spec-format`](https://github.com/curtisgalloway/driver-lab/blob/main/skills/spec-format/SKILL.md) skill (its JSON Schemas define each
record's fields); [`spec-verifier`](https://github.com/curtisgalloway/driver-lab/tree/main/skills/spec-verifier) writes the
verification records. CI checks out driver-lab at one pinned commit, installs `spec.py`'s
dependencies from its hash-pinned `skills/spec-format/requirements.txt`, and runs, through
`scripts/checks.sh`:

1. `check`: `spec.py check specs --require-license --require-verified <mode>`. The root
   marker, every spec and record against the schemas, names and references, composition, the
   license gate (every repos entry, cited or not) and each fact's verdict. On pull
   requests the mode is `pr`: a fact with no verdict, a stale one, or one staled by a change in
   another repository it references fails, so nothing merges unverified. On `main` the mode is
   `main`: a fact staled only by another repository's change is a warning, so an upstream merge
   never turns this repository red; the next pull request here must re-verify it. Re-run
   `spec-verifier` after editing a spec. The self-test's synthetic fixtures are exempt.
2. `resolve`: `spec.py resolve` on every spec. Each repos entry an anchor cites is fetched over
   HTTPS at its pinned commit (when `RESOLVE_SRC=1`, which the workflow sets), and every anchor's
   path, line range and symbol, and each cited file's license line, are checked. A repository
   whose fetch exceeds 50 MB (`SRC_FETCH_LIMIT_MB`) or the fetch timeout is skipped and counted
   in the summary; any other failure (a commit, file or symbol that does not exist) fails the
   build.
3. `render`: `spec.py render --format md` and `--format html`, each file's own view: a
   spec that cannot be shown safely fails the build.
4. `self-test`: proves the license gate with this repository's own root marker, on driver-lab's
   format 2 fixture specs copied into temporary roots: the run fails unless the specs that do not
   fit fail with the gate's message and those that fit pass. With `RESOLVE_SRC=1` it also proves
   that anchors really resolve: a real pinned repository is fetched, a good anchor must resolve
   and a bad one fail. The fixtures are never published here as specs.

`.github/workflows/publish.yml` builds both views of every spec with driver-lab's
`skills/spec-format/ci/publish.py`: on pull requests it uploads them as the `spec-views`
artifact; on `main` and weekly it verifies every page against the sources again and deploys the
site to GitHub Pages.

To run the same checks locally:

```bash
git clone https://github.com/curtisgalloway/driver-lab ../driver-lab
python3 -m venv .venv-spec
.venv-spec/bin/pip install --require-hashes -r ../driver-lab/skills/spec-format/requirements.txt
CHECKS_PYTHON=.venv-spec/bin/python RESOLVE_SRC=1 scripts/checks.sh all ../driver-lab
```

`scripts/checks.sh --mode main ...` applies the `main` policy instead of the default `pr`. The
script needs bash and a Python with exactly the pinned packages (the interpreter in
`CHECKS_PYTHON`, otherwise `python3`); without them every step exits 3 (`missing dependency`).
Exit codes: 0 passed, 1 a check failed, 2 usage error, 3 missing precondition.

Check out the driver-lab commit that `.github/workflows/checks.yml` pins for an identical run.

## License

The specs, their verification records and documentation are licensed CC-BY-4.0
([LICENSE](LICENSE)). The CI files (`.github/workflows/checks.yml`,
`.github/workflows/publish.yml`, `scripts/checks.sh`) are licensed Apache-2.0
([LICENSES/Apache-2.0.txt](LICENSES/Apache-2.0.txt)). Each file states its license in an
`SPDX-License-Identifier` line.
