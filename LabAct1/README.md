# Laboratory Activity #1: Working with Digital Signals

## 📖 Project Overview
The **Laboratory Activity #1** focuses on the fundamentals of **Arduino** as a tool for IoT systems. The goal was to design and implement a "Running Light" circuit that demonstrates the manipulation of digital signals using a microcontroller.

The system sequentially illuminates a series of LEDs and then turns them off, utilizing digital output pins and timing logic.

## 🎯 Objectives
* Review Arduino as a device for IoT systems implementation
* Discuss digital signals and its implementation in a Arduino circuit.

## 🛠️ Hardware & Tech Stack
* **Microcontroller:** Arduino Uno (or compatible board)
* **Components:**
    * 5x LEDs (Light Emitting Diodes)
    * 5x Resistors (10k Ω)
    * Breadboard and Jumper Wires
* **Software:** Arduino IDE
* **Simulation Tools:** TinkerCad / Fritzing

## 🔌 Circuit & Wiring Details
The LEDs are connected to the digital I/O pins of the Arduino as follows:

| LED Position | Arduino Pin |
| :--- | :--- |
| LED 1 | Pin 12 |
| LED 2 | Pin 11 |
| LED 3 | Pin 10 |
| LED 4 | Pin 9 |
| LED 5 | Pin 8 |

> **Note:** Ensure all LEDs are grounded properly via resistors to prevent burnout.

## 💡 How It Works (The Logic)
The firmware (`.ino` file) controls the LEDs in a specific sequence:
1.  **Initialization:** Pins 8 through 12 are set as `OUTPUT`.
2.  **Sequence A (Turn ON):** The loop turns on LEDs one by one, starting from Pin 12 down to Pin 8, with a **1-second delay** between each activation.
3.  **Sequence B (Turn OFF):** Once all LEDs are lit, the loop turns them off one by one in the same order (12 to 8), maintaining the 1-second delay.
4.  **Loop:** This pattern repeats indefinitely.

## 📸 Breadboard Diagram
<img width="1707" height="728" alt="Bitanga_Olano_Paciente_Breadboard_diagram" src="https://github.com/JoseAngelo15/Bitanga_COSC111_Laboratory-Portfolio/blob/main/LabAct1/Breadboard%20Diagram%20for%20Lab%20Act%20%231.png" />

## 🧠 Key Learnings
Through this activity, our group learned:
1.  **Digital Output Control:** We gained a practical understanding of how `digitalWrite()` changes the state of a pin between `HIGH` (5V) and `LOW` (0V) to control external components.
2.  **Timing & Sequencing:** We learned how `delay()` functions impact the program flow, allowing us to create distinct visual patterns rather than instantaneous changes.
3.  **Circuit Logic:** We reinforced our knowledge of current flow, specifically proper grounding and the necessity of current-limiting resistors for LED protection.
4.  **Pin Addressing:** We practiced addressing specific digital pins programmatically to match physical wiring configurations.

---

## 👥 Team Members
**Leader:**
* John Robert L. Olaño

**Members:**
* Jose Angelo B. Bitanga
* Gwen Marinie C. Paciente


