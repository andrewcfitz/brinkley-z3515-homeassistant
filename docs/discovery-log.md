# Discovery log

Dated, append-only notes on reverse-engineering the Z3515. Newest entries on
top. Dead ends are kept on purpose: a failed approach and the reason it failed
is as useful as a success.

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
