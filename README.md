# Robotic Arm — Arduino + RAMPS 1.4

A 4-DOF belt-driven robotic arm running on an **Arduino Mega 2560 + RAMPS 1.4 shield**, with three NEMA17 + A4988 joints and a 28BYJ-48 (or servo) gripper. The shoulder uses a parallel linkage, so the heavy motors stay at the base and the printed parts don't have to carry them.

📺 **It in motion:** <https://youtu.be/T5URUtJ1Zg4>

> Firmware lives here. The linked guides and Bill of Material PDF cover the mechanical build, wiring, ordering, and first-test workflow.

---

## Build resources

- [Bill of Material PDF](docs/BOM.pdf) — downloadable BOM with clickable Amazon / Geniuslink purchase links.
- [Assembly guide](docs/assembly-guide.md)
- [Wiring guide](docs/wiring.md)
- [Firmware guide](docs/firmware.md)
- [Arduino setup notes](docs/arduino.md)
- [Optional ESP32 / Wemos notes](docs/esp32.md)
- [Enclosure notes](docs/enclosure.md)


## What's in here

```
robotArm_v0.81/
├── robotArm_v0.81.ino     ← main sketch (open this in the Arduino IDE)
├── config.h               ← board + feature selection (gripper, controller, axes)
├── pinout/
│   ├── pinout.h           ← default pin map (Arduino Mega + RAMPS 1.4)
│   ├── pinout_uno.h       ← Uno variant
│   └── pinout_wemosD1R32.h← Wemos D1 R32 / ESP32 variant
├── config_esp32.h         ← ESP32 build overrides
├── RampsStepper.{h,cpp}   ← stepper wrapper for A4988 on RAMPS
├── endstop.{h,cpp}        ← homing
├── byj_gripper.{h,cpp}    ← 28BYJ-48 + ULN2003 gripper driver
├── servo_gripper.{h,cpp}  ← alternative hobby-servo gripper
├── controller_ps4.{h,cpp} ← optional PS4 manual controller
├── controller_wiimote.{h,cpp} ← optional Wiimote manual controller
├── command.{h,cpp}        ← serial command parser
├── queue.h                ← move queue
├── interpolation.{h,cpp}  ← linear interpolation between target poses
├── robotGeometry.{h,cpp}  ← inverse kinematics for this arm
├── fanControl.{h,cpp}     ← 12 V cooling fan
└── logger.{h,cpp}         ← serial logger

config.h_samples/           ← reference configs (incl. upstream ftobler config)
```

## Hardware target

| Part | Default |
|---|---|
| MCU | Arduino Mega 2560 |
| Driver shield | RAMPS 1.4 |
| Joint motors | 3× NEMA17 + A4988 (X / Y / Z = base / shoulder / elbow) |
| Gripper | 28BYJ-48 + ULN2003 (default) — or hobby servo, see `config.h` |
| Homing | 3× limit switches (one per stepper joint) |
| PSU | 12 V / 4 A via 2.1 mm DC jack |
| Cooling | 12 V 5010 fan |

The Uno and ESP32 (Wemos D1 R32) pinouts are checked-in but the Mega + RAMPS combination is the supported reference build.

## Pin assignments

> Authoritative pin map: [`robotArm_v0.81/pinout/pinout.h`](robotArm_v0.81/pinout/pinout.h) for Mega/RAMPS, and `pinout_uno.h` / `pinout_wemosD1R32.h` for the alternative boards.
>
> TODO (maintainer): hoist the X/Y/Z/E step+dir+enable pins, the three endstop pins, the gripper pins and the fan pin into a table here so visitors don't have to open the header.

## Build & flash

### Arduino IDE

1. Install the **Arduino IDE 1.8.x or 2.x**.
2. **Tools → Board → Arduino Mega 2560**.
3. Open `robotArm_v0.81/robotArm_v0.81.ino`.
4. Install the libraries the sketch `#include`s through **Tools → Manage Libraries…**
   - TODO (maintainer): list the exact libraries here once verified against the `#include` lines in the .ino — likely candidates given the file layout: `AccelStepper`, `Servo`, and (only if you enable that controller) `PS4Controller` / a Wiichuck library. Do **not** install controller libs you don't use.
5. Connect the Mega over USB, pick the right port, and **Upload**.

### Configuring before you flash

Open `robotArm_v0.81/config.h` and pick:

- **Board / pinout** — uncomment the matching pinout include
- **Gripper** — 28BYJ-48 stepper *or* hobby servo (mutually exclusive)
- **Manual controller** — none / PS4 / Wiimote (optional; the arm also works headless over serial)
- **Axis direction flips** and **steps-per-degree** — match these to your microstepping jumpers on the RAMPS board

> The `config.h_samples/` folder has reference configs you can diff against if your build behaves oddly.

## Running it

After flashing, open the Serial Monitor at the baud rate set in the sketch (TODO: confirm — `Serial.begin(...)` in `robotArm_v0.81.ino`).

The firmware accepts a queued command stream parsed by `command.cpp` — see that file for the exact syntax it understands. The `queue.h` + `interpolation.cpp` pair suggests G-code-style buffered moves with linear interpolation between targets, but please confirm against `command.cpp` before relying on specifics.

If you wired up a PS4 pad or a Wiimote and enabled it in `config.h`, the arm can also be jogged manually — pairing instructions live in the relevant `controller_*.cpp`.

### Homing

On startup the firmware drives each motorised joint toward its limit switch. Make sure all three switches are wired and triggering before you power up — an un-homed axis with a missing endstop will crash into the frame.

## Troubleshooting

- **Sketch won't compile, complaining about a missing library** — check the `#include` lines at the top of `robotArm_v0.81.ino` and the `controller_*.h` files; install whatever's missing through the Library Manager.
- **Steppers buzz but don't turn** — A4988 Vref not set, or microstepping jumpers under the drivers don't match `config.h`. Set Vref to ~0.7 V for a NEMA17 first, then tune.
- **Joint runs the wrong way** — flip its `INVERT_*` flag in `config.h` (or swap one coil pair on the motor connector).
- **Endstop never triggers / triggers permanently** — RAMPS endstop headers are 3-pin (S / – / +); a swapped connector latches the input. Test with a multimeter in continuity mode.
- **Gripper stepper just hums** — 28BYJ-48 coil order on the ULN2003 is `IN1-IN2-IN3-IN4`; getting the order wrong gives vibration without rotation.

## Credits

This codebase shares structure with **ftobler/robotArm** (see `config.h_samples/config_sample_V0.61/ftobler_original/`). If this repo is derived from that project, the upstream license applies — TODO (maintainer): confirm the lineage and add a proper attribution + matching LICENSE file.

## License

> TODO (maintainer): commit a `LICENSE` file. If this is derived from ftobler/robotArm it inherits **GPL-2.0**; if it's an independent rewrite, pick one (MIT / GPL-3.0 / CC-BY-NC-SA for the docs, etc.) and reference it here.