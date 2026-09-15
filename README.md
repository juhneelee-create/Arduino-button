# Arduino-button
Making the button click 
# Arduino Uno R3 Button Module Starter Kit

A simple, beginner-friendly starter project interfacing a 3-pin digital button module with an Arduino Uno R3 (or Elegoo Uno R3). Features state-change detection and software debouncing to ensure clean, single-trigger button presses without spamming the Serial Monitor.

---

## 🛠 Hardware Required

* **Microcontroller:** Arduino Uno R3 or Elegoo Uno R3
* **Input Device:** 3-pin Push Button Module
* **Wiring:** 3x Female-to-Male Jumper Wires
* **Cable:** USB Type-A to Type-B Cable (for uploading code and serial feedback)

---

## 🔌 Circuit & Wiring

No breadboard is required if using Female-to-Male jumper wires. Connect the button module headers directly to the Arduino pins as shown:

| Button Module Pin | Wire Type | Arduino Uno Pin | Description |
| :--- | :--- | :--- | :--- |
| **`S`** (Signal) | Female-to-Male | **Digital Pin 2** | Reads button state transitions |
| **Middle Pin** (`+` / VCC) | Female-to-Male | **5V** | Power supply |
| **`-`** (GND / Ground) | Female-to-Male | **GND** | Ground connection |

> **Safety Tip:** Disconnect the USB cable from your computer before making or altering jumper wire connections.

---

## 💻 Arduino Code (`button_test.ino`)

This sketch uses **State Change Detection** to ensure the Serial Monitor prints `"Button Pressed!"` **only once per physical press**, even if the button is held down continuously.

```cpp
/*
  Arduino Uno R3 - Button Module Single-Press Controller
  
  Detects state transitions (LOW to HIGH) to prevent spamming
  the Serial Monitor during held presses.
*/

const int buttonPin = 2;     // Digital pin connected to button signal
const int ledPin = 13;       // Built-in LED pin on Arduino

int lastButtonState = LOW;   // Stores previous loop state for edge detection

void setup() {
  pinMode(buttonPin, INPUT); // Configure signal pin as input
  pinMode(ledPin, OUTPUT);   // Configure built-in LED as output
  Serial.begin(9600);        // Initialize Serial Monitor at 9600 baud
}

void loop() {
  int currentButtonState = digitalRead(buttonPin);

  // Trigger ONLY on the rising edge (transition from LOW to HIGH)
  if (currentButtonState == HIGH && lastButtonState == LOW) {
    digitalWrite(ledPin, HIGH);
    Serial.println("Button Pressed!");
    delay(50); // Software debouncing to filter mechanical contact bounce
  } 
  // Handle button release
  else if (currentButtonState == LOW && lastButtonState == HIGH) {
    digitalWrite(ledPin, LOW);
  }

  // Update previous state for next pass
  lastButtonState = currentButtonState;
}
