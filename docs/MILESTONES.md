# Senville Aura ESPHome — Project Milestones

## Overview
This repository provides local, cloud-free ESPHome firmware configurations for Senville Aura mini-split heat pumps (Midea OEM), exposing complete climate control via standalone MQTT and Home Assistant Native API.

---

## Milestone Roadmap

| Milestone | Title & Functional Scope | Status | Verification & Target Deliverables |
|:---:|:---|:---:|:---|
| **M-01** | **Initial ESPHome Midea UART + MQTT Implementation**<br>ESPHome configs for pre-built USB-A dongles (SLWF-01pro, CloudFree) and DIY ESP32-C3 builds with dual HA API & MQTT control. | **COMPLETE** | • `senville-aura-slwf01pro.yaml`<br>• `senville-aura-esp32c3.yaml`<br>• `secrets.yaml.example`<br>• `README.md` hardware guide |
| **M-02** | **Physical Flashing & Production Qualification**<br>Compile verification, OTA deployment, in-chassis validation on Senville Aura indoor unit, Follow-Me remote sensor testing. | **PENDING** | • ESPHome compile pass<br>• Hardware UART frame capture<br>• Home Assistant entity registration |
| **M-03** | **Telemetry & Energy Dashboard Integration**<br>Outdoor inverter power reporting (Watts), cumulative kWh energy sensors, and dynamic defrost status alerts. | **FUTURE** | • Inverter power curve telemetry<br>• HA Energy Dashboard integration |

---

## Milestone Details

### Milestone M-01: Initial ESPHome Midea UART + MQTT Implementation [COMPLETE]
* **Scope**:
  - Deliver drop-in ESPHome firmware for pre-built Midea USB-A dongles (`SLWF-01pro`, `CloudFree`).
  - Deliver DIY ESP32-C3 configuration with dedicated hardware UART on `GPIO20` (RX) and `GPIO21` (TX).
  - Dual control path: Home Assistant native API (`api:`) + MQTT client (`mqtt:`) with auto-discovery and `mitsubishi2mqtt` topic schema parity.
  - Remote thermistor override (`senville_ac/follow_me/set`) and front panel LED display control (`senville_ac/display/set`).
  - Hardware connection reference and USB-A pinout documentation.

### Milestone M-02: Physical Flashing & Production Qualification [PENDING]
* **Scope**:
  - Compile-check firmware images with upstream ESPHome toolchain.
  - Flash target hardware and verify serial negotiation (9600 baud 8N1) on Midea UART bus.
  - Validate MQTT auto-discovery payload in Home Assistant / Mosquitto.
