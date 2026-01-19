# Final Laboratory Exam: Arduino-to-Python Client System

## 📖 Project Overview
This project constitutes our **Final Laboratory Exam**, where we designed an **IoT Client System** that bridges physical hardware with a remote web API.

The system allows a user to trigger a remote action (toggling an LED on a server) by pressing a physical button on an Arduino. The Arduino does not connect to the internet directly; instead, it acts as a **Serial Input Device** for a Python client running on a computer, which handles the network request.

## 🎯 Objectives
* To create a **Serial-to-HTTP Gateway** using Python.
* To implement **software debouncing** on Arduino to ensure reliable single-press detection.
* To construct a robust Python client that listens continuously for hardware signals and executes API calls dynamically.
* To demonstrate a client-server architecture where the Arduino is the trigger and Python is the courier.

## 🛠️ Hardware & Tech Stack
* **Microcontroller:** Arduino Uno
* **Input Device:** Push Button (connected to Pin 4)
* **Software:**
    * **Arduino IDE:** Firmware logic
    * **Python:** Client-side script
    * **Libraries:** `pyserial` (Serial communication), `requests` (HTTP API calls)
* **API Endpoint:** Remote server provided by the instructor (`/led/group/<number>/toggle`)

## 🔌 Circuit & Wiring Details
The setup is minimal to focus on the software logic.

| Component | Arduino Pin | Configuration |
| :--- | :--- | :--- |
| **Push Button** | `Pin 4` | Input with Internal Pull-up Resistor (`INPUT_PULLUP`) |

> **Note:** We used the internal pull-up resistor, meaning the button reads `LOW` when pressed and `HIGH` when released.

## 💡 System Architecture
The data flow follows this path:

1.  **Physical Event:** User presses the button.
2.  **Arduino Processing:**
    * The firmware detects the press.
    * It waits 50ms (Debounce Delay) to ensure it's a real press.
    * It prints the Group Number (`3`) to the Serial Monitor **once**.
3.  **Python Client:**
    * Detects the incoming data (`"3"`).
    * Validates that the data is a digit.
    * Constructs the API URL: `http://<server-ip>:8000/led/group/3/toggle`.
    * Sends a **GET Request**.
4.  **Feedback:** The Python terminal prints the server's response (Success/Error).



## 🚀 Key Features Implemented

### 1. Robust Debouncing (Arduino)
To satisfy the requirement that "One button press = one API request," we implemented a **millis()-based debounce algorithm**. This prevents the mechanical chatter of the button from sending multiple signals for a single press.

### 2. Input Validation (Python)
The Python script is designed to be crash-proof. It checks:
* `if raw_data.isdigit()`: Ensures only numbers are processed.
* `try...except`: Catches connection errors (if the API server is down) without crashing the program.

### 3. Non-Terminating Listener
The Python script runs in a `while True` loop, allowing it to stay active indefinitely and process multiple button presses over time without needing a restart.

## 👥 Team Members
**Leader:**
* John Robert L. Olaño

**Members:**
* Jose Angelo B. Bitanga
* Mark John Paul Pagarigan
* Gwen Marinie C. Paciente
* Mark I. Sanguir
* Rovick B. Padilla

---
### 📂 Code Snippet
*Arduino Debounce Logic:*
```cpp
if ((millis() - lastDebounceTime) > debounceDelay) {
    if (reading != buttonState) {
        buttonState = reading;
        if (buttonState == LOW) { // Valid Press
             Serial.println(groupNumber);
        }
    }
}
