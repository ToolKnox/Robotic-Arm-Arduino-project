# Arduino setup notes

The attached `source-arduino-instructions.pdf` contains the visual Arduino setup walkthrough. Use it with the firmware guide in this docs folder.

## Reference build

- Board: Arduino Mega 2560.
- Shield: RAMPS 1.4.
- Drivers: 3x A4988.
- Gripper driver: ULN2003 for a 28BYJ-48 stepper.
- Power: 12 V / 4 A supply through a 2.1 mm DC jack.

## Checklist

1. Install the Arduino IDE.
2. Open the `robotArm_v0.81/robotArm_v0.81.ino` sketch from the GitHub repo.
3. Select **Arduino Mega 2560** and the active USB port.
4. Confirm the board/gripper settings in `config.h` match the hardware in the BOM.
5. Compile before connecting motor power.
6. Upload over USB.
7. Test endstops over serial before powered homing.

## Notes

The upstream firmware supports multiple board/controller combinations. For this BOM, treat the Mega + RAMPS build as the reference path. Only use the ESP32/Wemos options if you are intentionally building the wireless-controller variant.
