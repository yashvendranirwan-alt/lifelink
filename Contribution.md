# Contributing to LifeLink 💉🔗

Thank you for your interest in contributing to **LifeLink** — an IoT + AI + Blockchain solution for vaccine cold chain monitoring. Whether you're fixing a bug, improving the AI decay model, or adding new hardware support, your contribution matters.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [How to Contribute](#how-to-contribute)
- [Development Guidelines](#development-guidelines)
- [Hardware Contributions](#hardware-contributions)
- [Submitting a Pull Request](#submitting-a-pull-request)
- [Reporting Issues](#reporting-issues)
- [Roadmap & Future Work](#roadmap--future-work)

---

## Project Overview

LifeLink monitors vaccine potency during transport using:

- **ESP32 + DHT22** — Real-time temperature & humidity sensing
- **Edge Alerts** — Immediate LED/buzzer response on threshold breach
- **Flask + AI Backend** — Thermal stress decay model for potency estimation
- **SHA-256 Blockchain Logging** — Tamper-proof chain of custody

**Live Demo:** [https://yashvendranirwan-alt.github.io/vaxsafesite/](https://yashvendranirwan-alt.github.io/vaxsafesite/)

---

## Code of Conduct

This project is maintained by a first-year B.Tech student. Please be respectful, constructive, and patient in all interactions. Harassment, trolling, or dismissive behaviour will not be tolerated.

---

## Getting Started

### Prerequisites

| Tool | Version | Purpose |
|------|---------|---------|
| Python | 3.8+ | Flask backend & AI logic |
| Arduino IDE | 2.x | ESP32 firmware |
| ESP32 Board Package | Latest | Arduino ESP32 support |
| pip | Latest | Python dependency management |

### Backend Setup

```bash
# 1. Clone the repository
git clone https://github.com/yashvendranirwan-alt/lifelink.git
cd lifelink

# 2. Install Python dependencies
pip install flask
# or
pip install -r requirements.txt

# 3. Note your machine's local IP address
#    Linux/Mac:  ifconfig
#    Windows:    ipconfig

# 4. Start the Flask server
python backend/server.py
```

### Firmware Setup

1. Open `firmware/vaxsafe_esp32.ino` in Arduino IDE.
2. Install the **ESP32 board package** via Boards Manager.
3. Replace the following placeholders in the sketch:

```cpp
#define BLYNK_AUTH_TOKEN  "your_blynk_token_here"
const char* ssid       = "your_wifi_ssid";
const char* serverURL  = "http://<YOUR_LAPTOP_IP>:5000";
```

4. Select **ESP32 Dev Module** as the board and upload.

### Pin Reference

| Component | ESP32 Pin | Function |
|-----------|-----------|----------|
| DHT22 Data | GPIO 15 | Temperature & Humidity Sensing |
| Red LED | GPIO 12 | Critical Alert |
| Green LED | GPIO 14 | System Secure |
| Buzzer | GPIO 13 | Audible Alarm |

---

## Project Structure

```
lifelink/
├── backend/
│   └── server.py          # Flask server, AI decay logic, SHA-256 blockchain
├── firmware/
│   └── vaxsafe_esp32.ino  # ESP32 C++ code (sensor + Blynk integration)
├── requirements.txt        # Python dependencies
└── CONTRIBUTING.md
```

---

## How to Contribute

### 1. Fork & Branch

```bash
git fork https://github.com/yashvendranirwan-alt/lifelink.git
git checkout -b feature/your-feature-name
# or
git checkout -b fix/your-bug-description
```

Use clear branch names:
- `feature/lstm-potency-prediction`
- `fix/buzzer-false-alarm`
- `docs/improve-readme`
- `hardware/gps-module-support`

### 2. Make Your Changes

Follow the guidelines in the [Development Guidelines](#development-guidelines) section.

### 3. Test Your Changes

- **Backend:** Run `python backend/server.py` and send test POST requests to verify AI logic and blockchain hashing.
- **Firmware:** Flash to an ESP32 and verify serial monitor output is correct before submitting.

### 4. Commit with Clear Messages

```bash
git add .
git commit -m "feat: add freeze-thaw cycle detection in potency model"
```

Follow this commit format:

| Prefix | Use For |
|--------|---------|
| `feat:` | New feature |
| `fix:` | Bug fix |
| `docs:` | Documentation change |
| `refactor:` | Code restructuring |
| `hardware:` | Hardware-related changes |
| `test:` | Adding or updating tests |

---

## Development Guidelines

### Python (Backend)

- Follow [PEP 8](https://pep8.org/) style.
- Keep AI logic inside `server.py` clearly commented.
- Do not break the existing blockchain chain structure — every new log entry must include:

```python
hash(new) = SHA256(temperature + health + timestamp + hash(previous))
```

- Potency decay rules (do not change without discussion):
  - **Heat:** Every 1°C above 8°C → potency drops **0.1%**
  - **Freeze:** Any dip below 2°C → potency drops **0.5%**

### Arduino / C++ (Firmware)

- Keep credentials out of committed code — use `#define` placeholders.
- Use `Serial.println()` for debug output (not permanent `delay()` loops).
- Comment all pin definitions at the top of the sketch.
- Do not change existing pin mappings without updating the README.

### General Rules

- Do not commit compiled binaries (`.bin`, `.elf`, `.hex`).
- Do not commit personal credentials (Wi-Fi passwords, Blynk tokens, API keys).
- Keep pull requests focused — one feature or fix per PR.

---

## Hardware Contributions

If you're adding support for new sensors or modules (e.g., GPS, new temperature sensors), please:

1. Document the new pin mapping in your PR description.
2. Update the pin reference table in both this file and the README.
3. Ensure backward compatibility with the existing DHT22 + ESP32 setup.
4. Test with real hardware before submitting if possible.

---

## Submitting a Pull Request

1. Push your branch to your fork:
   ```bash
   git push origin feature/your-feature-name
   ```
2. Open a Pull Request against the `main` branch.
3. Fill in the PR template with:
   - **What** you changed and **why**
   - Whether it affects hardware, firmware, or backend
   - Any testing you performed
4. Be responsive to review comments — this is a learning project and feedback goes both ways.

---

## Reporting Issues

Found a bug? Open a [GitHub Issue](https://github.com/yashvendranirwan-alt/lifelink/issues) and include:

- A clear description of the problem
- Steps to reproduce (for firmware issues, include serial monitor output)
- Expected vs actual behaviour
- Your hardware setup (if relevant)

---

## Roadmap & Future Work

Here are areas where contributions are especially welcome:

| Feature | Description | Difficulty |
|---------|-------------|------------|
| **Smart Contracts** | Port SHA-256 chain to Solidity (Ethereum/Polygon) | Advanced |
| **LSTM Prediction** | Use Long Short-Term Memory networks to predict spoilage before it happens | Advanced |
| **GPS Tracking** | Log shipment location during temperature breach events | Intermediate |
| **Unit Tests** | Add tests for the Python AI decay model | Beginner |
| **Web Dashboard** | Improve the live monitoring UI | Intermediate |
| **Multi-vaccine Profiles** | Different decay rates for different vaccine types | Intermediate |

---

## Author

**Yashvendra Singh**
1st Year B.Tech Student | IoT & Cold Chain Security

*Built with curiosity, solder, and a genuine desire to keep vaccines safe.*
