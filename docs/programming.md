# Programming Guide — UIFlow 2.0

## Overview

The anemometer program is built entirely using **visual blocks** in the UIFlow 2.0 IDE — no text-based coding is required. The source file is [`../src/anemometer_dial.m5f2`](../src/anemometer_dial.m5f2).

---

## Setting Up UIFlow 2.0

1. Open **UIFlow 2.0** in your browser: <https://uiflow2.m5stack.com/>  
   *(or download the desktop app for Windows/macOS/Linux)*
2. In M5Burner, configure the DIAL's Wi-Fi credentials so it connects to the same network as your computer.
3. Power on the DIAL — after a few seconds the **API Key** is displayed on screen.
4. In UIFlow 2.0, enter the API Key and click **Connect**.
5. The IDE shows "Connected" and the DIAL screen mirrors the canvas.

---

## Opening the Project

1. In UIFlow 2.0, click **File → Open local file**.
2. Browse to `src/anemometer_dial.m5f2` and open it.
3. The block canvas loads with all blocks pre-arranged.
4. Click **▶ Run** to execute immediately on the DIAL, or **Download** to save permanently.

---

## Block Structure

### Screen / UI Setup (On Start)

These blocks run once when the device powers on:

| Block | Purpose |
|-------|---------|
| `Screen → Set background color → Black` | Dark theme for outdoor readability |
| `Screen → Draw Arc` (x=120, y=120, r=100, start=135°, end=45°, width=8, color=DarkGray `0x222222`) | Full gauge background arc |
| `Screen → Add Label "0.0"` (x=82, y=88, font=48, color=White) | Main speed readout |
| `Screen → Add Label "m/s"` (x=105, y=148, font=18, color=Gray) | Unit label |
| `Screen → Add Label "Bft 0"` (x=90, y=172, font=14, color=Yellow) | Beaufort scale label |
| `UART1 → Init (TX=GPIO2, RX=GPIO1, baud=9600)` | Start sensor communication |
| `Set variable wind_speed → 0.0` | Initialise speed |
| `Set variable unit → "m/s"` | Default unit |
| `Set variable max_gust → 0.0` | Reset max gust memory |

### Main Loop (Loop Forever)

| Block | Purpose |
|-------|---------|
| `UART1 → Read line → raw_data` | Receive one sentence from sensor |
| `If raw_data starts with "$WINDSPC"` | Validate frame header |
| `Split raw_data by ","` | Parse CSV fields |
| `Set wind_speed → field[1] (as number)` | Extract speed in m/s |
| `If unit = "km/h" → wind_speed × 3.6` | Unit conversion |
| `If unit = "knots" → wind_speed × 1.944` | Unit conversion |
| `If wind_speed > max_gust → max_gust = wind_speed` | Track peak |
| `Screen → Update label (speed) text → wind_speed (2 dp)` | Refresh numeric display |
| `Set needle_angle → map(wind_speed, 0, 30, 135, 405)` | Map speed to arc angle |
| `Screen → Draw needle line` (calculated from centre + angle) | Move gauge needle |
| `Set beaufort → beaufort_scale(wind_speed_ms)` | Calculate Beaufort number |
| `Screen → Update label (beaufort) text → "Bft " + beaufort` | Show Beaufort scale |
| `Wait 200 ms` | 5 Hz refresh rate |

### Encoder Events

| Event | Action |
|-------|--------|
| **Rotate right (increment)** | Cycle unit: m/s → km/h → knots → m/s |
| **Rotate left (decrement)** | Switch display mode: normal → max gust → normal |
| **Long press (≥ 2 s)** | Reset `max_gust` to 0.0; flash screen briefly |

### Helper Function — Beaufort Scale

A custom function block `beaufort_scale(speed_ms)` returns the Beaufort number (0-12):

| Return | Wind (m/s) | Description |
|--------|-----------|-------------|
| 0 | 0.0 – 0.2 | Calm |
| 1 | 0.3 – 1.5 | Light air |
| 2 | 1.6 – 3.3 | Light breeze |
| 3 | 3.4 – 5.4 | Gentle breeze |
| 4 | 5.5 – 7.9 | Moderate breeze |
| 5 | 8.0 – 10.7 | Fresh breeze |
| 6 | 10.8 – 13.8 | Strong breeze |
| 7 | 13.9 – 17.1 | Near gale |
| 8 | 17.2 – 20.7 | Gale |
| 9 | 20.8 – 24.4 | Severe gale |
| 10 | 24.5 – 28.4 | Storm |
| 11 | 28.5 – 32.6 | Violent storm |
| 12 | ≥ 32.7 | Hurricane |

---

## Customisation

All tunable constants are defined as variable blocks at the top of the **On Start** section:

| Variable | Default | Description |
|----------|---------|-------------|
| `CALIBRATION_OFFSET` | `0.0` | Add/subtract a fixed offset (m/s) to calibrate |
| `MAX_SPEED_SCALE` | `30.0` | Full-scale deflection of gauge (m/s) |
| `REFRESH_MS` | `200` | Loop wait time (ms) — lower = faster but higher CPU load |
| `BG_COLOR` | `0x000000` | Background colour (hex) |
| `NEEDLE_COLOR` | `0xFF4500` | Gauge needle colour (hex, OrangeRed default) |

---

## Uploading a Modified Program

1. After editing blocks, click **Download** (cloud-with-down-arrow icon).
2. The DIAL reboots and starts the new program automatically.
3. To revert to the original, re-open `anemometer_dial.m5f2` and download again.

---

## Export / Backup

UIFlow 2.0 allows exporting your block program as:
- **`.m5f2`** — Block project file (re-openable in UIFlow 2.0)
- **Python** — The generated MicroPython code (File → Export Python)

Always keep a backup of `anemometer_dial.m5f2` in version control.
