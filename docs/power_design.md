# Power Design

## Power Flow
USB C 5V → fuse → 5V rail → PMS5003
                        → 3.3V LDO → ESP32 S3, SCD40, SHT40, OLED, RGB LED
USB D+/D- → ESD protection → ESP32 S3

## 3.3V Power Budget (peak, typical datasheet values)
| Part | Peak current |
|---|---|
| ESP32 S3 | ~500 mA (WiFi transmit) |
| SCD40 | ~200 mA (during measurement) |
| OLED | ~20 mA |
| RGB LED | ~15 mA |
| SHT40 | <1 mA |
| Total | ~735 mA |

5V rail also supplies PMS5003, up to ~100 mA.

## Decisions
* 3.3V regulator rated 1A for margin over the ~735 mA peak
* Decoupling capacitors next to every chip power pin
* Temperature sensor placed far from the regulator (regulator heat would skew readings)
* All values to be confirmed against datasheets during part selection

## USB C Input
* Connector: HRO TYPE-C-31-M-12 (LCSC C165948), USB 2.0, 16 pin
* CC1 and CC2 each get their own 5.1k pulldown to GND (required for C to C chargers to supply 5V)
* Both copies of VBUS, GND, D+ and D- tied together (plug works either way)
* Polyfuse (~750 mA hold) between VBUS and the 5V rail
* USBLC6-2SC6 ESD protection on D+, D- and VBUS
* ESP32 S3 native USB: GPIO20 = D+, GPIO19 = D-

## Assembly Plan
* JLCPCB economic PCBA for small SMD parts
* ESP32 module and connectors hand soldered
* Hole under the ESP32 ground pad so it can be soldered from the back
