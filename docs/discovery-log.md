# Discovery log

Dated, append-only notes on reverse-engineering the Z3515. Newest entries on
top. Dead ends are kept on purpose: a failed approach and the reason it failed
is as useful as a success.

## 2026-06-29: First add-on sensors bought (bay doors + LP tanks)

Two add-on sensor sets in hand, both riding existing channels:

- **Baggage / basement bay doors:** SONOFF **SNZB-04PR2** Zigbee contact sensors
  (4-pack). First real Zigbee devices; go through the SLZB-06 + Zigbee2MQTT.
- **LP tanks:** **Mopeka Pro Check** BLE ultrasonic level sensors. Read by HA's
  built-in Mopeka integration over BLE advertisements (broadcast, so no
  one-connection limit like the Power Watchdog has).

BLE coverage note: the LP compartment and the shore-power inlet are in different
spots, so a Screek BP1 proxy near the propane tanks may be needed even if the
Pi covers the Power Watchdog. ESPHome allows multiple BT proxies.

Both are `In progress` (bought, not yet installed/configured). Next: stand up
Mosquitto + Zigbee2MQTT for the door sensors, and the HA Mopeka integration for
the tanks.

## 2026-06-29: BT proxy on hand (Screek BP1) + Zigbee2MQTT chosen

Resolved the unidentified eBay device: it is a **Screek BP1**, an ESP32 running
ESPHome as a **Bluetooth proxy** (~$15, USB-C). That covers the Power Watchdog
BLE range concern without a new purchase. Plan: try the Pi 4's onboard Bluetooth
first; if the surge protector is out of range, run the BP1 near it.

Also decided: Zigbee runs through **Zigbee2MQTT** (not ZHA). This needs an MQTT
broker (Mosquitto on the Pi). Bonus: if the OneControl CAN bridge also publishes
over MQTT, a single broker serves both, which keeps the topology simple.

Hardware inventory is now complete for the planned channels. Next real work:
bench the Waveshare ESP32 and sniff the OneControl CAN bus read-only.

## 2026-06-29: Hub inventory (Pi 4 + SLZB-06)

Pinned down the hub side. **Home Assistant runs on a Raspberry Pi 4.** A SMLIGHT
**SLZB-06** (CC2652P + ESP32-S3) is on hand as a network Zigbee/Thread/Matter
coordinator over Ethernet/PoE/USB/WiFi.

This settles the architecture into three channels feeding one HA instance:

1. Shore power over BLE (Power Watchdog), working.
2. Coach systems over CAN (OneControl via ESP32 bridge), in progress.
3. Add-on sensors/controls over Zigbee/Thread (SLZB-06), available.

The Pi 4's onboard Bluetooth is the first thing to try for the Power Watchdog;
fall back to a dedicated ESP32 BT proxy only if range is bad.

(One more device, an eBay listing, is still to be identified and added.)

## 2026-06-29: First integration confirmed (Hughes Power Watchdog)

Shore-power monitoring is **working in Home Assistant**. The Hughes Autoformers
Power Watchdog Gen II WiFi 30A surge protector is integrated via the
[`jdaleo23/ha-power-watchdog`](https://github.com/jdaleo23/ha-power-watchdog)
HACS custom integration over BLE. Exposes voltage, current, power, energy,
frequency, error code/description, and a fault binary sensor. First `Confirmed`
row in the status table.

Notes for the build:

- Device allows only one BLE connection at a time; the phone app must be closed
  for HA to connect.
- BLE range is marginal. Plan to add a **separate** ESP32 ESPHome Bluetooth
  proxy (the CAN-bridge ESP32 stays dedicated to bus duty).

## 2026-06-29: Hardware plan locked in (LCI OneControl over CAN)

Identified the coach control system as **Lippert (LCI) OneControl**, which runs
**RV-C over CAN**. Plan: tap the CAN bus with a **Waveshare ESP32-S3 RS485/CAN**
board (Amazon B0FNJX8VGZ) as a bridge to Home Assistant over WiFi.

Big assist from an [OpenRange owners forum thread](https://www.openrangeowners.com/threads/adding-lci-onecontrol-myrv-wifi-gateway.1103287/)
documenting a first-party LCI MyRV WiFi Gateway (Lippert WR1001NS, p/n 406343)
install. It pinned down the exact connectors and the daisy-chain tap method.

Parts purchased to mate to the OneControl bus:

- CAN cable: **Molex 2451350220** (Mini-Fit Jr, 2 m)
- Power connector housing: **TE / AMP Mate-N-Lok 1-480318-0**
- Power connector pins: **TE 60619-4**

Open items before wiring in:

- Confirm whether the Waveshare board has an on-board switchable 120 ohm CAN
  terminator, or whether a Lippert terminating resistor (p/n 333041) is needed.
- Confirm the OneControl bus bitrate on the actual rig (expected 250 kbit/s for
  RV-C) by sniffing read-only first.
- Decide firmware platform: ESPHome vs. custom MQTT bridge.

Next step: bench-power the ESP32, then join the bus read-only and capture frames.

## 2026-06-29: Repo created

Starting from a blank slate. No working Home Assistant integration yet. Goal is
to identify the coach control system, figure out how to read its state, and
expose subsystems (tanks, lighting, climate, etc.) to Home Assistant. Next
step: identify the factory control system and how it is wired.
