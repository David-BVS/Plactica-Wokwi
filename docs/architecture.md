# Firmware Architecture

## Overview

The firmware is a single-loop, event-driven keypad handler:

1. Initialize 12 LED GPIO pins as outputs and drive LOW at boot.
2. Poll keypad for a pressed key.
3. If a valid key is detected, execute a `switch` action block.
4. Delay 10ms to reduce polling churn.

## File Layout

- `src/main.cpp`: original control logic, preserved.
- `docs/wiring.md`: pin-level integration details.
- `docs/architecture.md`: execution model and behavior map.

## Module Notes

### Keypad Definition

- `keys[4][4]` sets the key matrix character mapping.
- `rowPins[]` and `colPins[]` define matrix wiring to Pico GPIO.
- `Keypad keypad = Keypad(...)` binds map and pins.

### LED Control

- `ledPins[12]` maps logical channels to Pico GPIOs.
- `setup()` sets all LED pins to output and OFF.
- `loop()` updates outputs based on key actions.

## Behavior Table

| Key | Action |
|---|---|
| `1..8` | Turn ON a single LED in group 1 (indices 0..7) |
| `9` | Turn ON LEDs 0..7 |
| `0` | Turn OFF LEDs 0..7 |
| `A..D` | Turn ON a single LED in group 2 (indices 8..11) |
| `*` | Turn ON LEDs 8..11 |
| `#` | Turn OFF LEDs 8..11 |

## Constraints and Non-Goals

- No behavior changes made to source logic.
- No Wi-Fi/network runtime behavior implemented.
- Documentation prioritizes reproducibility in simulator and hardware.
