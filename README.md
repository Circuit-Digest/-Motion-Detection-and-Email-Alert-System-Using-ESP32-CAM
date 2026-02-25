📷 Motion Detection and Email Alert System Using ESP32-CAM
🚀 Project Overview

This project demonstrates a smart motion detection and email alert system using the ESP32-CAM and a PIR sensor.

When motion is detected, the ESP32-CAM captures an image and sends it to a registered email address through CircuitDigest Cloud. The system works automatically without continuous manual monitoring and provides a low-cost IoT-based security solution.

🎯 Features

Motion detection using PIR sensor

Automatic image capture on motion

Email alert with captured image

Secure HTTPS communication

LED status indication

Cooldown timer to prevent repeated triggers

Compact and cost-effective surveillance system

🛠️ Components Required
S.No	Component	Quantity	Purpose
1	ESP32-CAM	1	Main controller and image capture
2	PIR Sensor	1	Motion detection
3	Red LED	1	Status indication
4	220 Ohm Resistor	1	LED protection
5	Breadboard	1	Circuit setup
6	Jumper Wires	As required	Electrical connections
7	USB-to-Serial Converter (if required)	1	Programming ESP32-CAM
⚙️ Working Principle

The PIR sensor continuously monitors the surroundings.

When motion is detected, it sends a HIGH signal to GPIO13.

The ESP32-CAM activates the flash LED and captures an image.

The image is temporarily stored in the frame buffer.

The ESP32-CAM connects to Wi-Fi.

A secure HTTPS POST request is sent to CircuitDigest Cloud.

The server processes the request and sends the captured image to the registered email address.

🔌 Pin Configuration
#define PIR_PIN 13
#define RED_LED_PIN 14
#define FLASH_LED_PIN 4
🌐 Wi-Fi and Server Configuration
const char* ssid = "yourssidname";
const char* password = "yourpassword";

const char* host = "www.circuitdigest.cloud";
const int httpsPort = 443;
const char* apiKey = "yourapikey";
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
📡 Sending Image via HTTPS
WiFiClientSecure client;
client.setInsecure();
client.connect(host, httpsPort);

client.println("POST /api/v1/email/send-with-image HTTP/1.1");
client.println("Authorization: Bearer " + String(apiKey));
client.write(fb->buf, fb->len);
📡 Applications

Home Security Monitoring

Office Surveillance

Warehouse Protection

Remote Area Monitoring

IoT-Based Smart Alert Systems

🔧 Troubleshooting
ESP32-CAM Not Connecting to Wi-Fi

Check SSID and password

Ensure stable Wi-Fi signal

Provide stable 5V power supply

Camera Initialization Failed

Select AI Thinker ESP32-CAM in Arduino IDE

Verify camera pin configuration

Ensure sufficient current supply

Image Not Captured

Check camera module connections

Reduce frame size if memory issue occurs

Monitor Serial output for debugging

Email Not Sent

Verify API key

Ensure correct payload formatting

Check internet connectivity

False Motion Detection

Adjust PIR sensitivity

Avoid placing near heat sources

Increase cooldown time in code

🔮 Future Enhancements

Add microSD card image storage

Add buzzer alarm system

Real-time video streaming

Mobile app integration

Multi-camera support

📚 Learning Outcomes

After completing this project, you will understand:

ESP32-CAM configuration

PIR sensor working principle

HTTPS communication using WiFiClientSecure

Sending image data using HTTP POST

IoT cloud integration

Motion-based automation systems

📌 Conclusion

This ESP32-CAM motion detection and email alert system provides a simple and efficient smart security solution. By integrating PIR sensing, image capture, Wi-Fi communication, and cloud services, the system enables real-time remote monitoring without requiring complex infrastructure.

It is suitable for home, office, and industrial applications.

Author: Vedhathiri K.
