# PCB - SafetyDevice

**Document Version:** 1.0  
**Date:** May 2025

---

## 1. Features

| Feature                   | Description                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| **Microcontroller**      | STM32F411CEU6 (ARM Cortex-M4, 100 MHz, 512 KB Flash, 128 KB SRAM)           |
| **Motion Sensing**       | MPU-6050 6-axis IMU (accelerometer + gyroscope) via I²C                    |
| **Gas Sensor**           | TGS822 with heater control and analog sensing input                         |
| **Wireless Interface**   | UART interface with power supply for AG9 wireless communication module      |
| **Power Supply**         | Direct battery input (Li-ion/Li-Po), internally regulated to 3.3 V and 5 V  |
| **Push Button Input**    | User input via debounced button                                             |
| **Indicators**           | Two LED indicators: one controllable, one always on                         |
| **Buzzer Output**        | Buzzer connected for alerts                                                 |
| **Debugging**            | SWD header (SWCLK, SWDIO, GND)                                              |
| **USB Interface**        | USB available for data only (no USB VBUS power support)                     |
| **Test Pads**            | All critical signals are broken out to the bottom layer for debugging       |

---

## 2. Electrical Characteristics

| Parameter               | Value            |
|------------------------|------------------|
| Input Voltage (VIN)    | 2.0 V – 6.0 V     |
| Output Voltage (VOUT)  | 3.3 V and 5.0 V   |
| Logic Voltage (VLOGIC) | 3.3 V             |

---

## 3. Functional Overview

### 3.1 Microcontroller

- **Part Number**: STM32F411CEU6  
- **Core**: ARM Cortex-M4  
- **Clock**: Up to 100 MHz (24 MHz external crystal is used as high-speed clock source)  
- **Memory**: 512 KB Flash / 128 KB SRAM  
- **Peripherals**: I²C, UART, SPI, ADC, PWM, USB FS

### 3.2 Motion Sensing (MPU-6050)

Communication with the microcontroller is via I²C (bus 1), with an interrupt line also available.  
[MPU-6050 Datasheet](https://invensense.tdk.com/wp-content/uploads/2015/02/MPU-6000-Datasheet1.pdf)

| Signal | Pin   | Description                     |
|--------|-------|---------------------------------|
| SDA    | PB6   | I²C1 Data                       |
| SCL    | PB7   | I²C1 Clock                      |
| INT    | PB8   | Interrupt output from MPU-6050  |

### 3.3 Gas Sensor Interface (TGS822)

Sensor output is read by the microcontroller ADC. The heater enable pin (drive high to turn on) reduces power consumption.  
[TGS822 Datasheet](https://cdn.soselectronic.com/productdata/03/a8/53fad086/tgs-822.pdf)

| Signal     | Pin   | Description               |
|------------|-------|---------------------------|
| HEATER_EN  | PB15  | Digital output for heater |
| ANALOG_OUT | PA4   | Analog voltage input (ADC)|

### 3.4 Interface for AG9 Wireless Module

Microcontroller communicates via UART and supplies power to the module.  
[AG9 Datasheet](https://docs.ai-thinker.com/_media/b101ps01a4_a9g_product_specification.pdf)  
[Helpful Guide](https://wiki.dfrobot.com/A9G_Module_SKU_TEL0134)

| Signal | Pin   | Description         |
|--------|-------|---------------------|
| TX     | PB9   | UART1 transmit      |
| RX     | PB10  | UART1 receive       |
| VCC    | 5 V   | AG9 module power    |
| GND    | —     | Ground              |

### 3.5 Indicators and Buzzer

LED1 is placed at the top-top-left corner and is software-controllable. LED2 is always on (connected to 3.3 V).  
A push button is available and debounced in hardware.

| Component   | Pin   | Notes                             |
|-------------|-------|-----------------------------------|
| LED1        | PA17  | Software-controlled indicator     |
| LED2        | —     | Always on (3.3 V connected)       |
| Buzzer      | PA2   | Digital or PWM-controlled output  |
| Push Button | PA5   | Debounced user push button        |

---

## 4. USB and Debugging

- **USB Interface**: Standard USB FS (full-speed/12 Mbps) data lines (D+/D−)
- **Note**: USB power (VBUS) is not routed to the board supply – use battery input
- **Debug Port**: SWDIO, SWCLK, GND exposed for in-circuit programming/debugging

STM32CubeIDE is recommended for programming and firmware development.

---

## 5. Power Supply

- Designed for direct Li-ion cell input (3.7 V typical)
- Includes onboard switching regulators (external protection recommended)
- Capable of driving external 5 V modules such as AG9

---

## 6. Mechanical & Test Points

- **Mechanical**: See `mechanical.pdf` for mounting details and dimensions
- **Test Pads**: All critical signals are broken out on the bottom side for testing and debugging:

| Test Pad | Description              |
|----------|--------------------------|
| T1       | Power (3.3 V / GND)       |
| T2       | AG9 Module Interface      |
| T3       | IMU I²C Interface         |
| T4       | Gas Sensor Signals        |