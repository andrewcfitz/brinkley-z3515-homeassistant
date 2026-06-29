# Brinkley Z3515 + Home Assistant — Repo Design

**Date:** 2026-06-29
**Status:** Approved structure, pending spec review

## Purpose

A public, single-author documentation repo capturing everything about
connecting Home Assistant to a 2026 Brinkley Z3515 fifth wheel. It serves two
roles at once: a record of *what works* and a *discovery log* of what is still
being reverse-engineered.

This is a personal notebook that happens to be public. Other Brinkley owners
may read it; there is no contribution scaffolding. If it later graduates into a
community resource, that is a cheap addition.

## Non-Goals

- Not a software package. No build, no CI, no tests.
- Not a polished community guide (yet). No CONTRIBUTING.md, issue templates,
  or PR ceremony.
- Not an official Brinkley resource. The README states this plainly.

## Audience

Primarily the author. Secondarily, the next Brinkley Z3515 owner who wants
their rig talking to Home Assistant and is searching the web for prior art.

## Repository

- **Name:** `brinkley-z3515-homeassistant`
- **Host:** GitHub, under `andrewcfitz`
- **Visibility:** Public
- **License:** MIT
- **Default branch:** `main`

## Structure

```
README.md              Overview, status-at-a-glance table, unofficial disclaimer
LICENSE                MIT
.gitignore             Secrets, HA secrets.yaml, OS cruft
docs/
  hardware.md          Coach control system: model numbers, control modules,
                       network/CAN/RV-C details, added sensors/relays
  integration.md       How HA talks to it — confirmed methods, entity list
  discovery-log.md     Dated, append-only reverse-engineering notes (dead ends included)
  references.md        Links: forums, OEM docs, similar projects
homeassistant/
  README.md            How to use the published configs
  packages/            HA YAML to publish (empty placeholder for now)
```

## Key Design Choices

### Status table is the spine

The README carries a small table tracking each subsystem as
**Confirmed / In progress / Unknown**. Candidate rows: tanks (fresh/grey/black),
lighting, climate/HVAC, awning, slides, water heater, leveling, battery/power,
generator. This is what makes a discovery-driven notebook navigable.

### Discovery log is dated and append-only

Entries are timestamped and never rewritten to hide dead ends. A failed
approach with the reason it failed is worth as much as a success. Newest
entries on top.

### Secrets hygiene from commit one

Because the repo is public:
- `.gitignore` excludes `secrets.yaml`, `*.secret`, `.env`, and common HA
  secret patterns.
- README and `homeassistant/README.md` carry an explicit warning: never paste
  tokens, passwords, VIN, GPS coordinates, or network credentials.

### Markdown-first, no tooling

Everything is plain Markdown so it renders on GitHub with zero setup and stays
easy to edit from any device, including from inside the trailer on bad signal.

## Day-One Content

Since we start from a blank slate, each doc ships as a populated skeleton:

- **README.md** — project intro, the status table (all rows "Unknown" or
  "In progress"), unofficial disclaimer, repo map, secrets warning.
- **docs/hardware.md** — headings for the coach control system, network topology,
  added hardware, with prompts for what to fill in. The specific Z3515 control
  system is recorded here as it is identified (left as "to be confirmed" until
  verified rather than guessed).
- **docs/integration.md** — headings for integration method(s), HA entities,
  and setup steps. Reflects the three candidate paths: coach control system
  API, discrete sensors/relays, and undetermined.
- **docs/discovery-log.md** — one seed entry dated 2026-06-29 noting the repo's
  creation and current state (no working integration yet).
- **docs/references.md** — empty link list with section headings.
- **homeassistant/README.md** — explains the `packages/` layout and the secrets
  warning. `packages/` itself holds a `.gitkeep`.

## Out of Scope for v1

- Any actual reverse-engineering of the trailer (that is ongoing work the repo
  *records*, not something this scaffolding task performs).
- Real HA config (none exists yet).
- Community contribution infrastructure.

## Open Questions

None blocking. The Z3515's specific control system will be filled into
`hardware.md` as it is confirmed; the scaffolding does not assert it.
