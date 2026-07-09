# Optional ESP32 / Wemos D1 R32 notes

The attached `source-esp32-instructions.pdf` covers the optional ESP32/Wemos path. This is not the reference BOM build; the BOM image and portal BOM are for Arduino Mega 2560 + RAMPS 1.4.

Use the ESP32 path only if you deliberately want the Wemos D1 R32 controller variant from the firmware repo.

## Source files

- `robotArm_v0.81/config_esp32.h`
- `robotArm_v0.81/pinout/pinout_wemosD1R32.h`
- `robotArm_v0.81/controller_ps4.*`
- `robotArm_v0.81/controller_wiimote.*`

## Cautions

- The RAMPS pinout table in `wiring.md` does not apply directly to the Wemos D1 R32 path.
- Optional PS4/Wiimote controller support can require extra Arduino libraries.
- Re-check gripper mode: the BOM image uses 28BYJ-48 + ULN2003, while some checked-in configs use a servo gripper.
