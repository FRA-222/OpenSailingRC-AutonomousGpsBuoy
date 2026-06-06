# OpenSailingRC Autonomous GPS Buoy

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Platform: ESP32](https://img.shields.io/badge/Platform-ESP32-green.svg)](https://www.espressif.com/en/products/socs/esp32)
[![Hardware: M5Stack Core2](https://img.shields.io/badge/Hardware-M5Stack%20Core2-orange.svg)](https://shop.m5stack.com/products/m5stack-core2-esp32-iot-development-kit)
[![Version](https://img.shields.io/badge/Version-1.0.9-brightgreen.svg)](README.md)

🌐 [Version française](README.fr.md)

Autonomous GPS buoy firmware based on **M5Stack Core2**.
The system handles autonomous navigation, sensor supervision, and bidirectional communication with a ground station via **ESP-NOW**, **LoRa**, or **LTE** (depending on configuration).

At startup, the device displays a splash screen with firmware version **v1.0.9**.

This project is part of the **OpenSailingRC** ecosystem:

| Project | Role |
|---------|------|
| OpenSailingRC-BuoyJoystick | Buoy command controller |
| OpenSailingRC-Display | Multi-device display |
| **Autonomous-GPS-Buoy** | This project - autonomous buoy firmware |

---

## Required Hardware

| Component | Reference | Description |
|-----------|-----------|-------------|
| Main controller | M5Stack Core2 | ESP32 dual-core with touchscreen |
| GNSS module | M5 Module GNSS | Position, speed, heading inputs |
| IMU module | M5 Unit IMU Pro Mini | Orientation and yaw-rate estimation |
| LoRa module (optional) | M5 LoRa E220 unit | Long-range datalink mode |

---

## Software Prerequisites

- [PlatformIO](https://platformio.org/) (CLI or VS Code extension)
- Environment configured in `platformio.ini`: `m5stack-core2`
- Dependencies are installed automatically by PlatformIO

---

## Local Configuration (Secrets)

### ⚠️  Important: Protect Your Secrets

This project uses a **secrets file** (`src/Secrets.h`) to store sensitive configuration:
- MQTT broker address & credentials
- WiFi SSID & password
- Cellular/LTE APN & credentials

**The `src/Secrets.h` file is ignored by Git** and will never be committed to the public repository.

### Setup Instructions

1. **Copy the template file to create your secrets file:**
   ```bash
   cp src/Secrets.h.example src/Secrets.h
   ```

2. **Edit `src/Secrets.h` with your actual values:**
   ```cpp
   // MQTT Configuration
   #define MQTT_BROKER_SECRET "your-mqtt-broker.com"
   #define MQTT_USERNAME_SECRET "your_username"
   #define MQTT_PASSWORD_SECRET "your_password"
   
   // WiFi Configuration
   #define WIFI_SSID_SECRET "Your_WiFi_SSID"
   #define WIFI_PASS_SECRET "Your_WiFi_Password"
   
   // LTE/Cellular Configuration
   #define MODEM_APN_SECRET "your_apn"
   #define MODEM_USER_SECRET "modem_user"
   #define MODEM_PASS_SECRET "modem_pass"
   ```

3. **Build and flash normally** - the firmware will use the values from `src/Secrets.h`

### For Contributors

When cloning this repository:

```bash
git clone <repository-url>
cd Autonomous-GPS-Buoy
cp src/Secrets.h.example src/Secrets.h
# Edit src/Secrets.h with your configuration
```

Never commit `src/Secrets.h` - it's protected by `.gitignore` ✅

---

## Quick Start

### 1. Build

```bash
cd Autonomous-GPS-Buoy
platformio run --environment m5stack-core2
```

### 2. Flash

```bash
platformio run --environment m5stack-core2 --target upload
```

### 3. Serial monitor

```bash
platformio device monitor --baud 115200
```

---

## Main Configuration

The datalink strategy is selected in `src/Autonomous GPS Buoy.cpp`:

```cpp
extern const tDatalinkMode DATALINK_MODE = DATALINK_LORA;
```

Supported modes:
- `DATALINK_LORA`: ESP-NOW + LoRa
- `DATALINK_LTE`: ESP-NOW + LTE
- `DATALINK_NON`: ESP-NOW only

---

## Startup Splash Screen

Firmware version is centralized in:

```cpp
constexpr const char* BUOY_FIRMWARE_VERSION = "1.0.9";
```

At boot, the screen displays:
- OpenSailingRC
- GPS Buoy
- V1.0.9

Then normal display/log rendering starts.

---

## License

This project is distributed under the **GNU General Public License v3.0**.
See [LICENSE](LICENSE) for the full text.
