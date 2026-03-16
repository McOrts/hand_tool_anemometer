# Hand Tool Anemometer — DIY Manual

<img src="docs/images/WindQX_ HandToolAnemometer1_mini.png" width="300" align="right"/>

> **A portable handheld solid-state anemometer built with a M5Stack DIAL, a WindQX solid-state anemometer sensor, a 3D-printed case, and a rechargeable battery.**

---

## Table of Contents

1. [Overview](#overview)
2. [Bill of Materials](#bill-of-materials)
3. [3D-Printed Case](#3d-printed-case)
4. [Hardware Assembly](#hardware-assembly)
5. [Programming with UIFlow 2.0](#programming-with-uiflow-20)
6. [Usage](#usage)
7. [Calibration](#calibration)
8. [Troubleshooting](#troubleshooting)
9. [License](#license)

---

## Overview

This project turns an **M5Stack DIAL** and a **WindQX SA.01P solid-state anemometer** into a compact, portable handheld wind-speed meter. Unlike traditional cup-anemometers, the WindQX sensor has no moving parts it measures wind speed and temperature using thermal technology, making it robust and maintenance-free.

The M5Stack DIAL provides:
- A round 1.28″ TFT display (240 × 240 px) for an analogue-gauge-style readout.
- A rotary encoder (EC11) to switch between display units and modes.
- An ESP32-S3 processor with built-in Wi-Fi and Bluetooth.
- A USB-C port and built-in battery (or external battery pack via Grove).

The program is written **entirely with visual blocks** in the **UIFlow 2.0** IDE, making it accessible without prior text-based coding experience.

---

## Bill of Materials

| # | Component | Qty | Notes | Picture |
|---|-----------|-----|-------|---------|
| 1 | [M5Stack DIAL](https://shop.m5stack.com/products/m5stack-dial-v1-1) | 1 | ESP32-S3, 1.28″ round display |<img src="docs/images/M5D.png" width="200" align="center"/> |
| 2 | [WindQX SA.01P Solid-State Anemometer](https://tienda.bricogeek.com/otros-sensores/2106-anemometro-termico-profesional-sin-partes-moviles-sa01p.html) | 1 | 5 V, I2C output, UART optional |<img src="docs/images/WIndQX_SA01P.png" width="200" align="center"/> |
| 3 | 3D-printed case | 1 | Files in [`3d/`](3d/) | <img src="docs/images/M5D_WindQX_3Dpieces.png" width="400" align="center"/> |
| 4 | [Li-Po rechargeable battery](https://es.aliexpress.com/item/1005007257546981.html) | 1 | 3.7v 3400mAh 18650 Li-ion with PH1.25mm 2P | <img src="docs/images/18650_Li-ion_Cable.png" width="100" align="center"/> |
| 5 | [Grove cable 4-pin, 10 cm](https://es.aliexpress.com/item/1005007474983293.html) | 1 | HY2.0-4Pin for M5Stack Development Board | <img src="docs/images/groveCable.jpg" width="200" align="center"/> |
| 6 | USB-C cable | 1 | Programming and charging | <img src="docs/images/USBC_cable.png" width="100" align="center"/> |

**Tools required:** soldering iron, resing 3D printer (or printing service).

---

## 3D-Printed Case

The case is designed to hold the M5Stack DIAL in one hand with the WindQX sensor pointing forward. All design files are in the [`3d/`](3d/) directory.

Print settings: recommended 0.2 mm layer height, 20 % infill, PLA or PETG.

---

## Hardware Assembly

### Step 1 — Prepare the M5Stack DIAL

<img src="docs/images/UIFlow2_on_M5StackDial.jpg" width="300" align="right"/>

1. Charge the Li-Po rechargeable battery before starting. You can use the M5Stack DIAL via USB-C as a charger.
3. Confirm the firmware version is **UIFlow 2.x** (hold the encoder button while powering on; the screen shows the current mode and firmware).
4. If needed, flash UIFlow 2.x firmware using the [M5Burner](https://docs.m5stack.com/en/uiflow/uiflow2/m5burner) tool.
   
| Download the UIFlow firmware | Setup the firmware | Flash the firmaware |
|-----------|----------|-------------|
| <img src="docs/images/M5Burner_UIFlow2.png" width="300" align="center"/>| <img src="docs/images/UIFlow2_Configuration.png" width="300" align="center"/> | <img src="docs/images/UIFlow2_burn.png" width="300" align="center"/> |

### Step 2 — Connect the WindQX Sensor

The WindQX sensor communicates over **I2C** at address id 54. Using the i2c0 login interface at 100 KHz. Connect it to the M5Stack DIAL **Grove Port A** (bottom right connector) using a standard Grove cable:
<img src="docs/images/WindQX_HandTool_M5Dial_bb.png" width="300" align="right"/>

| Grove Pin | DIAL Pin | WindQX Wire |
|-----------|----------|-------------|
| 1 — White  | SCL (15) | SCL   |
| 2 — Yellow | SDA (13) | SDA   |
| 3 — Red    | 5 V      | VCC + |
| 4 — Black  | GND      | GND - |

> ⚠️ The WindQX sensor requires **5 V**. The M5Stack DIAL Grove port provides 5 V on pin 3. Do not exceed this voltage.

The WindQX sensors come with a connector that is not a Grove connector, so it will need to be replaced. The simplest way is to cut it off and solder the wires so that it looks like this:
<img src="docs/images/HTA_step1.jpg" width="500" align="center"/>

| 1 | 2 | 3 |
|-----------|----------|----------|
| <img src="docs/images/HTA_step2.jpg" width="300" align="center"/> | <img src="docs/images/HTA_step3.jpg" width="300" align="center"/> | <img src="docs/images/HTA_step4.jpg" width="300" align="center"/> | 

### Step 3 — Install the Battery
<img src="docs/images/HTA_step51.jpg" width="500" align="center"/>

The M5Stack DIAL has an internal JST PH 1.25mm connector for a Li-Po battery and includes a charge regulator. Insert the 3.7 V 500 mAh battery and secure it with the provided adhesive pad if your enclosure requires it.

| 1 | 2 | 3 |
|-----------|----------|----------|
| <img src="docs/images/HTA_step5.jpg" width="300" align="center"/> | <img src="docs/images/HTA_step6.jpg" width="300" align="center"/> | <img src="docs/images/HTA_step7.jpg" width="300" align="center"/> | 

---

## Programming with UIFlow 2.0

The firmware is a **UIFlow 2.0 block program** located in [`src/anemometer_dial.m5f2`](src/anemometer_dial.m5f2).

### Loading the Program

1. Open [UIFlow 2.0](https://uiflow2.m5stack.com/) in your browser (or the desktop app).
2. Connect the M5Stack DIAL via USB-C or Wi-Fi.

| 3. Create a new project | 4. Click **Import project from local file** and select `src/WindQX_Anemometer.m5f2` |
|-----------|----------|
| <img src="docs/images/UIFlow2_P1.png" width="300" align="center"/> | <img src="docs/images/UIFlow2_P2.png" width="300" align="center"/> |

| 5. Select run or download | 6. Setup the connection to burn the program |
|-----------|----------|
| <img src="docs/images/UIFlow2_IDE_upload.png" width="300" align="center"/> | <img src="docs/images/UIFlow2_burn.png" width="300" align="center"/> |

| 7. Click **Project files (+)** and select `src/iconAlarm.png` | Icon |
|-----------|-----------|
| <img src="docs/images/UIFlow2_P3.png" width="300" align="center"/> | <img src="src/iconAlarm.png" align="center"/> |

---

### Program Logic (Block Overview)

<img src="docs/images/WindQX_HandTool_M5Dial_UiFLOW.png" width="700" align="center"/>
The program is structured in three parts:

#### A. Initialisation (On Start)

```
[On Start]
  ├─ Init built-in hardware
  ├─ Init rotary encoder → built-in
  ├─ Set alarmHighSpeed → 20
  ├─ Set SA01_ADDR → 54
  ├─ Load page0
  ├─ Configure History chart
  │   ├─ Series name → speed
  │   ├─ Update mode → shift
  │   ├─ Point count → 10
  │   ├─ Y-axis min/max → 0 / 60
  │   └─ Point size → width 10, height 10
  ├─ Set last_min_ms → current ticks in milliseconds
  ├─ Set title text → "WindQX"
  ├─ Set dialWind current value → 20
  ├─ Set lblWind text → "20"
  ├─ Set kmh text → "km/h"
  ├─ Configure alarm selector
  │   ├─ Min → 0
  │   ├─ Max → 100
  │   └─ Value → alarmHighSpeed
  ├─ Set AlarmNumber text → alarmHighSpeed
  ├─ Turn AlarmLED off
  ├─ Set AlarmLED size → 0 x 0
  ├─ Set dialTemp current value → 0
  ├─ Set lblTemp text → "0"
  ├─ Set Celsius text → "C"
  └─ Init I2C bus → SCL 15, SDA 13, 100 kHz
```

#### B. Main Loop (Loop Forever)

```
[Loop Forever]
  ├─ If BtnA is holding
  │   └─ Turn off the device
  ├─ Read 4 bytes by I2C from SA01_ADDR → data
  ├─ Extract wind bytes
  │   ├─ rawWind1 ← data[1]
  │   └─ rawWind2 ← data[2]
  ├─ Calculate wind_kmh
  │   └─ wind_kmh = ((rawWind1 × 256) + rawWind2) ÷ 10
  ├─ Update wind display
  │   ├─ Set lblWind text → wind_kmh
  │   └─ Set dialWind value → rounded wind_kmh
  ├─ Extract temperature bytes
  │   ├─ rawTemp3 ← data[3]
  │   └─ rawTemp4 ← data[4]
  ├─ Calculate temp_c
  │   └─ temp_c = (((rawTemp3 × 256) + rawTemp4) ÷ 100) - 40
  ├─ Round temp_c to integer
  ├─ Update temperature display
  │   ├─ Set lblTemp text → temp_c
  │   └─ Set dialTemp value → temp_c
  ├─ If rotary encoder has rotated
  │   ├─ Change alarmHighSpeed by encoder increment
  │   ├─ If alarmHighSpeed > 100
  │   │   └─ Keep value unchanged
  │   ├─ Set AlarmSelector value → alarmHighSpeed
  │   └─ Set AlarmNumber text → alarmHighSpeed
  ├─ If wind_kmh > alarmHighSpeed
  │   ├─ Set AlarmLED size → 10 x 10
  │   ├─ Turn AlarmLED on
  │   └─ Repeat 3 times
  │       ├─ Speaker tone 1000 for 500 ms
  │       └─ Speaker tone 15000 for 500 ms
  ├─ Else
  │   ├─ Turn AlarmLED off
  │   └─ Set AlarmLED size → 0 x 0
  ├─ Set now_ms → current ticks in milliseconds
  ├─ If (now_ms - last_min_ms) > 10000
  │   ├─ Set last_min_ms → current ticks in milliseconds
  │   └─ Add rounded wind_kmh to History chart
  └─ Wait 250 ms
```

#### C. Encoder Interaction (On Encoder Rotate)

```
[When AlarmSelector value changed]
  ├─ Set AlarmNumber text → AlarmSelector value
  └─ Set alarmHighSpeed → AlarmSelector value
```

## Usage

1. **Power on** — Short-press the encoder button. The DIAL logo appears and the device boots (~3 s).
2. **Read wind speed** — The gauge needle and numeric value update every 250 ms. If the wind speed > 0, the LED color of sensor turns into red.
3. **Set alarm threshold**
   a. Rotating the encoder clockwise to increase the value or anti-clockwise to reduce the value.
   b. By dragging the dot on the slider with your finger to the desired alarm value.
5. **Alarm** — If the current wind speed is greater than the selected alert threshold, the device will emit beeps and the speed icon will display a red dot.
6. **Power off** — Long-press the encoder button.

### Display Layout

<img src="docs/images/UIFlow2_IDE_dashboard.png" width="400" align="center"/>

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
| Display shows `---` | No I2C data from sensor | Check wiring; confirm sensor is powered (5 V LED) |
| Wind speed always 0.0 | Low battery power |
| Device won't power on | Battery depleted | Charge via USB-C for 1 hour before retrying |
| UIFlow can't find device | Wi-Fi not connected | Use USB-C connection or check Wi-Fi credentials in M5Burner |

---

## License

This project is released under the [MIT License](LICENSE).  
© McOrts — contributions welcome via Pull Request.
