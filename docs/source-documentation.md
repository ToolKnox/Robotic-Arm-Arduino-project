# Source documentation bundle

This project now has structured portal docs plus the original visual PDFs from the upload.

## Copied source PDFs

| File | Purpose |
|---|---|
| `source-robotic-arm-assembly-guide.pdf` | Mechanical assembly sequence for the arm. |
| `source-wiring-guide.pdf` | Wiring diagrams for A4988s, endstops, fan, motors, and ULN2003. |
| `source-firmware-instructions.pdf` | Firmware setup visuals. |
| `source-arduino-instructions.pdf` | Arduino/Mega setup visuals. |
| `source-esp32-instructions.pdf` | Optional ESP32/Wemos setup visuals. |
| `source-enclosure-assembly-guide.pdf` | Enclosure/case assembly visuals. |

## BOM source notes

- The new structured BOM uses the attached BOM image as the quantity authority.
- The older Printables description supplied the Geniuslink purchase links and the printed-parts list.
- Differences found while importing:
  - `2GT-6mm Closed Belt 200mm`: attached image shows x3; older description table listed x1. The portal BOM uses x3.
  - `M2 10mm Button Head Screw`: attached image shows x6; older description table listed x10. The portal BOM uses x6.
  - `Paper clip (min. 35mm)` appears in the old description table but not in the attached image. It is included as a small gripper-link consumable.
