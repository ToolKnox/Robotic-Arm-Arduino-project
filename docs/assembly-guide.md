# Robotic Arm (Arduino project) build guide

## Overview

This is a 4-DOF belt-driven robotic arm built around an Arduino Mega 2560 + RAMPS 1.4, three NEMA17/A4988 axes, and a 28BYJ-48 gripper driven through a ULN2003 board. The shoulder uses a parallel linkage so most motor mass stays near the base.

Source material used here:

- `source-robotic-arm-assembly-guide.pdf` — visual mechanical assembly sequence.
- `source-enclosure-assembly-guide.pdf` — controller/enclosure assembly sequence.
- Current portal description and original Printables BOM tables.
- GitHub firmware repo: https://github.com/ToolKnox/Robotic-Arm-Arduino-project

## Print settings

- Material: PETG is the preferred budget option. Stiffer materials such as ASA, ABS, PC, or fiber-filled engineering filament are fine for the structural parts.
- Supports: none required in the original listing.
- Infill: 15% in the original listing.
- Inspect all bearing bores, belt paths, and screw holes before final assembly. Clean stringing and elephant-foot from moving interfaces.

## Printed parts

The structured BOM now contains the 21 printed parts from the original Printables description:

1. robot_belt_arm basering
2. robot_belt_arm leg (x3)
3. robot_belt_arm socket_v2
4. robot_belt_arm rotategear
5. robot_belt_arm main_body_v2
6. robot_belt_arm endstop
7. robot_belt_arm gear_body (x2)
8. robot_belt_arm lever
9. lower_shank_140
10. upper_shank_140
11. pleuel_140
12. robot_belt_arm triplate
13. pleuel_bend_140 (x2)
14. pleuel_bend_140 - mirrored (x2)
15. triplate_dual
16. manipulator_dual
17. gripperBase_by_ftobler
18. gripper_shaft
19. gripperFinger_by_ftobler (x2)
20. Wire_Guide
21. robot_belt_arm stabilizer - myMOD

## Mechanical assembly sequence

Use the visual assembly PDF as the drawing reference. The page titles indicate this order:

1. Base ring.
2. Socket.
3. Base.
4. Rotate gear.
5. Rotate base.
6. Body base.
7. Stepper installation.
8. Upper motor.
9. Lower motor.
10. Endstop mount.
11. Lever.
12. Lower shank.
13. Lever belt.
14. Lower shank belt.
15. Stabilizer.
16. Endstop body and triplate.
17. Pleuel link.
18. Pleuel bend lower.
19. Pleuel lever.
20. Upper shank.
21. Pleuel shank.
22. Manipulator.
23. Gripper.
24. Dual-support guide.

## Build notes

- Keep the pulleys loose until the belt path is aligned, then tighten gradually.
- The attached BOM image calls for 3x 2GT-6mm 200 mm closed belts and 3x 20-tooth 2GT pulleys.
- Fit F624ZZ/F686ZZ bearings squarely. Do not force them through printed bores with a screw if the bore is visibly undersized; clean the bore first.
- Mount the three endstops so each axis can home before it reaches a mechanical hard stop.
- Keep wiring accessible until the first homing and motion tests pass.

## First power-on checklist

1. Disconnect USB and 12 V before touching the RAMPS wiring.
2. Verify 12 V polarity at the DC jack and RAMPS input.
3. Check each A4988 orientation and set current limit before installing the motors.
4. Move every joint by hand with motors disabled; nothing should bind.
5. Run endstop tests before powered homing.
6. Test one axis at a time, at low speed, with one hand near the power disconnect.
