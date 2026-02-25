📷 Motion Detection and Email Alert System Using ESP32-CAM
📌 Overview

In recent years, the demand for smart security and automated monitoring systems has increased significantly due to growing safety concerns and the need for real-time surveillance. Traditional security systems often require continuous human supervision, which can be inefficient and time-consuming.

This project demonstrates an ESP32-CAM Motion Detection System with Email Alert designed for smart surveillance applications. The system detects motion using a PIR sensor, captures an image instantly, and sends it to a registered email address via the CircuitDigest Cloud platform.

It serves as a compact, low-cost, and efficient smart security solution.

🚀 Features

📸 Motion-triggered image capture

📧 Automatic email alert with captured image

🌐 Secure HTTPS communication

🔴 LED status indication

⏱ Cooldown timer to prevent repeated triggers

☁ Cloud integration using CircuitDigest API

🛠 Components Required
S.No	Component	Purpose
1	ESP32-CAM	Main controller and image capture
2	PIR Sensor	Motion detection
3	Breadboard	Circuit connections
4	Jumper Wires	Wiring
5	Red LED	Status indication
6	220Ω Resistor	LED protection

⚠ Note: If using a normal ESP32-CAM without USB, you will need a USB-to-Serial converter for programming.

🔌 Circuit Connections

PIR Sensor

VCC → 5V

GND → GND

OUT → GPIO13

Red LED

Anode → GPIO14 (via 220Ω resistor)

Cathode → GND

Flash LED

GPIO4 (built-in)

⚙️ How It Works

The PIR sensor continuously monitors movement.

When motion is detected:

Flash LED turns ON

ESP32-CAM captures an image

The image is stored in the frame buffer.

ESP32 connects to Wi-Fi.

An HTTPS POST request is sent to CircuitDigest Cloud.

The registered user receives an email with the image attached.

💻 Code Structure

The code is divided into:

Wi-Fi Configuration

Camera Initialization

Motion Detection Logic

Image Capture Function

HTTPS Email Transmission

🔑 Network Configuration
const char* ssid = "yourssidname";
const char* password = "yourpassword";

const char* host = "www.circuitdigest.cloud";
const int httpsPort = 443;
const char* apiKey = "yourapikey";
📍 Pin Definitions
#define PIR_PIN 13
#define RED_LED_PIN 14
#define FLASH_LED_PIN 4
📸 Camera Configuration
camera_config_t config;
config.pixel_format = PIXFORMAT_JPEG;
config.frame_size = FRAMESIZE_VGA;
config.jpeg_quality = 12;

if (esp_camera_init(&config) != ESP_OK) {
  Serial.println("Camera Init Failed!");
  while (1);
}
🔁 Motion Detection Logic
int pirState = digitalRead(PIR_PIN);
if (pirState == HIGH) {
    if (currentTime - lastMotionTime > MOTION_COOLDOWN) {
        lastMotionTime = currentTime;
        captureAndSendImage();
    }
}
🌐 Sending Image via HTTPS
WiFiClientSecure client;
client.setInsecure();
client.connect(host, httpsPort);

client.println("POST /api/v1/email/send-with-image HTTP/1.1");
client.println("Authorization: Bearer " + String(apiKey));
client.write(fb->buf, fb->len);
🏠 Applications

Home Security Monitoring

Office Surveillance

Warehouse Protection

Remote Area Monitoring

IoT-Based Smart Alert Systems

🛑 Troubleshooting
❌ ESP32-CAM Not Connecting to Wi-Fi

Check SSID and password

Ensure stable Wi-Fi

Verify power supply

❌ Camera Initialization Failed

Select correct board (AI Thinker ESP32-CAM)

Check camera pin configuration

Ensure proper power supply

❌ Email Not Sent

Verify API key

Check payload formatting

Confirm internet connectivity

❌ False Motion Detection

Adjust PIR sensitivity

Avoid heat sources

Increase cooldown time

✅ Conclusion

This ESP32-CAM motion detection with photo alert system provides a cost-effective and reliable security solution. By combining PIR sensing, real-time image capture, Wi-Fi communication, and cloud integration, it demonstrates how embedded systems and IoT technologies can create a practical smart surveillance system.

It is easy to build, scalable, and ideal for learning IoT-based automation projects.

❓ Frequently Asked Questions
1. Why does the ESP32-CAM fail to capture images?

Usually due to insufficient power supply or incorrect configuration.

2. Why is my email not being sent?

Check Wi-Fi connection, API key, and payload format.

3. What is the PIR detection range?

Typically 5–7 meters depending on the model.

4. Can I store images locally?

Yes, by adding microSD card support in the code.

5. Is it suitable for professional security?

For basic monitoring yes. Advanced encryption and authentication are recommended for professional systems.
