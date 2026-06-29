# Brinkley Z3515 + Home Assistant Scaffold — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Scaffold a public, single-author Markdown documentation repo for connecting Home Assistant to a 2026 Brinkley Z3515, then publish it to GitHub under `andrewcfitz`.

**Architecture:** Pure Markdown notebook, no build/CI/tests. A README status table is the navigational spine; a dated append-only discovery log records reverse-engineering progress. Secrets hygiene is enforced via `.gitignore` and explicit warnings. Day-one content is populated skeletons with no invented facts about the Z3515's control system.

**Tech Stack:** Markdown, Git, GitHub (`gh` CLI). No language runtime.

## Global Constraints

- License: MIT, copyright holder "Andrew Fitz", year 2026.
- Host: GitHub under `andrewcfitz`, repo name `brinkley-z3515-homeassistant`, **public**.
- Default branch: `main`.
- No em dashes in any prose (author style rule).
- Never assert the Z3515's specific control system as fact; mark unknowns "to be confirmed".
- The `docs/superpowers/` directory is planning scaffolding and is NOT pushed to the public remote (see Task 6).
- No build, no CI, no tests, no contribution scaffolding (CONTRIBUTING.md, issue/PR templates).

---

### Task 1: Repo metadata — LICENSE and .gitignore

**Files:**
- Create: `LICENSE`
- Create: `.gitignore`

**Interfaces:**
- Consumes: nothing.
- Produces: a repo with an MIT license and secret-ignoring rules that later tasks rely on for safe public commits.

- [ ] **Step 1: Write `LICENSE`** (standard MIT text)

```
MIT License

Copyright (c) 2026 Andrew Fitz

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

- [ ] **Step 2: Write `.gitignore`**

```gitignore
# Home Assistant secrets — never commit these to a public repo
secrets.yaml
**/secrets.yaml
*.secret
.env
.env.*
*.key
*.pem
known_devices.yaml

# OS / editor cruft
.DS_Store
Thumbs.db
*.swp
.idea/
.vscode/
```

- [ ] **Step 3: Verify and commit**

Run: `git status --short`
Expected: `LICENSE` and `.gitignore` show as new (`??` or staged `A`).

```bash
git add LICENSE .gitignore
git commit -m "Add MIT license and secret-ignoring gitignore"
```

---

### Task 2: README with status table

**Files:**
- Create: `README.md`

**Interfaces:**
- Consumes: nothing.
- Produces: the project entry point. Defines the subsystem rows and status vocabulary (`Confirmed` / `In progress` / `Unknown`) reused by `docs/` files.

- [ ] **Step 1: Write `README.md`**

```markdown
# Brinkley Z3515 + Home Assistant

An unofficial, personal notebook for connecting [Home Assistant](https://www.home-assistant.io/)
to a 2026 Brinkley Z3515 fifth wheel. Part working integration, part discovery
log of what is still being reverse-engineered.

> **Unofficial.** Not affiliated with, endorsed by, or supported by Brinkley
> RV. Everything here is one owner's notes. Use at your own risk; you are
> responsible for anything you wire, flash, or automate on your own rig.

> **Public repo, no secrets.** Never commit tokens, passwords, your VIN, GPS
> coordinates, or network credentials. See `.gitignore` and
> `homeassistant/README.md`.

## Status at a glance

Tracking how far each subsystem has gotten toward Home Assistant integration.

| Subsystem            | Status      | Notes |
|----------------------|-------------|-------|
| Fresh water tank     | Unknown     |       |
| Grey water tank      | Unknown     |       |
| Black water tank     | Unknown     |       |
| Lighting             | Unknown     |       |
| Climate / HVAC       | Unknown     |       |
| Water heater         | Unknown     |       |
| Awning               | Unknown     |       |
| Slides               | Unknown     |       |
| Leveling / jacks     | Unknown     |       |
| Battery / power      | Unknown     |       |
| Generator            | Unknown     |       |

**Legend:** `Confirmed` working in HA · `In progress` partially working or under
investigation · `Unknown` not yet started.

## Repository map

- [`docs/hardware.md`](docs/hardware.md) — the coach control system, network,
  and any added hardware.
- [`docs/integration.md`](docs/integration.md) — how Home Assistant talks to
  the trailer, and the entities exposed.
- [`docs/discovery-log.md`](docs/discovery-log.md) — dated reverse-engineering
  notes, dead ends included.
- [`docs/references.md`](docs/references.md) — links to forums, OEM docs, and
  related projects.
- [`homeassistant/`](homeassistant/) — publishable Home Assistant config.

## License

[MIT](LICENSE).
```

- [ ] **Step 2: Verify links resolve to planned paths**

Run: `grep -o '(docs/[a-z-]*\.md)' README.md`
Expected: lists `hardware.md`, `integration.md`, `discovery-log.md`, `references.md` (these files are created in Tasks 3-4).

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "Add README with subsystem status table"
```

---

### Task 3: Hardware and references docs

**Files:**
- Create: `docs/hardware.md`
- Create: `docs/references.md`

**Interfaces:**
- Consumes: status vocabulary from README (Task 2).
- Produces: `docs/hardware.md` and `docs/references.md`, linked from README.

- [ ] **Step 1: Write `docs/hardware.md`**

```markdown
# Hardware

What is physically in and on the trailer, and how it is wired. Fill in as
each piece is identified. Do not record anything as fact until verified on the
actual rig.

## Coach control system

- **System / brand:** _to be confirmed_
- **Control panel model(s):** _to be confirmed_
- **Smart-system app (if any):** _to be confirmed_

What it controls, and how (multiplexed wiring, relays, CAN/RV-C bus, etc.):

> _Notes go here._

## Network topology

How control modules, any gateway, and Home Assistant connect (Wi-Fi, Ethernet,
CAN bus, Bluetooth, Zigbee, etc.).

> _Diagram or notes go here._

## Added hardware

Sensors, relays, controllers, or bridges added beyond the factory system.

| Device | Purpose | Where wired | HA integration | Notes |
|--------|---------|-------------|----------------|-------|
|        |         |             |                |       |

## Model and reference numbers

- **Trailer:** 2026 Brinkley Z3515
- **VIN:** _do not record here — public repo_
- Other module / part numbers as identified.
```

- [ ] **Step 2: Write `docs/references.md`**

```markdown
# References

Links worth keeping. Add as you find them.

## Brinkley / OEM

-

## Control system / RV electronics

-

## Home Assistant

-

## Forums and other owners

-

## Related projects

-
```

- [ ] **Step 3: Commit**

```bash
git add docs/hardware.md docs/references.md
git commit -m "Add hardware and references docs"
```

---

### Task 4: Integration and discovery-log docs

**Files:**
- Create: `docs/integration.md`
- Create: `docs/discovery-log.md`

**Interfaces:**
- Consumes: status vocabulary from README (Task 2); reflects the three candidate integration paths (coach control system API, discrete sensors/relays, undetermined).
- Produces: `docs/integration.md` and `docs/discovery-log.md`, linked from README.

- [ ] **Step 1: Write `docs/integration.md`**

```markdown
# Integration

How Home Assistant talks to the Z3515. This is the "what works" half of the
repo. Update the README status table whenever an entry here moves to
`Confirmed`.

## Integration method

The approach (or approaches) in use. Candidate paths under consideration:

1. **Coach control system API** — read/write the factory control system over
   its network/CAN/Bluetooth interface.
2. **Discrete sensors and relays** — HA-native devices added alongside the
   factory system (tank sensors, relays, temperature probes).
3. **Undetermined** — still being investigated; see the discovery log.

> _Document the chosen method(s) here as they are confirmed._

## Home Assistant entities

| Entity ID | Type | Subsystem | Source | Status |
|-----------|------|-----------|--------|--------|
|           |      |           |        |        |

## Setup

Step-by-step to reproduce a working setup. Empty until there is one to
describe.

> _Steps go here._
```

- [ ] **Step 2: Write `docs/discovery-log.md`** (note the seed entry date)

```markdown
# Discovery log

Dated, append-only notes on reverse-engineering the Z3515. Newest entries on
top. Dead ends are kept on purpose: a failed approach and the reason it failed
is as useful as a success.

## 2026-06-29 — Repo created

Starting from a blank slate. No working Home Assistant integration yet. Goal is
to identify the coach control system, figure out how to read its state, and
expose subsystems (tanks, lighting, climate, etc.) to Home Assistant. Next
step: identify the factory control system and how it is wired.
```

- [ ] **Step 3: Commit**

```bash
git add docs/integration.md docs/discovery-log.md
git commit -m "Add integration and discovery-log docs"
```

---

### Task 5: Home Assistant config directory

**Files:**
- Create: `homeassistant/README.md`
- Create: `homeassistant/packages/.gitkeep`

**Interfaces:**
- Consumes: secrets-warning convention from README (Task 2).
- Produces: the `homeassistant/` tree referenced by README, ready to hold published YAML.

- [ ] **Step 1: Write `homeassistant/README.md`**

```markdown
# Home Assistant config

Publishable Home Assistant configuration for the Brinkley Z3515. Empty for now;
YAML lands in `packages/` as integrations are confirmed.

## Layout

- `packages/` — self-contained [HA packages](https://www.home-assistant.io/docs/configuration/packages/),
  one file per subsystem (e.g. `tanks.yaml`, `lighting.yaml`). Include with
  `homeassistant: packages: !include_dir_named packages` in your
  `configuration.yaml`.

## Secrets — read before copying anything

This is a public repo. Configuration here must never contain secrets. Keep
tokens, passwords, and credentials in your own `secrets.yaml` (which is
gitignored) and reference them with `!secret`. Never paste your VIN, GPS
coordinates, or network credentials into any file here.
```

- [ ] **Step 2: Create the packages placeholder**

Run: `touch homeassistant/packages/.gitkeep`
Expected: empty file exists so the otherwise-empty directory is tracked by git.

- [ ] **Step 3: Commit**

```bash
git add homeassistant/README.md homeassistant/packages/.gitkeep
git commit -m "Add Home Assistant config directory skeleton"
```

---

### Task 6: Publish to GitHub

**Files:**
- None created. This task folds the worktree branch back to `main` and pushes a public remote, excluding planning scaffolding.

**Interfaces:**
- Consumes: all committed files from Tasks 1-5.
- Produces: a public GitHub repo `andrewcfitz/brinkley-z3515-homeassistant` whose tree contains the docs but NOT `docs/superpowers/`.

- [ ] **Step 1: Confirm `gh` is authenticated as the right account**

Run: `gh auth status`
Expected: logged in to github.com as `andrewcfitz` (or an account with access to create repos there). If not, stop and tell the user to run `! gh auth login`.

- [ ] **Step 2: Remove planning scaffolding from what will be published**

The `docs/superpowers/` specs and plans are useful locally but are not Brinkley
documentation. Remove them from the tree before publishing (the local copies
remain in git history of the worktree branch and on disk if needed).

Run:
```bash
git rm -r docs/superpowers
git commit -m "Remove planning scaffolding from published tree"
```
Expected: `docs/superpowers/` no longer tracked.

- [ ] **Step 3: Consolidate onto `main`**

The scaffolding was built on the `worktree-brinkley-z3515-scaffold` branch.
Bring it into `main` (the empty initial commit is the shared ancestor).

Run from the main checkout (`/Users/andrew/workspace/brinkley-z3515-homeassistant`):
```bash
git merge --ff-only worktree-brinkley-z3515-scaffold
```
Expected: `main` fast-forwards to include all scaffold commits. If ff-only
fails, stop and report — do not force.

- [ ] **Step 4: Create the public remote and push**

Run:
```bash
gh repo create andrewcfitz/brinkley-z3515-homeassistant \
  --public \
  --source=. \
  --remote=origin \
  --description "Unofficial notes on connecting Home Assistant to a 2026 Brinkley Z3515" \
  --push
```
Expected: repo created, `main` pushed, command prints the repo URL.

- [ ] **Step 5: Verify the published tree excludes scaffolding**

Run: `gh api repos/andrewcfitz/brinkley-z3515-homeassistant/contents/docs --jq '.[].name'`
Expected: lists `discovery-log.md`, `hardware.md`, `integration.md`,
`references.md` and does NOT list `superpowers`.

---

## Self-Review

**Spec coverage:**
- Repo metadata (name/host/license/branch) → Tasks 1, 6. ✓
- Status-table spine → Task 2. ✓
- Dated append-only discovery log → Task 4 (seed entry 2026-06-29). ✓
- Secrets hygiene (`.gitignore` + warnings) → Tasks 1, 2, 5. ✓
- Markdown-first, no tooling → whole plan; no build files. ✓
- Day-one populated skeletons for every doc → Tasks 2-5. ✓
- No invented Z3515 control-system facts → "to be confirmed" in Task 3. ✓
- No contribution scaffolding → none added. ✓
- `docs/superpowers/` kept out of public tree → Task 6 Step 2. ✓

**Placeholder scan:** The `_to be confirmed_` and empty-table cells are
intentional notebook content (the repo's whole point is to fill them in), not
plan placeholders. Every file's full text is present in the plan. No "TODO" or
"implement later" steps. ✓

**Type consistency:** Status vocabulary `Confirmed` / `In progress` / `Unknown`
is identical across README, integration, and hardware docs. Branch name
`worktree-brinkley-z3515-scaffold` and repo name
`andrewcfitz/brinkley-z3515-homeassistant` are consistent across tasks. ✓
