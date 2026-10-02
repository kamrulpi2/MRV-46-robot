
# 🤖 MRV-46 — Multi-Role Vehicle

> ESP32-based Wi-Fi controlled robotic vehicle with BTS7960 motor control, robotic arm, metal detector and camera control.
> 

---

## 🚀 Project Overview

**MRV-46** is a multi-role robotic vehicle developed using an **ESP32**.

The robot is controlled wirelessly through a mobile phone or computer using the ESP32's built-in Wi-Fi Access Point.

The system provides:

- 🚗 Forward / Backward movement
- ◀️ Left / Right movement
- ⚡ Variable motor speed
- 🦾 Robotic arm control
- 🤏 Gripper control
- 🔩 Elbow control
- 🦿 Shoulder control
- 🔎 Metal Detector ON/OFF
- 📷 Camera ON/OFF
- 📱 Mobile-friendly web control panel
- 📶 Standalone Wi-Fi Access Point
- 🎯 Smooth servo movement
- 🛑 Emergency Stop button

---

# 📌 Robot Name

## MRV-46

**MRV = Multi-Role Vehicle**

MRV-46 is designed as a compact robotic platform that can be adapted for different field applications.

---

# 🧠 System Architecture

```text
                    ┌─────────────────────┐
                    │     MOBILE / PC     │
                    │    Web Browser      │
                    └──────────┬──────────┘
                               │
                               │ Wi-Fi
                               │
                    ┌──────────▼──────────┐
                    │       ESP32         │
                    │   MRV-46 Controller │
                    └──────┬───────┬──────┘
                           │       │
              ┌────────────┘       └─────────────┐
              │                                  │
      ┌───────▼────────┐                 ┌───────▼────────┐
      │   BTS7960      │                 │    3× Servo    │
      │ Motor Driver   │                 │   Robot Arm    │
      └───────┬────────┘                 └────────────────┘
              │
      ┌───────▼────────┐
      │   DC Motors    │
      │  Left + Right  │
      └────────────────┘

                    ESP32
                      │
              ┌───────┴────────┐
              │                │
       ┌──────▼──────┐  ┌──────▼──────┐
       │ Metal       │  │   Camera    │
       │ Detector    │  │   Relay     │
       └─────────────┘  └─────────────┘
````

---

# 🔧 Hardware Components

| Component            |                         Quantity |
| -------------------- | -------------------------------: |
| ESP32 DevKit         |                                1 |
| BTS7960 Motor Driver | 2 / required motor configuration |
| DC Motors            |             According to chassis |
| Servo Motor          |                                3 |
| Relay Module         |                                2 |
| Metal Detector       |                                1 |
| Camera               |                                1 |
| DC-DC Buck Converter |                                1 |
| Robot Chassis        |                                1 |
| Battery              |                                1 |
| Jumper Wires         |                         Required |

---

# 📍 ESP32 Pin Map

## Motor Driver

| ESP32 GPIO | Function   |
| ---------: | ---------- |
|    GPIO 16 | Left RPWM  |
|    GPIO 17 | Left LPWM  |
|    GPIO 18 | Right RPWM |
|    GPIO 19 | Right LPWM |

### Motor Control

```text
GPIO 16 → LEFT_RPWM
GPIO 17 → LEFT_LPWM

GPIO 18 → RIGHT_RPWM
GPIO 19 → RIGHT_LPWM
```

---

# 🦾 Robot Arm Servo Map

The MRV-46 uses three servo motors.

| Servo   | Function |    GPIO |    Range |
| ------- | -------- | ------: | -------: |
| Servo 2 | Gripper  | GPIO 26 | 83°–180° |
| Servo 3 | Elbow    | GPIO 32 | 72°–180° |
| Servo 4 | Shoulder | GPIO 33 |  0°–130° |

### Gripper

```text
GPIO 26
Range: 83° → 180°
```

### Elbow

```text
GPIO 32
Range: 72° → 180°
```

### Shoulder

```text
GPIO 33
Range: 0° → 130°
```

The web interface controls the servo position with a slider.

The ESP32 uses a smooth target-position system where the servo moves toward the selected angle step-by-step.

---

# 🔌 Relay Map

| Relay   | Device         |    GPIO |
| ------- | -------------- | ------: |
| Relay 1 | Metal Detector | GPIO 13 |
| Relay 2 | Camera         | GPIO 14 |

### Relay 1

```text
GPIO 13 → Metal Detector
```

### Relay 2

```text
GPIO 14 → Camera
```

The web interface displays:

```text
🟢 ON
🔴 OFF
```

---

# 🗺️ Complete Pin Map

```text
                    MRV-46 ESP32
                  ┌───────────────┐
                  │               │
GPIO 16 ──────────┤ Left RPWM     │
GPIO 17 ──────────┤ Left LPWM     │
GPIO 18 ──────────┤ Right RPWM    │
GPIO 19 ──────────┤ Right LPWM    │
                  │               │
GPIO 26 ──────────┤ Gripper       │
GPIO 32 ──────────┤ Elbow         │
GPIO 33 ──────────┤ Shoulder      │
                  │               │
GPIO 13 ──────────┤ Metal Detector│
GPIO 14 ──────────┤ Camera        │
                  │               │
                  └───────────────┘
```

---

# ⚡ Power Wiring

## Important

Do **not** power the motors and multiple servos directly from the ESP32.

Use an appropriate external power supply.

Recommended architecture:

```text
              BATTERY
                 │
        ┌────────┴────────┐
        │                 │
        ▼                 ▼
   BTS7960 Power     DC-DC Buck
        │                 │
        ▼                 ▼
     Motors          Servo Power
                          │
                          ▼
                       Servos

                    Buck Converter
                          │
                          ▼
                       ESP32 VIN
```

### Common Ground

All components must share a common ground:

```text
ESP32 GND
   │
   ├── BTS7960 GND
   ├── Servo GND
   ├── Relay GND
   └── Power Supply GND
```

> ⚠️ Make sure the servo and motor power supply can provide enough current.

---

# 📶 Wi-Fi Configuration

MRV-46 creates its own Wi-Fi network.

### Wi-Fi Name

```text
MRV-46
```

### Password

```text
12345678
```

### IP Address

```text
192.168.4.1
```

---

# 📱 How to Control

## Step 1

Upload the Arduino code to the ESP32.

## Step 2

Power on the MRV-46.

## Step 3

Open Wi-Fi settings on your phone or laptop.

Connect to:

```text
MRV-46
```

Password:

```text
12345678
```

## Step 4

Open a browser.

Go to:

```text
http://192.168.4.1
```

The MRV-46 control panel will appear.

---

# 🎮 Motion Control

The web controller provides:

```text
              ▲
              │
             FORWARD

       ◀       STOP       ▶

              ▼
             BACKWARD
```

### Commands

| Command | Function |
| ------- | -------- |
| F       | Forward  |
| B       | Backward |
| L       | Left     |
| R       | Right    |
| S       | Stop     |

Diagonal movement commands were intentionally removed from the final MRV-46 version.

---

# ⚡ Speed Control

Motor speed can be controlled using the slider.

Range:

```text
0 ─────────────── 255
```

Example:

```text
0   = Stop
100 = Low speed
180 = Medium/High speed
255 = Maximum PWM value
```

---

# 🦾 Servo Control

## Gripper

```text
83° ───────────────── 180°
```

## Elbow

```text
72° ───────────────── 180°
```

## Shoulder

```text
0° ────────────────── 130°
```

The slider shows the current angle.

Example:

```text
GRIPPER

83° ─────────────●──────── 180°
                 125°
```

---

# 🔎 Metal Detector

Relay 1 controls the metal detector.

```text
ESP32 GPIO 13
      │
      ▼
Relay 1
      │
      ▼
Metal Detector
```

Web interface:

```text
Metal Detector

● OFF
```

or

```text
● ON
```

### Status Colors

```text
GREEN → ON
RED   → OFF
```

---

# 📷 Camera

Relay 2 controls the camera power.

```text
ESP32 GPIO 14
      │
      ▼
Relay 2
      │
      ▼
Camera
```

Web interface:

```text
Camera

● OFF
```

or

```text
● ON
```

### Status Colors

```text
GREEN → ON
RED   → OFF
```

---

# 💻 Software

## Required Software

* Arduino IDE
* ESP32 Board Package
* ESP32Servo Library

### Libraries

```cpp
#include <WiFi.h>
#include <WebServer.h>
#include <ESP32Servo.h>
```

---

# 📁 Recommended GitHub Structure

```text
MRV-46/
│
├── MRV-46.ino
│
├── README.md
│
├── docs/
│   ├── pin-map.png
│   ├── wiring-diagram.png
│   └── system-architecture.png
│
├── images/
│   ├── mrv46-front.jpg
│   ├── mrv46-side.jpg
│   └── controller.jpg
│
└── LICENSE
```

---

# 🛠️ Features

### Vehicle

* [x] Forward
* [x] Backward
* [x] Left
* [x] Right
* [x] Stop
* [x] Speed control

### Robotic Arm

* [x] Gripper
* [x] Elbow
* [x] Shoulder
* [x] Angle slider
* [x] Servo angle limits
* [x] Smooth servo movement

### Auxiliary

* [x] Metal Detector control
* [x] Camera control
* [x] ON/OFF status
* [x] Green/Red status indicator

### Communication

* [x] ESP32 Wi-Fi Access Point
* [x] Mobile browser control
* [x] PC browser control
* [x] No external internet required

---

# 🔐 Safety

> ⚠️ Always test the robot with the wheels lifted from the ground first.

> ⚠️ Keep hands away from moving wheels and the robotic arm.

> ⚠️ Use separate suitable power supplies for motors and servos.

> ⚠️ Always connect all grounds together.

> ⚠️ Check servo mechanical limits before operating the arm.

> ⚠️ Do not exceed the voltage/current rating of the relay, servo, motor driver, or camera.

---

# 🔮 Future Development

Possible future upgrades:

* [ ] GPS navigation
* [ ] Live camera streaming
* [ ] Obstacle detection
* [ ] Ultrasonic/LiDAR
* [ ] Autonomous navigation
* [ ] Metal detection data logging
* [ ] Firebase integration
* [ ] Mobile application
* [ ] Robotic arm inverse kinematics
* [ ] AI-based object detection
* [ ] Remote internet control
* [ ] Battery monitoring
* [ ] ROS integration

---
