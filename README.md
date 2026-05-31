# Arduino Line-Following Robot — Hardware Connection Guide

A complete wiring and connection reference for the real-life build of the Arduino-based line-following robot with obstacle detection.

---

## Components

| Component | Quantity |
|---|---|
| Arduino Uno | 1 |
| L298N Motor Controller Module | 1 |
| DC Motor | 2 |
| IR Line Sensor | 2 |
| Ultrasonic Sensor (HC-SR04) | 2 |
| Breadboard | 1 |
| 9V Battery | 1 |

> **Note:** The real-life circuit does **not** use switches. The two switches from the Tinkercad simulation are replaced by two IR sensors in the actual build.

---

## Mounting Guidelines

- Arduino and motor controller are **screwed onto the chassis**.
- IR sensors and ultrasonic sensors use **dedicated holders**.
- Glue, tape, and zip ties are **not allowed**.

---

## A. Arduino to Breadboard Power

| From Arduino | To Breadboard |
|---|---|
| 5V | Positive `+` rail |
| GND | Negative `–` rail |

IR sensors and ultrasonic sensors draw power from these breadboard rails.

---

## B. L298N Power Connection

> For the first test, power the Arduino via USB — it is the safest option.

| Connection | Connect To |
|---|---|
| 9V Battery `+` | L298N `12V / VIN` terminal |
| 9V Battery `–` | L298N `GND` terminal |
| L298N `GND` | Arduino GND / Breadboard `–` rail |

**Power flow summary:**
- `9V Battery → L298N → Motors`
- `Arduino USB / 5V → Sensors and Arduino logic`
- `Arduino GND + L298N GND + Sensor GND → Common ground`

### ⚠️ Important Warnings

- Never connect the 9V battery `+` to the Arduino `5V` pin.
- Do not connect the L298N `5V` output pin and Arduino `5V` pin together at this stage.
- A rectangular 9V battery may cause the motors to spin weakly — this is a battery limitation, not a wiring error.

---

## C. L298N Jumper Settings

The L298N module ships with jumper caps on the ENA and ENB pins.

| Jumper | Action |
|---|---|
| ENA jumper | **Remove** |
| ENB jumper | **Remove** |

Motor speed is controlled via Arduino pins D5 (left) and D6 (right):
- **Normal movement** → fast speed
- **Turning** → slow speed
- **Obstacle detected** → motors off

---

## D. L298N to Arduino Control Connections

| L298N Pin | Purpose | Arduino Pin |
|---|---|---|
| ENA | Left motor speed (PWM) | D5 |
| IN1 | Left motor direction 1 | D2 |
| IN2 | Left motor direction 2 | D3 |
| IN3 | Right motor direction 1 | D4 |
| IN4 | Right motor direction 2 | D13 |
| ENB | Right motor speed (PWM) | D6 |
| GND | Common ground | Arduino GND |

---

## E. Motor Connections

| Motor | L298N Terminal |
|---|---|
| Left Motor (terminals 1 & 2) | OUT1 and OUT2 |
| Right Motor (terminals 1 & 2) | OUT3 and OUT4 |

> If a motor spins in the wrong direction during a forward command, **swap the two wires** on that motor's output terminal.

---

## F. IR Sensor Connections

### Left IR Sensor

| IR Sensor Pin | Connect To |
|---|---|
| VCC | Breadboard `+` rail / Arduino 5V |
| GND | Breadboard `–` rail / Arduino GND |
| OUT / DO | Arduino **D7** |

### Right IR Sensor

| IR Sensor Pin | Connect To |
|---|---|
| VCC | Breadboard `+` rail / Arduino 5V |
| GND | Breadboard `–` rail / Arduino GND |
| OUT / DO | Arduino **D8** |

> The Arduino pin assignments (D7 / D8) match the Tinkercad simulation, so **no code changes** are needed for the IR pins.

### Sensor Behavior Note

Some IR sensors output `LOW` when a black line is detected; others output `HIGH`. If the sensor behaves unexpectedly, change this line in the code:

```cpp
const int LINE_DETECTED = LOW;
```

to:

```cpp
const int LINE_DETECTED = HIGH;
```

---

## G. Ultrasonic Sensor Connections

### Left Ultrasonic Sensor (HC-SR04)

| Sensor Pin | Connect To |
|---|---|
| VCC | Breadboard `+` rail / Arduino 5V |
| GND | Breadboard `–` rail / Arduino GND |
| TRIG | Arduino **D9** |
| ECHO | Arduino **D10** |

### Right Ultrasonic Sensor (HC-SR04)

| Sensor Pin | Connect To |
|---|---|
| VCC | Breadboard `+` rail / Arduino 5V |
| GND | Breadboard `–` rail / Arduino GND |
| TRIG | Arduino **D11** |
| ECHO | Arduino **D12** |

---

## H. Complete Arduino Pin Reference

| Arduino Pin | Connected Component |
|---|---|
| 5V | Breadboard positive rail (sensors) |
| GND | Breadboard negative rail + L298N GND |
| D2 | L298N IN1 |
| D3 | L298N IN2 |
| D4 | L298N IN3 |
| D5 | L298N ENA — Left motor speed |
| D6 | L298N ENB — Right motor speed |
| D7 | Left IR Sensor OUT |
| D8 | Right IR Sensor OUT |
| D9 | Left Ultrasonic TRIG |
| D10 | Left Ultrasonic ECHO |
| D11 | Right Ultrasonic TRIG |
| D12 | Right Ultrasonic ECHO |
| D13 | L298N IN4 |
