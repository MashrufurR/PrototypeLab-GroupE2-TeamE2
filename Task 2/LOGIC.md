# Line Following Robot - Code Logic

## Overview

The robot follows a black tape line on a white surface using two IR sensors and differential motor control. Every loop cycle it checks for obstacles first, then reads the IR sensors and decides how to move.

---

## Main Loop Flow

```
Every 10ms:
    |
    1. Read both ultrasonic sensors
    |     obstacle closer than 15cm -> brake and stop
    |
    2. Read both IR sensors
    |
    3. Decide movement:
          Both on black  -> go straight
          Left on black  -> turn left
          Right on black -> turn right
          Both off black -> pivot search
```

---

## Line Following Logic

IR sensors output LOW on black tape and HIGH on white floor.

| Left IR | Right IR | Action | Reason |
|---|---|---|---|
| BLACK | BLACK | Go straight | Robot is centred on line |
| BLACK | WHITE | Turn left | Robot drifted right |
| WHITE | BLACK | Turn right | Robot drifted left |
| WHITE | WHITE | Pivot search | Line completely lost |

### Turning - Differential Speed

When turning, the two wheels run at different speeds instead of stopping one wheel completely. This gives a smooth curve rather than a sharp snap.

- Outer wheel runs at TURN_OUTER = 80
- Inner wheel runs at TURN_INNER = 20

The inner wheel still moves forward at low speed. This reduces oscillation and juggling on smooth curves.

### Lost Line Recovery

When both sensors lose the line completely, the robot remembers which direction it last turned using `lastTurnDirection`. It then counter-rotates both wheels in opposite directions to pivot in place toward that direction until one sensor finds black tape again.

- Last turned left -> pivot left: drive(-60, 60)
- Last turned right -> pivot right: drive(60, -60)

This handles sharp U-turns where both sensors briefly lose the line.

---

## Motor Control Logic

The `setMotor` function accepts positive values for forward and negative values for reverse. One motor on the chassis is physically mounted in the opposite direction, so `RIGHT_MOTOR_REVERSED = true` flips its direction in software without changing any wiring.

### Active Brake

`brakeNow()` sets both IN pins HIGH with full PWM instead of just cutting power to zero. This locks the motor shaft immediately instead of letting the robot coast forward.

---

## Obstacle Detection

Both ultrasonic sensors are read every loop cycle before any motor decision is made. If either sensor reads a distance below 15cm the robot brakes immediately and skips the rest of the loop. This gives obstacle detection the highest priority over line following.

---

## Speed Constants

| Constant | Value | Purpose |
|---|---|---|
| FORWARD_SPEED | 80 | Both wheels straight |
| TURN_OUTER | 80 | Outside wheel during turn |
| TURN_INNER | 20 | Inside wheel during turn |
| SEARCH_SPEED | 60 | Pivot speed when line lost |
| STOP_DISTANCE | 15.0 cm | Obstacle brake threshold |
