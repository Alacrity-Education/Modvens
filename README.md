# Modvens
Inspired by IKEA’s Frekvens, this project turns individual LED matrix cubes into building blocks. You can combine them into a custom-sized display for scrolling text, animations and data visualizations.

## Hardware

Each cube has two boards connected by a 15-pin flat flex cable:
- **Interface board** – soldered to the back of the Frekvens LED matrix; adapts its pads to the FFC connector and filters the LED supply.
- **Main board** – sits inside the cube and drives the matrix *(in progress)*.

### Interface board

A small adapter board (32 × 48.5 mm) that attaches to the back of the original Frekvens LED matrix PCB and breaks its signals out to a flat flex cable. The cable connects to the main board inside the cube.

The KiCad project lives in [`CAD/modvens/`](CAD/modvens/).

#### Front side (F.Cu) – matrix interface

The custom footprint `Project_Components:frekvens` (U1) mirrors the row of pads on the edge of the Frekvens LED matrix PCB, so the two boards can be soldered pad-to-pad. The pad positions and spacing come from measurements of the original board:

| Pin | Signal | Notes |
|-----|--------|-------|
| 1 | GND | |
| 2 | Latch | |
| 3 | Clk | |
| 4 | DA | Serial data |
| 5 | EN | Output enable |
| 6 | VCC | Fed from the filtered `VLED` rail |
| 7 | MIC | |
| 8 | CH6 | Not connected |

#### Back side (B.Cu) – connector and power filter

- **J1 – GCT FFC3A20-15-G**, a 15-pin flat flex connector carrying the matrix signals and power:

  | J1 pin | Signal |
  |--------|--------|
  | 1, 3, 5, 7, 9, 11, 12 | GND |
  | 2 | MIC |
  | 4 | EN |
  | 6 | Latch |
  | 8 | Clk |
  | 10 | DA |
  | 13, 14, 15 | +4V |

  Each signal sits between ground pins to reduce crosstalk along the cable.

- **LC filter** between the incoming `+4V` supply and the `VLED` rail that powers the matrix:
  - Input (`+4V`): 100 µF electrolytic (C12), 2 × 10 µF 1812 (C13, C14), 10 µF 0603 (C15)
  - Series inductor: 10 µH TaiTech TMPC1265 (L1)
  - Output (`VLED`): 3 × 1000 µF electrolytic (C9–C11), 4 × 10 µF 1812 (C5–C8), 4 × 10 µF 0603 (C1–C4)

  The bulk capacitance on `VLED` buffers the large current spikes from the multiplexed LEDs, and the inductor keeps them from going back onto the shared supply.

## Status

- [x] Interface board design
- [ ] Main board design
- [ ] Send boards to manufacturing
- [ ] Assembly
- [ ] Write the firmware
- [ ] Test the firmware on the boards
