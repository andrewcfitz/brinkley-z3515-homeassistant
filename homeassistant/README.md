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
