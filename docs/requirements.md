# Air Quality Monitor: Requirements (Rev A)

## 1. Overview

A wall powered indoor air quality monitor built on a custom PCB. It measures particulate matter, CO2, temperature and humidity, shows readings on a small screen, and makes the data available over WiFi. One unit, deployed in Irvine, CA.

**Goal:** a reliable device whose readings are validated against nearby reference sensors (AirNow / PurpleAir).

## 2. Functional Requirements (what it does)

| ID | Requirement |
|----|-------------|
| F1 | Measure PM1.0, PM2.5 and PM10 in µg/m³ |
| F2 | Measure CO2 in ppm |
| F3 | Measure temperature (°C/°F) and relative humidity (%) |
| F4 | Sample PM every 10 s and CO2/temp/humidity every 5 s |
| F5 | Compute 1 minute averages of all readings |
| F6 | Show current readings on an onboard OLED screen |
| F7 | Show device status (booting, OK, WiFi lost, sensor error) with an RGB LED |
| F8 | Serve a local web page with live readings over WiFi |
| F9 | Log readings as CSV over USB serial |
| F10 | Detect a failed or missing sensor and report it instead of showing bad data |

## 3. Non Functional Requirements (how well it does it)

| ID | Requirement |
|----|-------------|
| N1 | Powered from USB C, 5 V |
| N2 | Programmable and debuggable over the same USB C port |
| N3 | Runs continuously for 7 days with no crash or freeze |
| N4 | Reconnects to WiFi automatically after a dropout |
| N5 | Sensor readings stay within manufacturer accuracy specs |
| N6 | Temperature sensor placed away from heat sources on the board |
| N7 | 2 layer PCB, under 100 x 100 mm (cheapest fab pricing) |
| N8 | Parts hand solderable where possible (0805 size or larger, no BGA) |
| N9 | Total parts + PCB cost under $80 |
| N10 | ESD protection on the USB port |

## 4. Hardware Choices (initial)

| Block | Part | Why |
|-------|------|-----|
| Microcontroller | ESP32-S3-WROOM-1 module | WiFi built in, native USB so no extra USB to serial chip, prebuilt module means no RF design needed |
| Particulate sensor | Plantower PMS5003 | Cheap, widely used, easy to buy, UART interface |
| CO2 sensor | Sensirion SCD40 | True CO2 measurement (not estimated), I2C, small |
| Temp / humidity | Sensirion SHT40 | More accurate than the SCD40's built in sensor, which warms itself |
| Power | USB C 5 V in, 3.3 V regulator (at least 800 mA) | PMS5003 needs 5 V; ESP32 needs 3.3 V with current spikes during WiFi |
| Display | 128x64 OLED (SSD1306, I2C) | Cheap, low power, easy to drive |

## 5. Out of Scope for Rev A

* Battery power
* Cloud dashboard
* Enclosure
* VOC / gas sensing
* Updates over WiFi (OTA)
* Outdoor use / weatherproofing

## 6. Roadmap

**MVP (Rev A board):** everything in sections 2 and 3.

**Version 2:**
* Cloud database + dashboard for long term history
* Firmware updates over WiFi
* AQI calculation using the EPA formula
* SolidWorks enclosure with proper airflow
* Validation study vs nearby AirNow / PurpleAir data

**Advanced:**
* Rev B board fixing Rev A issues
* VOC sensor (Sensirion SGP41)
* Humidity correction for PM readings
* Alerts (phone notification when air quality gets bad, e.g. wildfire smoke)

## 7. Definition of Done (MVP)

* [ ] Board manufactured and assembled
* [ ] All sensors read correctly
* [ ] Readings visible on OLED, web page and serial log
* [ ] 7 day continuous run passed
* [ ] Each requirement above has a test result recorded
