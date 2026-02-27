# 📷 Motion Detection and Email Alert System Using ESP32-CAM

In recent years, smart security systems have become essential for homes and workplaces. Traditional systems require continuous monitoring, but with IoT and embedded systems, we can automate surveillance.

This project demonstrates a **motion-activated security camera using ESP32-CAM and PIR sensor**. When motion is detected, the ESP32-CAM captures an image and sends it to a registered email address using the CircuitDigest Cloud platform.

---

## 🚀 Project Overview

- Detects motion using PIR sensor
- Captures image instantly
- Sends captured image via Email
- Uses secure HTTPS communication
- Wi-Fi enabled IoT-based security system

---

## 🧰 Components Required

| S.No | Component | Purpose |
|------|-----------|----------|
| 1 | ESP32-CAM | Main controller & image capture |
| 2 | PIR Sensor | Motion detection |
| 3 | Breadboard | Hardware connections |
| 4 | Jumper Wires | Interfacing |
| 5 | Red LED | Status indication |
| 6 | 220Ω Resistor | LED protection |

> **Note:** If using standard ESP32-CAM (without USB), you need a USB-to-Serial converter for programming.

---

## 🔌 Circuit Connections

- PIR OUT → GPIO13
- PIR VCC → 5V
- PIR GND → GND
- Red LED → GPIO14 (with 220Ω resistor)
- Flash LED → GPIO4 (Onboard)

---

## ⚙️ Working Principle

1. ESP32-CAM connects to Wi-Fi
2. PIR sensor continuously monitors motion
3. When motion detected:
   - Flash LED turns ON
   - Image is captured
   - Image stored in frame buffer
   - HTTPS POST request sent to cloud
4. Cloud server processes request
5. Email with image attachment is delivered

System LED indications:

- Fast blink → Initialization
- Slow blink → Monitoring mode
- Flash ON → Capturing image

---

## 💻 Code Snippet

```cpp
#define PIR_PIN 13
#define RED_LED_PIN 14
#define FLASH_LED_PIN 4

const char* ssid = "yourssidname";
const char* password = "yourpassword";

const char* host = "www.circuitdigest.cloud";
const int httpsPort = 443;
const char* apiKey = "yourapikey";

Camera configuration:

camera_config_t config;
config.pixel_format = PIXFORMAT_JPEG;
config.frame_size = FRAMESIZE_VGA;
config.jpeg_quality = 12;

if (esp_camera_init(&config) != ESP_OK) {
  Serial.println("Camera Init Failed!");
  while (1);
}

Motion detection logic:

int pirState = digitalRead(PIR_PIN);
if (pirState == HIGH) {
    captureAndSendImage();
}

Secure email transmission:

WiFiClientSecure client;
client.setInsecure();
client.connect(host, httpsPort);
client.println("POST /api/v1/email/send-with-image HTTP/1.1");
client.println("Authorization: Bearer " + String(apiKey));
client.write(fb->buf, fb->len);
🌍 Applications

🏠 Home Security Monitoring

🏢 Office Surveillance

📦 Warehouse Protection

🌾 Remote Area Monitoring

🌐 IoT Smart Alert Systems

🛠 Troubleshooting
ESP32-CAM Not Connecting to Wi-Fi

Check SSID & password

Ensure stable 5V power supply

Keep router within range

Camera Initialization Failed

Select correct board (AI Thinker ESP32-CAM)

Check camera pin configuration

Provide stable power

Image Not Captured

Reduce frame size

Check memory usage

Verify camera module

Email Not Sent

Verify API key

Check internet connection

Confirm payload format

False Motion Detection

Adjust PIR sensitivity

Avoid direct sunlight/heat sources

Increase cooldown timer

📌 Conclusion

This project provides a cost-effective smart surveillance solution using ESP32-CAM. It combines:

Motion sensing

Real-time image capture

Secure Wi-Fi communication

Cloud-based email alerts

It demonstrates how embedded systems and IoT technology can build practical real-time security applications.

❓ Frequently Asked Questions

1. Why does ESP32-CAM fail to capture images?
Usually due to insufficient power supply or incorrect configuration.

2. Why is email not being sent?
Check Wi-Fi, API key, and server endpoint configuration.

3. PIR detection range?
Typically 5–7 meters depending on model and environment.

4. Can images be stored locally?
Yes, using microSD card integration.

5. Is this suitable for real security systems?
For basic monitoring yes. Professional systems require additional security features.
