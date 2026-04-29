# Wiring (Raspberry Pi Pico W)

## Assumptions

- The provided firmware is Arduino-style C++ and uses raw GPIO numbers.
- Pico W pin naming below follows **GPx** labels.
- LEDs are active-high through 220Ω resistors to GPIO, with cathodes tied to GND.

## Component List

- Raspberry Pi Pico W (RP2040)
- 4x4 membrane keypad (R1-R4, C1-C4)
- 12x LEDs
- 12x 220Ω resistors (LED current limiting)
- 4x 1kΩ resistors (row pull-ups to 3V3)

## Keypad GPIO Mapping

| Keypad Signal | Pico W GPIO |
|---|---|
| R1 | GP26 |
| R2 | GP22 |
| R3 | GP21 |
| R4 | GP20 |
| C1 | GP19 |
| C2 | GP18 |
| C3 | GP17 |
| C4 | GP16 |

## LED GPIO Mapping

| Logical LED Index (`ledPins[]`) | Pico W GPIO | Diagram Label |
|---:|---:|---|
| 0 | GP11 | 1 |
| 1 | GP10 | 2 |
| 2 | GP9  | 3 |
| 3 | GP8  | 4 |
| 4 | GP7  | 5 |
| 5 | GP6  | 6 |
| 6 | GP5  | 7 |
| 7 | GP4  | 8 |
| 8 | GP3  | A |
| 9 | GP2  | B |
| 10 | GP28 | C |
| 11 | GP27 | D |

## Power and Ground

- All LED cathodes connect to Pico GND.
- Keypad row pull-up resistor network ties rows to 3V3.
- Ensure shared ground among all parts.

## Wokwi vs Real Hardware

- Wokwi can simulate keypad/LED interaction directly.
- On physical hardware, keep wire lengths short and verify resistor values before powering.
