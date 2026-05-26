# Bluetooth Smart Car with Obstacle Avoidance Capabilities - Public Overview

Public technical overview of an Arduino/C++ Bluetooth smart car controlled with a PlayStation 5 controller. The project includes manual driving, analog acceleration, steering control, headlights, safety behavior, and an autonomous obstacle-avoidance mode using an ultrasonic sensor.

<p align="center">
  <img src="assets/smart-car-with-ps5-controller.jpg" width="520" alt="Bluetooth smart car with PlayStation 5 controller" />
</p>

## Demo Videos

### Manual Bluetooth Control

https://github.com/user-attachments/assets/1adfdc49-4890-4309-a1f3-69dc6789783e

### Obstacle Avoidance Mode

https://github.com/user-attachments/assets/57011fe0-489b-4c29-a6c8-5f0f7d069c5e
> The full source code is private due to intellectual property considerations. This repository documents the project’s functionality, architecture, technologies, hardware setup, demo media, and implementation approach without exposing private source code, generated files, or internal Git history.

---

## Project Summary

This project presents a Bluetooth-controlled smart car built around an Arduino Uno and the ATmega328P microcontroller. The car can be manually controlled with a PlayStation 5 controller and can also switch into an autonomous obstacle-avoidance mode.

The system combines embedded C++ programming, wireless controller communication, motor control, steering control, ultrasonic distance sensing, LED control, and a 3D-printed chassis into a compact smart-car prototype.

---

## Core Functionality

### Manual Bluetooth Control

The car is controlled through a PlayStation 5 controller connected over Bluetooth. The controller communicates with the Arduino system through a USB Bluetooth receiver and a USB Host Shield.

The manual driving mode supports:

- forward movement;
- reverse movement;
- analog acceleration through trigger pressure;
- left/right steering through the left stick;
- headlights on/off;
- automatic mode activation;
- automatic mode deactivation;
- controller connect/disconnect behavior.

### Obstacle Avoidance Mode

The autonomous mode uses an HC-SR04 ultrasonic sensor to detect obstacles in front of the car. When the mode is active, the car moves forward, checks for obstacles, and decides how to reposition itself when an object is detected.

<p align="center">
  <img src="assets/obstacle-avoidance-decision-graph.png" width="360" alt="Obstacle avoidance decision graph" />
</p>

The decision flow is based on these behaviors:

1. Move forward.
2. Measure the distance to the object ahead.
3. If an obstacle is detected, slowly reposition toward one side.
4. Check the new direction.
5. If another obstacle is detected, reposition in the opposite direction.
6. If both directions are blocked, turn around.

<p align="center">
  <img src="assets/obstacle-avoidance-development.jpg" width="420" alt="Obstacle avoidance development sequence" />
</p>

### Headlights

Two LEDs simulate car headlights. They are controlled directly from the PS5 controller, giving the user a more realistic driving experience.

### Safety Behavior

The implementation includes several safety-oriented behaviors:

- the car remains stationary when no movement command is received;
- the steering servo returns to a straight position when no steering command is active;
- controller disconnection can stop the car;
- an emergency-stop style behavior halts control commands.

---

## Technologies Used

### Programming & Embedded Control

- C++
- Arduino IDE
- Arduino Uno
- ATmega328P microcontroller
- PWM motor and servo control
- timer-based control logic
- USB Host Shield communication
- Bluetooth HID controller input

### Hardware Components

- Arduino Uno development board
- PlayStation 5 controller
- USB Host Shield
- USB Bluetooth receiver
- Gravity IO Expansion Shield
- L298N dual H-bridge motor driver
- 2x DC motors, 3–6V
- MG90S servo motor
- HC-SR04 ultrasonic sensor
- 2x LEDs for headlights
- 3x switches
- 3x battery holders
- 3D-printed chassis
- screws, nuts, bolts, bearings, and 3D-printed spacers

---

## Component Gallery

<table>
  <tr>
    <td align="center"><img src="assets/arduino-uno.png" width="120" alt="Arduino Uno" /><br/>Arduino Uno</td>
    <td align="center"><img src="assets/ps5-controller.png" width="130" alt="PlayStation 5 controller" /><br/>PS5 Controller</td>
    <td align="center"><img src="assets/usb-host-shield.png" width="120" alt="USB Host Shield" /><br/>USB Host Shield</td>
    <td align="center"><img src="assets/usb-bluetooth-receiver.png" width="90" alt="USB Bluetooth receiver" /><br/>Bluetooth Receiver</td>
  </tr>
  <tr>
    <td align="center"><img src="assets/gravity-io-expansion-shield.png" width="130" alt="Gravity IO Expansion Shield" /><br/>Gravity IO Shield</td>
    <td align="center"><img src="assets/l298n-motor-driver.png" width="120" alt="L298N motor driver" /><br/>L298N Motor Driver</td>
    <td align="center"><img src="assets/dc-motor-3-6v.png" width="130" alt="DC motor 3-6V" /><br/>DC Motor</td>
    <td align="center"><img src="assets/mg90s-servo-motor.png" width="120" alt="MG90S servo motor" /><br/>MG90S Servo</td>
  </tr>
  <tr>
    <td align="center" colspan="4"><img src="assets/hc-sr04-ultrasonic-sensor.png" width="150" alt="HC-SR04 ultrasonic sensor" /><br/>HC-SR04 Ultrasonic Sensor</td>
  </tr>
</table>

---

## 3D-Printed Chassis and Assembly

The chassis was 3D-printed and designed to hold the Arduino board, shields, motor driver, battery holders, steering system, ultrasonic sensor, and other components. The layout was chosen to make wiring and component replacement easier.

<p align="center">
  <img src="assets/chassis-main-part.png" width="300" alt="Main 3D printed chassis part" />
</p>

Additional printed parts were used for the steering system and structural mounting points.

<p align="center">
  <img src="assets/chassis-steering-link.png" width="220" alt="3D printed steering link" />
  <img src="assets/chassis-top-plate.png" width="220" alt="3D printed chassis top plate" />
</p>

The Arduino Uno is mounted on the chassis, with the USB Host Shield and Gravity IO Expansion Shield stacked above it. The front section contains the steering system and ultrasonic sensor.

<p align="center">
  <img src="assets/chassis-assembly-arduino-stack.jpg" width="320" alt="Arduino stack mounted on chassis" />
  <img src="assets/chassis-assembly-front-steering.jpg" width="320" alt="Front steering assembly" />
</p>

---

## Controller Mapping

The PlayStation 5 controller is mapped to the car’s main functions:

| Controller input | Function |
|---|---|
| PS Button | Connect / disconnect controller |
| Right Trigger | Move forward |
| Left Trigger | Move backward |
| Left Stick X-axis | Steering left / right |
| UP Button | Headlights ON / OFF |
| Triangle Button | Auto mode ON |
| Circle Button | Auto mode OFF |

This mapping creates a natural driving experience, with analog trigger acceleration and stick-based steering similar to a real vehicle interface.

---

## Software Architecture

The code was designed with a modular structure. Each major hardware area has dedicated logic, making the system easier to understand, test, and extend.

```text
Controller input handling
  ↓
Bluetooth communication through USB Host Shield
  ↓
Manual driving command interpretation
  ↓
Motor control through L298N
  ↓
Servo steering control
  ↓
Headlight control
  ↓
Obstacle avoidance mode when enabled
```

### Motor Control

The L298N motor driver receives direction and speed commands from the Arduino. PWM is used to control motor speed, allowing smoother movement than simple on/off control.

### Servo Control

The MG90S steering servo is controlled through PWM. The steering logic includes a neutral reset behavior that brings the wheels back to a straight position when no steering input is received.

### Ultrasonic Distance Measurement

The ultrasonic sensor measures distance using trigger and echo pins. The distance is calculated from the time of the returned pulse:

```text
Distance = (Time of Impulse × 0.034) / 2
```

The factor `0.034` approximates the speed of sound in centimeters per microsecond, and the division by two accounts for the signal traveling to the object and back.

### Bluetooth Communication

The USB Bluetooth receiver is paired with the PS5 controller through the USB Host Shield. Once connected, the Arduino reads controller inputs continuously and converts them into movement, steering, light, or mode commands.

---

## Results

The implementation successfully integrated the main hardware and software components into a functional smart-car prototype.

Key results:

- The car responded correctly to PlayStation 5 controller commands.
- Analog acceleration improved the realism of the driving experience.
- Steering behavior worked through the MG90S servo motor.
- The headlights could be toggled from the controller.
- The car could switch into autonomous obstacle-avoidance mode.
- The ultrasonic sensor was used to detect nearby obstacles.
- Safety behavior prevented unintended movement when no command was active.
- Compatibility issues with USB receivers were handled through testing and component selection.
- Servo accuracy improved after using a dedicated power supply.
- DC motor performance improved after recognizing the need for separate motor power.

---

## Challenges Solved

| Challenge | Solution |
|---|---|
| USB receiver compatibility issues | Tested multiple receivers and selected one compatible with the PS5 controller |
| USB Host Shield manufacturing issue | Identified a 5V issue and re-soldered the board to restore USB power |
| Servo accuracy and reset problems | Added a dedicated power supply for the servo motor |
| DC motors initially underpowered | Moved toward a dedicated motor power supply |
| Obstacle avoidance limitations | Implemented and iterated on a rule-based detection and repositioning flow |
| Future outdoor control range | Planned 5G-based control expansion for wider operating range |

---

## Future Improvements

The project can be extended in several directions:

- add an optical sensor for indoor 2D localization;
- add stoplights and turning signals;
- add a point-of-view camera for remote driving;
- create a return-to-home mode;
- mount the ultrasonic sensor on a rotating servo;
- improve the obstacle-avoidance algorithm;
- explore an Arduino Mega or Raspberry Pi version for additional I/O and networking capabilities;
- extend the control range through a 5G-based communication layer.

---

## Learning Outcomes

This project demonstrates practical experience with:

- embedded C++ programming;
- microcontroller-based robotics;
- Bluetooth controller integration;
- USB Host Shield communication;
- PWM-based DC motor control;
- PWM-based servo control;
- ultrasonic sensing;
- autonomous rule-based navigation;
- 3D-printed mechanical design;
- debugging hardware compatibility issues;
- integrating software, electronics, and mechanical components into one working prototype.

---

## Source Code Availability

The full source code is private due to intellectual property considerations.

A technical walkthrough, selected implementation details, or a sanitized explanation of the architecture can be provided upon request.
