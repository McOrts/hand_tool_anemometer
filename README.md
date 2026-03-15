# Hand Tool Anemometer — DIY Manual

> **A portable handheld solid-state anemometer built with an M5Stack DIAL, a WindQX solid-state anemometer sensor, a 3D-printed case, and a rechargeable battery.**

---

## Table of Contents

1. [Overview](#overview)
2. [Bill of Materials](#bill-of-materials)
3. [Hardware Assembly](#hardware-assembly)
4. [Wiring Diagram](#wiring-diagram)
5. [3D-Printed Case](#3d-printed-case)
6. [Programming with UIFlow 2.0](#programming-with-uiflow-20)
7. [Usage](#usage)
8. [Calibration](#calibration)
9. [Troubleshooting](#troubleshooting)
10. [License](#license)

---

## Overview

This project turns an **M5Stack DIAL** and a **WindQX SA.01P solid-state anemometer** into a compact, portable handheld wind-speed meter. Unlike traditional cup-anemometers, the WindQX sensor has no moving parts it measures wind speed and temperature using thermal technology, making it robust and maintenance-free.

The M5Stack DIAL provides:
- A round 1.28″ TFT display (240 × 240 px) for an analogue-gauge-style readout.
- A rotary encoder (EC11) to switch between display units and modes.
- An ESP32-S3 processor with built-in Wi-Fi and Bluetooth.
- A USB-C port and built-in battery (or external battery pack via Grove).

The program is written **entirely with visual blocks** in the **UIFlow 2.0** IDE, making it accessible without prior text-based coding experience.

![Assembled anemometer](docs/images/assembled.png)
> *Assembled prototype (rendering — replace with your own photo)*

---

## Bill of Materials

| # | Component | Qty | Notes | Picture |
|---|-----------|-----|-------|---------|
| 1 | [M5Stack DIAL](https://shop.m5stack.com/products/m5stack-dial-v1-1) | 1 | ESP32-S3, 1.28″ round display |<img src="docs/images/M5D.png" width="400" align="center"/> |
| 2 | [WindQX SA.01P Solid-State Anemometer](https://tienda.bricogeek.com/otros-sensores/2106-anemometro-termico-profesional-sin-partes-moviles-sa01p.html) | 1 | 5 V, UART output, RS-485 optional |<img src="docs/images/WIndQX_SA01P.png" width="200" align="center"/> |
| 3 | 3D-printed case | 1 | Files in [`3d/`](3d/) | <img src="docs/images/M5D_WindQX_3Dpieces.png" width="400" align="center"/> |
| 4 | [Li-Po rechargeable battery](https://es.aliexpress.com/item/1005007257546981.html) | 1 | 3.7v 3400mAh 18650 Li-ion with PH1.25mm 2P | <img src="docs/images/18650_Li-ion_Cable.png" width="200" align="center"/> |
| 5 | [Grove cable 4-pin, 10 cm](https://es.aliexpress.com/item/1005007474983293.html) | 1 | HY2.0-4Pin for M5Stack Development Board | <img src="docs/images/groveCable.jpg" width="200" align="center"/> |
| 6 | USB-C cable | 1 | Programming and charging | <img src="docs/images/USBC_cable.png" width="200" align="center"/> |

**Tools required:** soldering iron, resing 3D printer (or printing service).

---

## Hardware Assembly

### Step 1 — Prepare the M5Stack DIAL

1. Charge the Li-Po rechargeable battery before starting. You can use the M5Stack DIAL via USB-C as a charger.
2. Confirm the firmware version is **UIFlow 2.x** (hold the encoder button while powering on; the screen shows the current mode and firmware).
3. If needed, flash UIFlow 2.x firmware using the [M5Burner](https://docs.m5stack.com/en/uiflow/uiflow2/m5burner) tool.
   
| Download the UIFlow firmware | Setup the firmware | Flash the firmaware |
|-----------|----------|-------------|
| <img src="docs/images/M5Burner_UIFlow2.png" width="200" align="center"/>| <img src="docs/images/UIFlow2_Configuration.png" width="200" align="center"/> | <img src="docs/images/UIFlow2_burn.png" width="200" align="center"/> |

### Step 2 — Connect the WindQX Sensor

The WindQX sensor communicates over **UART** at 9 600 baud (default). Connect it to the M5Stack DIAL **Grove Port A** (bottom connector) using a standard Grove cable:

| Grove Pin | DIAL Pin | WindQX Wire |
|-----------|----------|-------------|
| 1 — Yellow | GPIO 1 (RX) | TX |
| 2 — White  | GPIO 2 (TX) | RX |
| 3 — Red    | 5 V         | VCC |
| 4 — Black  | GND         | GND |

> ⚠️ The WindQX sensor requires **5 V**. The M5Stack DIAL Grove port provides 5 V on pin 3. Do not exceed this voltage.

### Step 3 — Install the Battery

The M5Stack DIAL has an internal JST connector for a Li-Po battery. Insert the 3.7 V 500 mAh battery and secure it with the provided adhesive pad if your enclosure requires it.

### Step 4 — Place Components in the Case

See [3D-Printed Case](#3d-printed-case) for full instructions on printing and assembling the enclosure.

---

## Wiring Diagram

```
  WindQX Sensor                   M5Stack DIAL
  ┌────────────┐                 ┌──────────────────┐
  │  VCC (5V)  │────────────────▶│ Grove Pin 3 (5V) │
  │  GND       │────────────────▶│ Grove Pin 4 (GND)│
  │  TX        │────────────────▶│ Grove Pin 1 (RX) │
  │  RX        │◀───────────────│ Grove Pin 2 (TX) │
  └────────────┘                 └──────────────────┘
                                          │
                                   Li-Po Battery
                                   3.7V / 500mAh
```

---

## 3D-Printed Case

The case is designed to hold the M5Stack DIAL in one hand with the WindQX sensor pointing forward. All design files are in the [`3d/`](3d/) directory.

See [`3d/README.md`](3d/README.md) for:
- Print settings (recommended 0.2 mm layer height, 20 % infill, PLA or PETG)
- Assembly instructions
- STL file list

---

## Programming with UIFlow 2.0

The firmware is a **UIFlow 2.0 block program** located in [`src/anemometer_dial.m5f2`](src/anemometer_dial.m5f2).

### Loading the Program

1. Open [UIFlow 2.0](https://uiflow2.m5stack.com/) in your browser (or the desktop app).
2. Connect the M5Stack DIAL via USB-C or Wi-Fi.
3. Click **File → Open** and select `src/anemometer_dial.m5f2`.
4. Click the **▶ Run** button to deploy it wirelessly, or **Download** to save it permanently on the device.

### Program Logic (Block Overview)

The program is structured in three parts:

#### A. Initialisation (On Start)

```
[On Start]
  ├─ Set screen background → Black
  ├─ Draw gauge background arc (dark gray, r=100 px, 135°–45° sweep)
  ├─ Draw label "m/s" at centre-bottom
  ├─ Init UART1 (TX=GPIO2, RX=GPIO1, 9600 baud)
  └─ Set wind_speed variable → 0
```

#### B. Main Loop (Loop Forever)

```
[Loop Forever]
  ├─ UART read line → raw_data
  ├─ Parse raw_data → wind_speed (float, m/s)
  ├─ Update gauge needle angle (0° = 0 m/s, 270° = 30 m/s)
  ├─ Update centre numeric label with wind_speed
  ├─ Update Beaufort scale label
  └─ Wait 200 ms
```

#### C. Encoder Interaction (On Encoder Rotate)

```
[On Encoder Rotate]
  ├─ If rotate RIGHT → cycle unit (m/s → km/h → knots)
  ├─ If rotate LEFT  → cycle display mode (gauge → graph → max)
  └─ Refresh display
```

#### WindQX UART Data Format

The WindQX sensor sends ASCII frames at 9 600 baud, 8N1:

```
$WINDSPC,<speed_ms>,<direction_deg>,<temp_c>*<checksum>\r\n
```

Example: `$WINDSPC,3.40,270,22.5*6F\r\n`

The program extracts the `<speed_ms>` field and converts it to the selected unit.

### Detailed Block Screenshot

See [`docs/programming.md`](docs/programming.md) for annotated screenshots of every block group.

---

## Usage

1. **Power on** — Short-press the encoder button. The DIAL logo appears and the device boots (~3 s).
2. **Read wind speed** — The gauge needle and numeric value update every 200 ms.
3. **Change units** — Rotate the encoder clockwise to cycle: **m/s → km/h → knots → m/s**.
4. **Max gust mode** — Rotate counter-clockwise to switch to Max Gust display (shows the highest reading since power-on).
5. **Reset max** — Long-press (>2 s) the encoder button to reset the max gust memory.
6. **Power off** — Double-press the encoder button.

### Display Layout

```
          ╭─────────────────╮
         ╱  ●────────────╮  ╲
        │  /    3.4 m/s   │   │
        │  │   ─────────  │   │
        │  │  Beaufort 2  │   │
        │  ╰──────────────╯   │
         ╲                   ╱
          ╰─────────────────╯
```

---

## Calibration

The WindQX sensor is factory-calibrated. For best results:

1. Point the sensor directly into the wind (0° incidence).
2. Hold the device at arm's length, away from body turbulence.
3. For field calibration against a reference anemometer, use the `CALIBRATION_OFFSET` constant at the top of the program (block: **Set CALIBRATION_OFFSET to `0.0`**). A positive value adds to every reading; a negative value subtracts.

---

## Troubleshooting

| Symptom | Possible cause | Fix |
|---------|---------------|-----|
| Display shows `---` | No UART data from sensor | Check wiring; confirm sensor is powered (5 V LED) |
| Wind speed always 0.0 | UART baud rate mismatch | Confirm WindQX is set to 9600; see sensor manual |
| Needle jumps erratically | Electrical noise on cable | Use a shielded Grove cable or add 100 nF cap on VCC |
| Device won't power on | Battery depleted | Charge via USB-C for 30 min before retrying |
| UIFlow can't find device | Wi-Fi not connected | Use USB-C connection or check Wi-Fi credentials in M5Burner |

---

## License

This project is released under the [MIT License](LICENSE).  
© McOrts — contributions welcome via Pull Request.
