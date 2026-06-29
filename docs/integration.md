# Integration

How Home Assistant talks to the Z3515. This is the "what works" half of the
repo. Update the README status table whenever an entry here moves to
`Confirmed`.

**HA host:** Raspberry Pi 4. See [hardware](hardware.md) for the full host and
radio inventory.

This rig has more than one integration. Each is tracked separately below.

## Shore power: Hughes Power Watchdog (working)

**Status: Confirmed.** A **Hughes Autoformers Power Watchdog Gen II WiFi 30A**
smart surge protector monitors incoming shore power. It is integrated into Home
Assistant with the [`jdaleo23/ha-power-watchdog`](https://github.com/jdaleo23/ha-power-watchdog)
custom integration.

- **Transport:** Bluetooth Low Energy (BLE). Local, real-time notifications, no
  cloud or polling.
- **Install:** via HACS (add the repo as a custom repository, install "Hughes
  Power Watchdog," restart HA).
- **Entities (30A, single line):** voltage (V), current (A), power (W), energy
  (kWh), frequency (Hz), error code, error description, and a `Fault Active`
  binary sensor for automations.
- **Gotchas:**
  - The device allows only **one BLE connection at a time**. Close the official
    Power Watchdog phone app or HA cannot connect.
  - BLE source: start with the **Pi 4's onboard Bluetooth**. If the surge
    protector is out of range (likely if it lives in an outside bay or at the
    pedestal), use the **Screek BP1** ESPHome Bluetooth proxy (on hand) placed
    near it. Do not reuse the OneControl CAN ESP32 for this; keep it dedicated to
    bus duty.

## Add-on sensors and controls: Zigbee / Thread via SLZB-06 (available)

**Status: Available, not yet populated.** A SMLIGHT **SLZB-06** network
coordinator (CC2652P + ESP32-S3) gives Home Assistant a Zigbee and Thread radio
over Ethernet / PoE / USB / WiFi. This is the channel for anything added beyond
the factory systems: Zigbee tank sensors, temperature/humidity, contact and
motion sensors, smart relays, etc.

- **Integration:** **Zigbee2MQTT** (chosen over ZHA). Needs an MQTT broker;
  Mosquitto on the Pi 4 is the usual choice. The OneControl CAN bridge may also
  publish over MQTT, so one broker can serve both.
- **Why network-attached:** the radio can sit centrally in the rig instead of
  next to the Pi, improving mesh coverage; PoE means one cable for data + power.
- **Use it for:** subsystems the OneControl CAN bridge cannot reach or that are
  simpler to add as standalone sensors.

## Coach systems: OneControl CAN bus via ESP32 bridge (in progress)

**Chosen approach: tap the OneControl CAN bus with an ESP32 bridge.**

A Waveshare ESP32-S3 RS485/CAN board joins the OneControl CAN daisy chain as a
node, reads the RV-C traffic, and exposes decoded values to Home Assistant over
WiFi. See [hardware](hardware.md) for the physical connection and parts.

Planned phases:

1. **Read-only sniff.** Join the bus, capture frames, log raw CAN/RV-C traffic.
   Goal: confirm the bitrate (RV-C is 250 kbit/s) and see live IDs without
   touching anything.
2. **Decode.** Map RV-C DGNs and instance IDs to subsystems (tanks, lights,
   slides, climate, leveling). RV-C is a published standard, so much of this is
   table lookup rather than blind reverse-engineering.
3. **Expose to HA.** Bridge decoded values into Home Assistant. Likely via
   ESPHome (native CAN support) or an MQTT bridge running on the ESP32. Decision
   recorded here once made.
4. **Control (later, carefully).** Writing to the bus can actuate slides,
   awnings, and jacks. Read everything first; only attempt control once the
   protocol is well understood and safety interlocks are considered.

**Firmware platform:** _to be decided._ Candidates: ESPHome (with `canbus` /
RV-C handling) vs. custom firmware publishing over MQTT. Tradeoffs to be noted
here.

> Update the README status table as each subsystem moves from `Unknown` to
> `In progress` to `Confirmed`.

## Home Assistant entities

Power Watchdog entities are provided by the `ha-power-watchdog` integration.
Exact entity IDs depend on your HA naming; types and subsystem are fixed.

| Entity | Type | Subsystem | Source | Status |
|--------|------|-----------|--------|--------|
| Voltage | sensor (V) | Shore power | ha-power-watchdog (BLE) | Confirmed |
| Current | sensor (A) | Shore power | ha-power-watchdog (BLE) | Confirmed |
| Power | sensor (W) | Shore power | ha-power-watchdog (BLE) | Confirmed |
| Energy | sensor (kWh) | Shore power | ha-power-watchdog (BLE) | Confirmed |
| Frequency | sensor (Hz) | Shore power | ha-power-watchdog (BLE) | Confirmed |
| Error code / description | sensor | Shore power | ha-power-watchdog (BLE) | Confirmed |
| Fault Active | binary_sensor | Shore power | ha-power-watchdog (BLE) | Confirmed |

## Setup

### Hughes Power Watchdog (BLE)

1. Ensure HA has a working Bluetooth adapter or an ESPHome Bluetooth proxy in
   range of the surge protector.
2. Install HACS if not already present.
3. In HACS, add `https://github.com/jdaleo23/ha-power-watchdog` as a custom
   repository (type: Integration), then install "Hughes Power Watchdog."
4. Restart Home Assistant.
5. Close the official Power Watchdog phone app so the BLE connection is free.
6. Add the integration; it auto-detects the model and creates the sensors above.

### OneControl CAN bridge

Empty until there is a working bridge to describe. See the
[discovery log](discovery-log.md) for progress.
