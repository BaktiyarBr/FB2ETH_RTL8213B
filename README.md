# FB2ETH for OpenIPC

A small fiber-to-Ethernet media converter board for [OpenIPC](https://openipc.org) cameras. It connects a camera's 10/100 Ethernet port to a single-fiber BiDi SFP optical module, so the camera can be linked over optical fiber instead of copper.

The board is designed in KiCad 10.

## How it works

| Block | Part | Role |
|-------|------|------|
| Ethernet / SerDes switch | **RTL8213B-CG** (U1) | Bridges the camera's copper Ethernet (MDI) to the SFP's high-speed serial lanes |
| Power | **MP9943GQ-Z** (U3) + 10 µH inductor (L2) | Buck converter that makes the +3V3 rail from the camera's VBUS |
| Clock | 25 MHz crystal (X2) | Reference clock for the RTL8213B |
| Status | 3 × 0402 LEDs (PWR1, P6S1, P6S2) | Power and port link/activity |
| Camera side | **J1** – JST SH 1.0 mm, 6-pin | Ethernet + power from the camera |
| Fiber side | **J2** – 9-pin SFP BiDi module socket | Optical transceiver |

### J1 – camera connector (JST SH, 6-pin)

| Pin | Signal |
|-----|--------|
| 1 | ETH_TX+ |
| 2 | ETH_TX− |
| 3 | ETH_RX+ |
| 4 | ETH_RX− |
| 5 | GND |
| 6 | VBUS (power in) |

### J2 – SFP BiDi module (9-pin)

| Pin | Signal |
|-----|--------|
| 1 | GND |
| 2 | RXP |
| 3 | RXN |
| 4 | SD (signal detect) |
| 5, 6 | +3V3 |
| 7 | SFP_TX_N |
| 8 | SFP_TX_P |
| 9 | GND |

## Repository layout

```
FB2ETH_3/FB2ETH_3/     Current revision (4-layer PCB) — start here
  FB2ETH_3.kicad_pro   KiCad project
  FB2ETH_3.kicad_sch   Schematic
  FB2ETH_3.kicad_pcb   PCB layout
  FB2ETH_3.kicad_dru   Custom design rules
  FB2ETH_3.csv         Full BOM with LCSC part numbers
  production/          Gerbers (zip), BOM, pick-and-place, IPC netlist for JLCPCB
FB2ETH_2/FB2ETH_2/     Previous revision
FB2ETH/                First revision
from_epro/             Original EasyEDA Pro design imported into KiCad (reference)
projectLibs.pretty/    Project footprints (SFP connectors, QFN-8 for MP9943)
```

The `*-backups/` folders are KiCad's automatic project backups.

## Manufacturing

The `production/` folder of each revision is generated with the KiCad [Fabrication Toolkit](https://github.com/bennymeg/Fabrication-Toolkit) plugin (settings in `fabrication-toolkit-options.json`) and is ready to upload to JLCPCB:

- `FB2ETH_3.zip` – Gerber and drill files
- `bom.csv` – bill of materials for assembly
- `positions.csv` – component placement (CPL)

To regenerate them, open `FB2ETH_3.kicad_pcb` in KiCad and run **Fabrication Toolkit** from the PCB editor's plugin menu.

## Opening the project

1. Install [KiCad](https://www.kicad.org/) 10 or newer.
2. Open `FB2ETH_3/FB2ETH_3/FB2ETH_3.kicad_pro`.
3. If footprints show as missing, add `projectLibs.pretty` to the project's footprint library table under the nickname `projectLibs`.
