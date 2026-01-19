# Laboratory Activity #4: Arduino Serial Connection

## 📖 Project Overview
This laboratory activity focuses on **Two-Way Serial Communication** between the Arduino and a computer. Unlike previous activities where the Arduino only sent data *to* the computer, this system accepts user commands *from* the computer to control hardware behavior.

The system acts as a **Latching Alarm**:
1.  A **Photoresistor** monitors light levels.
2.  If brightness exceeds a threshold (220), the system enters an **"ALARM STATE"** (LED Blinking).
3.  Crucially, the alarm **stays on** even if the light level drops back down.
4.  The only way to stop the alarm is for a user to type **"STOP"** (case-insensitive) in the Serial Monitor.

## 🎯 Objectives
* Understand and implement Arduino Serial Connection
* Utilize and familiarize students on different basic Serial connection functions
* Create a simple circuit that can be controlled using Serial connection

## 🛠️ Hardware & Tech Stack
* **Microcontroller:** Arduino Uno
* **Sensors:** Photoresistor (LDR)
* **Output:** LED (Pin 8)
* **Resistors:** 10k Ω (Sensor Divider)

## 🔌 Circuit & Wiring Details
The circuit uses a standard voltage divider for the Photoresistor and a digital output for the LED.

| Component | Arduino Pin | Notes |
| :--- | :--- | :--- |
| **Photoresistor** | `A0` | Input (Analog) |
| **Red LED** | `Pin 8` | Output (Digital) |

## 💡 How It Works (The Logic)
The code uses a **Boolean Flag (`isBlinking`)** to manage the system's state.

1.  **Monitoring:** The system continuously reads the brightness from Pin A0.
2.  **Triggering:** If `Brightness >= 220`, the `isBlinking` variable is set to `true`.
    * *Note:* Because this is a boolean "flag," the LED continues to blink even if the sensor value drops below 220 immediately after.
3.  **Command Parsing:** The loop checks `Serial.available()` to see if the user sent a message.
    * The code reads the string until a newline character (`\n`).
    * It executes `.trim()` to remove accidental spaces.
    * It executes `.toLowerCase()` so that inputs like "STOP", "Stop", and "stop" all work.
4.  **Reset:** If the processed string equals `"stop"`, `isBlinking` is set to `false`, and the LED is forced LOW.

## 📸 Breadboard Diagram

<img width="1707" height="728" alt="Bitanga_Olano_Paciente_Breadboard_diagram" src="https://github.com/JoseAngelo15/Bitanga_COSC111B_Laboratory-Portfolio/blob/main/LabAct4/Breadboard%20Diagram%20for%20Lab%20Act%20%234.png" />

## 🧠 Key Learnings
In this activity, our team learned:
1.  **String Sanitization:** We learned that raw input from the Serial Monitor often contains invisible characters (like newlines) or casing differences. We used `.trim()` and `.toLowerCase()` to "clean" the input so our code could understand it reliably.
2.  **Latching Logic:** We implemented a "latching" state using a `bool` variable. This taught us how to separate the *trigger event* (high brightness) from the *ongoing state* (blinking), which is essential for real-world alarm systems.
3.  **Serial Input Handling:** We moved beyond `Serial.print` and mastered `Serial.readStringUntil()`, allowing us to create interactive programs where the Arduino responds to human commands.
4.  **Non-Blocking Inputs:** We used `if (Serial.available())` to ensure the Arduino only tries to read data when a message is actually sent, preventing the program from freezing or waiting unnecessarily.

## 👥 Team Members
**Leader:**
* Jose Angelo B. Bitanga

**Members:**
* Gwen Marinie C. Paciente
* John Robert L. Olaño

---
### 📂 Code Snippet
*String Processing Logic Used:*
```cpp
if (Serial.available()) {
  String inputString = Serial.readStringUntil('\n'); // Read input
  inputString.trim();                                // Remove whitespace
  inputString.toLowerCase();                         // Handle "STOP" vs "stop"

  if (inputString == "stop") {
    isBlinking = false; // Reset the Latch
  }
}
