# Taekwondo Kick Speed Trainer

A portable Arduino-based system for measuring your students’ kick reaction times and speeds. Features an OLED display, buzzer countdown, and three measurement modes—single kick time, two-kick times, and time between kicks—using interrupt-driven detection for maximum precision.

---

## Features

- **Ultra-fast, interrupt-driven detection** with piezo/microphone sensor on Arduino R4 Wi-Fi (or Uno)
- **Three modes**:
  1. Single kick reaction time  
  2. First & second kick reaction times  
  3. Time interval between two kicks
- **OLED feedback** via Adafruit SSD1306 (128×64), showing countdown, results, and mode selection
- **Buzzer countdown** and “go” tone to cue the athlete
- **Debounced buttons** for mode selection and start
- **Non-blocking state machine**—no `delay()` during timing, so the MCU is always instantly responsive
- **Easy calibration** and threshold adjustment for noisy environments

---

## Hardware Requirements

- **Arduino R4 Wi-Fi** (or Arduino Uno/ESP32)
- **Adafruit SSD1306 OLED** (I²C, 128×64)
- **Piezo disc sensor** (or electret mic + comparator circuit)
- **Passive buzzer** or piezo buzzer
- **3 momentary push-buttons** (mode ±, START)
- Breadboard, jumper wires, 220 Ω–1 kΩ resistors for sensor conditioning if needed

---

## Wiring

| Component         | Arduino Pin         |
|-------------------|---------------------|
| OLED SCL          | A5 (or SCL)         |
| OLED SDA          | A4 (or SDA)         |
| Piezo/trigger     | D2 (INT0)           |
| Buzzer            | D5                  |
| Mode – button     | D3                  |
| Mode + button     | D2* (if not using INT) |
| Start button      | D4                  |
| 3.3 V / 5 V       | VCC                 |
| GND               | GND                 |

> **Note:** If you use D2 for both “mode+” and the sensor interrupt, move one of them to another digital pin or switch your interrupt to D3 (INT1) on supported boards.

---

## Software Installation

1. **Clone this repo**  
   ```bash
   git clone https://github.com/yourusername/taekwondo-kick-speed-trainer.git
   cd taekwondo-kick-speed-trainer
