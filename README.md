---

# SafeAccess: IoT Smart Access Control (ESP32 + Python + Discord)

An intelligent IoT security project that provides real-time facial recognition and access control. By integrating an **ESP32-CAM** with a **Python-based backend**, the system validates authorized users, triggers physical locks via relays, and sends instant notifications—including snapshots—directly to a **Discord** server.

> **Automated access with security and connectivity.**

---

## Key Features

* **Biometric Recognition:** High-accuracy facial recognition using `face_recognition` and `dlib`.
* **Discord Integration:** Real-time alerts via Webhooks, sending access status and captured images.
* **Physical Control:** Automated relay activation for electronic door locks.
* **Motion-Triggered:** PIR sensor integration to activate the camera only when movement is detected.
* **Local Feedback:** Visual status updates ("Access Granted" / "Access Denied") on an I2C OLED display.

---

## Hardware & Software Requirements

### Hardware
* **ESP32 Board:** ESP32-CAM (AI-Thinker) or TTGO T-Camera.
* **Sensors:** PIR Motion Sensor (HC-SR501).
* **Actuators:** 5V Relay Module (for the electronic lock).
* **Display:** SSD1306 OLED Display (I2C).

### Software
* **Python 3.8+**
* **Arduino IDE** (with ESP32 board support).
* **Python Libraries:** `Flask`, `face_recognition`, `opencv-python`, `requests`, `Pillow`.

---

## Folder Structure

```text
.
├── AccessControl/          # ESP32 Firmware (Arduino IDE)
│   └── AccessControl.ino
├── known_faces/            # Database for authorized users (Name.jpg)
├── templates/              # Optional Flask HTML templates
├── cloud_bridge_server.py  # Python Backend & Integration Server
├── exemplo.png             # Preview of Discord notifications
└── README.md
```

---

## Setup & Installation

### 1. Database Setup
1. Create a folder named `known_faces` in the root directory.
2. Add clear photos of authorized individuals.
3. The filename will be used as the person's name (e.g., `Hugo_Takeda.jpg`, `John_Doe.png`).

### 2. Discord Webhook
1. In your Discord server, go to **Server Settings** > **Integrations** > **Webhooks**.
2. Create a new Webhook and copy the **Webhook URL**.
3. Open `cloud_bridge_server.py` and update the `DISCORD_WEBHOOK_URL` variable.

### 3. Start the Python Server
Run the following commands in your terminal:
```bash
pip install Flask requests Pillow face_recognition opencv-python
python cloud_bridge_server.py
```
*Note: Note down your local IP address (e.g., `192.168.x.x`) to configure the ESP32.*

### 4. ESP32 Configuration
Open `AccessControl/AccessControl.ino` in Arduino IDE:
1. Update `ssid` and `password` with your Wi-Fi credentials.
2. Update the `serverUrl` with your Python server's IP:
   ```cpp
   String serverUrl = "http://192.168.x.x:5000/recognize";
   ```
3. Compile and upload the code to your ESP32.

---

## How it Works

1. **Detection:** The PIR sensor detects movement.
2. **Capture:** The ESP32-CAM takes a photo and sends it to the Flask API via a POST request.
3. **Analysis:** The Python server processes the image, comparing it against the `known_faces` database.
4. **Action:**
   * **If Recognized:** The server sends a "success" response. The ESP32 triggers the relay to open the door and displays "Access Granted" on the OLED.
   * **If Unrecognized:** "Access Denied" is displayed.
5. **Notification:** A message with the result and the photo is instantly sent to the Discord channel.

---

## Contributing

Contributions and suggestions are welcome! Feel free to open an issue or submit a pull request.

---

## License

This project is licensed under the **MIT License**.

---
