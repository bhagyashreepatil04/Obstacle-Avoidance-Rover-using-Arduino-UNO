# 🤖 Obstacle Avoidance Rover using Arduino UNO

An autonomous obstacle avoidance rover built using an **Arduino UNO**, **L298N motor driver**, **two DC motors**, and **three HC-SR04 ultrasonic sensors**.

The rover continuously monitors the distance in front of it. When an obstacle is detected within a predefined threshold, it compares the available space on the left and right sides and automatically turns toward the side with more available space.

## 📌 Project Overview

The main objective of this project is to develop a simple autonomous robotic vehicle that can navigate through an environment without colliding with obstacles.

Three ultrasonic sensors are used:

* **Front sensor** – Detects obstacles in the rover's path.
* **Left sensor** – Measures available space on the left.
* **Right sensor** – Measures available space on the right.

The Arduino UNO processes the sensor readings and controls the motors through the L298N motor driver.

## ⚙️ Features

* Autonomous obstacle detection and avoidance
* Continuous front-distance monitoring
* Left and right distance comparison
* Automatic selection of the clearer direction
* Corner and stuck-condition handling
* Controlled turning using differential motor rotation
* PWM-based motor speed control
* Serial Monitor output for sensor readings and debugging

## 🧩 Components Required

| Component                  |    Quantity |
| -------------------------- | ----------: |
| Arduino UNO                |           1 |
| L298N Motor Driver         |           1 |
| DC Geared Motors           |           2 |
| HC-SR04 Ultrasonic Sensors |           3 |
| Robot Chassis              |           1 |
| Wheels                     |           2 |
| Caster Wheel               |           1 |
| Battery Pack               |           1 |
| Jumper Wires               | As required |

## 🔌 Pin Connections

### HC-SR04 Sensors

| Sensor | TRIG | ECHO |
| ------ | ---: | ---: |
| Front  |   D5 |   D4 |
| Left   |  D12 |  D13 |
| Right  |   D2 |   D3 |

### L298N Motor Driver

| L298N Pin | Arduino UNO |
| --------- | ----------: |
| ENA       |         D10 |
| IN1       |          D6 |
| IN2       |          D7 |
| IN3       |          D8 |
| IN4       |          D9 |
| ENB       |         D11 |

## 🧠 Working Principle

The rover continuously measures the distance in front of it.

### 1. Path is clear

If:

`Front Distance > 23 cm`

the rover moves forward.

### 2. Obstacle detected

If:

`Front Distance ≤ 23 cm`

the rover stops and measures the left and right distances.

### 3. Direction selection

If:

`Left Distance > Right Distance`

the rover turns left.

Otherwise, it turns right.

### 4. Corner handling

During a turn, the front ultrasonic sensor is continuously checked.

The rover keeps turning until the front distance becomes sufficiently clear.

A minimum turning duration and an additional turning period are used to help the rover clear corners instead of immediately switching back to forward motion.

### 5. Stuck condition

If both side distances are very small, the rover treats the situation as a possible corner/stuck condition.

It briefly reverses and then chooses the side with more available space.

## 📏 Current Parameters

```cpp
int motorSpeed = 90;
int turnSpeed = 60;

int frontThreshold = 23;
int sideThreshold = 15;
```

### Parameter Description

| Parameter        |  Value | Purpose                   |
| ---------------- | -----: | ------------------------- |
| Motor speed      | 90/255 | Forward/backward PWM      |
| Turning speed    | 60/255 | Turning PWM               |
| Front threshold  |  23 cm | Obstacle detection        |
| Side threshold   |  15 cm | Stuck/corner detection    |
| Minimum turn     | 250 ms | Prevents very small turns |
| Extra turn       | 120 ms | Helps clear corners       |
| Reverse duration | 300 ms | Escape from tight corners |

The motor speed values are **PWM values from 0–255**, not direct voltage or power ratings.

## 🔄 Control Flow

```text
                 START
                   │
                   ▼
          Read Front Distance
                   │
             ┌─────┴─────┐
             │           │
        > 23 cm       ≤ 23 cm
             │           │
             ▼           ▼
        Move Forward   Stop
                         │
                         ▼
                  Read Left & Right
                         │
              ┌──────────┴──────────┐
              │                     │
        Left > Right          Right ≥ Left
              │                     │
              ▼                     ▼
         Turn LEFT             Turn RIGHT
              │                     │
              └──────────┬──────────┘
                         │
                         ▼
                Check Front Again
                         │
                  Front Clear?
                    │       │
                   No      Yes
                    │       │
                    └───┐   ▼
                        │  Extra Turn
                        │       │
                        └───────┤
                                ▼
                         Move Forward
```

## 💻 Software

* **Arduino IDE**
* **C/C++**
* Arduino UNO board support

## 📁 Repository Structure

```text
Obstacle-Avoidance-Rover/
│
├── obstacle_avoidance_rover.ino
├── README.md
└── images/
    └── rover.jpg
```

## 🚀 How to Run

1. Assemble the rover according to the pin connections.
2. Connect the Arduino UNO to the computer.
3. Open `obstacle_avoidance_rover.ino` in Arduino IDE.
4. Select **Arduino UNO** under Board.
5. Select the appropriate COM port.
6. Upload the program.
7. Open the Serial Monitor at **9600 baud** to observe sensor readings.
8. Power the motors using the appropriate external motor supply.
9. Place the rover in an obstacle course and test its navigation.

## ⚠️ Notes

* The Arduino and motors should have an appropriate power arrangement; avoid powering the motors directly from the Arduino 5V pin.
* HC-SR04 sensors should be positioned so their fields of view do not excessively interfere with each other.
* Motor speed and turning parameters may need adjustment depending on the motor, battery, chassis, wheel size, and floor surface.
* Ultrasonic sensors can occasionally produce noisy readings, so practical testing and calibration are important.

## 🔮 Possible Future Improvements

* Add wheel encoders for more accurate movement.
* Implement sensor filtering to reduce ultrasonic noise.
* Add PID-based motor control.
* Add Bluetooth/Wi-Fi monitoring.
* Add an OLED display for real-time sensor readings.
* Implement mapping and path planning.
* Add a rechargeable battery management system.
* Improve corner detection using combined front and side sensor data.

## 👩‍💻 Project

**Obstacle Avoidance Rover**

Built with **Arduino UNO + L298N + 3× HC-SR04 + 2 DC Motors**.

The project demonstrates basic **embedded systems, robotics, ultrasonic sensing, PWM motor control, and autonomous navigation**.
