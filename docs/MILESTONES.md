# Senville Aura ESPHome — Project Milestones

## Overview
This repository provides local, cloud-free ESPHome firmware configurations for Senville Aura mini-split heat pumps (Midea OEM), exposing complete climate control via standalone MQTT and Home Assistant Native API.

---

## Milestone Roadmap

| Milestone | Title & Functional Scope | Status | Verification & Target Deliverables |
|:---:|:---|:---:|:---|
| **M-01** | **Initial ESPHome Midea UART + MQTT Implementation**<br>ESPHome configs for pre-built USB-A dongles (SLWF-01pro, CloudFree) and DIY ESP32-C3 builds with dual HA API & MQTT control. | **COMPLETE** | • `senville-aura-slwf01pro.yaml`<br>• `senville-aura-esp32c3.yaml`<br>• `secrets.yaml.example`<br>• `README.md` hardware guide |
| **M-02** | **Full Protocol Controls & In-Chassis Surface**<br>Discrete louver positions, presets, power/beeper/breeze-away switches, self-clean button, custom UART B0 frame builder, and full MQTT topic matrix. | **COMPLETE** | • `includes/midea_ext.h`<br>• Louver & preset selects<br>• Power, beeper & breeze switches<br>• 10-topic MQTT control suite |
| **M-03** | **Telemetry & Energy Dashboard Integration**<br>Outdoor inverter power reporting (Watts), cumulative kWh energy sensors, and dynamic defrost status alerts. | **PENDING** | • Inverter power curve telemetry<br>• HA Energy Dashboard integration |

---

## Milestone Details

### Milestone M-01: Initial ESPHome Midea UART + MQTT Implementation [COMPLETE]
* **Scope**:
  - Deliver drop-in ESPHome firmware for pre-built Midea USB-A dongles (`SLWF-01pro`, `CloudFree`).
  - Deliver DIY ESP32-C3 configuration with dedicated hardware UART on `GPIO20` (RX) and `GPIO21` (TX).
  - Dual control path: Home Assistant native API (`api:`) + MQTT client (`mqtt:`) with auto-discovery and `mitsubishi2mqtt` topic schema parity.
  - Remote thermistor override (`senville_ac/follow_me/set`) and front panel LED display control (`senville_ac/display/set`).
  - Hardware connection reference and USB-A pinout documentation.

### Milestone M-02: Full Protocol Controls & In-Chassis Surface [COMPLETE]
* **Scope**:
  - Create modular C++ header `includes/midea_ext.h` implementing standard 18-byte Midea `0xAA` UART `0xB0` Set Property frame builder.
  - Include Dallas/Maxim 1-Wire CRC8 lookup table (polynomial `0x31`) over payload bytes `[10..15]` and additive two's-complement checksum over bytes `[1..16]`.
  - Expose named constants for vertical louver angle (`0x0009`), horizontal louver angle (`0x000A`), evaporator self-clean (`0x0039`), and breeze-away / indirect wind (`0x0042`).
  - Implement Home Assistant `select` entities for discrete 5-position vertical louver angle, 5-position horizontal louver angle, and preset modes.
  - Implement Home Assistant `switch` entities for discrete Power, runtime Beeper Feedback, and Breeze Away.
  - Implement Home Assistant `button` entities for LED Display Toggle, Louver Swing Step, Power Toggle, and Start Self-Clean Cycle.
  - Implement comprehensive MQTT `on_message` topic handlers for power, beeper, display, swing step, vertical/horizontal angles, presets, breeze away, self-clean, and follow-me.

### Milestone M-03: Telemetry & Energy Dashboard Integration [PENDING]
* **Scope**:
  - Inverter power curve telemetry reporting in Watts.
  - Daily/monthly cumulative kWh energy calculation sensors.
  - Dynamic defrost cycle status detection and maintenance alerts.
