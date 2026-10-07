<!--
SPDX-FileCopyrightText: 2026 contributors
SPDX-License-Identifier: CC-BY-4.0
-->

# hardware-specs-docs

Instructions for coding agents working in this repository. The user guide is the
[README](README.md).

- **What this is:** published hardware specs, licensed CC-BY-4.0. The method (how a spec is
  written, the anchor grammar, the checkers, verification) lives in
  [driver-lab](https://github.com/curtisgalloway/driver-lab): the `anchored-peripheral-spec` skill for peripheral specs, and
  `board-expert`'s `SPEC-FORMAT.md` for board specs and the root marker. Read those before writing
  or changing a spec; do not restate them here.
- **Placement first:** apply the README's placement rule before adding a spec. A spec citing a
  source this repository's `accepts:` does not list belongs in another spec repository, or in
  none.
- **What a spec here may cite:** documents only. The root accepts no source tree
  (`accepts: []`), and under `--require-license` every document anchor must be named: list the
  document under `docs:` in the spec's front matter (name, title, URL, SHA-256, pages) and cite
  `[doc:<name> p.N]`. A free-text `[doc: Title §4]` fails here.
- **Two licenses:** specs, the root marker and prose are CC-BY-4.0; the CI files
  (`.github/workflows/checks.yml`, `scripts/checks.sh`) are Apache-2.0. Keep each file's header
  with its kind.
- **Names:** peripheral specs `specs/<device>-spec.md`; board specs `specs/<id>.spec.md`
  (`spec_check.py` reads every `*.spec.md` as a board spec).
- **Checks:** `scripts/checks.sh all <driver-lab checkout>` runs what CI runs. CI pins driver-lab
  to the commit in `.github/workflows/checks.yml`; check against that commit.
- **Do not** widen `accepts:` in `specs/board-specs.yaml` to make a spec pass: that changes the
  repository's license policy, which is the user's decision. Do not add the self-test's fixture
  specs to `specs/`.
- **License headers:** every file except the license texts carries
  `SPDX-FileCopyrightText: 2026 contributors` and its `SPDX-License-Identifier`, as an HTML
  comment in Markdown, after the front matter when there is one.
- **Privacy:** this repository is public. No host names, addresses, user names or home paths in
  any file or commit message.
- **Git:** a topic branch and a pull request per change; push or open a pull request only on the
  user's explicit "push".
