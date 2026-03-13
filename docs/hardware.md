# Hardware Reference

## M5Stack DIAL

| Specification | Value |
|--------------|-------|
| MCU | ESP32-S3FN8 (Xtensa LX7 dual-core, 240 MHz) |
| Flash | 8 MB |
| PSRAM | 8 MB |
| Display | 1.28″ round TFT IPS, 240 × 240 px, GC9A01 driver |
| Touch | Capacitive (single-point) |
| Encoder | EC11 rotary encoder with push button |
| Speaker | 1 W, 8 Ω |
| RGB LED | SK6812 (1 × addressable LED) |
| Grove Port | Port A — I2C / UART (GPIO 1/2) |
| Battery connector | JST 1.25 mm, 2-pin (3.7 V Li-Po) |
| USB | USB-C (programming, charging, serial) |
| Dimensions | 54 × 54 × 19 mm |
| Mass | ~47 g (without external battery) |

Datasheet: <https://docs.m5stack.com/en/core/M5Dial>

---

## WindQX Solid-State Anemometer

The WindQX is a solid-state (no moving parts) anemometer that uses thermal/ultrasonic principles to measure wind speed with high reliability and low maintenance requirements.

| Specification | Value |
|--------------|-------|
| Measurement range | 0 – 30 m/s |
| Accuracy | ±0.3 m/s (< 5 m/s), ±3 % (≥ 5 m/s) |
| Resolution | 0.1 m/s |
| Power supply | 5 V DC ±5 % |
| Current consumption | < 80 mA |
| Interface | UART (TTL 3.3 V / 5 V tolerant) |
| Baud rate | 9 600 bps (default), 8N1 |
| Output rate | 5 Hz (every 200 ms) |
| Operating temperature | −20 °C to +70 °C |
| Protection class | IP54 |
| Connector | Grove 4-pin or bare wire |

### UART Frame Format

```
$WINDSPC,<speed_ms>,<direction_deg>,<temp_c>*<checksum>\r\n
```

| Field | Type | Example |
|-------|------|---------|
| `speed_ms` | Float, 2 decimals | `3.40` |
| `direction_deg` | Integer 0-359 | `270` |
| `temp_c` | Float, 1 decimal | `22.5` |
| `checksum` | XOR of all bytes between `$` and `*` (hex) | `6F` |

---

## Li-Po Battery

| Specification | Value |
|--------------|-------|
| Chemistry | Lithium Polymer (LiPo) |
| Nominal voltage | 3.7 V |
| Capacity | 500 mAh |
| Connector | JST 1.25 mm (matching M5Stack DIAL) |
| Charge current | 300 mA max via USB-C |
| Approximate runtime | ~4 hours at normal operation |
| Dimensions | 35 × 25 × 6 mm (typical) |

> ⚠️ **Safety:** Never puncture, crush, or expose the Li-Po battery to temperatures above 60 °C. Always use a charger with over-current and over-voltage protection.

---

## Grove Cable

Standard M5Stack 4-pin Grove cable (HY2.0-4P), 20 cm, for sensor-to-DIAL connection.

Pin assignment (from sensor side):

| Pin | Colour | Function |
|-----|--------|----------|
| 1 | Yellow | TX (sensor) → RX (DIAL GPIO 1) |
| 2 | White | RX (sensor) ← TX (DIAL GPIO 2) |
| 3 | Red | 5 V |
| 4 | Black | GND |
