# Custom PCB Smartwatch

A self-contained smartwatch built around an ESP32 microcontroller and a round color display — a first custom PCB project, scoped to be finishable by a beginner and demoable as a resume/portfolio piece.

## Status

- [ ] Breadboard prototype validated
- [ ] Schematic complete (KiCad)
- [ ] ERC clean
- [ ] PCB layout complete
- [ ] DRC clean
- [ ] Gerbers exported
- [ ] PCB ordered
- [ ] Components ordered
- [ ] Board assembled
- [ ] Power-on / bring-up passed
- [ ] Firmware complete

*(Update this checklist as the project progresses.)*

## Core features

- ESP32-WROOM-32 — WiFi, NTP time sync, runs the firmware
- GC9A01 1.28" round SPI display, 240x240 — analog watch face with drawn hands/ticks
- Weather via Open-Meteo, refreshed periodically, with icons
- Single button to cycle screens: time / weather / date / extra info
- LDR-based auto-dim at night
- Battery percentage via a voltage-divider on one ADC pin
- WS2812 ("NeoPixel") RGB status LED — notifications / charging / low battery
- Coin vibration motor (single NPN transistor + flyback diode) for alerts
- MCP73831 LiPo charger IC + USB-C power input

## Repo structure

- KiCad project files (schematic, PCB, footprints, libraries) live at the repo root.
- `.gitignore` excludes KiCad backup/cache/autosave files, netlists, and exported BOM files — see that file for the full list.

## Collaboration workflow

KiCad's `.kicad_sch` / `.kicad_pcb` files don't diff or merge cleanly like code, so two people editing the same file at once causes painful conflicts. Rule of thumb:

1. `git pull` before opening KiCad.
2. Let the other person know you're working on the schematic/PCB.
3. Close KiCad, then `git add -A && git commit -m "..." && git push`.
4. Tell the other person when you're done so they can pull.

## Bill of materials (summary)

ESP32-WROOM-32 · GC9A01 240x240 round SPI display · MCP73831T-2ACI/OT LiPo charger · USB-C power-only connector · 3.7V 500mAh LiPo (JST-PH, protected) · SMD tactile button · LDR + divider resistor · WS2812 RGB LED · coin vibration motor + NPN transistor + flyback diode + base resistor · AMS1117-3.3 regulator (if needed) · 0603/0805 resistor & capacitor kits · programming header.

## Build sequence

1. Learn the basics (voltage/current, resistors/capacitors, SPI/I2C).
2. Breadboard prototype using dev boards (ESP32 dev board + GC9A01 breakout) to validate the concept.
3. KiCad: schematic → ERC → footprints/layout → DRC → Gerbers.
4. Order the bare PCB and all components (matching footprints exactly).
5. Solder the board; verify voltages with a multimeter before connecting the battery.
6. Bring up subsystems one at a time: LED blink → display → WiFi → vibration motor.
7. Write and flash firmware: WiFi + NTP → weather API → watch-face rendering → button screen-cycling → auto-dim → RGB LED status → vibration alerts.
