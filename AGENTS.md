<!--
SPDX-FileCopyrightText: 2026 contributors
SPDX-License-Identifier: CC-BY-4.0
-->

# hardware-specs-docs

Instructions for coding agents working in this repository. The user guide is the
[README](README.md).

- **What this is:** published hardware specs in spec format 2, licensed CC-BY-4.0. The method lives
  in [driver-lab](https://github.com/curtisgalloway/driver-lab): the contract for specs, the
  root marker and verification records is its `skills/spec-format/SKILL.md` (with the JSON
  Schemas it names, which define every record's fields), `spec.py` is the tool, and
  `spec-verifier` writes the verification records. Read the contract before writing or changing
  a spec; do not restate it here.
- **Placement first:** apply the README's placement rule before adding a spec. A spec citing a
  source this repository's `accepts:` does not list belongs in another spec repository, or in
  none.
- **What a spec here may cite:** documents only. The root accepts no source tree
  (`accepts: []`), so a spec has no `resources.repos` entry: list each document under
  `resources.documents` (name, class, title, URL, SHA-256 and page count when the bytes are
  available) and cite it in a fact's `support` by `doc: <name>` with a locator (`at:` page,
  section, table or clause). Free-text citations in `claim` carry no evidence.
- **Two licenses:** specs, the root marker and prose are CC-BY-4.0; the CI files
  (`.github/workflows/checks.yml`, `.github/workflows/publish.yml`,
  `scripts/checks.sh`) are Apache-2.0. Keep each file's header
  with its kind.
- **Names:** every spec is `specs/<name>.spec.yaml` (`kind` says what it is; an overlay uses a
  filename distinct from its target's id when the target is in this root), and its verification
  record is `specs/resources/<name>.verify.yaml`. A fact's `id` is stable: never reuse or rename
  one, since verdicts and other repositories' references name it. After editing a spec, re-run
  `spec-verifier` on the changed facts (`spec.py status --stale` lists them).
- **Generated views:** the Markdown view and the viewer are built by CI (`publish.yml`) and
  published to the repository's GitHub Pages site; never commit or hand-edit a view.
- **Checks:** `CHECKS_PYTHON=<venv>/bin/python RESOLVE_SRC=1 scripts/checks.sh all <driver-lab
  checkout>` runs what CI runs; the venv holds driver-lab's
  `skills/spec-format/requirements.txt`, installed with `pip install --require-hashes`. The
  default `--mode pr` is the pull-request policy; CI runs `--mode main` on `main`. CI pins
  driver-lab to the commit in `.github/workflows/checks.yml`, and `publish.yml`'s `TOOL_COMMIT`
  names the same commit: bump both together, and check against that commit.
- **Do not** widen `accepts:` in `specs/board-specs.yaml` to make a spec pass: that changes the
  repository's license policy, which is the user's decision. Do not add the self-test's fixture
  specs to `specs/`.
- **License headers:** every file except the license texts carries
  `SPDX-FileCopyrightText: 2026 contributors` and its `SPDX-License-Identifier`. In a spec, a
  verification record or the root marker they are YAML comment lines (`# SPDX-...`) at the top
  of the file; in Markdown, an HTML comment.
- **Privacy:** this repository is public. No host names, addresses, user names or home paths in
  any file or commit message.
- **Git:** a topic branch and a pull request per change; push or open a pull request only on the
  user's explicit "push".
