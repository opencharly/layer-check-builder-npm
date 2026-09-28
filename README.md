# layer-check-builder-npm

An npm-builder check fixture for the `check-builder-vm` R10 bed.

The `check-builder-npm` candy is a **fixture**: its `package.json` declares a
single tiny zero-dependency package (`is-number@7.0.0`) so the built-in npm
detection builder installs it into `~/.npm-global`. Composed onto a `kind: local`
deploy nested in the eval-VM guest, it drives the builder **deploy** leg
(`runVenueBuilderStep` → `runVenueHomeArtifactBuilder`): the host builds the npm
global prefix in the `fedora-builder` image and ships `~/.npm-global` onto the
venue.

`is-number` is zero-dependency, so the install is small and deterministic. No box
composes this candy — it exists solely as the builder-deploy fixture that closes
the "no dedicated bed deploys a pixi/npm/cargo/aur builder candy to a `local:` /
`vm:` venue" gap. It has no `skill:` entity; the owning family skill is
`/charly-check:check`, and the missing entity is tracked by
[opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `check-builder-npm` |
| Install files | `package.json` (declares `is-number@7.0.0`) |
| Effect | npm global install into `~/.npm-global/lib/node_modules/` |
| Plan | one `context: [runtime]` `command:` `check:` probe |
| Owns | 0 `skill:` entities (fixture) |
| Service / port | none |

## How to use it

Compose it as a layer ref in a box's nested `candy:` list (the composition list).
A box is a `candy:` node carrying the box's `base:` image and a nested `candy:`
list of layer refs (the nested `candy:` is the composition list; the outer
`candy:` is the box body):

```yaml
my-box:
  candy:                  # the box body (an IMAGE is a `candy:` node carrying `base:`)
    base: fedora-builder  # the box's base image
    candy:                # the box's composition list
      - '@github.com/opencharly/layer-check-builder-npm:v2026.239.1633'
```

The fixture is driven by the `check-builder-vm` bed, not by a user box.

## Layout

- `charly.yml` — the `check-builder-npm:` candy entity (no `skill:` entity).
- `package.json` — the zero-dependency npm package declaration.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning family skill: `/charly-check:check`
- Missing `skill:` entity: [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291)
- Authoring reference: `/charly-image:layer`
- [`opencharly/marketplace`](https://github.com/opencharly/marketplace) — the projected corpus
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
