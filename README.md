#  Thrust Stand Electronics System Using ESP32

##  Project Overview

This project involves designing a **compact, high-performance electronics system for a brushless motor thrust stand**. The goal is to **measure and log real-time thrust, voltage, current, temperature, and RPM** while using a **LiPo battery power source**. The system is built around an **ESP32-WROOM-32UE MCU** and logs data **wirelessly over Wi-Fi** to a remote client/server or dashboard.

The thrust stand is essential for testing propulsion systems in drones, RC planes, or electric propulsion experiments. It helps validate performance under various loads and environmental conditions.

---

##  Key Features

- Supports **up to 60V** input from a **4S–6S LiPo battery**
- Measures:
  - **Current up to 100A** (using ACS758)
  - **Voltage up to 60V** (via voltage divider)
  - **Thrust up to 5kg** (using HX711 + load cell)
  - **Temperature up to 150°C** (using MAX6675 + K-Type thermocouple)
  - **RPM up to 100,000** (using high-speed IR sensor circuit)
- ESP32 handles:
  - **Sensor data acquisition**
  - **Motor control via ESC**
  - **Wi-Fi data logging**
- Compact and efficient **PCB design**
- Modular, upgradable hardware for future sensors or actuators

---

##  Design Approach

1. **Requirement Analysis**:
   - Power: Up to 60V, 100A from 4S–6S LiPo batteries
   - Sensors must be **ESP32-compatible**, operate within **3.3V logic**, and offer **high sampling rates**

2. **Microcontroller Selection**:
   - Chose **ESP32-WROOM-32UE** for:
     - Dual-core performance
     - Rich GPIOs
     - Native Wi-Fi
     - Compact size with external antenna (UE variant)

3. **Sensor Selection**:
   Each sensor was carefully chosen for accuracy, ESP32 compatibility, and environmental robustness.

---

##  Components & Justifications

| Component | Function | Why Chosen |
|----------|----------|------------|
| **ESP32-WROOM-32UE** | Central MCU | Wi-Fi capability, 3.3V logic, rich GPIOs, external antenna for better signal in lab/field environments |
| **LiPo Battery (4S–6S)** | Power source | Industry standard for RC & propulsion testing |
| **Voltage Divider (20kΩ : 1kΩ)** | Scales 60V to ~3V | Safe for ESP32 ADC input (60V / 21 ≈ 2.86V) |
| **ACS758-100U** | High-side current sensing (±100A) | Hall-effect sensor with analog output, supports high current, electrically isolated, precise |
| **HX711 + 4-wire Load Cell** | Measures thrust up to 5kg | Digital output, high-resolution 24-bit ADC, stable under varying conditions |
| **MAX6675 + K-Type Thermocouple** | Measures temperature (up to 150°C and more) | SPI interface, robust against noise, works with industrial thermocouples |
| **RPM sensor** | Measures RPM up to 100,000 | Custom high-speed solution for detecting high rotational speeds; clean digital pulse output for interrupt-based counting |
| **ESC (SkyWalker 60A UBEC)** | Motor controller | Compatible with brushless motors, supports PWM input from ESP32, regulated 5V output for logic supply |

---

##  System Architecture

The system architecture includes:

- **Power Input** from LiPo battery to power motor + electronics
- **ESC** takes PWM signals from ESP32 to control motor speed
- **Sensors** feed data into ESP32 over GPIO, ADC, and SPI
- **Wi-Fi Module (built-in)** sends real-time data logs
- **No Display**: Data is monitored wirelessly, reducing complexity and power draw

**[ Block Diagram and Schematic provided in project folder]**

---

##  Data Logging Strategy

- ESP32 uses onboard Wi-Fi to:
  - Connect to local network or act as an access point
  - Transmit sensor data to a remote dashboard or terminal
- Logging Format: JSON or CSV (configurable)
- Real-time feedback loop allows:
  - Live visualization
  - Time-series logging for analysis

---

##  Pin Connections Summary

| ESP32 Pin | Connected Device | Notes |
|-----------|------------------|-------|
| GPIO04    | IR Sensor OUT     | For RPM interrupt counting |
| GPIO36 (VP) | Voltage Divider OUT | ADC input |
| GPIO39 (VN) | ACS758 OUT        | ADC input |
| GPIO32     | HX711 DT         | Digital input |
| GPIO33     | HX711 SCK        | Digital output |
| GPIO15     | MAX6675 SCK      | SPI Clock |
| GPIO14     | MAX6675 CS       | SPI Chip Select |
| GPIO12     | MAX6675 SO       | SPI MISO |
| GPIO25     | PWM to ESC       | PWM motor control |
| GND / 3.3V / 5V | Power rails  | Shared across sensors |

---

##  Future Improvements

- Add SD card logging as backup
- Integrate OLED display for field debugging
- Use INA226 for high-precision power monitoring
- Replace manual IR setup with optical encoder for even higher RPM accuracy

---

##  Conclusion

This project showcases how to design a **high-speed, high-current thrust stand electronics system** using **modern embedded technology**. Every sensor and design choice was made based on **reliability, safety, and real-world applicability** — especially in environments where wireless communication and high-speed data collection are critical.

By combining:
- **Accurate analog measurements**
- **Digital signal processing**
- **Wireless logging**, and
- **Motor control**

...this thrust stand becomes an **essential tool for aerospace, drone testing, or propulsion R&D** projects.

---
