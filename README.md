# Taekwondo Kick Speed Trainer

An Arduino-based system for measuring your students’ kick reaction times and speeds, using a simple foil-sheet contact switch under the pad. Features an OLED display, buzzer countdown, and three measurement modes—single kick, two-kick times, and interval between kicks—using interrupt-driven detection for maximum precision.

---

## Features

- **Ultra-fast, interrupt-driven detection** via a foil-sheet contact switch (pads close to GND)  
- **Three modes**:  
  1. Single kick reaction time  
  2. First & second kick reaction times  
  3. Time interval between two kicks  
- **OLED feedback** via Adafruit SSD1306 (128×64), showing countdown, results, and mode selection  
- **Buzzer countdown** and “go” tone to cue the athlete  
- **Debounced buttons** for mode selection and start  
- **Non-blocking state machine**—no `delay()` during the timing window, so the MCU remains instantly responsive  

---

## Hardware Requirements

- **Arduino R4 Wi-Fi** (or Uno/ESP32)  
- **Adafruit SSD1306 OLED** (I²C, 128×64)  
- **Foil-sheet contact switch** under the kicking pad (two overlapping foil strips that short to GND when pressed)  
- **Passive buzzer**  
- **3 momentary push-buttons** (mode − / mode + / START)  
- Jumper wires, breadboard  

---

## Wiring

| Component         | Arduino Pin         | Notes                                      |
|-------------------|---------------------|--------------------------------------------|
| OLED SCL          | A5 (or SCL)         | I²C clock                                  |
| OLED SDA          | A4 (or SDA)         | I²C data                                   |
| Foil contact pad  | D2 (INT0)           | `INPUT_PULLUP`; contacts short to GND      |
| Buzzer            | D5                  | Passive buzzer                             |
| Mode − button     | D3                  | `INPUT_PULLUP` to GND                      |
| Mode + button     | D4                  | `INPUT_PULLUP` to GND                      |
| Start button      | D6                  | `INPUT_PULLUP` to GND                      |
| 5 V / 3.3 V       | VCC                 | Match your OLED’s voltage requirement      |
| GND               | GND                 | Common ground                              |

> **Note:** All “button” and foil-pad pins use internal pull-ups; when the pad or button closes, it pulls the pin to GND.

---

## Software Installation

1. **Clone this repo**  
   ```bash
   git clone https://github.com/yourusername/taekwondo-kick-speed-trainer.git
   cd taekwondo-kick-speed-trainer
Install libraries
In Arduino IDE Library Manager:

Adafruit SSD1306

Adafruit GFX

Open the sketch

Load KickSpeedTrainer.ino in the IDE.

Select your board & port.

Upload

Click Upload and wait for completion.

Usage
Power on the board. The OLED shows Press START.

Select mode (use Mode − / +):

1 – single kick time

2 – first & second kick times

3 – interval between kicks

Press START.

OLED displays GET READY, then after 2 s the buzzer sounds and timing begins.

Kick the pad.

The foil contact closes, triggering an interrupt that records micros().

Read results on the screen. After 5 s it resets to the mode-select prompt.

How It Works
Interrupt-driven timing:

Sensor pin configured as INPUT_PULLUP + attachInterrupt(..., FALLING).

ISR records micros() and sets a flag—no polling required.

Non-blocking loop:

Main loop() waits on the flag without repeated digital reads or delay().

State machine:

Phases: WAIT_FOR_START → COUNTDOWN → AWAIT_KICKS → DISPLAY_RESULT.

Calibration & Tuning
Debounce interval (debounceUS): adjust 2 ms–10 ms to eliminate contact chatter.

Timeouts: add a maximum wait if no kick is detected to avoid hanging.

Troubleshooting
No trigger: check foil wiring, ensure both strips make firm contact.

Multiple triggers: increase debounceUS or add a small RC filter (e.g., 100 nF + 10 kΩ).

OLED blank: confirm wiring & I²C address (0x3C).

Future Improvements
Log times over Wi-Fi to a dashboard

Mobile-app interface for session control and result storage

Aggregate stats (best, average, consistency) on-screen
