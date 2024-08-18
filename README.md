# Temperature and Humidity Monitoring with NodeMCU and Firebase

This project involves monitoring temperature and humidity using a DHT11 sensor and NodeMCU (ESP8266). The data is sent to Firebase Realtime Database, where it can be accessed remotely. The project also includes controlling an LED via Firebase, making it a useful example of IoT applications.

## Table of Contents
- [Hardware Requirements](#hardware-requirements)
- [Software Requirements](#software-requirements)
- [Circuit Connections](#circuit-connections)
- [Firebase Setup](#firebase-setup)
- [Code Explanation](#code-explanation)
- [Usage Instructions](#usage-instructions)
- [Troubleshooting](#troubleshooting)

## Hardware Requirements
- **NodeMCU (ESP8266) x 1**
- **DHT11 Sensor x 1**
- **LED x 1** (or any output device)
- **Resistor (220 ohm) x 1** (for the LED)
- **Connecting Wires**

## Software Requirements
- **Arduino IDE**: [Download and Install Arduino IDE](https://www.arduino.cc/en/software)
- **Firebase Library for Arduino**: [Firebase ESP Client](https://github.com/mobizt/Firebase-ESP-Client)
- **DHT Sensor Library**: [DHT Sensor Library for Arduino](https://github.com/adafruit/DHT-sensor-library)

## Circuit Connections

### DHT11 Sensor to NodeMCU:
- **VCC** → 3V3 (3.3V) on NodeMCU
- **Signal (S)** → D1 on NodeMCU
- **GND** → GND on NodeMCU

### LED to NodeMCU:
- **VCC** → 3V3 (3.3V) on NodeMCU
- **LED Pin** → D0 on NodeMCU (through a 220-ohm resistor)
- **GND** → GND on NodeMCU

## Firebase Setup

1. **Create a Firebase Project:**
   - Go to the [Firebase Console](https://console.firebase.google.com/).
   - Click on "Add Project" and follow the on-screen instructions.
   - Once created, go to the Project Settings to get your **API Key** and **Database URL**.

2. **Enable Realtime Database:**
   - In your Firebase console, navigate to the "Realtime Database" section.
   - Click on "Create Database" and choose a suitable location.
   - Set the database rules to:
     ```json
     {
       "rules": {
         ".read": "auth != null",
         ".write": "auth != null"
       }
     }
     ```

3. **Install the Firebase Library in Arduino IDE:**
   - Open the Arduino IDE.
   - Go to **Sketch** → **Include Library** → **Manage Libraries**.
   - Search for "Firebase ESP Client" and install it.

## Code Explanation

This project consists of reading temperature and humidity from a DHT11 sensor and sending the data to Firebase. It also checks Firebase for commands to control an LED.

### Key Parts of the Code:

- **WiFi Connection:**
  ```cpp
  WiFi.begin(WIFI_SSID, WIFI_PASSWORD);
  while (WiFi.status() != WL_CONNECTED) {
    delay(300);
  }

# Firebase and DHT11 Sensor Integration with NodeMCU

## Overview

This project demonstrates how to use a NodeMCU (ESP8266) with a DHT11 sensor to read temperature and humidity data, send it to Firebase Realtime Database, and control an LED based on data stored in Firebase.

## Components Required

- NodeMCU (ESP8266)
- DHT11 Sensor
- LED
- Resistor (for LED)
- Breadboard and Jumper Wires

### Firebase Initialization

To set up Firebase in your NodeMCU project, use the following code:

```cpp
config.api_key = API_KEY;
config.database_url = DATABASE_URL;
Firebase.begin(&config, &auth);
Firebase.reconnectWiFi(true);
```

## DHT11 Sensor with Firebase and LED Control

### Overview

This project uses a DHT11 sensor to read temperature and humidity data, which is then sent to Firebase. Additionally, it allows for controlling an LED based on a value set in Firebase.

### Code

### DHT11 Sensor Reading

```cpp
float humidity = dht.getHumidity();
float temperature = dht.getTemperature();
```

### Sending Data to Firebase
```cpp
Firebase.RTDB.setInt(&fbdo, "mainbucket/temp", temperature);
Firebase.RTDB.setInt(&fbdo, "mainbucket/humid", humidity);
```

### LED Control via Firebase
```cpp
if (Firebase.RTDB.getString(&fbdo, "/mainbucket/bulb")) {
  if (fbdo.stringData() == "1") {
    digitalWrite(lpin, HIGH);
  } else {
    digitalWrite(lpin, LOW);
  }
}
```

### Upload the Code
- Open the Arduino IDE and load the provided code.

- Update the following with your credentials:

```cpp
#define WIFI_SSID "your_wifi_ssid"
#define WIFI_PASSWORD "your_wifi_password"
#define API_KEY "your_firebase_api_key"
#define DATABASE_URL "your_firebase_database_url"
```
- Select the correct board (NodeMCU) and port.

- Upload the code to the NodeMCU.

### Power the NodeMCU
- Connect the NodeMCU to a power source.

- It will connect to your Wi-Fi and start sending data to Firebase.

### Monitor the Data
- In your Firebase console, you can monitor the temperature and humidity data.
- You can also control the LED by updating the bulb value in Firebase.

## Troubleshooting
### Wi-Fi Connection Issues
- Ensure your Wi-Fi credentials are correct.
- Check if the NodeMCU is within range of the Wi-Fi network.
### Firebase Data Not Updating
- Ensure your Firebase API key and database URL are correctly entered.
- Check the Firebase rules to ensure write access is allowed.
### License
- This project is licensed under the MIT License. See the LICENSE file for details.

Save the above content in a file with a `.md` extension, for example, `README.md`. This file will provide clear instructions and code examples for users of your project.
