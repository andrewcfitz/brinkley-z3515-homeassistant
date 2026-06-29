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

## LP tanks: Mopeka Pro Check (BLE)

**Status: In progress (hardware on hand).** A **Mopeka Pro Check** ultrasonic
propane level sensor magnet-mounts to the bottom of each LP tank and reports fill
level, temperature, and battery over BLE.

- **Transport:** BLE **advertisements** (broadcast), not a paired connection.
  This matters: unlike the Power Watchdog, the Mopeka does not consume a single
  connection, so HA and the phone app can both read it at once, and one receiver
  can hear multiple sensors.
- **Integration:** Home Assistant has a **built-in Mopeka integration** (BLE);
  ESPHome also has a Mopeka component if reading via a proxy.
- **BLE coverage:** rides the same BLE infrastructure as the Power Watchdog (Pi 4
  onboard Bluetooth, or a Screek BP1 proxy). The LP tanks and the shore-power
  inlet are in different parts of the rig, so a proxy near the propane
  compartment may be needed even if the Pi covers the Power Watchdog. ESPHome
  supports multiple BT proxies on one HA instance.
- **Entities (per sensor):** tank level (%), reading distance, temperature,
  battery.

## Add-on sensors and controls: Zigbee via SLZB-06 + Zigbee2MQTT (in use)

**Status: In progress.** A SMLIGHT **SLZB-06** network coordinator (CC2652P +
ESP32-S3) gives Home Assistant a Zigbee radio over Ethernet / PoE / USB / WiFi,
run through **Zigbee2MQTT**. This is the channel for anything added beyond the
factory systems.

**Devices on hand:**

- **Baggage / basement bay doors:** SONOFF **SNZB-04PR2** Zigbee contact sensors
  (4-pack). One per bay door; each exposes an open/closed `binary_sensor` plus
  battery, and supports tamper detection. Use for "is a bay open" alerts and
  travel-readiness checks.

Notes:

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
| Tank level / temp / battery | sensor | LP tanks | Mopeka Pro Check (BLE) | In progress |
| Bay door open/closed (per bay) | binary_sensor | Baggage doors | SONOFF SNZB-04PR2 (Zigbee2MQTT) | In progress |

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

### Mopeka Pro Check (LP tanks, BLE)

1. Install the sensor: snap the magnetic mount to the bottom center of the LP
   tank (steel tank required for the magnet).
2. Ensure BLE coverage near the propane compartment (Pi 4 onboard BT or a Screek
   BP1 proxy placed nearby).
3. In HA, add the built-in **Mopeka** integration; it discovers the sensor's BLE
   advertisements automatically. Pair/identify by pressing the sensor button.
4. Set the tank type/size in the integration so level (%) reads correctly.
5. Repeat per tank.

### SONOFF SNZB-04PR2 bay door sensors (Zigbee2MQTT)

1. Stand up an MQTT broker (Mosquitto) and Zigbee2MQTT against the SLZB-06, if
   not already running.
2. Put Zigbee2MQTT in permit-join, then pair each SNZB-04PR2 (press its pairing
   button until the LED blinks).
3. Mount sensor + magnet across each bay door seam so the gap closes when shut.
4. Rename each device by bay in Zigbee2MQTT; the open/closed `binary_sensor` and
   battery flow into HA over MQTT discovery.

### OneControl CAN bridge

Empty until there is a working bridge to describe. See the
[discovery log](discovery-log.md) for progress.
