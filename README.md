*This project is incomplete!*
# 💊 Smart Pill Reminder System for Alzheimer's Patients

![Status](https://img.shields.io/badge/Status-Active-success)
![Tools](https://img.shields.io/badge/Tools-Arduino%20IDE%20|%20ESP32-blue)
![Tech](https://img.shields.io/badge/Technology-IoT%20|%20Embedded%20Systems-orange)

Designing, testing, and implementing an intelligent pill organizer to assist patients with medication adherence.

## 📌 Project Overview

[cite_start]This repository documents the design and implementation of a **smart pill reminder system** tailored for Alzheimer's patients[cite: 1]. [cite_start]The system integrates a 7-compartment pill organizer with an **ESP32 microcontroller** to monitor medication intake in real-time[cite: 2]. [cite_start]It utilizes **IR sensors** to detect the presence of pills [cite: 4] [cite_start]and provides immediate feedback via an **LCD display** and a **Buzzer**[cite: 5, 6]. [cite_start]Additionally, the system hosts a local **Web Server** to allow caregivers to check the patient's status remotely via a WiFi hotspot[cite: 2, 7].

---
## ⚡ Objectives
- [cite_start]To automate daily medication reminders using auditory (Buzzer) and visual (LCD) cues[cite: 6, 7].
- [cite_start]To detect the physical presence of pills in a 7-day organizer using TCRT5000 IR sensors[cite: 4].
- [cite_start]To host a local web interface for remote status monitoring using the ESP32[cite: 2].
- [cite_start]To simulate a weekly cycle for testing purposes (rapid day transition)[cite: 14].

---

## 📐 Circuit Theory & Logic

The system operates by cycling through the days of the week. For the active day, it continuously checks the specific compartment for a pill.

### Logic Table

| Sensor Input (IR) | Pill Status | Buzzer State | LCD Message | Web Status |
|:-:|:-:|:---:|:---:|:----:|
| **LOW** (0V) | Present (Not Taken) | **HIGH** (ON) | "Take the pill!" | "Take the pill!" |
| **HIGH** (3.3V) | Absent (Taken) | **LOW** (OFF) | "You're fine!" | "You're fine!" |

### Functional Expressions
* [cite_start]**Pill Detected** = `digitalRead(DaySensorPin) == LOW` [cite: 25]
* [cite_start]**Alarm Trigger** = `if (Pill Detected) -> Buzzer HIGH` [cite: 27]
* [cite_start]**Day Cycle** = `millis()` timer loops every 5000ms (for testing) to simulate 24 hours[cite: 14].

### Circuit Schematic

*Note: The system utilizes 7 sensors connected to the ESP32 GPIO pins, alongside an LCD and Buzzer.*

<div align = "center">
<img width="600" alt="Circuit_Schematic" src="https://via.placeholder.com/600x300?text=Circuit+Schematic+(ESP32+%2B+Sensors+%2B+LCD)" />
</div>

---

## 🛠️ Implementation Details

### 1. Embedded Software (C++/Arduino)
The core logic is written in C++ using the Arduino framework.
* [cite_start]**WiFi Mode:** The ESP32 operates as an Access Point (AP) with the SSID `"PillReminder"` and password `"12345678"`[cite: 9].
* [cite_start]**Web Server:** A server runs on port 80 [cite: 15][cite_start], serving an HTML page that auto-refreshes every second to show the current day and status[cite: 31].
* [cite_start]**Timing:** Non-blocking `millis()` timers are used to manage day transitions without pausing sensor readings[cite: 22].

### 2. Hardware Integration
* [cite_start]**Microcontroller:** ESP32 (or compatible Arduino) handles I/O and WiFi[cite: 2, 3].
* [cite_start]**Sensors:** 7x TCRT5000 IR sensors are mapped to GPIO pins `[23, 22, 21, 19, 18, 5, 4]` corresponding to Sunday through Saturday[cite: 10].
* **Output:**
    * [cite_start]**16x2 LCD:** Connected via pins `[13, 12, 14, 27, 26, 25]` (RS, EN, D4, D5, D6, D7)[cite: 8].
    * [cite_start]**Buzzer:** Connected to GPIO 15, active High[cite: 12].

### 3. Web Interface
[cite_start]The ESP32 generates a simple HTML dashboard accessible by connecting to the "PillReminder" network[cite: 9].

* [cite_start]**Structure:** Displays the current Day and Status ("Take the pill!" or "You're fine!")[cite: 37, 38].
* [cite_start]**Updates:** The page includes JavaScript to reload automatically, providing near real-time updates for the caregiver[cite: 31].

<div align = "center">
<img width="300" alt="Web_Interface" src="https://via.placeholder.com/300x500?text=Web+Interface+Screenshot" />
</div>
