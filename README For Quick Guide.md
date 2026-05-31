# Line Following Robot

This is the hardware connection guide for our Arduino-based line following robot. We built this for real-life use, so if you're coming from the Tinkercad simulation, note that we replaced the two switches with actual IR sensors here.

---

## What We Used

| Component | How Many |
|---|---|
| Arduino Uno | 1 |
| L298N Motor Driver | 1 |
| DC Motor | 2 |
| IR Line Sensor | 2 |
| HC-SR04 Ultrasonic Sensor | 2 |
| Breadboard | 1 |
| 9V Battery | 1 |

---

## How We Mounted Everything

We screwed the Arduino and motor driver directly onto the chassis. For the IR and ultrasonic sensors, we used proper sensor holders. We did not use any glue, tape, or zip ties — that's a hard rule in our design guidelines.

---

## A. Powering the Breadboard from Arduino

This is the first thing we connect. The sensors all draw power from the breadboard rails.

| Arduino | Breadboard |
|---|---|
| 5V | `+` positive rail |
| GND | `–` negative rail |

---

## B. Connecting the 9V Battery to L298N

For our first test we always power the Arduino through USB — it's safer and easier to debug.

| What | Where it goes |
|---|---|
| 9V Battery `+` | L298N `VIN / 12V` terminal |
| 9V Battery `–` | L298N `GND` terminal |
| L298N `GND` | Arduino GND (and breadboard `–` rail) |


## C. The ENA / ENB Jumpers on L298N

By default the L298N comes with jumper caps sitting on the ENA and ENB pins. We need to remove both of them.

| Jumper | What to do |
|---|---|
| ENA | Pull it off |
| ENB | Pull it off |

We do this because we want to control motor speed ourselves from the Arduino. We use D5 for the left motor and D6 for the right. The way our code works — normal driving is full speed, turning slows one side down, and if an obstacle shows up the motors stop completely.

---

## D. L298N to Arduino

| L298N Pin | What it does | Arduino Pin |
|---|---|---|
| ENA | Left motor speed | D5 |
| IN1 | Left motor direction | D2 |
| IN2 | Left motor direction | D3 |
| IN3 | Right motor direction | D4 |
| IN4 | Right motor direction | D13 |
| ENB | Right motor speed | D6 |
| GND | Common ground | GND |

---

## E. Motors to L298N

| Motor | L298N Output |
|---|---|
| Left motor (2 wires) | OUT1 and OUT2 |
| Right motor (2 wires) | OUT3 and OUT4 |

One thing that often happens during first testing — if we send a forward command and one motor spins backwards, we just swap that motor's two wires on the output terminal. No code change needed.

---

## F. IR Sensors

In Tinkercad we had Switch 1 on D7 and Switch 2 on D8. In the real build, our IR sensors go to the exact same pins, so we didn't have to change anything in the code.

**Left IR Sensor**

| Sensor Pin | Connect to |
|---|---|
| VCC | Breadboard `+` rail |
| GND | Breadboard `–` rail |
| OUT | Arduino D7 |

**Right IR Sensor**

| Sensor Pin | Connect to |
|---|---|
| VCC | Breadboard `+` rail |
| GND | Breadboard `–` rail |
| OUT | Arduino D8 |

One thing we noticed — some IR sensors give `LOW` when they detect a black line, and some give `HIGH`. If our robot behaves weirdly on the line, we try flipping this one line in the code:

```cpp
// default
const int LINE_DETECTED = LOW;

// change to this if the sensor is inverted
const int LINE_DETECTED = HIGH;
```

---

## G. Ultrasonic Sensors

**Left HC-SR04**

| Sensor Pin | Connect to |
|---|---|
| VCC | Breadboard `+` rail |
| GND | Breadboard `–` rail |
| TRIG | Arduino D9 |
| ECHO | Arduino D10 |

**Right HC-SR04**

| Sensor Pin | Connect to |
|---|---|
| VCC | Breadboard `+` rail |
| GND | Breadboard `–` rail |
| TRIG | Arduino D11 |
| ECHO | Arduino D12 |

---

## H. Full Pin Summary

This is the complete picture of what goes where on the Arduino.

| Arduino Pin | What's connected |
|---|---|
| 5V | Breadboard `+` rail |
| GND | Breadboard `–` rail and L298N GND |
| D2 | L298N IN1 |
| D3 | L298N IN2 |
| D4 | L298N IN3 |
| D5 | L298N ENA (left motor speed) |
| D6 | L298N ENB (right motor speed) |
| D7 | Left IR sensor OUT |
| D8 | Right IR sensor OUT |
| D9 | Left ultrasonic TRIG |
| D10 | Left ultrasonic ECHO |
| D11 | Right ultrasonic TRIG |
| D12 | Right ultrasonic ECHO |
| D13 | L298N IN4 |
