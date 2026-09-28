# AGENTS.md — layer-check-builder-npm

Standalone candy repo for the `check-builder-npm` fixture — the npm
builder-deploy fixture for the `check-builder-vm` R10 bed. It ships a
`package.json` declaring `is-number@7.0.0` and carries **no `skill:` entity**.

Canonical files:

- `charly.yml` — the `check-builder-npm:` candy entity (one `context: [runtime]`
  `command:` probe; no `skill:` entity).
- `package.json` — the zero-dependency npm package declaration the builder
  installs.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-check:check` — the owning family skill: the disposable beds, the
  builder-deploy leg, and the check plan authoring reference. Load before editing
  the `plan:` or the fixture package.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, package sections, service declarations).
  Load before editing any entity field or plan step.
- **Missing owning skill:** this fixture has no `skill:` entity, so no
  `/charly-check-builder-npm:*` page is projected for it. The gap is recorded
  against the named batch
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).

## Build / validate / test

- `charly box validate` at the repo root — the structural check: the manifest
  must parse and validate at the installed charly.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has no
  per-repo candy gate. Its only workflow file is
  `.github/workflows/tag-on-merge.yml`.
- The fixture's proof is the `context: [runtime]` `command:` probe — the artifact
  present in the venue's `~/.npm-global` — exercised by the `check-builder-vm`
  bed.

## Modify this repo

- Keep the fixture minimal and deterministic: a zero-dependency package keeps the
  install small and reproducible. A dependency-bearing package would slow the bed
  and add network nondeterminism.
- Keep the `check:` probe aligned with the artifact path the builder writes
  (`$HOME/.npm-global/lib/node_modules/is-number/package.json`).
- If an owning skill is authored, add the `skill:` entity here and update this
  signpost and the README in the same change.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
