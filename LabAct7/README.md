# Laboratory Activity #7: Web Controlled IoT System with FastAPI

## 📖 Project Overview
This last activity evolves our IoT system into a **Web-Controlled Architecture**. Instead of running a local Python script in the terminal, we deployed a **REST API** using **FastAPI**.

This allows users to control the Arduino hardware (LEDs) via HTTP requests from a web browser or API client (like Postman). This architecture mimics modern IoT gateways where a server acts as a bridge between the internet and physical hardware.

## 🎯 Objectives
* Understand and implement Arduino Serial Connection
* Utilize Python as a tool for implementing serial connection, and implement a HTTP-based solution using FastAPI
* Create a simple circuit what will implement bi-directional connection between Arduino and Python using FastAPI

## 🛠️ Hardware & Tech Stack
* **Microcontroller:** Arduino Uno
* **Backend Framework:** Python **FastAPI** (running on Uvicorn)
* **Libraries:** `pyserial`, `uvicorn`, `fastapi`
* **Components:**
    * 3x LEDs (Red, Green, Blue)
    * 3x Push Buttons (Hardware integrated for future expansion)
    * 6x Resistors (10k Ω)

## 🔌 Circuit & Wiring Details
The circuit follows the standard configuration used in previous labs, with LEDs connected to digital pins.

| Component | Arduino Pin | Mode |
| :--- | :--- | :--- |
| **Red LED** | `Pin 7` | Output |
| **Green LED** | `Pin 6` | Output |
| **Blue LED** | `Pin 5` | Output |
| **Button 1** | `Pin 12` | Input |
| **Button 2** | `Pin 11` | Input |
| **Button 3** | `Pin 10` | Input |

## 💡 How It Works (The Architecture)


The system operates using a **Request-Response** model:

1.  **The Client (Browser/Postman):** Sends an HTTP GET request to a specific URL (e.g., `http://localhost:8000/led/red`).
2.  **The Server (FastAPI):**
    * Receives the request.
    * Translates the URL parameter (`red`) into the corresponding Serial command (`'1'`).
    * Sends the byte data over the USB port to the Arduino.
3.  **The Hardware (Arduino):**
    * Reads the serial buffer.
    * Executes the command (e.g., toggles Pin 7).

## 🚀 API Endpoints
Once the server is running, the following endpoints are available:

| Endpoint | Method | Description | Serial Command Sent |
| :--- | :--- | :--- | :--- |
| `/` | `GET` | Health check to see if API is running. | - |
| `/led/on` | `GET` | Turns **ALL** LEDs ON. | `'9'` |
| `/led/off` | `GET` | Turns **ALL** LEDs OFF. | `'0'` |
| `/led/red` | `GET` | Toggles the **Red** LED. | `'1'` |
| `/led/green` | `GET` | Toggles the **Green** LED. | `'2'` |
| `/led/blue` | `GET` | Toggles the **Blue** LED. | `'3'` |

## 📸 Breadboard Diagram

<img width="1707" height="728" alt="Breadboard_diagram" src="https://github.com/JoseAngelo15/Bitanga_COSC111B_Laboratory-Portfolio/blob/main/LabAct7/Diagram/Breadboard%20Diagram%20for%20Lab%20Act%20%237.png" />

## 🧠 Key Learnings
In this activity, our team learned:
1.  **REST API Development:** We learned how to build API endpoints using **FastAPI** decorators (like `@app.get`). This allows our hardware to be controlled by any device on the network, not just the local keyboard.
2.  **Lifecycle Management:** We utilized FastAPI's `@app.on_event("startup")` and `@app.on_event("shutdown")`. This is crucial for hardware projects because it ensures the Serial connection is **opened securely** when the server starts and **closed properly** when it stops, preventing "Port Busy" errors.
3.  **CORS Middleware:** We implemented `CORSMiddleware` to allow cross-origin requests. This ensures that if we build a frontend website later (e.g., in React or HTML), it can talk to our API without security blocking it.
4.  **Hardware Abstraction:** We improved our Arduino code by creating a helper function `toggleLED()` in a header file, making the main logic cleaner and easier to read.

## 👥 Team Members
**Leader:**
* Mark John Paul Pagarigan

**Members:**
* Jose Angelo B. Bitanga
* John Robert L. Olaño
* Gwen Marinie C. Paciente
* Mark I. Sanguir
* Rovick B. Padilla

---
### 📂 Code Snippet
*FastAPI Endpoint Logic:*
```python
@app.get("/led/{color}")
def control_led(color: str):
    if color == "red":
        arduino.write(b'1') # Sends command to hardware
        return {"status": "Red LED toggled"}
