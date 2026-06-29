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
