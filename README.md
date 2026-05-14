# 🎮 Gesture-Controlled Game Controller

A motion-based game controller that converts physical hand movements into real-time 3D character control inside Unity. Built with an MPU6050 sensor and Arduino Nano — no joystick, no keyboard, just tilt.

---

## 📽️ How It Works

Tilt the device → Arduino detects the angle → sends a command over USB → Unity moves the character.

```
MPU6050 → Arduino Nano → USB Serial → Unity (SerialPort) → 3D Character
```

| Gesture | Command Sent | In-Game Effect |
|---|---|---|
| Tilt left | `LEFT` | Character walks left |
| Tilt right | `RIGHT` | Character walks right |
| Tilt forward | `FORWARD` | Character walks forward |
| Tilt backward | `BACK` | Character walks backward |
| Hold still | `IDLE` | Character stops |

---

## 🔧 Hardware

| Component | Purpose |
|---|---|
| Arduino Nano | Reads sensor data, sends commands over USB |
| MPU6050 GY-521 | Measures tilt angle (gyroscope + accelerometer) |
| Breadboard | Holds connections without soldering |
| Jumper cables | Connect sensor to Arduino |
| USB cable | Power + data link to PC |

### Wiring

| MPU6050 Pin | Arduino Nano Pin |
|---|---|
| VCC | 5V |
| GND | GND |
| SCL | A5 |
| SDA | A4 |

---

## 💻 Software

### Arduino
- Library: [MPU6050_light](https://github.com/rfetick/MPU6050_light) by rfetick
- Reads roll and pitch angles every 50ms
- Sends `LEFT`, `RIGHT`, `FORWARD`, `BACK`, or `IDLE` over Serial at 9600 baud

### Unity
- **SerialController.cs** — Opens the COM port, reads incoming commands on a background thread
- **PlayerController.cs** — Reads the latest command and moves the character accordingly
- Character: [Robot Kyle URP](https://assetstore.unity.com/packages/3d/characters/robots/robot-kyle-urp-4696) (free Unity Asset Store)

---

## 🚀 Setup

### Arduino Side
1. Install the MPU6050_light library: `Sketch → Include Library → Manage Libraries → MPU6050_light`
2. Upload `sketch_may8a.ino` to the Arduino Nano
3. Open Serial Monitor and verify `LEFT`, `RIGHT`, `IDLE` etc. are printed when tilting
4. **Close Arduino IDE and Serial Monitor** before running Unity

### Unity Side
1. Open the project in Unity
2. Go to `Edit → Project Settings → Player → Other Settings`
3. Set **Api Compatibility Level** to `.NET Framework`
4. Plug in the Arduino via USB
5. Check Device Manager for the COM port number (e.g. `COM4`)
6. Select `SerialManager` in the Hierarchy → set `portName` to your COM number
7. Press **Play**

---

## 📁 Project Structure

```
Assets/
├── Scripts/
│   ├── SerialController.cs    # Handles USB Serial communication
│   └── PlayerController.cs   # Moves the character based on commands
├── Scenes/
│   └── SampleScene.unity
└── Robot Kyle URP/            # Character asset (imported from Asset Store)
```

---

## ⚠️ Notes

- Windows only — `System.IO.Ports.SerialPort` is not supported on all platforms
- Arduino IDE and Unity **cannot use the same COM port simultaneously** — always close Arduino IDE before pressing Play
- Tilt sensitivity threshold is set to 15° — adjustable in the Arduino code

---

## ⚙️ Known Limitations
- No wireless communication — USB cable required at all times
- No gesture smoothing or filtering — raw angle values are used directly
- Single-player only
- Uses threshold-based input instead of continuous analog movement

---

## 📚 Built With

- [Arduino IDE](https://www.arduino.cc/en/software)
- [Unity 6](https://unity.com/)
- [MPU6050_light Library](https://github.com/rfetick/MPU6050_light)
- [Robot Kyle URP — Unity Asset Store](https://assetstore.unity.com/packages/3d/characters/robots/robot-kyle-urp-4696)

---

## 📹 Demo

![Demo](media/demo.mp4)

https://github.com/user-attachments/assets/0f3273d9-c59d-4467-88b4-2fa524457f1c




*This project was built as a mandatory assignment for the Introduction to Embedded Systems course. My primary focus is game development — this repo demonstrates cross-domain work combining embedded hardware with Unity.*
