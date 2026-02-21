![Platform](https://img.shields.io/badge/platform-Raspberry%20Pi%20Pico-blue?logo=raspberry-pi)
![License](https://img.shields.io/badge/license-MIT-green)

# 🛰️ eHack Portable Module

This is the repository for the **eHack Portable Module**, a wireless companion designed to extend the capabilities of the main [eHack Project](https://github.com/kelv1n9/eHack).

This module acts as a remote-controlled agent, receiving commands from the main eHack device to perform a wide range of radio frequency tasks. Its portable, battery-powered design makes it perfect for flexible and discreet security research operations.

---

## ✨ Key Features

The module is packed with features, all controlled wirelessly from the main unit's interface.

### Sub-GHz Operations (CC1101)
* **Live Spectrum Analysis:** Scans the Sub-GHz spectrum to identify active frequencies and sends the RSSI data back to the main unit for visualization.
* **Signal Capture & Replay:** Captures raw OOK/ASK signals from remotes and replays them.
* **Gate & Barrier Toolkit:** Includes specialized functions for popular access control systems:
    * Capture and replay codes.
    * Brute-force attacks for **CAME** and **NICE** protocols.
* **Store-and-Forward HF Scan:** In standalone `HF_SCAN`, the module can cache up to 20 captured RF signals in EEPROM and transmit them later when the master requests the scan mode.
* **Tesla Charge-Port Opener:** Transmits signals known to open Tesla charge ports.
* **Wide-Band Noise Jammer:** Generates RF noise to jam communications on a specific frequency.

### 2.4 GHz Operations (NRF24L01+)
* **Multi-Mode Jamming:** Capable of targeted jamming across various 2.4 GHz protocols by rapidly hopping between channels. Supported modes include:
    * All Channels
    * Wi-Fi
    * Bluetooth
    * Bluetooth Low Energy (BLE)
    * Wireless USB Dongles
    * Wireless Video
    * RC (Remote Controls)

### FM Radio Operations (Si4713)
* **FM Transmission with RDS:** Configures FM transmit frequency and broadcasts RDS station/buffer text.
* **Remote FM Control:** Receives FM frequency commands from the main unit over NRF24L01.
* **Input Level Telemetry:** Sends current FM input level back to the main unit while in FM mode.

### Core System
* **Wireless C&C:** Uses a robust communication protocol to receive commands and send data back to the main eHack device.
* **Connection Handshake & Keepalive:** Uses PING/PONG exchange and timeout checks to detect link loss.
* **Serial Console Control:** Supports `MODE` and `REPLAY` commands over USB Serial for direct bench testing and debugging.
* **Visual Status Feedback:** Built-in LED patterns and a 128x32 OLED status screen show mode, link status, battery, and RX activity.
* **Autonomous Power:** Powered by a Li-Ion battery with voltage monitoring that is reported back to the main unit.
* **Power Saving:** Includes a deep-sleep feature that automatically powers down the module if a connection with the master device is lost for an extended period.

---

## 🛠️ Hardware Components

The device is built around a few key components:
* **Microcontroller:** A Raspberry Pi Pico to handle logic and communication.
* **Sub-GHz Transceiver:** A **CC1101** module for operations in the 315/433/868/915 MHz bands.
* **2.4 GHz Transceiver:** An **NRF24L01+** module for all 2.4 GHz jamming functions and communication.
* **FM Transmitter:** A **Si4713** module for FM transmission with RDS.
* **Display:** A **128x32 OLED** for local status and diagnostics.

---

## 🔌 Pinout (Raspberry Pi Pico)

| Function | Pins |
|---|---|
| CC1101 (SPI) | `SCK=GP2`, `MOSI=GP3`, `MISO=GP4`, `CSN=GP5`, `GDO0=GP6` |
| NRF24L01+ (SPI1) | `CE=GP7`, `SCK=GP10`, `MOSI=GP11`, `MISO=GP12`, `CSN=GP13` |
| Si4713 Control | `RESET=GP8`, `ENABLE=GP9` |
| Si4713 I2C (`Wire`) | `SDA=GP0`, `SCL=GP1` |
| OLED I2C (`Wire1`) | `SDA=GP18`, `SCL=GP19` |
| Battery ADC | `A3` |
| Auto-shutdown control | `GP22` |

---

## ⚙️ How It Works

1.  **Connection:** On power-up, the Portable Module waits for a handshake signal from the main eHack device.
2.  **Command:** Once connected, the user selects an action (e.g., "Brute-Force CAME") on the main eHack's UI. The main unit sends a corresponding mode command to the module.
3.  **Execution:** The module receives the command, enters the appropriate function (e.g., `HF_BARRIER_BRUTE_CAME`), and begins executing the task using its onboard radio transceivers.
4.  **Feedback:** During certain operations (like spectrum analysis), the module sends data back to the main unit. It continuously listens for new commands, allowing the user to switch tasks on the fly.
5.  **Standalone Buffering:** If running disconnected in `HF_SCAN`, received RF signals are stored in EEPROM and can later be flushed back through the normal master-driven workflow.

---

## 🚨 Disclaimer

> **For Educational & Research Use Only**
> Use this hardware and software responsibly and **only on frequencies and systems you are legally allowed to transmit on**.
> The author (**Elvin Gadirov**) accepts **no liability** for any damage, data loss, or legal consequences that may result from its use.
> **All actions are at your own risk.** Always comply with local laws and regulations.
