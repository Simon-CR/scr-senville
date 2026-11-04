# Senville Aura Local MQTT & Home Assistant Controller (ESPHome)

Replace the cloud/Alexa-only Senville Aura USB-A Wi-Fi dongle (`OSK103` / `OSK105` / SmartHome stick) with 100% local **MQTT** (like `mitsubishi2mqtt`) and **Home Assistant** control using ESPHome.

---

## 1. Hardware Guide: Why Senville Uses a Special USB-A Port

Senville Aura mini-splits are OEM **Midea** units. Although the Wi-Fi module plugs into what looks like a standard **USB Type-A** receptacle underneath the front panel, **it is NOT USB 2.0**.
Instead, Midea wired the 4-pin USB-A receptacle directly to a **5V TTL Serial UART (9600 baud, 8N1)** bus on the indoor display board:

| USB-A Pin | Standard USB Name | Senville / Midea Function | Connects To (on ESP Dongle) | Logic Voltage |
| :--- | :--- | :--- | :--- | :--- |
| **Pin 1** | `VBUS` | **+5V DC Power** | 5V Input (to 3.3V LDO Regulator) | +5.0V |
| **Pin 2** | `D-` | **AC UART TX** (Data from AC) | **ESP UART RX** (via 5V→3.3V shifter) | **5V TTL** |
| **Pin 3** | `D+` | **AC UART RX** (Data to AC) | **ESP UART TX** (via 3.3V→5V shifter) | **5V TTL** |
| **Pin 4** | `GND` | **Ground** | **ESP GND** | 0V |

> **Important Electrical Note:** You cannot plug a standard USB-C/Micro-USB ESP32 dev board into this port via a passive USB adapter cable because standard dev boards wire their USB port to a USB-serial bridge chip (CH340/CP2102) expecting differential USB 2.0 packets—not raw 5V UART. Furthermore, the AC signals at **5V logic**, whereas ESP8266/ESP32 GPIOs are **3.3V** (requiring a 5V-to-3.3V logic level shifter).

### Recommended Hardware (Ranked by Size, Availability, and Price)

| Rank | Hardware Option | Size & Fit | Where to Buy | Price | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **1 (Best Overall)** | **SMLIGHT SLWF-01pro (v2.1)** or **CloudFree Ductless HVAC Module** | **52 × 20 × 9 mm** — Exact drop-in replacement for the OEM USB dongle; fits inside the Senville front-panel cradle with the lid closed. | **CloudFree.shop** (US fast shipping), **Amazon**, **AliExpress** (SMLIGHT Official Store) | **$13 – $22** | Purpose-built for Midea/Senville USB-A ports. Has onboard 3.3V regulator, 5V↔3.3V level shifter, and pre-flashed ESPHome. Use [`senville-aura-slwf01pro.yaml`](./senville-aura-slwf01pro.yaml). |
| **2 (Best DIY)** | **Seeed Studio XIAO ESP32C3** + **2-ch BSS138 Level Shifter** + **Male USB-A Breakout** | **~45 × 18 × 8 mm** heat-shrunk — Easily fits inside the Senville dongle pocket. | **Amazon**, **Seeed**, **DigiKey**, **Mouser**, **AliExpress** | **~$7 – $9** | Tiny thumb-sized ESP32-C3 with hardware UART + USB-C flashing port. Solder USB-A Pin 2/3 through a mini BSS138 level shifter to `GPIO20`/`GPIO21`. Use [`senville-aura-esp32c3.yaml`](./senville-aura-esp32c3.yaml). |
| **3 (Cheapest DIY)** | **ESP-01S** + **ESP-01 Breadboard/Level Shifter Adapter** + **Male USB-A Plug** | **~55 × 25 × 14 mm** — Tight vertical clearance in the plastic cradle unless direct-soldered without header sockets. | **Amazon**, **AliExpress** | **~$4 – $6** | Uses `board: esp01_1m` in [`senville-aura-slwf01pro.yaml`](./senville-aura-slwf01pro.yaml). |

> **Tip on the Senville USB-A Plastic Keying Tab:** Some Senville Aura sockets have a tiny plastic ridge inside the USB-A female housing (or use a 4-pin JST-XH pigtail behind the display cover). Pre-built Midea sticks (`SLWF-01pro` / `CloudFree`) slot right in; if using a generic DIY male USB-A plug, you can either shave the small plastic nub inside the socket or unplug the USB-A pigtail from the display PCB's 4-pin `CN3`/`WIFI` JST header and wire straight to the 4-pin header.

---

## 2. Firmware Architecture & Files

1. **[`senville-aura-slwf01pro.yaml`](./senville-aura-slwf01pro.yaml)**: For pre-built USB-A Midea dongles (`SMLIGHT SLWF-01pro`, `CloudFree`, `iot-uni-dongle`) and `ESP-01S` (`GPIO1` TX / `GPIO3` RX).
2. **[`senville-aura-esp32c3.yaml`](./senville-aura-esp32c3.yaml)**: For DIY `Seeed Studio XIAO ESP32C3` / `ESP32-C3 SuperMini` builds (`GPIO21` TX / `GPIO20` RX).
3. **[`includes/midea_ext.h`](./includes/midea_ext.h)**: Modular C++ helper implementing the Midea `0xB0` Set Property frame builder with Dallas/Maxim 1-Wire CRC8 table and two's-complement checksum calculation.
4. **[`secrets.yaml.example`](./secrets.yaml.example)**: Template for Wi-Fi, OTA, and MQTT broker credentials.

### Building & Flashing

```bash
cp secrets.yaml.example secrets.yaml
# Edit secrets.yaml with your Wi-Fi and MQTT broker details
esphome run senville-aura-slwf01pro.yaml
```

---

## 3. Protocol Controls & Exposed Home Assistant Entities

The configuration exposes 100% of controllable Senville Aura / Midea OEM capabilities across native Home Assistant entities:

### Climate Platform (`climate:`)
- **Modes**: `OFF`, `HEAT_COOL`, `COOL`, `HEAT`, `DRY`, `FAN_ONLY`
- **Swing Modes**: `OFF`, `VERTICAL`, `HORIZONTAL`, `BOTH`
- **Presets**: `ECO`, `BOOST`, `SLEEP`, `FREEZE_PROTECTION` (8°C / 46°F freeze guard)
- **Fan Modes**: `SILENT`, `TURBO`
- **Display Control**: `AUTO` (Hardware display toggle integration)

### Select Entities (`select:`)
- **Vertical Louver Position**: `Auto Swing`, `1 - Top (0°)`, `2 - Upper (25%)`, `3 - Middle (50%)`, `4 - Lower (75%)`, `5 - Bottom (90°)` (Midea Property `0x0009`)
- **Horizontal Louver Position**: `Auto Swing`, `1 - Far Left`, `2 - Mid Left`, `3 - Center`, `4 - Mid Right`, `5 - Far Right` (Midea Property `0x000A`)
- **Preset Mode**: `None`, `Eco`, `Boost`, `Sleep`, `Freeze Protection`

### Switch Entities (`switch:`)
- **Power**: Discrete On/Off control (`midea_ac.power_on` / `midea_ac.power_off`)
- **Beeper Feedback**: Runtime toggleable buzzer sound on command reception (`midea_ac.beeper_on` / `midea_ac.beeper_off`)
- **Breeze Away (Indirect Wind)**: Directs airflow away from room occupants (Midea Property `0x0042`)

### Button Entities (`button:`)
- **Toggle Display LED**: Turns indoor unit 7-segment temperature display on or off (`midea_ac.display_toggle`)
- **Louver Swing Step**: Increments louver angle by one physical step (`midea_ac.swing_step`)
- **Power Toggle**: Inverts power state (`midea_ac.power_toggle`)
- **Start Self-Clean Cycle**: Initiates indoor evaporator freeze & defrost cleaning cycle (Midea Property `0x0039`)
- **Restart Dongle**: Hardware restart of the ESP controller

---

## 4. MQTT Topics & Auto-Discovery (`mitsubishi2mqtt` Parity)

With `mqtt:` and `discovery: true` enabled, all entities automatically register in Home Assistant over MQTT while also accepting standalone MQTT commands:

| Feature | MQTT Topic | Direction | Accepted Payloads / Description |
| :--- | :--- | :--- | :--- |
| **Availability** | `senville_ac/status` | State | `online` / `offline` |
| **Climate Mode** | `senville_ac/climate/climate/mode/command` | Command | `off`, `cool`, `heat`, `dry`, `fan_only`, `heat_cool` |
| **Target Temp (°C)** | `senville_ac/climate/climate/target_temperature/command` | Command | `22.0` |
| **Fan Speed** | `senville_ac/climate/climate/fan_mode/command` | Command | `auto`, `low`, `medium`, `high`, `silent`, `turbo` |
| **Swing Mode** | `senville_ac/climate/climate/swing_mode/command` | Command | `off`, `vertical`, `horizontal`, `both` |
| **Power Control** | `senville_ac/power/set` | Command | `ON`, `OFF`, `TOGGLE` |
| **Beeper Feedback** | `senville_ac/beeper/set` | Command | `ON`, `OFF` |
| **Toggle Display LED** | `senville_ac/display/set` | Command | `TOGGLE` (or any payload) |
| **Louver Swing Step** | `senville_ac/swing_step/set` | Command | `STEP` (or any payload) |
| **Vertical Position** | `senville_ac/vertical_position/set` | Command | `auto`, `1`..`5`, `0`, `25`, `50`, `75`, `100`, or option name |
| **Horizontal Position** | `senville_ac/horizontal_position/set` | Command | `auto`, `1`..`5`, `0`, `25`, `50`, `75`, `100`, or option name |
| **Preset Mode** | `senville_ac/preset/set` | Command | `NONE`, `ECO`, `BOOST`, `SLEEP`, `FREEZE_PROTECTION` |
| **Breeze Away** | `senville_ac/breeze_away/set` | Command | `ON`, `OFF` |
| **Self-Clean Cycle** | `senville_ac/self_clean/set` | Command | `START` |
| **Follow-Me Room Temp** | `senville_ac/follow_me/set` | Command | `21.5` (Overrides internal thermistor with external room sensor) |
| **Outdoor Temp** | `senville_ac/sensor/outdoor_temperature/state` | State | `14.5` (°C) |
| **Power Usage** | `senville_ac/sensor/power_usage/state` | State | `420` (Watts, outdoor inverter reporting) |
