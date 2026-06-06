# OpenSailingRC Autonomous GPS Buoy

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Platform: ESP32](https://img.shields.io/badge/Platform-ESP32-green.svg)](https://www.espressif.com/en/products/socs/esp32)
[![Hardware: M5Stack Core2](https://img.shields.io/badge/Hardware-M5Stack%20Core2-orange.svg)](https://shop.m5stack.com/products/m5stack-core2-esp32-iot-development-kit)
[![Version](https://img.shields.io/badge/Version-1.0.9-brightgreen.svg)](README.md)

🌐 [English version](README.md)

Firmware de bouee GPS autonome base sur **M5Stack Core2**.
Le systeme gere la navigation autonome, la supervision des capteurs et la communication bidirectionnelle avec une station sol via **ESP-NOW**, **LoRa** ou **LTE** (selon la configuration).

Au demarrage, l'appareil affiche un ecran splash avec la version firmware **v1.0.9**.

Ce projet fait partie de l'ecosysteme **OpenSailingRC** :

| Projet | Role |
|--------|------|
| OpenSailingRC-BuoyJoystick | Controleur de commandes des bouees |
| OpenSailingRC-Display | Affichage multi-appareils |
| **Autonomous-GPS-Buoy** | Ce projet - firmware de bouee autonome |

---

## Materiel requis

| Composant | Reference | Description |
|-----------|-----------|-------------|
| Controleur principal | M5Stack Core2 | ESP32 double coeur avec ecran tactile |
| Module GNSS | M5 Module GNSS | Position, vitesse, cap |
| Module IMU | M5 Unit IMU Pro Mini | Orientation et estimation du taux de lacet |
| Module LoRa (optionnel) | M5 LoRa E220 unit | Mode de liaison longue portee |

---

## Prerequis logiciels

- [PlatformIO](https://platformio.org/) (CLI ou extension VS Code)
- Environnement configure dans `platformio.ini` : `m5stack-core2`
- Les dependances sont installees automatiquement par PlatformIO

---

## Configuration locale (Secrets)

### ⚠️  Important : Protégez vos secrets

Ce projet utilise un **fichier de secrets** (`src/Secrets.h`) pour stocker les configurations sensibles :
- Adresse du broker MQTT et identifiants
- SSID et mot de passe WiFi
- APN et identifiants cellulaires/LTE

**Le fichier `src/Secrets.h` est ignoré par Git** et ne sera jamais commité dans le répo public.

### Instructions de configuration

1. **Copier le fichier template pour créer votre fichier de secrets :**
   ```bash
   cp src/Secrets.h.example src/Secrets.h
   ```

2. **Éditer `src/Secrets.h` avec vos vraies valeurs :**
   ```cpp
   // Configuration MQTT
   #define MQTT_BROKER_SECRET "votre-broker-mqtt.com"
   #define MQTT_USERNAME_SECRET "votre_username"
   #define MQTT_PASSWORD_SECRET "votre_password"
   
   // Configuration WiFi
   #define WIFI_SSID_SECRET "Votre_SSID_WiFi"
   #define WIFI_PASS_SECRET "Votre_Mot_de_passe_WiFi"
   
   // Configuration LTE/Cellulaire
   #define MODEM_APN_SECRET "votre_apn"
   #define MODEM_USER_SECRET "user_modem"
   #define MODEM_PASS_SECRET "pass_modem"
   ```

3. **Compiler et flasher normalement** - le firmware utilisera les valeurs de `src/Secrets.h`

### Pour les contributeurs

Lors du clonage du répo :

```bash
git clone <url-du-repository>
cd Autonomous-GPS-Buoy
cp src/Secrets.h.example src/Secrets.h
# Éditer src/Secrets.h avec votre configuration
```

Ne commitez jamais `src/Secrets.h` - il est protégé par `.gitignore` ✅

---

## Demarrage rapide

### 1. Compiler

```bash
cd Autonomous-GPS-Buoy
platformio run --environment m5stack-core2
```

### 2. Flasher

```bash
platformio run --environment m5stack-core2 --target upload
```

### 3. Moniteur serie

```bash
platformio device monitor --baud 115200
```

---

## Configuration principale

La strategie de liaison de donnees se choisit dans `src/Autonomous GPS Buoy.cpp` :

```cpp
extern const tDatalinkMode DATALINK_MODE = DATALINK_LORA;
```

Modes supportes :
- `DATALINK_LORA` : ESP-NOW + LoRa
- `DATALINK_LTE` : ESP-NOW + LTE
- `DATALINK_NON` : ESP-NOW seul

---

## Ecran splash au demarrage

La version firmware est centralisee dans :

```cpp
constexpr const char* BUOY_FIRMWARE_VERSION = "1.0.9";
```

Au boot, l'ecran affiche :
- OpenSailingRC
- GPS Buoy
- V1.0.9

Ensuite l'affichage standard et les logs demarrent.

---

## Licence

Ce projet est distribue sous **GNU General Public License v3.0**.
Voir [LICENSE](LICENSE) pour le texte complet.
