# Hardware

What is physically in and on the trailer, and how it is wired. Fill in as
each piece is identified. Do not record anything as fact until verified on the
actual rig.

## Home Assistant host and radios

- **HA host:** Raspberry Pi 4 running Home Assistant. Hub for everything below.
  Has onboard 2.4 GHz WiFi and Bluetooth, plus Ethernet.
- **Zigbee / Thread coordinator:** SMLIGHT **SLZB-06** (CC2652P radio + ESP32-S3).
  Network coordinator over Ethernet / PoE / USB / WiFi. Runs via **Zigbee2MQTT**
  (chosen over ZHA), and can sit centrally instead of next to the Pi. This is the
  path for any added Zigbee/Thread sensors and relays.
- **Bluetooth proxy:** Screek **BP1** (ESP32 running ESPHome, USB-C powered, ~$15).
  An ESPHome Bluetooth proxy to give the Power Watchdog a stable BLE link when the
  Pi 4's onboard Bluetooth is out of range. Place it near the surge protector.

The result is three integration channels into one HA instance:

1. **Shore power** over BLE (Power Watchdog): working.
2. **Coach systems** over CAN (OneControl via ESP32 bridge): in progress.
3. **Add-on sensors/controls** over Zigbee/Thread (via SLZB-06): available.

## Coach control system

- **System / brand:** Lippert (LCI) **OneControl**
- **Control panel model(s):** OneControl touchscreen panel _(exact model to be confirmed on this rig)_
- **Smart-system app (if any):** LCI OneControl / MyRV app
- **Bus:** CAN bus. OneControl is an **RV-C** system (RV-C runs over CAN at
  250 kbit/s). Treat the bitrate as _to be confirmed on this rig_ until verified
  with the ESP32 or a logic analyzer.

What it controls: lighting, slides, awning, tanks, climate, leveling, and so on
are managed over the OneControl CAN bus and surfaced on the touchscreen panel.
The plan is to tap that same CAN bus read-only first, then decode it.

### Reference: LCI MyRV WiFi Gateway

Lippert sells a first-party gateway that hangs off the same CAN bus. It is a
useful reference for how to physically tap in (connectors, termination, daisy
chain), even though this project uses a custom ESP32 instead.

- **Gateway:** Lippert WR1001NS, marketed as LCI MyRV WiFi Gateway, p/n 406343.
- Source: [OpenRange owners forum thread](https://www.openrangeowners.com/threads/adding-lci-onecontrol-myrv-wifi-gateway.1103287/)
  (see [references](references.md)).

## Network topology

CAN bus is a **daisy chain** with a terminating resistor at each end. The
OneControl touchscreen sits on the bus with a terminator. To add a node (the
gateway, or our ESP32), the approach documented in the forum thread:

1. Unplug the CAN cable from the touchscreen.
2. Run a new CAN cable from the new node to the touchscreen.
3. Move the end terminating resistor from the touchscreen to the new node so
   the chain stays terminated at both ends.

```
[ ... OneControl modules ... ]---[ ESP32-S3 node ]---[ touchscreen ]
                                        |                    |
                                  (new CAN cable)      (terminator moved
                                                        here from panel)
```

> _Confirm the actual bus layout on the Z3515 before cutting into anything._

## Added hardware

Hardware bought for the Home Assistant integration: the CAN bridge plus add-on
sensors.

| Device | Purpose | Part number | Notes |
|--------|---------|-------------|-------|
| Waveshare ESP32-S3 RS485/CAN board | The bridge: reads OneControl CAN, talks to Home Assistant over WiFi | Waveshare ESP32-S3-RS485-CAN | [Amazon B0FNJX8VGZ](https://www.amazon.com/dp/B0FNJX8VGZ). Industrial board with isolated CAN and RS485, wide-voltage input. |
| CAN bus cable | Connect ESP32 node into the OneControl CAN daisy chain | Molex **2451350220** (Mini-Fit Jr, 2 m) | Same cable used in the forum build. Lippert equivalents: #331111 (2 ft), #331114 (10 ft). |
| Power connector (housing) | 12 V power tap for the node | TE / AMP Mate-N-Lok **1-480318-0** | |
| Power connector pins | Crimp sockets for the power connector | TE **60619-4** | |
| Hughes Power Watchdog Gen II WiFi 30A | Shore-power surge protection + monitoring (already integrated into HA) | Hughes Autoformers Power Watchdog Gen II 30A | Talks to HA over BLE via the `ha-power-watchdog` integration. See [integration](integration.md). |
| Bay door contact sensors | Open/closed sensing on basement / baggage bay doors | SONOFF **SNZB-04PR2** (Zigbee, 4-pack) | Via Zigbee2MQTT through the SLZB-06. One per bay door. [Amazon B0GKFB66JZ](https://www.amazon.com/dp/B0GKFB66JZ). |
| LP tank level sensors | Propane fill level, temp, battery | **Mopeka Pro Check** (BLE, ultrasonic) | Magnetic mount on steel tank bottom; BLE advertisements read by HA's Mopeka integration. [Amazon B09J6MXJKT](https://www.amazon.com/dp/B09J6MXJKT). |
| Temp / humidity meters | Climate logging, indoor + outdoor | **SwitchBot IP65 Hygrometer Thermometer** (BLE, 3-pack) | Reused from old RV. IP65 (one can go outside). Read by HA's SwitchBot Bluetooth integration. [Amazon B0BVZC9Q31](https://www.amazon.com/dp/B0BVZC9Q31). |

### Still to buy or confirm

- **CAN terminating resistor:** Lippert p/n **333041** (about $9). Needed if the
  ESP32 is inserted at the end of the chain. First confirm whether the Waveshare
  board has a switchable 120 ohm terminator on-board; if it does, that may cover
  it.
- **Power wire:** the forum build used 16/2 AWG tinned marine duplex, spliced
  into the touchscreen's 12 V feed.

(The Bluetooth proxy for the Power Watchdog is already on hand: a Screek BP1.
See the host and radios section above.)

## Model and reference numbers

- **Trailer:** 2026 Brinkley Z3515
- **VIN:** _do not record here; this is a public repo._
- Coach system: LCI OneControl (RV-C over CAN)
- Bridge MCU: Waveshare ESP32-S3 (RS485 + CAN)
