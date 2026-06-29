# References

Links worth keeping. Add as you find them.

## Brinkley / OEM

-

## Control system / RV electronics

- LCI OneControl: Lippert's RV coach control system (RV-C over CAN).
- RV-C protocol: open standard for RV CAN networks (DGNs, instances, 250 kbit/s).
  Specification at <https://www.rv-c.com/>.
- Lippert MyRV WiFi Gateway: first-party CAN gateway, WR1001NS / p/n 406343.

## Hardware

- Waveshare ESP32-S3 RS485/CAN board: [Amazon B0FNJX8VGZ](https://www.amazon.com/dp/B0FNJX8VGZ).
- CAN cable: Molex 2451350220 (Mini-Fit Jr, 2 m).
- Power connector: TE / AMP Mate-N-Lok 1-480318-0 with 60619-4 sockets.
- CAN terminator: Lippert p/n 333041.
- Hughes Autoformers Power Watchdog Gen II WiFi 30A surge protector:
  [product page](https://mobilemusthave.com/products/hughes-autoformers-power-watchdog-gen-ii-wifi-30a-smart-surge-protector).
- SONOFF SNZB-04PR2 Zigbee contact sensors (bay doors, 4-pack):
  [Amazon B0GKFB66JZ](https://www.amazon.com/dp/B0GKFB66JZ).
- Mopeka Pro Check BLE propane level sensor (LP tanks):
  [Amazon B09J6MXJKT](https://www.amazon.com/dp/B09J6MXJKT).

## Home Assistant

- [`jdaleo23/ha-power-watchdog`](https://github.com/jdaleo23/ha-power-watchdog):
  HACS integration for the Hughes Power Watchdog over BLE. **In use.**
- [Mopeka integration](https://www.home-assistant.io/integrations/mopeka/):
  built-in HA integration for Mopeka BLE tank sensors.
- [ESPHome](https://esphome.io/): candidate firmware for the ESP32 CAN bridge
  (has CAN bus support) and for Bluetooth proxies (Power Watchdog, Mopeka).
- HA host: Raspberry Pi 4.

## Radios and hubs

- [SMLIGHT SLZB-06](https://smlight.tech/global/slzb06): network Zigbee/Thread/Matter
  coordinator (CC2652P + ESP32-S3), Ethernet/PoE/USB/WiFi. Run via **Zigbee2MQTT**.
  **On hand.**
- [Screek BP1](https://shop.screek.io/products/bp1): ESP32 + ESPHome Bluetooth
  proxy, USB-C powered. Extends BLE range for the Power Watchdog. **On hand.**
- [Zigbee2MQTT](https://www.zigbee2mqtt.io/): chosen Zigbee stack (needs an MQTT
  broker such as Mosquitto).

## Forums and other owners

- [Adding LCI OneControl MyRV WiFi Gateway](https://www.openrangeowners.com/threads/adding-lci-onecontrol-myrv-wifi-gateway.1103287/)
  (OpenRange owners forum). Source for connector part numbers, termination, and
  the daisy-chain tap method.

## Related projects

- coachproxy / RV-C open-source projects: RV-C decoding prior art (to be
  catalogued).

## To review (reading list)

Saved to read later; not yet vetted. Descriptions are from titles only.

- [manos/OneControl-RV-C-Protocol](https://github.com/manos/OneControl-RV-C-Protocol/tree/main/rvc):
  reverse-engineered OneControl RV-C protocol notes/data. Most directly relevant
  to the decode phase.
- [spbrogan/rvc2mqtt](https://github.com/spbrogan/rvc2mqtt/): RV-C to MQTT bridge.
  Possible off-the-shelf path from the CAN bus to HA via the planned MQTT broker.
- [rperciaccante/HADynamicWeather](https://github.com/rperciaccante/HADynamicWeather):
  Home Assistant project (relevance to be confirmed).
- [Interfacing with the OneControl system via CAN bus](https://www.gdrvowners.com/forum/operation/appliances/45716-interfacing-with-the-onecontrol-system-via-canbus)
  (Grand Design RV owners forum). Owner discussion of tapping OneControl over CAN.
