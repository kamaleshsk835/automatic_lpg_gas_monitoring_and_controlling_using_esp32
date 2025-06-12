#  Automatic LPG Gas Monitoring and Controlling using ESP32

This project demonstrates an automatic LPG (Liquefied Petroleum Gas) leak detection and control system using an **ESP32**, **MQ6 gas sensor**, and a **servo motor**. The system detects gas leaks and automatically turns a valve on or off depending on the gas concentration levels detected.

## 🚀 Features

- Real-time gas leak detection using MQ6 gas sensor
- Automatic servo motor control to shut off gas in case of a leak
- Serial monitor logging for easy debugging and monitoring
- Configurable gas threshold level

## 🧰 Required Components

| Component       | Description                            |
|-----------------|----------------------------------------|
| ESP32           | Microcontroller for processing logic   |
| MQ6 Gas Sensor  | Detects LPG/Butane/Propane gases       |
| Servo Motor     | Rotates valve to open/close position   |
| Breadboard      | For prototyping                        |
| Jumper Wires    | For connections                        |

## 🔧 How It Works

- The MQ6 sensor continuously monitors the gas concentration.
- If the gas level exceeds a preset threshold, the servo motor rotates to close the gas valve.
- When the gas level returns to safe limits, the servo motor rotates back to reopen the valve.
- System status is printed every 5 seconds via the Serial Monitor.

## 🛠️ Setup Instructions

- Connect the MQ6 sensor to ESP32's analog pin (GPIO 34).
- Connect the servo motor to digital pin (GPIO 18).
- Upload the code using Arduino IDE.
- Open the Serial Monitor (115200 baud rate) to see gas levels and system messages.

## 💻 Code
```cpp
#include <ESP32Servo.h>

#define MQ6_PIN 34             
#define THRESHOLD 1150         
#define SERVO_PIN 18           
#define PRINT_INTERVAL 5000    

Servo myServo;
int currentServoAngle = 0;
unsigned long lastPrintTime = 0;

void setup() {
  Serial.begin(115200);
  myServo.setPeriodHertz(50);
  myServo.attach(SERVO_PIN, 500, 2400);
  myServo.write(0);  // Start position
  Serial.println("Gas Sensor System Initialized...");
}

void loop() {
  int gasValue = analogRead(MQ6_PIN);
  unsigned long currentTime = millis();

  if (currentTime - lastPrintTime >= PRINT_INTERVAL) {
    lastPrintTime = currentTime;

    if (gasValue > THRESHOLD) {
      Serial.println("⚠️ Gas Detected!");

      if (currentServoAngle != 90) {
        myServo.write(90);        
        currentServoAngle = 90;
      }
    } else {
      Serial.println("No Gas Detected.");

      if (currentServoAngle != 0) {
        myServo.write(0);         
        currentServoAngle = 0;
      }
    }
  }

  delay(100); 
}


