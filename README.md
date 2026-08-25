# DHRUV - Last-Mile Vaccine Cold Chain Monitoring System (MYOSA 6.0)

[![MYOSA 6.0](https://img.shields.io/badge/MYOSA-6.0--Shortlisted-0284c7.svg)](https://blog.myosa-sensors.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Platform: ESP32](https://img.shields.io/badge/Platform-ESP32-red.svg)](https://www.espressif.com/)

**Team:** Arin T S & Andrewjos Sebastian  
**Institution:** Saintgits College of Engineering (Autonomous), Kottayam, Kerala  
**Mentor:** Dr. Abraham George  

> **DHRUV** is a sensor-fusion IoT monitoring system engineered specifically for ASHA (Accredited Social Health Activist) workers delivering vaccines to remote rural India. Powered strictly by the **MYOSA 6.0 hardware kit**, DHRUV monitors thermal stability, tamper events, physical handling shocks, and seal degradation—calculating a dynamic **Chain Integrity Score (0–100%)** to ensure no spoiled vaccine reaches a child.

---

## 🏆 Why DHRUV is Built to Win MYOSA 6.0

1. **Direct High-Impact Real World Problem:** Solves last-mile vaccine cold chain failures (responsible for up to 50% global vaccine wastage per WHO).
2. **100% Kit Hardware Compliance:** Employs strictly the kit sensors (ESP32, BMP180, MPU6050/6500, APDS9960, and SSD1306 OLED) without requiring extra external modules.
3. **Novel Sensor Fusion Matrix:** 
   - **BMP180:** Temperature ($2^\circ\text{C}-8^\circ\text{C}$ safe range) & Altitude/Barometric route tracking.
   - **MPU6050/6500:** Accelerometer shock & drop impact magnitude ($>2.5\text{G}$).
   - **APDS9960:** Dual ambient light lux spike (box opening tamper) + Proximity seal gap + **Touchless gesture screen navigation** (SWIPE hands-free while wearing gloves!).
4. **Dynamic Chain Integrity Score Algorithm:** Converts complex raw multi-sensor streams into an instant, actionable 0–100% score for health supervisors.
5. **Hybrid Offline-Online Architecture:** Operates 100% offline in non-connected rural terrain with NVS flash memory logging, and auto-syncs to the PHC Base Station Web Dashboard upon return.

---

## 📌 Hardware Pinout Table

All sensors connect over the shared **I2C Bus (SDA = GPIO 21, SCL = GPIO 22)**:

| Module / Sensor | I2C Address | ESP32 Pin | Function |
| :--- | :--- | :--- | :--- |
| **SSD1306 OLED (0.96")** | `0x3C` | SDA: 21, SCL: 22 | Live Telemetry & Status Display |
| **BMP180 Pressure** | `0x77` | SDA: 21, SCL: 22 | Precision Temperature & Altitude |
| **MPU6050 / MPU6500** | `0x68` / `0x69` | SDA: 21, SCL: 22 | 6-Axis Acceleration & Drop Shock |
| **APDS9960 Sensor** | `0x39` | SDA: 21, SCL: 22 | Light Lux (Tamper), Proximity & Gestures |
| **Built-in / Alert LED** | Direct GPIO | GPIO 2 | Red Alert Indicator (Flashes on Breach) |

---

## 🚀 Quick Start & Installation

### 1. ESP32 Firmware Upload
1. Open `DHRUV_Firmware/DHRUV_Firmware.ino` in Arduino IDE.
2. Select Board: **ESP32 Dev Module**.
3. Install standard libraries: `Adafruit_SSD1306`, `Adafruit_GFX`, `Adafruit_BMP085`, `Wire`, `WiFi`, `WebServer`, `Preferences`.
4. Click **Upload** to flash the ESP32.

### 2. PHC Base Station Dashboard Setup
1. Power up the ESP32 node.
2. Connect your laptop/phone to WiFi Access Point: `DHRUV_Vaccine_Node` (Password: `myosa2026`).
3. Open browser and go to `http://192.168.4.1/` (or open `web_dashboard/index.html` locally).

---

## 🎬 3-Minute Interactive Judge Demonstration Guide

When presenting DHRUV to competition judges, follow this step-by-step interactive demonstration script:

```
Step 1: Normal State Overview (0:00 - 0:45)
- Show DHRUV node inside the vaccine carrier box.
- Show OLED display reading 100% Chain Integrity Score and status [EXCELLENT].
- Show Base Station Web Dashboard connected live with green status pill.

Step 2: Thermal Excursions Demo (0:45 - 1:30)
- Place warm hand over node (or click "Heat Breach (+12.5°C)" on Dashboard).
- BMP180 detects temperature rising above 8°C.
- Alert LED flashes red instantly, OLED updates to "BREACH: TEMP_EXCURSION".
- Chain Integrity Score drops dynamically on OLED and Dashboard.

Step 3: Box Opening & Tamper Demo (1:30 - 2:00)
- Open carrier box lid (or click "Lid Open (180 Lux)" on Dashboard).
- APDS9960 detects light lux spike (>60 Lux).
- Instant "BOX_OPEN_TAMPER" event is logged with timestamp into Flash memory.

Step 4: Drop Impact Shock Demo (2:00 - 2:30)
- Gently tap/drop carrier box (or click "Drop Shock (4.2G)" on Dashboard).
- MPU6050 registers G-force impact > 2.5G.
- Severity reading and shock drop penalty are logged.

Step 5: Touchless Gesture Navigation & Auto-Sync (2:30 - 3:00)
- Swipe hand over APDS9960 sensor to switch OLED display pages hands-free.
- Click "Export Audit CSV" on the Web Dashboard to demonstrate generating the final official cold chain delivery report for PHC supervisors!
```

---

## 📑 File Structure

```
Myosa/
├── DHRUV_Firmware/
│   └── DHRUV_Firmware.ino         # Production ESP32 Arduino Firmware
├── web_dashboard/
│   ├── index.html                 # Base Station Dashboard HTML
│   ├── style.css                  # Glassmorphism Styling
│   └── app.js                     # Live Telemetry & Simulation Controller
├── dhruv-myosa-coldchain.md       # Official MYOSA Blog Submission Markdown
├── README.md                      # Complete Project & Hardware Guide
└── test_code.ino                  # Original test sketch
```

---

## 📜 Submission Checklist Verification

- [x] Blog submission post formatted as `.md` (`dhruv-myosa-coldchain.md`).
- [x] Front matter YAML metadata included (`publishDate`, `title`, `excerpt`, `image`, `tags`).
- [x] One-line tagline with `>` included.
- [x] Mandatory headers (`Acknowledgements`, `Overview`, `Demo / Examples`, `Features`, `Usage Instructions`, `Tech Stack`, `Requirements`, `File Structure`, `License`).
- [x] Image tags formatted as `<p align="center"><img src="..." width="800"><br/><i>Caption</i></p>`.
- [x] Video tags formatted as `<video controls width="100%"><source src="..." type="video/mp4"></video>`. No YouTube links!
- [x] Lowercase, no-space file naming (`dhruv-myosa-coldchain.md`).

---
*Built for MYOSA 6.0 | Saintgits College of Engineering*
