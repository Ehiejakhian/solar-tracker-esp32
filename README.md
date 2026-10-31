# Solar Tracker (ESP32)

A solar tracking system that follows the sun to capture more energy. Light sensors detect which direction is brightest, and servo motors tilt the panel to face it.

![Solar tracker prototype](images/solar-tracker.png)

> Add a short demo video or GIF here if you have one.

## Features

- Detects the sun's direction using LDR (light-dependent resistor) sensors
- Moves the panel with servo motors to face the brightest light
- LED indicators for status
- Written in embedded C++ for the ESP32

## Parts list

| Part | Quantity | Notes |
| --- | --- | --- |
| ESP32 development board | 1 | |
| LDR sensors | _N_ | Fill in how many you used |
| Resistors for the LDR voltage dividers | _N_ | Fill in the value (e.g. 10 kΩ) |
| Servo motors | _N_ | Fill in the model (e.g. SG90) |
| LEDs | _N_ | |
| Solar panel | 1 | |
| Jumper wires, breadboard, custom-built frame | | |
| Power supply | 1 | Fill in what powers the servos |

## Wiring

| Component | ESP32 pin |
| --- | --- |
| LDR 1 | GPIO __ |
| LDR 2 | GPIO __ |
| Servo (horizontal) | GPIO __ |
| Servo (vertical) | GPIO __ |
| Status LED | GPIO __ |

Add a wiring diagram or a photo of the circuit in the `images/` folder and link it here.

## Getting started

1. Install the [Arduino IDE](https://www.arduino.cc/en/software) and add ESP32 board support through the Boards Manager.
2. Install any libraries the sketch needs (for example, an ESP32 servo library). List them here.
3. Clone this repository:
   ```bash
   git clone https://github.com/Ehiejakhian/Solar-Tracker-ESP32.git
   ```
4. Open the sketch in the Arduino IDE, select your ESP32 board and port, and upload.

## How it works

1. The ESP32 reads the voltage from each LDR.
2. It compares the readings to work out which side is receiving more light.
3. It rotates the servos in small steps toward the brighter side.
4. When the readings are balanced, the panel holds its position.

Update these steps to match what your code does.

## What I learned / next steps

- What worked well:
- What I'd improve (for example, smoother servo movement, a larger panel, or logging power output):

## Author

Built by [Ehi Ejakhian](https://ehiejakhian.github.io). See more of my projects on my [portfolio](https://ehiejakhian.github.io/embedded-sys/).
