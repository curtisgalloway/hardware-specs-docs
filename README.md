<!--
SPDX-FileCopyrightText: 2026 contributors
SPDX-License-Identifier: CC-BY-4.0
-->

# hardware-specs-docs

Hardware specs built only from public datasheets, technical reference manuals (TRMs) and
standards, licensed CC-BY-4.0. No spec here cites source code, so you may use them whatever your
own project's license is, with attribution. Each spec lists its documents with a URL and a
SHA-256 hash and cites them by name and page.

**Every claim is anchored, and the checker runs in CI.** Each fact in a peripheral spec here names the page of a listed document it rests on; each fact in a board spec carries a tag for its kind of source and, for documents, names the document. Every push and pull
request runs driver-lab's checkers over the whole repository: a malformed anchor or tag, or a
source whose license this repository does not accept, fails the build. There are no specs here
yet; they are being regenerated with the current tools.

**Terms.** A *spec* is a hardware description that driver authors, people or AI agents, read
instead of the original sources; a *peripheral spec* covers one device, a *board spec* a board or
chip. An *anchor* is a citation inside a spec: `[src:<pin>: path:L]` points at a line of a source
tree, `[doc:<name> p.N]` at a page of a document. A *pin* (`Source pin: linux@<commit>
GPL-2.0-only`) names that tree, its commit and the license of the cited files, written in SPDX,
the standard license-identifier language. `specs/board-specs.yaml` is this repository's *root
marker*: it states the repository's license and its *accepts list*, the licenses a cited source
may carry. The *license gate* is the check that fails a spec citing a source outside that list.
Full definitions: driver-lab's [glossary](https://github.com/curtisgalloway/driver-lab/blob/main/GLOSSARY.md).

## Which repo does my spec go in?

**Placement rule:** a spec lives in the most restrictive repository among the sources it anchors
to. A spec may reference repositories with less restrictive licenses, never ones with more
restrictive licenses.

| Repo | License | Anchors allowed | Holds |
|---|---|---|---|
| `hardware-specs-gpl` | GPL-2.0-only | `[src:]` into any GPL-2.0-only or GPL-2.0-or-later tree, plus `[doc:]`, plus anything the permissive repo accepts | Linux-derived specs: references for Linux work, or for anyone who doesn't care about license. Easiest to verify. |
| `hardware-specs-docs` | CC-BY-4.0 (specs); per-file Apache-2.0 SPDX headers on CI files | `[doc:]` only | Specs built only from public datasheets, TRMs and standards |
| `hardware-specs-permissive` | Apache-2.0, plus a NOTICE file for the BSD/MIT sources | `[src:]` into BSD, MIT or Apache trees (and `GPL-2.0 OR MIT` files), plus `[doc:]` | TF-A, rpi-tools, Zephyr, FreeBSD, dual-licensed device trees. First material: the bcm2711 overlay (facts 2, 3, 6 below) |

The table is the license-split design's ([LICENSE-SPLIT.md](https://github.com/curtisgalloway/driver-lab/blob/main/docs/LICENSE-SPLIT.md#the-repos)),
verbatim; its "facts 2, 3, 6 below" are three boot-stub facts from BSD-licensed Raspberry Pi
tools, listed in that design's audit of the deleted specs.

This repository accepts no source license (`accepts: []` in [specs/board-specs.yaml](specs/board-specs.yaml)): its specs cite documents only.

## What is here

- `specs/`: the specs and the root marker. Peripheral specs are named `<device>-spec.md`, board
  specs `<id>.spec.md`.
- `LICENSE`: the license.
- `LICENSES/Apache-2.0.txt`: the license of the CI files.
- `.github/workflows/checks.yml` and `scripts/checks.sh`: the checks below.

## How a spec is written and checked

The method lives in [driver-lab](https://github.com/curtisgalloway/driver-lab): the
[`peripheral-spec`](https://github.com/curtisgalloway/driver-lab/tree/main/skills/peripheral-spec) skill writes and
verifies peripheral specs and defines the anchor grammar;
[`SPEC-FORMAT.md`](https://github.com/curtisgalloway/driver-lab/blob/main/skills/board-expert/SPEC-FORMAT.md) defines board specs and
the root marker. CI checks out driver-lab at one pinned commit and runs, through
`scripts/checks.sh`:

1. `spec_check.py specs --require-license`: the root marker's license fields and every
   board spec, including the board-spec license gate on `resources.repos` licenses.
2. `anchor_check.py <spec> --root specs --require-license` on every peripheral spec: anchors,
   pins and the license gate. CI has no checkout of the cited source trees, so it checks the
   anchors' form and licenses; resolving each `[src:]` line against its tree is part of
   verification (the skill's verify step).
3. A self-test that proves the gate works with this repository's own root marker: driver-lab's
   fixture specs that fit and do not fit this repository are copied into a temporary root, and
   the run fails unless the misfits fail with the gate's message and the fits pass. The fixtures
   are never published here as specs.

To run the same checks locally:

```bash
git clone https://github.com/curtisgalloway/driver-lab ../driver-lab
scripts/checks.sh all ../driver-lab
```

The script needs bash and python3 (standard library only).

Check out the driver-lab commit that `.github/workflows/checks.yml` pins for an identical run.

## License

The specs and documentation are licensed CC-BY-4.0 ([LICENSE](LICENSE)). The CI files
(`.github/workflows/checks.yml`, `scripts/checks.sh`) are licensed Apache-2.0
([LICENSES/Apache-2.0.txt](LICENSES/Apache-2.0.txt)). Each file states its license in an
`SPDX-License-Identifier` line.
