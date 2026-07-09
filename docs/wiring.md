# Robotic Arm wiring guide

## Source references

- `source-wiring-guide.pdf` — visual wiring pages for A4988, limit switches, fan, motors, and ULN2003.
- GitHub reference pin map: `robotArm_v0.81/pinout/pinout.h`.

## Safety

- Do not plug or unplug stepper motors while the RAMPS board is powered.
- Set A4988 current limits before sustained motion.
- Use the 12 V / 4 A supply from the BOM and keep all grounds common.
- Use ferrules or secured screw terminals for the power input; loose RAMPS power wiring overheats quickly.

## RAMPS 1.4 / Mega pin map

The GitHub firmware maps the supported Mega + RAMPS build like this:

| Function | Step | Dir | Enable | Endstop |
|---|---:|---:|---:|---:|
| X / upper arm stepper | 54 | 55 | 38 | X_MIN 3 |
| Y / lower arm stepper | 60 | 61 | 56 | Y_MIN 14 |
| Z / base rotation stepper | 46 | 48 | 62 | Z_MIN 18 |
| E0 / optional rail stepper | 26 | 28 | 24 | E0_MIN 20 |

Other checked-in Mega/RAMPS pins:

| Function | Pin |
|---|---:|
| 28BYJ/ULN2003 IN1 | 40 |
| 28BYJ/ULN2003 IN2 | 63 |
| 28BYJ/ULN2003 IN3 | 59 |
| 28BYJ/ULN2003 IN4 | 64 |
| Servo alternative | 4 |
| Fan | 9 |
| Pump optional output | 8 |
| Laser optional output | 10 |
| LED | 13 |

## Wiring sequence from the source PDF

1. Wire and configure the three A4988 driver sockets.
2. Wire the limit switches for X, Y, and Z homing.
3. Wire the 12 V 5010 fan to the configured fan output.
4. Wire the lower-arm motor.
5. Wire the base-rotation motor.
6. Wire the ULN2003 driver board for the 28BYJ-48 gripper motor.
7. Wire the upper-arm motor.

## Endstop checks

Before the first homing command, use serial command `M119` from the firmware to confirm each switch changes state when pressed. If an endstop reads triggered all the time, check the signal/ground orientation and the normally-open/normally-closed setting in `config.h`.

## Axis direction checks

If an axis moves away from its endstop or moves opposite the expected direction, use the inversion flags in `config.h` rather than rewiring the whole machine:

- `INVERSE_X_STEPPER`
- `INVERSE_Y_STEPPER`
- `INVERSE_Z_STEPPER`
- `INVERSE_E0_STEPPER` if using the rail axis
