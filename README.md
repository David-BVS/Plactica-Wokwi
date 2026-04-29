# Pico W Keypad-to-LED Controller

This repository documents and organizes a Raspberry Pi Pico W project that reads a **4x4 matrix keypad** and controls **12 LEDs** based on the pressed key.

> Core behavior is preserved from the provided source logic.

## Repository Structure

```text
.
├── CMakeLists.txt
├── diagram.json
├── src/
│   └── main.cpp
├── include/
└── docs/
    ├── architecture.md
    └── wiring.md
```

## Features

- 4x4 keypad scanning using row/column mapping.
- 12 individually addressable LEDs.
- Numeric and symbol keys trigger single LEDs or grouped on/off behavior.
- Compatible wiring documentation for Wokwi and real Pico W hardware.

## Hardware Components (from diagram)

- 1x Raspberry Pi Pico / Pico W board target
- 1x 4x4 membrane keypad
- 12x LEDs (8 blue + 4 red)
- 12x 220Ω series resistors for LEDs
- 4x 1kΩ pull-up resistors for keypad rows
- Jumper wires

## GPIO Summary

- **LED outputs (12):** GP11, GP10, GP9, GP8, GP7, GP6, GP5, GP4, GP3, GP2, GP28, GP27
- **Keypad rows (4):** GP26, GP22, GP21, GP20
- **Keypad columns (4):** GP19, GP18, GP17, GP16

See `docs/wiring.md` for the full table.

## Run in Wokwi

1. Create a new Raspberry Pi Pico project in Wokwi.
2. Replace `diagram.json` with this repository's `diagram.json`.
3. Use `src/main.cpp` logic in your simulation firmware source.
4. Start simulation and press keypad buttons to observe LED behavior.

## Run on Real Hardware (Pico W)

1. Wire components exactly as listed in `docs/wiring.md`.
2. Install Pico SDK toolchain.
3. Build firmware:
   ```bash
   mkdir -p build
   cd build
   cmake ..
   cmake --build .
   ```
4. Hold **BOOTSEL**, connect Pico W via USB, then copy generated `.uf2` to the RPI-RP2 drive.

## Wi-Fi Note

The current firmware does **not** use Wi-Fi APIs. If Wi-Fi is added later, store credentials in a separate untracked config header/source (e.g., `secrets.h`) and never commit secrets.

## Behavior Reference

- `1..8`: turn ON one of LED1..LED8
- `9`: turn ON LEDs 1..8
- `0`: turn OFF LEDs 1..8
- `A..D`: turn ON one of LED9..LED12
- `*`: turn ON LEDs 9..12
- `#`: turn OFF LEDs 9..12

## Self-Critique and Improvement Pass

- Initial draft captured structure and basic mapping.
- Improved pass added explicit GPIO tables/docs split, build steps, and security note for future Wi-Fi extension.
