# scr-senville Task & Progress Log

## Milestone M-01: Initial ESPHome Midea + MQTT Configurations
- [x] Document physical 4-pin USB-A UART pinout, 5V/3.3V logic level shifting requirements, and hardware options in `README.md`.
- [x] Create plug-and-play ESP8266/ESP-01S configuration for `SMLIGHT SLWF-01pro` / `CloudFree` dongles (`senville-aura-slwf01pro.yaml`).
- [x] Create DIY hardware configuration for `Seeed Studio XIAO ESP32C3` with dedicated UART GPIOs (`senville-aura-esp32c3.yaml`).
- [x] Provide sanitized secrets configuration template (`secrets.yaml.example`) compliant with Rule 6 (RFC 5737 doc IP).
- [x] Establish Git and repository governance with LF line endings (`.gitattributes`, `.gitignore`).

## Milestone M-02: Full Protocol Controls & In-Chassis Surface
- [x] Implement reusable C++ Midea `0xB0` Set Property frame builder with Dallas CRC8 & additive checksum (`includes/midea_ext.h`).
- [x] Expose discrete 5-position vertical louver angle select entity (`0x0009`) in `senville-aura-slwf01pro.yaml` & `senville-aura-esp32c3.yaml`.
- [x] Expose discrete 5-position horizontal louver angle select entity (`0x000A`) in `senville-aura-slwf01pro.yaml` & `senville-aura-esp32c3.yaml`.
- [x] Expose climate preset mode select entity (None, Eco, Boost, Sleep, Freeze Protection).
- [x] Expose discrete Power, runtime Beeper Feedback, and Breeze Away (Indirect Wind `0x0042`) switches.
- [x] Expose Display LED Toggle, Louver Swing Step, Power Toggle, and Evaporator Self-Clean (`0x0039`) buttons.
- [x] Expose 10-topic MQTT `on_message` control matrix (`power/set`, `beeper/set`, `display/set`, `swing_step/set`, `vertical_position/set`, `horizontal_position/set`, `preset/set`, `breeze_away/set`, `self_clean/set`, `follow_me/set`).
- [x] Synchronize documentation, architecture, and milestone roadmaps (`README.md`, `PROJECT_STATE.md`, `docs/MILESTONES.md`, `todo.md`).

## Milestone M-03: Telemetry & Energy Dashboard Integration
- [ ] Implement outdoor inverter power curve telemetry (Watts).
- [ ] Add cumulative daily/monthly kWh energy sensors for Home Assistant Energy Dashboard.
- [ ] Implement defrost cycle status monitoring and maintenance alerts.
