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

## Home Assistant

- [`jdaleo23/ha-power-watchdog`](https://github.com/jdaleo23/ha-power-watchdog):
  HACS integration for the Hughes Power Watchdog over BLE. **In use.**
- [ESPHome](https://esphome.io/): candidate firmware for the ESP32 CAN bridge
  (has CAN bus support) and for a Bluetooth proxy for the Power Watchdog.
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
