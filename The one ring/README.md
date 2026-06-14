# The One Ring — Hardware

KiCad PCB design for the *One Ring* wearable camera + speaker device.

## Contents
- `The one ring.kicad_sch` — schematic
- `The one ring.kicad_pcb` — PCB layout
- `The one ring.kicad_pro` — KiCad project file
- `fp-lib-table` — footprint library table
- `custom.pretty/` — custom footprints (including Adafruit MAX98357A breakout)

## Key components
| Ref | Part | Function |
|-----|------|----------|
| U1  | XIAO-ESP32-S3-Sense | MCU + camera + **onboard PDM mic** + WiFi/BLE |
| U2  | MAX98357A | I2S 3.2 W mono Class-D audio amp |
| SP1 | 8 ohm speaker | Audio output |
| SW1 | Tactile push button | User input |
| J1  | 1×2 pin header | Battery / power input |

## I2S audio wiring
| Signal | XIAO pin | MAX98357A pin |
|--------|----------|---------------|
| BCLK   | GPIO1 / D0 | BCLK (16) |
| LRCLK  | GPIO2 / D1 | LRCLK (14) |
| DIN    | GPIO3 / D2 | DIN (1) |
| SD     | GPIO4 / D3 | ~SD_MODE (4) |

Power rails: +3.3 V and +BATT (Li-Po via XIAO onboard JST).

## Onboard PDM microphone
The XIAO ESP32S3 Sense module has an **integrated PDM mic** (MSM261S4030H0R). Its pins are NOT exposed on the DIP header — they are internal module connections:

| Signal  | GPIO | Notes |
|---------|------|-------|
| PDM_CLK | 41   | Internal to module |
| PDM_DIN | 42   | Internal to module |

> ⚠️ **Firmware constraint:** On ESP32S3, PDM RX is **only supported on I2S0**. If your firmware assigns the PDM mic to `I2S_NUM_1` or `I2S_NUM_AUTO`, the device will crash into a boot loop. The speaker (standard I2S TX) should use `I2S_NUM_1`.

## Notes
- Connect +BATT to the XIAO onboard JST connector (not routed to the DIP header).
- Gain pin on MAX98357A is left floating for 9 dB default gain; tie to GND for 6 dB or VIN for 12 dB / 15 dB if needed.
- Speaker pads are top-left on the Adafruit breakout footprint; 7-pin signal header is bottom.

## Companion project
The matching PlatformIO firmware lives in the sibling repo/directory:
`~/projects/the one ring/` (note the space in the name).
