# 3D-Printed Case

## Overview

The enclosure holds the M5Stack DIAL and the WindQX sensor together in a handheld form factor. The case is split into two shells joined by four M3 screws.

```
Front view:
  ╭────────────────────────╮
  │  ╭──────────────────╮  │
  │  │  WindQX sensor   │  │  ← sensor exposed at front
  │  ╰──────────────────╯  │
  │                        │
  │    ╭──────────────╮    │
  │    │  M5 DIAL     │    │  ← dial face visible through lid window
  │    ╰──────────────╯    │
  │          ⊙             │  ← encoder knob protrudes
  ╰────────────────────────╯

Side view:
  ┌────────────────────────┐
  │  upper shell           │  19 mm (DIAL height)
  ├────────────────────────┤
  │  lower shell           │  12 mm (sensor + battery)
  └────────────────────────┘
```

---

## Files

| File | Description |
|------|-------------|
| `upper_shell.stl` | Top half — DIAL window, encoder cutout, USB-C slot |
| `lower_shell.stl` | Bottom half — sensor recess, battery compartment |
| `assembly.step` | Full assembly (STEP, for editing in FreeCAD/Fusion 360) |

> **Note:** STL files and the STEP assembly are provided in this directory. Open them with your slicer (Cura, PrusaSlicer, Bambu Studio, etc.) or CAD tool.

---

## Print Settings

| Parameter | Recommended value |
|-----------|------------------|
| Material | PLA or PETG |
| Layer height | 0.20 mm |
| Infill | 20 % (gyroid or grid) |
| Perimeters / walls | 3 |
| Supports | None required (designed for no-support printing) |
| Bed adhesion | Brim (3 mm) for the lower shell |
| Print time | ~3 h total (both parts) |
| Estimated filament | ~40 g |

---

## Assembly

1. **Clean the prints** — remove any strings or elephant-foot with a craft knife.
2. **Test-fit all components** before applying any adhesive.
3. **Lower shell first** — seat the WindQX sensor in its recess; route the Grove cable through the internal channel.
4. **Insert the battery** into the battery compartment in the lower shell.
5. **Upper shell** — lower the M5Stack DIAL into the window recess, align the Grove cable.
6. **Close the shells** — mate the upper and lower halves and insert 4× M3 × 8 mm screws.

---

## Customisation

The STEP file (`assembly.step`) can be opened in FreeCAD (free) or Autodesk Fusion 360 to adjust:
- Case thickness
- Sensor opening size
- Mounting features (belt clip, wrist strap hole)
- Label embossing
