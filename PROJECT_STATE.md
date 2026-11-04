# scr-senville Repository State

Senville Aura (Midea OEM) local MQTT & Home Assistant ESPHome firmware configurations.

## Architecture & Integration
- Drop-in local replacement for cloud-dependent Senville Aura / Midea USB Wi-Fi dongles (OSK103 / OSK105).
- 5V TTL Serial UART communication at 9600 baud directly with indoor unit display board (CN3/WIFI header).
- Native ESPHome `midea` climate integration supporting modes (OFF, HEAT_COOL, COOL, HEAT, DRY, FAN_ONLY), swing modes (VERTICAL, HORIZONTAL, BOTH), presets (ECO, BOOST, SLEEP, FREEZE_PROTECTION), and custom fan modes (SILENT, TURBO).
- Simultaneous Home Assistant Native API (`api:`) and standalone MQTT (`mqtt:`) broker integration.
- MQTT topics parity with `mitsubishi2mqtt`, Home Assistant MQTT auto-discovery, custom topics for Follow-Me room temperature override (`senville_ac/follow_me/set`) and indoor LED display toggle (`senville_ac/display/set`).

## Execution Milestones
- [x] **Milestone M-01 — Initial ESPHome Midea + MQTT Configurations**:
  - Pre-built USB-A dongles (`SMLIGHT SLWF-01pro` v1/v2, `CloudFree Ductless HVAC Module`, `iot-uni-dongle`, `ESP-01S`).
  - DIY breadboard/breakout builds (`Seeed Studio XIAO ESP32C3` with BSS138 level shifter).
  - Production secrets template (`secrets.yaml.example`) and comprehensive hardware/pinout documentation (`README.md`).
- [ ] **Milestone M-02 — Bench & In-Chassis Empirical Testing**:
  - Hardware flashing, UART loopback/in-chassis verification, MQTT and Home Assistant entity validation.
