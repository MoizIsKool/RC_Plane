# RC Plane Firmware

This project contains the firmware for an RC Plane, based on the STM32F446xx microcontroller. It is managed using STM32CubeMX and built with CMake.

## Project Overview

- **MCU:** STM32F446xx (ARM Cortex-M4)
- **Framework:** STM32Cube HAL
- **Build System:** CMake
- **Peripherals Used:**
  - **CAN2:** Controller Area Network for internal communication.
  - **I2C1, I2C2:** For sensors (IMU, Barometer, etc.).
  - **SPI1, SPI2:** For high-speed peripherals (Radio, SD Card, etc.).
  - **TIM2, TIM3, TIM4:** PWM generation for servos/motors and timing.
  - **UART4, UART5, USART6:** Telemetry, GPS, and debugging.
  - **USB:** Virtual COM Port (CDC) for configuration and telemetry.

## Directory Structure

- `firmware/Core/`: Main application logic and peripheral initialization.
- `firmware/Drivers/`: STM32 HAL and CMSIS drivers.
- `firmware/USB_DEVICE/`: USB Device library and CDC interface.
- `firmware/cmake/`: CMake configuration files for the STM32 toolchain.
- `firmware/RC_Plane.ioc`: STM32CubeMX project file.

## Building and Running

### Prerequisites

- ARM GNU Toolchain (`arm-none-eabi-gcc`)
- CMake (version 3.22 or higher)
- Build tool (e.g., Ninja or Make)

### Build Instructions

1.  Navigate to the `firmware` directory.
2.  Configure the project:
    ```bash
    cmake --preset Debug
    ```
    Alternatively, manual configuration:
    ```bash
    cmake -B build/Debug -DCMAKE_BUILD_TYPE=Debug -G Ninja
    ```
3.  Build the project:
    ```bash
    cmake --build build/Debug
    ```

### Flashing

Use an ST-LINK debugger with `openocd` or the STM32CubeProgrammer.

Example using `openocd`:
```bash
openocd -f interface/stlink.cfg -f target/stm32f4x.cfg -c "program build/Debug/RC_Plane.elf verify reset exit"
```

## Development Conventions

- **STM32CubeMX Integration:** Always use the `/* USER CODE BEGIN */` and `/* USER CODE END */` blocks in generated files to ensure your changes are preserved when re-generating code from the `.ioc` file.
- **Coding Style:** Follow standard C11 conventions. Use HAL-provided types and macros for peripheral access.
- **USB Logging:** The Virtual COM Port (CDC) is intended for telemetry and debugging. Use the `usbd_cdc_if.c` interface for communication.
- **Error Handling:** Use the `Error_Handler()` function for critical failures.
