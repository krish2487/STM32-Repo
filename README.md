- [Enterprise Platform Development](#enterprise-platform-development)
  - [Pre-Requisites](#pre-requisites)
  - [Links](#links)
    - [STM32L4](#stm32l4)
    - [STM32F7](#stm32f7)
- [Repository Structure](#repository-structure)
- [Project Structure](#project-structure)
  - [L0 Lowlevel](#l0-lowlevel)
  - [L1 Third Party device dependent](#l1-third-party-device-dependent)
  - [L2 Drivers](#l2-drivers)
  - [L2 Board](#l2-board)
  - [L3 Functional third party library](#l3-functional-third-party-library)
  - [L4 Modules](#l4-modules)
  - [L5 Application](#l5-application)
- [Roadmap](#roadmap)
  - [Understanding your ARM Toolchain](#understanding-your-arm-toolchain)
  - [Cross Compilation with Buildsystems](#cross-compilation-with-buildsystems)
  - [ARM Linker Script](#arm-linker-script)
  - [Platform](#platform)
  - [RTOS](#rtos)
  - [Languages](#languages)
  - [Debugging](#debugging)
  - [Unit-Test](#unit-test)
  - [Mocking](#mocking)
  - [Third Party Modules](#third-party-modules)
  - [Tool Integration](#tool-integration)

# Enterprise Platform Development

- Developing an Enterprise-level Embedded software stack from scratch by identifying and integrating required components
- Flexibility and control over every part of Firmware development i.e from the Linker script level to the firmware application design.
- Use modern build systems (CMake and Meson) to make a IDE independent development environment to reduce cost of development and maintenance.
- Integrate tooling such as static analysis, clang-format, unit testing, mocking and debugging to improve the quality of code.

**Download [this PDF](doc/pdf/main.pdf) for a diagrammatic explanation.**

## Pre-Requisites

- `arm-none-eabi-*` GNU Toolchain
- CMake/Meson
- Ninja
- STM32CubeProgrammer
- OpenOCD
- VSCode Plugins
  - CMake
  - Cortex-Debug
  - C/C++

## Links

### STM32L4

- [STM32 Website for B-L475E-IOT01A](https://www.st.com/en/evaluation-tools/b-l475e-iot01a.html)
- STM32L475_x examples use [the B-L475E-IOT01A board](https://os.mbed.com/platforms/ST-Discovery-L475E-IOT01A/) 

### STM32F7

- [STM32 Website for NUCLEO-F746ZG](https://www.st.com/en/evaluation-tools/nucleo-f746zg.html)
- STM32F7ZG examples use [the NUCLEO-F746ZG board](https://os.mbed.com/platforms/ST-Nucleo-F746ZG/)

# Repository Structure

- Baremetal
  - Low level Linker script and learning
  - Integrate critical external components to a project
- doc
  - Component datasheeets
  - Board schematics and microcontroller datasheets
- STM32_HAL
  - STM32 HAL based examples for various microcontrollers
- Template
  - Projects built after Minimal_CMSIS in Baremetal
  - Actual application level code with various levels of tooling support
  - Takes each core part of an embedded system and creates a project around it
- third_party
  - Third Party software ZIP files used in Template projects
- tools
  - Tools used by the project, pre-installed in your system
  - Links or ZIP files

# Project Structure

Create a folder structure that would need minimal change when porting between different architectures and microcontrollers

To port our project to a different controller we would need to update these folders
- l0_lowlevel
- l1_third_party_device_specific
- l2_drivers

To port our project to a different board we would need to update these folders
- l2_board_specific
- l5_application

Platform independent code
- l3_functional_third_party
- l4_module (Design drivers that use the platform agnostic l2_drivers)

```
+-- l0_lowlevel
| +-- arm_cmsis
| +-- linker_script
| +-- device
| +-- toolchain_specific_syscalls
+-- l1_third_party_device_specific
| +-- RTOS
| +-- External_Libraries_device_specific
+-- l2_drivers
| +-- gpio_device_specific
| +-- uart_device_specific
+-- l2_board_specific
| +-- peripheral_initialization
+-- l3_functional_third_party
| +-- ring_buffer
+-- l4_module
| +-- sensors/actuators
| +-- technology
| +-- protocol
+-- l5_application
| +-- application_logic
| +-- main.c
```

## L0 Lowlevel

- ARM CMSIS 5 has different compiler and architecture specific changes
- ARM CMSIS 5 Linker script is dependent on architecture of microcontroller
- Device Header is dependent of the Microcontroller Manufacturer
- Device Startup is dependent on ARM Architecture and Microcontroller Manufacturer

## L1 Third Party device dependent

- Certain Third Party software changes its behaviour based on the architecture and device
- For example: FreeRTOS needs to be configured differently according to different microcontroller family and architectures

## L2 Drivers

- Basic drivers for **GPIO**, **Interrupt Handing**
- Basic protocol for **UART**, **SPI**, **I2C**
- These are microcontroller and architecture dependent

## L2 Board

- For internal board_specific initialization
- Syscalls based externed functions

## L3 Functional third party library

- Functional Third Party code integrated into the project
- For example: **Ring Buffer**, **JSON Library**

## L4 Modules

Writing code for various hardwares i.e

- Communication
- Sensors
- Actuators

## L5 Application

- Application Logic for the project
- `main.c` resides on the top level

# Roadmap

## Understanding your ARM Toolchain

- [x] arm-none-eabi-*
- [ ] clang for ARM

## Cross Compilation with Buildsystems

- [x] CMake
- [x] Meson
- [ ] Bazel
- [ ] Premake
- [ ] Scons

## ARM Linker Script

- [x] Custom Linker Script
- [x] Custom Startup
- [x] Integrate CMSIS Linker Script
- [x] Integrated CMSIS Startup

## Platform

- [x] CMSIS_5
- [x] STM32 HAL

## RTOS

- [x] Amazon FreeRTOS
- [x] Microsoft ThreadX

## Languages

- [x] C
  - [x] Baremetal
  - [x] Basic Driver
  - [x] RTOS
  - [x] Unit Testing
  - [x] Design
  - [ ] Tooling
- [ ] C++
  - [x] Baremetal
  - [ ] Basic Driver
  - [ ] RTOS
  - [ ] Unit Testing
  - [ ] Design
  - [ ] Tooling
- [ ] Rust
  - [x] Baremetal
  - [ ] Basic Driver
  - [ ] RTOS
  - [ ] Unit Testing
  - [ ] Design
  - [ ] Tooling

## Debugging

- [x] openOCD
- [x] arm-none-eabi-gdb
- [x] Semihosting

## Unit-Test

- [x] Unity
- [ ] CppUTest
- [ ] Google Tests
- [ ] Catch

## Mocking

- [x] Fake Function Framework (FFF)
- [ ] CMock
- [ ] CppUMock
- [ ] Google Mock

## Third Party Modules

- [ ] Ring Buffer
- [ ] JSON Parser

## Tool Integration

- [ ] Clang Format
- [ ] Static Analysis
- [ ] Code Coverage
- [ ] Continuous Integration
- [ ] Documentation Generator
