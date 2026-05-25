# RC Plane Flight Controller

This repository contains the firmware and hardware design documentation for a custom-built RC Plane Flight Controller based on the STM32F446xx microcontroller.

## Hardware Specifications

### Communication Interfaces
- **SPI (x2):**
  - **SPI1:** Dedicated to the IMU.
  - **SPI2:** Available for one additional peripheral.
- **I2C (x2):**
  - **I2C1:** Dedicated to the Barometer.
  - **I2C2:** Available for one additional peripheral.
- **USART (x3):**
  - **USART1:** Receiver input.
  - **USART2/3:** Exposed as pads on the PCB for extra peripherals.
  - All ports configured for asynchronous communication.
- **CAN (x1):** Included for future-proofing and high-reliability internal communication.

### GPIO & IO
- **Status Outputs:**
  - 1x LED output (Left side).
  - 1x Buzzer output (Left side).
- **Expansion IO:**
  - 4x Extra IOs (Right side), labeled as `GPIO_OUTPUT` and outputted as physical pins.
- **Pin Configuration:**
  - All unused/floating pins are set to **GPIO Analog** to minimize power consumption and noise.

### Timers & PWM
- **3 Timers:** All running on the internal clock.
- **10 PWM Channels:** Outputted as standard **GND | 5V | PWM** headers for direct servo/ESC connection.

## Power Management

### PCB Power Distribution
- **UBEC Input:** Splits into two separate rails:
  - **Servo Rail:** Provides +5V directly to the servo headers.
  - **System Rail:** Passes through a Schottky diode into the 3.3V regulator for the logic circuit.
- **USB Power:** Fed through a Schottky diode into the 3.3V regulator to allow logic-only operation without a battery.
- **Filtering:** Additional decoupling capacitors are placed throughout the board for stability.
- **Status LEDs:** Dedicated LEDs for monitoring both the 5V and 3.3V rails.

### STM32 Power Configuration
- **VDD:** Decoupled via capacitors to the 3.3V rail.
- **V_BAT:** Tied to 3.3V.
- **VSS:** Tied to Common Ground.
- **V_CAP:** Connected to a 2.2uF capacitor to GND (Internal regulator stability).

## Programming & Debugging

### Serial Wire Debug (SWD) Header
A dedicated header is provided for flashing and debugging using an ST-LINK:
| Pin | Function | Pin | Function |
|:---:|:---:|:---:|:---:|
| 1 | RST | 2 | SWDIO |
| 3 | GND | 4 | GND |
| 5 | NC | 6 | SWCLK |
| 7 | 3.3V | 8 | 3.3V |

### USB Interface (USB-C)
- **Mode:** USB Device - CDC (Communication Device Class / Virtual COM Port).
- **Hardware Wiring:**
  - CC1/CC2: 5.1k pulldown resistors to GND (UFP detection).
  - D+/D-: Integrated series resistors for impedance matching.
- **Flashing Options:**
  - Primary: ST-LINK via SWD.
  - Secondary: **BOOT0 button** included to pull BOOT0 HIGH for DFU (Device Firmware Upgrade) mode.

### User Interaction
- **Reset Button:** Wired to NRST (Pulls LOW to reset).
- **BOOT0 Button:** Used for entry into the internal bootloader.

## Clock Configuration
- **HSE:** 8MHz Crystal Resonator (High-Speed External).
- **PLL:** Configured with HSE as the source mux.
- **System Clock:** Sourced from the PLLCLK for maximum performance.

## Build & Development
See [GEMINI.md](./GEMINI.md) for detailed firmware build instructions and coding conventions.
