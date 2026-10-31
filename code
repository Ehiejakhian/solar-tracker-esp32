#include <Servo.h>

// ---------------- SERVOS ----------------
Servo xAxis;
Servo yAxis;


// Pins
const int servoXPin = 2;
const int servoYPin = 13;

// ---------------- RGB LED (COMMON ANODE) ----------------
const int redPin = 3;
const int greenPin = 5;
const int bluePin = 6;

// ---------------- MODE CONTROL ----------------
bool auto_mode = true;
bool last_button_state = HIGH;
const int modeButton = 8;

// ---------------- MANUAL INPUT (POTS) ----------------
const int potX = A4;
const int potY = A5;

// ---------------- POSITIONS ----------------
int servoX = 90;
int servoY = 90;

// Limits
int servoXLow = 5;
int servoXHigh = 175;

int servoYLow = 40;
int servoYHigh = 140;

// ---------------- LDR PINS ----------------
const int ldr_tl = A0;
const int ldr_tr = A1;
const int ldr_br = A2;
const int ldr_bl = A3;

// ---------------- STABILITY CONTROL ----------------
int stableCountX = 0;
int stableCountY = 0;

const int deadZone = 80;        // ignore small noise
const int stableThreshold = 3;  // how long to lock

// ---------------- SMOOTH LDR ----------------
int readLDR(int pin) {
  long sum = 0;

  for (int i = 0; i < 20; i++) { // What is a for loop?
    sum += analogRead(pin);
  }

  return sum / 20;
}

// ---------------- RGB FUNCTION ----------------
void setColor(bool r, bool g, bool b) {

  digitalWrite(redPin, !r);
  digitalWrite(greenPin, !g);
  digitalWrite(bluePin, !b);
}

// ---------------- SETUP ----------------
void setup() {

  xAxis.attach(servoXPin);
  yAxis.attach(servoYPin);

  pinMode(redPin, OUTPUT);
  pinMode(greenPin, OUTPUT);
  pinMode(bluePin, OUTPUT);

  setColor(0, 1, 0); // Wetin be the difference between parameters and arguments?

  xAxis.write(servoX); // Wetin be the difference between object and method?
  yAxis.write(servoY);

  pinMode(modeButton, INPUT_PULLUP);

  Serial.begin(9600); // Wetin be serial monitor and baud rate for arduino matter?
}

// ---------------- LOOP ----------------
void loop() {

  // ===== MODE TOGGLE =====
  bool currentState = digitalRead(modeButton);

  if (last_button_state == HIGH && currentState == LOW) {

    auto_mode = !auto_mode;

    delay(200);

    if (auto_mode) {
      Serial.println("AUTO MODE");
      setColor(0, 1, 0);
    } else {
      Serial.println("MANUAL MODE");
      setColor(1, 0, 0);
    }
  }

  last_button_state = currentState;

  // =====================================================
  //                    AUTO MODE
  // =====================================================
  if (auto_mode) {

    int tl = readLDR(ldr_tl);
    int tr = readLDR(ldr_tr);
    int br = readLDR(ldr_br);
    int bl = readLDR(ldr_bl);

    int top = (tl + tr) / 2;
    int bottom = (bl + br) / 2;
    int left = (tl + bl) / 2;
    int right = (tr + br) / 2;

    int dY = top - bottom;
    int dX = left - right;

    // ================= Y AXIS =================
    if (abs(dY) > deadZone) {

      stableCountY = 0;

      int stepY = constrain(dY / 60, -2, 2);

      if (stepY != 0) {
        servoY += stepY;
        servoY = constrain(servoY, servoYLow, servoYHigh);
        yAxis.write(servoY);
      }

    } else {

      stableCountY++;

      if (stableCountY >= stableThreshold) {
        yAxis.write(servoY); // lock position
      }
    }

    // ================= X AXIS =================
    if (abs(dX) > deadZone) {

      stableCountX = 0;

      int stepX = constrain(dX / 60, -2, 2);

      if (stepX != 0) {
        servoX -= stepX;
        servoX = constrain(servoX, servoXLow, servoXHigh);
        xAxis.write(servoX);
      }

    } else {

      stableCountX++;

      if (stableCountX >= stableThreshold) {
        xAxis.write(servoX); // lock position
      }
    }

    Serial.print("AUTO | dX: "); // Wetin be debugging?
    Serial.print(dX);

    Serial.print(" dY: ");
    Serial.print(dY);

    Serial.print(" X: ");
    Serial.print(servoX);

    Serial.print(" Y: ");
    Serial.print(servoY);

    Serial.print(" top: ");
    Serial.print(top);
    Serial.print("    ");
    Serial.print(" bottom:  ");
    Serial.print(bottom);
    Serial.print("    ");
    Serial.print(" right:  ");
    Serial.print(right);
    Serial.print("    ");
    Serial.print(" left:  ");
    Serial.println(left);
  }

  // =====================================================
  //                 MANUAL MODE
  // =====================================================
  else {
      // 0 to 1023
      // 0 degrees to 180degrees
    int manualX = map(analogRead(potX), 0, 1023, servoXHigh, servoXLow);
    int manualY = map(analogRead(potY), 0, 1023, servoYHigh, servoYLow);

    manualX = constrain(manualX, servoXLow, servoXHigh);
    manualY = constrain(manualY, servoYLow, servoYHigh);

    servoX = manualX;
    servoY = manualY;

    xAxis.write(servoX);
    yAxis.write(servoY);

    Serial.print("MANUAL | X: ");
    Serial.print(servoX);

    Serial.print(" Y: ");
    Serial.println(servoY);
  }

  Serial.println("------------------");

  delay(100);
}
