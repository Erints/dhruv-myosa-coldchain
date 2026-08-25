<div align="center">

# 🩺 DHRUV
### Dynamic Hybrid Real-time Uninterrupted Vaccine-chain Monitor

**A sensor-fusion IoT node that protects vaccines during the last mile of India's cold chain**

[![MYOSA 6.0](https://img.shields.io/badge/MYOSA-6.0-0284c7.svg)](https://blog.myosa-sensors.org/)
[![Platform: ESP32](https://img.shields.io/badge/Platform-ESP32-red.svg)](https://www.espressif.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

[Overview](#-overview) • [Problem](#-the-problem) • [Solution](#-the-solution) • [How It Works](#-how-it-works) • [Tech Stack](#-tech-stack) • [Setup](#-setup--installation) • [Demo](#-live-demo) • [Team](#-team)

</div>

---

## 📖 Overview

<!-- EDIT: 2-3 sentence pitch judges will read first -->
**DHRUV** is a compact, sensor-fusion monitoring node that sits inside a vaccine carrier bag and tracks the conditions vaccines experience on their final journey — from a Primary Health Centre (PHC) to a child's doorstep. It detects temperature and humidity breaches, tampering, rough handling, and seal failures in real time, works fully offline in the field, and automatically syncs a complete delivery report to a supervisor dashboard the moment connectivity returns.

Built on the **MYOSA sensor platform**, DHRUV converts raw multi-sensor data into a single actionable number — the **Chain Integrity Score (0–100%)** — so health workers and supervisors instantly know whether a vaccine is still safe to administer.

---

## 🚨 The Problem

<!-- EDIT: keep or replace with your own framing/stats -->
India's immunization programme depends on vaccines staying between **2°C and 8°C** from factory to child. District-level cold storage is well monitored — but the **last mile**, where an ASHA (Accredited Social Health Activist) worker carries vaccines in an insulated bag to remote villages, has **zero sensor coverage**.

During this journey:
- 🌡️ **Thermal excursions** — heat exposure pushes internal temperature outside the safe zone
- 🔓 **Unrecorded tampering** — the bag is opened repeatedly, letting warm air in
- 📉 **Rough handling** — drops and vibrations on rural terrain damage vaccine vials
- 💧 **Seal degradation** — worn seals let in humidity over time

None of this is currently detected. According to the WHO, up to **50% of vaccines are wasted globally each year**, with cold chain failure a leading cause — and the child often still receives the (now ineffective) dose, believing they're protected.

---

## 💡 The Solution

DHRUV is not trying to replace industrial cold chain systems — it targets the **one link with zero sensor-based coverage today**: the worker's bag itself.

- 📡 Monitors temperature, humidity, pressure, tampering, and shock **continuously**, with no connectivity required
- 🚨 Alerts the worker instantly via OLED display and LED indicator on any breach
- 💾 Logs every event with a timestamp to local flash memory — fully functional offline
- ☁️ Auto-syncs to a base station dashboard over WiFi the moment the worker returns to the PHC
- 📊 Gives supervisors a real-time **Chain Integrity Score** and full event timeline per delivery

---

## ⚙️ How It Works

### Sensor Fusion Matrix

| Sensor | Measures | Breach Condition | Response |
|---|---|---|---|
| **DHT22** | Temperature (°C) & Humidity (%RH) | Temp outside 2–8°C, or RH > 85% | `ALERT: Thermal Excursion` |
| **APDS9960** (light) | Ambient light (lux) | Lux spike inside bag | `ALERT: Tamper Detected` |
| **APDS9960** (proximity) | Distance to inner tray/lid | Gap or displacement | `ALERT: Seal Breach` |
| **MPU6050** | 3-axis acceleration (G) | Impact above threshold | `ALERT: Mishandling` |
| **BMP180** | Barometric pressure/altitude | Abnormal altitude shift | `LOG: Route Deviation` |

<!-- EDIT: replace with your final threshold values once field-tested -->

### Chain Integrity Score

Starting at 100%, the score deducts for each breach type based on severity and duration, giving a single number that reflects whether the vaccine is still viable:

```
Score = 100 − (Thermal Penalty + Tamper Penalty + Shock Penalty + Seal Penalty)
```

### Architecture

```
 ┌─────────────────────┐        offline         ┌──────────────────────┐
 │   DHRUV Node         │  ── flash logging ──▶  │   ASHA Worker's Bag   │
 │  (ESP32 + sensors)   │                        │   (field deployment)  │
 └──────────┬───────────┘                        └──────────────────────┘
            │ WiFi sync on PHC return
            ▼
 ┌─────────────────────┐
 │  Base Station        │
 │  Web Dashboard        │
 │  (Chain Integrity,    │
 │   event log, alerts)  │
 └─────────────────────┘
```

<!-- EDIT: swap this ASCII diagram for a real image once you have one — 
<p align="center"><img src="path/to/architecture-diagram.png" width="700"></p> -->

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Microcontroller** | ESP32 Dev Module |
| **Sensors** | BMP180, MPU6050, APDS9960, DHT22, SSD1306 OLED |
| **Firmware** | Embedded C++ / Arduino Framework |
| **Connectivity** | WiFi (HTTP), Firebase Realtime Database |
| **Dashboard** | HTML5, CSS3, Vanilla JavaScript |
| **Storage** | ESP32 NVS Flash (offline logging) |

---

## 📌 Hardware Pinout

All I2C sensors share the bus (**SDA = GPIO 21, SCL = GPIO 22**):

| Module | I2C Address | Function |
|---|---|---|
| SSD1306 OLED | `0x3C` | Live status display |
| BMP180 | `0x77` | Pressure & altitude |
| MPU6050 | `0x68` | Shock/drop detection |
| APDS9960 | `0x39` | Light (tamper) + proximity (seal) |
| DHT22 | GPIO 4 (digital) | Temperature & humidity |
| Alert LED | GPIO 2 | Visual breach indicator |

---

## 🚀 Setup & Installation

### 1. Flash the Firmware
```bash
# Open in Arduino IDE
DHRUV_Firmware/DHRUV_Firmware.ino

# Install libraries:
- Adafruit SSD1306 / Adafruit GFX
- Adafruit BMP085
- MPU6050 (Electronic Cats or i2cdevlib)
- Adafruit APDS9960
- DHT sensor library
- Firebase ESP Client / HTTPClient (built-in)

# Board: ESP32 Dev Module → Upload
```

### 2. Configure WiFi & Firebase
Edit these lines in the firmware before flashing:
```cpp
#define WIFI_SSID     "YOUR_WIFI_SSID"
#define WIFI_PASSWORD "YOUR_WIFI_PASSWORD"
#define FIREBASE_HOST "https://your-project.firebaseio.com"
```

### 3. Run the Dashboard
Open `web_dashboard/index.html` in any browser — it connects live to Firebase.

---

## 🎬 Live Demo

<!-- EDIT: replace with your real image/video paths once captured -->
<p align="center">
  <img src="assets/images/hardware-setup.jpg" width="700"><br/>
  <i>DHRUV node integrated inside a vaccine carrier bag</i>
</p>

<video controls width="100%">
  <source src="assets/videos/demo.mp4" type="video/mp4">
</video>

### 3-Minute Demonstration Script
| Time | Step | What Happens |
|---|---|---|
| 0:00–0:45 | Normal state | Score at 100%, dashboard green |
| 0:45–1:30 | Thermal breach | Warm object near node → alert fires, score drops |
| 1:30–2:00 | Tamper | Lid opened → light spike detected, logged |
| 2:00–2:30 | Shock | Bag gently dropped → G-force spike logged |
| 2:30–3:00 | Sync | WiFi reconnects → all events push to dashboard |

---

## 🌍 Impact

<!-- EDIT: this is your "why it matters" pitch for judges -->
Over one million ASHA workers in India carry vaccines to remote villages daily with no monitoring. DHRUV directly targets this last-mile blind spot, contributing toward **UN SDG 3: Good Health and Well-being**. A successful deployment could help prevent thousands of ineffective vaccinations annually by giving supervisors visibility they've never had before.

---

## 👥 Team

| Name | Role | Institution |
|---|---|---|
| **Arin T S** | Team Lead | Saintgits College of Engineering (Autonomous), Kottayam, Kerala |
| **Andrewjos Sebastian** | Co-Investigator | Saintgits College of Engineering (Autonomous), Kottayam, Kerala |
| **Dr. Abraham George** | Faculty Mentor | Saintgits College of Engineering (Autonomous), Kottayam, Kerala |

---

## 📄 License

This project is open-source under the [MIT License](LICENSE).

---

<div align="center">

**Built for MYOSA 6.0** | Saintgits College of Engineering (Autonomous)

</div>
