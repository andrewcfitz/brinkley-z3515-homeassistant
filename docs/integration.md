# Integration

How Home Assistant talks to the Z3515. This is the "what works" half of the
repo. Update the README status table whenever an entry here moves to
`Confirmed`.

## Integration method

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

| Entity ID | Type | Subsystem | Source | Status |
|-----------|------|-----------|--------|--------|
|           |      |           |        |        |

## Setup

Step-by-step to reproduce a working setup. Empty until there is one to
describe.

> _Steps go here._
