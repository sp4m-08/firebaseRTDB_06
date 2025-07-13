<img width="1526" height="985" alt="image" src="https://github.com/user-attachments/assets/379b6f10-e1e3-4b27-ba16-a755ed8de878" /># Smart IoT Room Monitoring System

A comprehensive smart room monitoring system built for ESP32 that collects environmental data and streams it to Firebase Realtime Database in real-time.

## Dashboard
-`https://rms-frontend-peach.vercel.app/`

## 🌟 Features

- **Smart Room Monitoring**: Real-time tracking of room conditions including temperature, humidity, lighting, and air quality
- **Real-time Data Streaming**: Automatic data upload to Firebase every 3 seconds
- **Firebase Integration**: Secure authentication and real-time database connectivity
- **Robust Error Handling**: Built-in error detection and logging
- **Wi-Fi Connectivity**: Automatic connection management with retry logic

## Web Dashboard Repository
-`https://github.com/tanishka3001/RMS-Frontend`

## 📋 Hardware Requirements

### Microcontroller

- ESP32 development board

### Sensors

- **DHT11**: Temperature and humidity sensor (Pin 4)
- **Flame Sensor**: Fire detection sensor (Pin 5)
- **LDR (Light Dependent Resistor)**: Light sensor (Pin 18)
- **MQ135**: Air quality/gas sensor (Pin 34)

### Connections

```
DHT11 → Pin 4
Flame Sensor → Pin 5 (Digital)
LDR → Pin 18 (Digital)
MQ135 → Pin 34 (Analog)
```

## 🔧 Software Requirements

### Libraries

Install these libraries through Arduino IDE Library Manager:

- `WiFi` (ESP32 built-in)
- `WiFiClientSecure` (ESP32 built-in)
- `FirebaseClient`
- `DHT sensor library`

### Platform

- Arduino IDE 1.8+ or PlatformIO
- ESP32 board package

## ⚙️ Configuration

### 1. Wi-Fi Setup

Update your Wi-Fi credentials:

```cpp
#define WIFI_SSID "Your_WiFi_Network"
#define WIFI_PASSWORD "Your_WiFi_Password"
```

### 2. Firebase Setup

Configure your Firebase project credentials:

```cpp
#define API_KEY "your_firebase_api_key"
#define USER_EMAIL "your_firebase_auth_email"
#define USER_PASSWORD "your_firebase_auth_password"
#define DATABASE_URL "your_firebase_realtime_database_url"
```

### 3. Firebase Project Setup

1. Create a new Firebase project at [Firebase Console](https://console.firebase.google.com/)
2. Enable **Authentication** with Email/Password
3. Create a **Realtime Database**
4. Set database rules (for development):
   ```json
   {
     "rules": {
       ".read": "auth != null",
       ".write": "auth != null"
     }
   }
   ```
5. Get your project's API key and database URL

## 📊 Data Structure

The system uploads room monitoring data to Firebase with the following structure:

```
/iot/device/
├── temperature: (float) Temperature in Celsius
├── humidity: (float) Humidity percentage
├── flame: (string) "Flame!" or "No flame :D"
├── ldr: (string) "Light" or "Dark"
└── gas: (int) Gas sensor analog value (0-4095)
```

## 🚀 Installation & Usage

### 1. Hardware Setup

1. Connect all sensors to your ESP32 according to the pin configuration
2. Ensure proper power supply (3.3V/5V depending on sensor requirements)
3. Double-check all connections

### 2. Software Installation

1. Install required libraries in Arduino IDE
2. Update configuration parameters in the code
3. Select your ESP32 board and COM port
4. Upload the code to your ESP32

### 3. Monitoring

1. Open Serial Monitor (115200 baud rate)
2. Watch for successful Wi-Fi connection and Firebase initialization
3. Monitor real-time sensor readings and Firebase upload status
4. Check your Firebase Realtime Database for incoming data

## 📈 Serial Output Example

```
=== Setup Start ===
[WiFi] Connecting to TESTESP..........
[WiFi] Connected! IP: 192.168.1.100
[Firebase] Initializing app...
[Firebase] Waiting for app to be ready...
[Firebase] App is ready!
[Firebase] Binding Realtime Database...

[loop] Reading sensors and sending data...
[DHT] Temp: 25.30 °C, Humidity: 60.20 %
[Flame] Value: 1
[LDR] Value: 0
[Gas MQ135] Analog Value: 1205
[processData] Success (SetTemp):
[processData] Success (SetHumidity):
```

## 🔧 Troubleshooting

### Common Issues

**Wi-Fi Connection Failed**

- Verify SSID and password
- Check signal strength
- Ensure ESP32 is within range

**Firebase Authentication Error**

- Verify API key and credentials
- Check internet connection
- Ensure Firebase project is properly configured

**Sensor Reading Errors**

- Check sensor connections
- Verify power supply
- Test individual sensors

**Upload Failures**

- Monitor serial output for error messages
- Verify Firebase database rules
- Check network stability

**Firebase Authentication Issues**

- Running the code multiple times may cause authentication issues with the Firebase SDK
- Use Postman to debug user login separately:
  - Method: POST
  - URL: `https://identitytoolkit.googleapis.com/v1/accounts:signInWithPassword?key=your_api_key`
  - Body (JSON):
    ```json
    {
      "email": "your_email@gmail.com",
      "password": "your_password",
      "returnSecureToken": true
    }
    ```
- If authentication works via Postman but fails in code, try power cycling the ESP32

## 📝 Customization

### Changing Upload Interval

Modify the `sendInterval` variable:

```cpp
const unsigned long sendInterval = 5000;  // 5 seconds
```

### Adding New Sensors

1. Define new sensor pins
2. Add sensor initialization in `setup()`
3. Add sensor reading in `loop()`
4. Add Firebase upload commands

### Modifying Data Paths

Change the Firebase paths in the upload section:

```cpp
Database.set<float>(async_db, "/your/custom/path", value, processData, "TaskName");
```

## 🔒 Security Notes

- Change default Firebase credentials before deployment
- Use environment variables for sensitive data in production
- Consider implementing proper SSL certificate validation
- Set appropriate Firebase security rules

## 🤝 Contributing

Feel free to submit issues, fork the repository, and create pull requests for improvements.

## 📞 Support

For issues and questions:

1. Check the troubleshooting section
2. Review Firebase documentation [here](https://github.com/mobizt/FirebaseClient)
3. Check sensor datasheets for proper connections
