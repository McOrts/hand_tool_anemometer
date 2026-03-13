# Assembly Instructions

## Prerequisites

- All components from the [Bill of Materials](../README.md#bill-of-materials) are available.
- The 3D-printed case parts are printed and cleaned (see [`../3d/README.md`](../3d/README.md)).
- You have a small Phillips screwdriver and a clean, well-lit workspace.

---

## Step 1 — Flash UIFlow 2.0 Firmware

1. Download and install **M5Burner** from <https://docs.m5stack.com/en/uiflow/uiflow2/m5burner>.
2. Connect the M5Stack DIAL to your computer via USB-C.
3. Open M5Burner, select **M5DIAL** from the device list.
4. Select the latest **UIFlow2** firmware and click **Burn**.
5. Once flashing completes, configure the Wi-Fi credentials in M5Burner so the device can connect to UIFlow 2.0 OTA updates.

---

## Step 2 — Install the Battery

1. Locate the JST 1.25 mm connector inside the M5Stack DIAL housing.
2. Connect the Li-Po battery — align the polarity (red = positive).
3. Fold the battery cable neatly inside the housing to avoid pinching.
4. Apply the adhesive pad to the battery if the case requires it.

> The M5Stack DIAL will begin charging the battery as soon as it is connected to USB-C.

---

## Step 3 — Connect the WindQX Sensor

1. Take the 20 cm Grove cable.
2. Plug one end into the **Grove Port A** (bottom of the M5Stack DIAL).
3. Connect the other end to the WindQX sensor:
   - Yellow → TX pad on sensor PCB
   - White  → RX pad on sensor PCB
   - Red    → VCC (5 V)
   - Black  → GND

4. Verify the WindQX sensor power LED lights up when the DIAL is powered on.

---

## Step 4 — Test the Connection (Before Closing the Case)

1. Load the program [`src/anemometer_dial.m5f2`](../src/anemometer_dial.m5f2) via UIFlow 2.0 (see [Programming Guide](programming.md)).
2. Power on the DIAL.
3. Blow gently across the WindQX sensor — the gauge needle and numeric value should respond.
4. If the display shows `---`, refer to the [Troubleshooting section](../README.md#troubleshooting).

---

## Step 5 — Install Components in the Case

### 5a. Lower shell (sensor mount)

1. Route the Grove cable through the sensor channel at the front of the lower shell.
2. Seat the WindQX sensor in its mount pocket; it should click in with a friction fit.
3. Apply a small drop of hot glue at the rear of the sensor if extra retention is needed.

### 5b. Install the M5Stack DIAL

1. Lower the M5Stack DIAL into the upper shell recess, rotary encoder facing upward.
2. Align the USB-C port cutout.
3. The Grove cable should exit cleanly through the internal channel.

### 5c. Close the case

1. Mate the upper and lower shells.
2. Insert 4× M3 × 8 mm screws into the corner posts and tighten finger-tight.
3. Do **not** overtighten — the PLA/PETG posts can crack.

---

## Step 6 — Final Check

- [ ] Encoder rotates freely and clicks when pressed.
- [ ] USB-C port is accessible through the case cutout.
- [ ] WindQX sensor is fully exposed and not obstructed.
- [ ] Wind speed reading is stable and responds to airflow.
- [ ] Battery charges when USB-C is connected.

---

## Case Exploded View

```
         ┌─────────────────────────┐
         │  Upper Shell            │
         │  ┌─────────────────┐   │
         │  │  M5Stack DIAL   │   │  ← encoder knob protrudes through lid
         │  └────────┬────────┘   │
         │           │ Grove cable │
         └───────────┼─────────────┘
                     │
         ┌───────────┼─────────────┐
         │  Lower Shell            │
         │  ┌────────┴────────┐   │
         │  │  WindQX Sensor  │   │  ← sensor faces forward
         │  └─────────────────┘   │
         │  Battery compartment    │
         └─────────────────────────┘
```
