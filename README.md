# Dual-Axis Solar Tracker (Arduino)

A dual-axis solar tracker that finds the brightest light and turns a panel toward it. Four light sensors (LDRs) are arranged in a quadrant, and two servo motors tilt and rotate the platform to keep it facing the sun. It also has a manual mode where two potentiometers control the servos by hand, switched with a push button.

![Solar tracker prototype](images/solar-tracker.png)

> Add a short demo video or GIF here if you have one.

## Features

- **Auto mode:** compares light from four LDRs and steers two servos (X and Y axis) toward the brighter side
- **Manual mode:** two potentiometers set the X and Y servo angles directly
- **One-button switching:** a push button toggles between auto and manual
- **Status LED:** a common-anode RGB LED shows the mode (green = auto, red = manual)
- **Noise handling:** each LDR reading is the average of 20 samples, and a dead zone ignores small differences so the servos don't jitter
- **Safe travel limits:** each servo is restricted to a set angle range so the frame can't be driven past its mechanical limits
- **Serial debugging:** live sensor values and servo angles are printed at 9600 baud

## Parts list

| Part | Quantity | Notes |
| --- | --- | --- |
| Arduino Uno-class board | 1 | Fill in your exact board |
| LDR (light-dependent resistor) | 4 | One per corner of the sensor cross |
| Fixed resistors for the LDR voltage dividers | 4 | Fill in the value (commonly 10 kΩ) |
| Hobby servo motors | 2 | Fill in the model (e.g. SG90 or MG90S) |
| RGB LED (common anode) | 1 | With current-limiting resistors |
| Push button | 1 | Uses the internal pull-up, no resistor needed |
| Potentiometers | 2 | For manual control |
| Solar panel or mounting platform | 1 | |
| Jumper wires, breadboard, custom-built frame | | |
| External 5 V supply for the servos | 1 | Recommended, see the note below |

## Wiring

| Component | Pin |
| --- | --- |
| LDR top-left | A0 |
| LDR top-right | A1 |
| LDR bottom-right | A2 |
| LDR bottom-left | A3 |
| Manual pot, X axis | A4 |
| Manual pot, Y axis | A5 |
| X-axis servo (signal) | D2 |
| Y-axis servo (signal) | D13 |
| RGB LED red | D3 |
| RGB LED green | D5 |
| RGB LED blue | D6 |
| Mode button | D8 (other leg to GND) |

Each LDR sits in a voltage divider (LDR on one side, a fixed resistor on the other) with the midpoint going to its analog pin.

**Power note:** servos can pull more current than a USB port or the board's regulator can safely supply. If the board resets or the servos twitch, power the servos from a separate 5 V supply and connect its ground to the Arduino's ground.

Add a wiring diagram or circuit photo in the `images/` folder and link it here.

## Getting started

1. Install the [Arduino IDE](https://www.arduino.cc/en/software).
2. The sketch only uses the built-in `Servo` library, so there is nothing extra to install.
3. Clone this repository:
   ```bash
   git clone https://github.com/Ehiejakhian/Solar-Tracker-Arduino.git
   ```
4. Open the sketch in the Arduino IDE, choose your board and port, and upload.
5. Open the Serial Monitor at **9600 baud** to watch the readings.

## How it works

### Auto mode

1. Each LDR is read 20 times and averaged to smooth out noise.
2. The four readings are combined into `top`, `bottom`, `left` and `right` brightness values.
3. The differences are calculated: `dY = top - bottom` and `dX = left - right`.
4. If a difference is larger than the **dead zone**, the matching servo moves a small step toward the brighter side. The step is the difference divided by 60, limited to 2° per cycle, so it moves faster when the light is very uneven and slows down as it lines up.
5. When a difference stays inside the dead zone for a few cycles in a row, the servo holds its position.

### Manual mode

The two potentiometers are read and mapped to the allowed servo angles, so turning a pot moves its axis directly.

### Mode button and LED

Pressing the button toggles between auto and manual (with a short delay to ignore contact bounce). The board starts in auto mode with the servos centred at 90°.

## Settings you can tune

| Constant | Default | What it does |
| --- | --- | --- |
| `deadZone` | 80 | Smallest brightness difference that triggers movement. Raise it if the tracker jitters. |
| `stableThreshold` | 3 | Cycles inside the dead zone before holding position |
| `servoXLow` / `servoXHigh` | 5 / 175 | X-axis angle limits |
| `servoYLow` / `servoYHigh` | 40 / 140 | Y-axis angle limits |
| Step divisor (`/ 60`) and clamp (`-2, 2`) | 60, ±2° | How fast the servos chase the light |
| Samples per reading | 20 | More samples = smoother but slower readings |

If the tracker moves away from the light instead of toward it, flip the direction in the code (the X axis uses `-=` and the Y axis uses `+=`). Which one is right depends on how your servos are mounted.

## Serial output

In auto mode the sketch prints lines like:

```
AUTO | dX: <value> dY: <value> X: <angle> Y: <angle> top: <value> bottom: <value> right: <value> left: <value>
```

In manual mode it prints `MANUAL | X: <angle> Y: <angle>`. This is handy for checking that your LDRs are wired the right way round.

## Limitations and next steps

- It chases relative differences between the four sensors, so it is not tuned to a particular sun position or time of day.
- Readings depend on your LDRs and resistors, so the dead zone may need adjusting for a new build.
- Ideas to improve it: smoother servo motion, logging the panel's power output, a weatherproof enclosure, and a night or low-light parking position.

## Author

Built by [Ehi Ejakhian](https://ehiejakhian.github.io). See more of my projects on my [portfolio](https://ehiejakhian.github.io/embedded-sys/).
