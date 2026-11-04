# scr-senville Repository State

Senville Aura (Midea OEM) local MQTT & Home Assistant ESPHome firmware configurations.

## Architecture & Integration
- Drop-in local replacement for cloud-dependent Senville Aura / Midea USB Wi-Fi dongles (OSK103 / OSK105).
- 5V TTL Serial UART communication at 9600 baud directly with indoor unit display board (CN3/WIFI header).
- Native ESPHome `midea` climate integration supporting modes (OFF, HEAT_COOL, COOL, HEAT, DRY, FAN_ONLY), swing modes (VERTICAL, HORIZONTAL, BOTH), presets (ECO, BOOST, SLEEP, FREEZE_PROTECTION), and custom fan modes (SILENT, TURBO).
- Modular C++ Midea `0xB0` Set Property frame builder (`includes/midea_ext.h`) implementing Dallas/Maxim 1-Wire CRC8 and additive two's-complement checksum.
- Full Home Assistant entity surface: `climate`, `select` (vertical/horizontal louver positions, presets), `switch` (power, runtime beeper feedback, breeze away), `button` (display toggle, swing step, power toggle, self-clean cycle, restart).
- Simultaneous Home Assistant Native API (`api:`) and standalone MQTT (`mqtt:`) broker integration.
- Comprehensive MQTT `on_message` control matrix (`mitsubishi2mqtt` parity + custom Senville topics for power, beeper, display, swing step, louver positions, presets, breeze away, self-clean, and follow-me temperature override).

## Execution Milestones
- [x] **Milestone M-01 — Initial ESPHome Midea + MQTT Configurations**:
  - Pre-built USB-A dongles (`SMLIGHT SLWF-01pro` v1/v2, `CloudFree Ductless HVAC Module`, `iot-uni-dongle`, `ESP-01S`).
  - DIY breadboard/breakout builds (`Seeed Studio XIAO ESP32C3` with BSS138 level shifter).
  - Production secrets template (`secrets.yaml.example`) and comprehensive hardware/pinout documentation (`README.md`).
- [x] **Milestone M-02 — Full Protocol Controls & In-Chassis Surface**:
  - Reusable `includes/midea_ext.h` frame builder with CRC8 lookup table and two's complement checksum.
  - Discrete 5-position vertical and horizontal louver position selects via Midea properties `0x0009` and `0x000A`.
  - HA template switches for Power, Beeper Feedback, and Breeze Away (Indirect Wind `0x0042`).
  - HA template buttons for Display LED Toggle, Louver Swing Step, Power Toggle, and Evaporator Self-Clean (`0x0039`).
  - Standalone MQTT `on_message` command topics for 100% feature coverage without requiring Home Assistant API.
- [ ] **Milestone M-03 — Telemetry & Energy Dashboard Integration**:
  - Inverter power curve telemetry (Watts), cumulative kWh energy sensors, and dynamic defrost status alerts.
