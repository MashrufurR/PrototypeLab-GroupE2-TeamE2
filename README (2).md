# Line Following Robot with Obstacle Detection
### CSE / EEE Robotics Project — Initial Submission

---

## What We Are Building

We are building a two-wheeled autonomous robot that follows a line on the ground and reacts to obstacles in its path. The goal is to demonstrate basic autonomous navigation using low-cost sensors and an Arduino microcontroller — no manual control, no remote input, just the robot making its own decisions based on what it sees.

This is our initial submission covering the design, simulation, and hardware setup phase of the project.

---

## Planned Behavior

Once fully complete, the robot will:

- Drive forward along a line at normal speed
- Slow down one wheel to gently correct its direction when it starts to drift
- Trigger an emergency stop if an obstacle comes within 30cm
- Stop safely on its own if the line is lost for too long

---

## Hardware Overview

| Component | Quantity |
|---|---|
| Arduino Uno R3 | 1 |
| L298N Motor Driver Module | 1 |
| DC Gear Motor | 2 |
| HC-SR04 Ultrasonic Sensor | 2 |
| IR Line Tracking Sensor (TCRT5000) | 2 |
| Breadboard | 1 |
| 9V Battery | 1 |

---

## Pin Map

| Arduino Pin | Connected To |
|---|---|
| 5V | Breadboard `+` rail |
| GND | Breadboard `–` rail + L298N GND |
| D2 | L298N IN1 |
| D3 | L298N IN2 |
| D4 | L298N IN3 |
| D5 (PWM) | L298N ENA — left motor speed |
| D6 (PWM) | L298N ENB — right motor speed |
| D7 | Left IR sensor OUT |
| D8 | Right IR sensor OUT |
| D9 | Left ultrasonic TRIG |
| D10 | Left ultrasonic ECHO |
| D11 | Right ultrasonic TRIG |
| D12 | Right ultrasonic ECHO |
| D13 | L298N IN4 |

---

## Tools We Are Using

- **Tinkercad** — for virtual circuit simulation before physical assembly
- **Arduino IDE** — for writing and uploading code
- **GitHub** — for version control and documentation

---

## Our Team

| Name | Role |
|---|---|
| Aynul | Circuit Diagrams & Schematics |
| Taha | Tinkercad Simulation |
| Jisan | Physical Wiring & Hardware Assembly |
| Mashruf | Arduino Code Development |

### What each of us did so far

**Aynul** drew the full circuit diagrams showing how every component connects — motors, sensors, motor driver, and power lines. This became the reference that the rest of the team worked from.

**Taha** built the virtual simulation in Tinkercad based on Aynuls's diagrams. We used this to verify our wiring logic and pin assignments before touching any real hardware.

**Jisan** took the simulation and assembled it on actual hardware — connecting the breadboard, wiring the L298N motor driver, setting up the motors, and bridging the common ground between all components.

**Mashrufur** started writing the Arduino code — motor control functions, sensor reading logic, and the serial monitor output so we can watch what the robot is sensing in real time.

---

## Current Status

| Task | Status |
|---|---|
| Circuit diagrams | ✅ Done |
| Tinkercad simulation | ✅ Done |
| Physical wiring | ✅ Done |
| Arduino code (base) | 🔄 In progress |
| Real-world testing | ⏳ Upcoming |
| Final calibration | ⏳ Upcoming |

---

## What's Next

- Complete and test the Arduino code on the physical robot
- Run the robot on a test track and adjust sensor thresholds based on real surface readings
- Fine-tune turning speed ratios for smooth line correction
- Document test results and prepare the final project report

