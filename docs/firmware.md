# Robotic Arm firmware guide

## Firmware source

GitHub repo: https://github.com/ToolKnox/Robotic-Arm-Arduino-project

Main sketch: `robotArm_v0.81/robotArm_v0.81.ino`

Important source files:

- `config.h` — board choice, shank lengths, homing, gear ratio, gripper mode, speed profile.
- `config_esp32.h` — Wemos D1 R32 / ESP32-specific options.
- `pinout/pinout.h` — Arduino Mega + RAMPS 1.4 pin map.
- `RampsStepper.*` — A4988/RAMPS stepper wrapper.
- `endstop.*` — homing support.
- `byj_gripper.*` — 28BYJ-48 gripper support.
- `servo_gripper.*` — alternative servo gripper support.
- `command.*` — serial command parser.

## Arduino Mega + RAMPS setup

The BOM created for this project is the Mega + RAMPS + 28BYJ/ULN2003 build. Before flashing that version, confirm `config.h` is set for:

- `BOARD_CHOICE MEGA2560` for Arduino Mega 2560 + RAMPS 1.4.
- `GRIPPER BYJ` if using the BOM's 28BYJ-48 + ULN2003 gripper.
- `MICROSTEPS 16`, matching the RAMPS microstepping jumpers under the A4988 drivers.
- `MOTOR_GEAR_TEETH 20.0` and `MAIN_GEAR_TEETH 90.0` for the belt version.
- `HOME_X_STEPPER`, `HOME_Y_STEPPER`, and `HOME_Z_STEPPER` enabled when all three endstops are installed.
- `BAUD 115200` for Serial Monitor / host control.

The checked-in repo also contains ESP32/Wemos options. Those are optional controller variants, not the reference BOM build.

## Flashing with Arduino IDE

1. Install Arduino IDE 1.8.x or 2.x.
2. Open `robotArm_v0.81/robotArm_v0.81.ino`.
3. Select **Arduino Mega 2560** as the board for the BOM build.
4. Select the USB serial port.
5. Compile once before powering the motors. If an optional controller library is missing, either install it or disable that controller path in `config.h`.
6. Upload over USB.
7. Open Serial Monitor at `115200` baud.

## Useful serial commands

The firmware parses a G-code-like command stream:

| Command | Purpose |
|---|---|
| `G0` / `G1` | Linear/interpolated move |
| `G4` | Dwell |
| `G28` | Home enabled axes |
| `G90` / `G91` | Absolute / relative coordinate mode |
| `G92` | Set position offset |
| `M3` / `M5` | Close/open gripper or gripper on/off depending selected gripper driver |
| `M17` / `M18` | Enable / disable steppers |
| `M106` / `M107` | Fan on / off |
| `M114` | Report current position |
| `M119` | Report endstop states |
| `M205 S<n>` | Set speed profile |

## First firmware test

1. Upload with motors disconnected or motor power off.
2. Confirm Serial Monitor prints the online prompt.
3. Send `M119` and manually press each endstop.
4. Enable motor power and run only one short move at a time.
5. Send `G28` only after endstops are verified.
