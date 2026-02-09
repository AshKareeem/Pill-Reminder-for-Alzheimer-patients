*This project is incomplete!*
# 💊 Smart Pill Reminder System for Alzheimer's Patients

![Status](https://img.shields.io/badge/Status-Active-success)
![Tools](https://img.shields.io/badge/Tools-Arduino%20IDE%20|%20ESP32-blue)
![Tech](https://img.shields.io/badge/Technology-IoT%20|%20Embedded%20Systems-orange)

Designing, testing, and implementing an intelligent pill organizer to assist patients with medication adherence.

## 📌 Project Overview

This repository documents the design and implementation of a **smart pill reminder system** tailored for Alzheimer's patients. The system integrates a 7-compartment pill organizer with an **ESP32 microcontroller** to monitor medication intake in real-time. It utilizes **IR sensors** to detect the presence of pills  and provides immediate feedback via an **LCD display** and a **Buzzer**. Additionally, the system hosts a local **Web Server** to allow caregivers to check the patient's status remotely via a WiFi hotspot.

---
## ⚡ Objectives
- To automate daily medication reminders using auditory (Buzzer) and visual (LCD) cues.
- To detect the physical presence of pills in a 7-day organizer using TCRT5000 IR sensors.
- To host a local web interface for remote status monitoring using the ESP32.
- To simulate a weekly cycle for testing purposes (rapid day transition).

---

## 📐 Circuit Theory & Logic

The system operates by cycling through the days of the week. For the active day, it continuously checks the specific compartment for a pill.

### Logic Table

| Sensor Input (IR) | Pill Status | Buzzer State | LCD Message | Web Status |
|:-:|:-:|:---:|:---:|:----:|
| **LOW** (0V) | Present (Not Taken) | **HIGH** (ON) | "Take the pill!" | "Take the pill!" |
| **HIGH** (3.3V) | Absent (Taken) | **LOW** (OFF) | "You're fine!" | "You're fine!" |

### Functional Expressions
* **Pill Detected** = `digitalRead(DaySensorPin) == LOW` 
* **Alarm Trigger** = `if (Pill Detected) -> Buzzer HIGH` 
* **Day Cycle** = `millis()` timer loops every 5000ms (for testing) to simulate 24 hours.

### Circuit Schematic

*Note: The system utilizes 7 sensors connected to the ESP32 GPIO pins, alongside an LCD and Buzzer.*

<div align = "center">
<img width="600" alt="Circuit_Schematic" src="https://github.com/AshKareeem/Pill-Reminder-for-Alzheimer-patients/blob/3423079e2657d214e663397688dcdd1c6212f798/image.png" />
</div>

---

## 🛠️ Implementation Details

### 1. Embedded Software (C++/Arduino)
The core logic is written in C++ using the Arduino framework.
* **WiFi Mode:** The ESP32 operates as an Access Point (AP) with the SSID `"PillReminder"` and password `"12345678"`.
* **Web Server:** A server runs on port 80 , serving an HTML page that auto-refreshes every second to show the current day and status.
* **Timing:** Non-blocking `millis()` timers are used to manage day transitions without pausing sensor readings.

### 2. Hardware Integration
* **Microcontroller:** ESP32 (or compatible Arduino) handles I/O and WiFi.
* **Sensors:** 7x TCRT5000 IR sensors are mapped to GPIO pins `[23, 22, 21, 19, 18, 5, 4]` corresponding to Sunday through Saturday.
* **Output:**
    * **16x2 LCD:** Connected via pins `[13, 12, 14, 27, 26, 25]` (RS, EN, D4, D5, D6, D7).
    * **Buzzer:** Connected to GPIO 15, active High.

### 3. Web Interface
The ESP32 generates a simple HTML dashboard accessible by connecting to the "PillReminder" network.

* **Structure:** Displays the current Day and Status ("Take the pill!" or "You're fine!").
* **Updates:** The page includes JavaScript to reload automatically, providing near real-time updates for the caregiver.

<div align = "center">
The Web Interface is connected to the local IP-Address of the ESP-Hotsopt 
</div>
