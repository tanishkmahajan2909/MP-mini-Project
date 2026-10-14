# 🚗 Web-Controlled GPS RC Car with Voice Command
An IoT-based remote control car powered by an **Arduino UNO**, controlled in real time through a web dashboard. The web interface features **voice recognition** for hands-free navigation, directional on-screen controls, and **live GPS map tracking** to monitor the vehicle's exact coordinates.
---
## ✨ Features
* **🌐 Web-Based Control Panel:** Control the car (Forward, Backward, Left, Right, Stop) from any browser over a local network or the internet.
* **🎙️ Voice Control Integration:** Uses the browser's **Web Speech API** to translate spoken commands into immediate motor actions.
* **📍 Live GPS Map Tracking:** Streams real-time latitude and longitude data from the onboard GPS module to an interactive map (**Leaflet.js / Google Maps API**) on the dashboard.
* **⚡ Arduino UNO Core:** Reliable PWM motor speed and direction control compiled and executed on an Arduino UNO.
* **📡 Bi-Directional Telemetry:** Sends movement commands from the web UI to the car while receiving live GPS coordinates and connection status back.
---
## 🛠️ Hardware Requirements

| Component | Quantity | Purpose |
| :--- | :--- | :--- |
| **Arduino UNO** | 1 | Main microcontroller for motor control and sensor parsing |
| **L298N Motor Driver** | 1 | Dual H-Bridge driver to control DC motor direction and speed |
| **NEO-6M GPS Module** | 1 | Captures satellite coordinates for live map tracking |
| **ESP8266 (ESP-01 / NodeMCU)** | 1 | Wi-Fi serial bridge to connect the Arduino UNO to the web server |
| **DC Gear Motors + Wheels** | 4 | Drivetrain for the RC chassis |
| **RC Car Chassis** | 1 | Mounting frame for electronics and motors |
| **Li-Ion Battery Pack (7.4V - 11.1V)** | 1 | Power supply for the motors and Arduino |
| **Jumper Wires & Breadboard** | 1 set | Circuit prototyping and connections |

---
## 💻 Tech Stack
* **Microcontroller Firmware:** C / C++ (Arduino IDE)
* **Libraries Used:** `TinyGPS++`, `SoftwareSerial`
* **Frontend Dashboard:** HTML5, CSS3, JavaScript (ES6)
* **Map Rendering:** Leaflet.js (OpenStreetMap) / Google Maps JavaScript API
* **Voice Recognition:** Web Speech API (`webkitSpeechRecognition`)
* **Communication Protocol:** WebSockets / HTTP REST Requests via Wi-Fi
---
## 🔌 Pin Configuration (Arduino UNO)
### 1. L298N Motor Driver to Arduino UNO

| L298N Pin | Arduino UNO Pin | Function |
| :--- | :--- | :--- |
| **ENA** | `D5` (PWM) | Left Motors Speed |
| **IN1** | `D6` | Left Motors Forward |
| **IN2** | `D7` | Left Motors Backward |
| **IN3** | `D8` | Right Motors Forward |
| **IN4** | `D9` | Right Motors Backward |
| **ENB** | `D10` (PWM) | Right Motors Speed |
| **GND** | `GND` | Common Ground |

### 2. NEO-6M GPS Module to Arduino UNO

| GPS Pin | Arduino UNO Pin | Function |
| :--- | :--- | :--- |
| **VCC** | `5V` | Power Supply |
| **RX** | `D4` | SoftwareSerial TX |
| **TX** | `D3` | SoftwareSerial RX |
| **GND** | `GND` | Common Ground |

### 3. Wi-Fi Module (ESP8266) to Arduino UNO

| ESP8266 Pin | Arduino UNO Pin | Function |
| :--- | :--- | :--- |
| **VCC / CH_PD** | `3.3V` | Power Supply (Strict 3.3V) |
| **RX** | `TX (D1)` | Serial Receive (via voltage divider) |
| **TX** | `RX (D0)` | Serial Transmit |
| **GND** | `GND` | Common Ground |

---
## 📂 Repository Structure
```text
├── arduino-firmware/
│   ├── rc_car_controller.ino      # Main Arduino UNO sketch (Motor + GPS logic)
│   └── esp_wifi_bridge.ino        # Wi-Fi communication script (if applicable)
├── web-dashboard/
│   ├── index.html                 # Main dashboard layout & map container
│   ├── style.css                  # Styling for controls and telemetry UI
│   └── app.js                     # Voice recognition, map updates, and API calls
└── README.md                      # Project documentation
```
---
## 🚀 Getting Started
### Step 1: Hardware & Arduino Setup
1. Assemble the chassis and wire the **L298N**, **NEO-6M GPS**, and **Wi-Fi module** to the **Arduino UNO** according to the pin table above.
2. Open the **Arduino IDE** and install the required library:
   * Go to **Sketch** -> **Include Library** -> **Manage Libraries** -> Search for `TinyGPSPlus` and install it.
3. Open `arduino-firmware/rc_car_controller.ino`.
4. Update the Wi-Fi credentials (`SSID` and `PASSWORD`) in the configuration section.
5. Select **Board: "Arduino Uno"** and the appropriate **COM Port**, then click **Upload** (disconnect pins `D0/D1` during upload if using hardware serial).
### Step 2: Launch the Web Dashboard
1. Navigate to the `web-dashboard/` directory:
   ```bash
   cd web-dashboard
   ```
2. Open `app.js` and set the `CAR_IP_ADDRESS` variable to the IP address printed in your Arduino Serial Monitor.
3. Start a local web server (HTTPS or `localhost` is required by modern browsers for microphone access with the Web Speech API):
   ```bash
   # Using Python
   python -m http.server 8000
   ```
4. Open your browser and navigate to `http://localhost:8000`.
---
## 🗣️ Voice Commands Reference
Click the **"🎙️ Activate Voice Control"** button on the website and grant microphone permissions. Speak any of the following trigger words clearly:

| Spoken Command | Action Triggered |
| :--- | :--- |
| **"Forward"** / **"Go"** | Drives the car straight ahead |
| **"Backward"** / **"Reverse"** | Drives the car in reverse |
| **"Left"** / **"Turn Left"** | Pivots the car to the left |
| **"Right"** / **"Turn Right"** | Pivots the car to the right |
| **"Stop"** / **"Halt"** | Immediately cuts power to all motors |

---
## 🛠️ Troubleshooting
* **GPS shows `0.0000, 0.0000` on the map:** NEO-6M modules require a clear view of the sky to acquire a cold-start satellite lock. Take the car outdoors and wait 1–3 minutes for the onboard GPS LED to start blinking.
* **Voice recognition isn't starting:** Ensure you are running the site on `localhost` or `HTTPS` in a Chromium-based browser (Google Chrome / Microsoft Edge) for full `webkitSpeechRecognition` support.
* **Arduino resets when motors start:** Use separate battery power for the L298N motor driver or ensure your battery pack can supply sufficient peak current without voltage sag, keeping a shared `GND` between the Arduino and L298N.
---
## 📜 License
This project is open-source and available under the [MIT License](LICENSE).
