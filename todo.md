# scr-senville Task & Progress Log

## Milestone M-01: Initial ESPHome Midea + MQTT Configurations
- [x] Document physical 4-pin USB-A UART pinout, 5V/3.3V logic level shifting requirements, and hardware options in `README.md`.
- [x] Create plug-and-play ESP8266/ESP-01S configuration for `SMLIGHT SLWF-01pro` / `CloudFree` dongles (`senville-aura-slwf01pro.yaml`).
- [x] Create DIY hardware configuration for `Seeed Studio XIAO ESP32C3` with dedicated UART GPIOs (`senville-aura-esp32c3.yaml`).
- [x] Provide sanitized secrets configuration template (`secrets.yaml.example`) compliant with Rule 6 (RFC 5737 doc IP).
- [x] Establish Git and repository governance with LF line endings (`.gitattributes`, `.gitignore`).

## Milestone M-02: Verification & Flashing Validation
- [ ] Compile and validate syntax with `esphome compile senville-aura-slwf01pro.yaml`.
- [ ] Compile and validate syntax with `esphome compile senville-aura-esp32c3.yaml`.
- [ ] Flash physical target dongle and verify Home Assistant auto-discovery & MQTT status topics.
