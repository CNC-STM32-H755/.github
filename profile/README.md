<p align="center"><img src="./assets/sml-github-logo-horizontal.svg" alt="Surowiecki Motion Labs" width="800"></p>

# Surowiecki Motion Labs

Embedded motion control projects focused on STM32 firmware, CNC systems and desktop HMI tools.

The first project developed under this lab is **SML CNC Control System**: a prototype CNC control platform based on a dual-core STM32H755 controller and a Qt desktop HMI.
It was originally created as an engineering thesis prototype and is now being expanded as an open development platform for CNC motion control, firmware architecture experiments and desktop-machine communication.

## Repositories

| Repository | Description |
|---|---|
| [stm32h755-cnc-firmware](https://github.com/Surowiecki-Motion-Labs/stm32h755-cnc-firmware) | Dual-core STM32H755 firmware for G-code parsing, IPC, motion execution and STEP/DIR generation. |
| [qt-cnc-hmi](https://github.com/Surowiecki-Motion-Labs/qt-cnc-hmi) | Qt desktop HMI used to send G-code, stream programs, monitor machine state and control manual jog operations. |

## SML CNC Control System

### System Overview

The controller is split into two main layers:

- **PC HMI application** sends high-level commands and displays machine status.
- **STM32H755 firmware** interprets commands, manages program execution and generates real-time control signals.

Inside the microcontroller, responsibilities are divided between two cores:

- **Cortex-M7** handles USB CDC communication, command parsing, G-code validation, program buffering and IPC command creation.
- **Cortex-M4** handles real-time execution, axis state, STEP/DIR/ENA signal generation and status reporting.

This separation keeps timing-sensitive motion control on the embedded side while the desktop application remains an operator and diagnostic interface.

### Main Features

- Dual-core STM32H755 firmware architecture.
- USB CDC text protocol over virtual COM port.
- G-code support for basic motion, positioning modes and spindle commands.
- Program streaming from desktop HMI to firmware buffer.
- Shared-memory IPC between Cortex-M7 and Cortex-M4.
- Timer-based STEP/DIR/ENA generation for three stepper axes.
- Configurable axis mechanics through header configuration files.
- Qt Widgets desktop HMI with serial communication, jog mode, feed override and status monitoring.

### Current Configuration

The current mechanical configuration assumes:

- 3 controlled axes: X, Y, Z.
- 6400 driver pulses per motor revolution.
- 5000 um linear travel per screw revolution.
- 1280 pulses per millimeter.
- Feed values expressed in `mm/min`.

These values can be changed in the firmware configuration files.

### Communication Example

```text
ADD:G91;
ADD:G1 X10000 F1000;
ADD:G90;
RUN;
STATUS;
RTSTATUS;
```

The HMI sends high-level text commands.
The firmware validates them, stores program lines, executes motion and reports status.

### Development Roadmap

#### v1.1 Cleanup

- Improve code formatting consistency.
- Add protocol documentation.
- Add wiring documentation for CL57Y stepper drivers.
- Add screenshots and diagrams to repository documentation.

#### v1.2 Safety

- Add physical limit switch input handling.
- Add E-stop diagnostic input.
- Add driver alarm inputs.
- Improve alarm and recovery workflow.

#### v1.3 Motion Planner

- Add acceleration and deceleration ramps.
- Add per-axis speed limits.
- Add trapezoidal or S-curve motion profiles.

#### v1.4 HMI Pro

- Add G-code preview.
- Add improved program queue visualization.
- Add diagnostic export.
- Add connection recovery workflow.

#### v2.0 Controller Platform

- Add command IDs and acknowledgements.
- Add protocol checksums.
- Add machine configuration from HMI.
- Add more robust long-program streaming.

### Project Status

The current version is a working prototype baseline.
It is suitable for further development, experiments and gradual transformation into a more complete CNC controller platform.

