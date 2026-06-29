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

## Approach

Home Assistant runs on a **Raspberry Pi 4** and reaches the trailer through three
channels:

1. **Shore power** over BLE: a Hughes Power Watchdog Gen II via `ha-power-watchdog`.
   Working today.
2. **Coach systems** over CAN: the Z3515 runs **Lippert (LCI) OneControl**
   (**RV-C over CAN**); a **Waveshare ESP32-S3 RS485/CAN** board taps the bus and
   bridges to HA. In progress.
3. **Add-on sensors and controls** over Zigbee/Thread: a SMLIGHT **SLZB-06**
   network coordinator. Available for anything the factory systems do not cover.

Details in [`docs/integration.md`](docs/integration.md) and
[`docs/hardware.md`](docs/hardware.md); running notes in
[`docs/discovery-log.md`](docs/discovery-log.md).

## Status at a glance

Tracking how far each subsystem has gotten toward Home Assistant integration.

| Subsystem            | Status      | Notes |
|----------------------|-------------|-------|
| Shore power monitor  | Confirmed   | Hughes Power Watchdog Gen II 30A over BLE (`ha-power-watchdog`) |
| Fresh water tank     | Unknown     | Target via OneControl CAN bridge |
| Grey water tank      | Unknown     | Target via OneControl CAN bridge |
| Black water tank     | Unknown     | Target via OneControl CAN bridge |
| Lighting             | Unknown     | Target via OneControl CAN bridge |
| Climate / HVAC       | Unknown     | Target via OneControl CAN bridge |
| Water heater         | Unknown     | Target via OneControl CAN bridge |
| Awning               | Unknown     | Target via OneControl CAN bridge |
| Slides               | Unknown     | Target via OneControl CAN bridge |
| Leveling / jacks     | Unknown     | Target via OneControl CAN bridge |
| Battery / power      | Unknown     | Target via OneControl CAN bridge |
| Generator            | Unknown     | Target via OneControl CAN bridge |

**Legend:** `Confirmed` working in HA · `In progress` partially working or under
investigation · `Unknown` not yet started.

## Repository map

- [`docs/hardware.md`](docs/hardware.md): the coach control system, network,
  and any added hardware.
- [`docs/integration.md`](docs/integration.md): how Home Assistant talks to
  the trailer, and the entities exposed.
- [`docs/discovery-log.md`](docs/discovery-log.md): dated reverse-engineering
  notes, dead ends included.
- [`docs/references.md`](docs/references.md): links to forums, OEM docs, and
  related projects.
- [`homeassistant/`](homeassistant/): publishable Home Assistant config.

## License

[MIT](LICENSE).
