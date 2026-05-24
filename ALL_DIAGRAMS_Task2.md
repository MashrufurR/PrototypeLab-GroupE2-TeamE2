# Requirement Diagram – v1

```mermaid
flowchart TD
    R0["REQ-00: Autonomous Vehicle System<br/>The vehicle shall drive autonomously on a track."]

    R1["REQ-01: Line Following<br/>The vehicle shall follow a line using two IR sensors."]
    R2["REQ-02: Obstacle Detection<br/>The vehicle shall detect obstacles using an ultrasonic sensor."]
    R3["REQ-03: Obstacle Reaction<br/>The vehicle shall stop or avoid an obstacle when it is detected."]
    R4["REQ-04: Motor Control<br/>The vehicle shall control two DC motors using a motor driver."]
    R5["REQ-05: Controller<br/>The vehicle shall use an Arduino as the main controller."]
    R6["REQ-06: Power Supply<br/>The vehicle shall include a battery, switch, motor driver, and Arduino power connection."]
    R7["REQ-07: Tinkercad Prototype<br/>The first version shall be designed and coded in Tinkercad."]
    R8["REQ-08: Physical Prototype<br/>The team shall assemble the first prototype vehicle."]
    R9["REQ-09: Provided Components<br/>The vehicle shall use the provided project components."]
    R10["REQ-10: Documentation<br/>The team shall upload diagrams, code, circuit design, and prototype picture to GitHub."]

    R0 --> R1
    R0 --> R2
    R0 --> R3
    R0 --> R4
    R0 --> R5
    R0 --> R6
    R0 --> R7
    R0 --> R8
    R0 --> R9
    R0 --> R10

    R1 -.satisfied by.-> S1["Left IR Sensor + Right IR Sensor"]
    R2 -.satisfied by.-> S2["Ultrasonic Sensor"]
    R3 -.satisfied by.-> S3["Arduino Decision Logic"]
    R4 -.satisfied by.-> S4["Motor Driver + Two DC Motors"]
    R6 -.satisfied by.-> S5["Battery + Switch + Common GND"]
```


---

# Block Definition Diagram (BDD) – v1

```mermaid
classDiagram
    class AutonomousVehicle {
      +followLine()
      +detectObstacle()
      +controlMotors()
      +stopVehicle()
    }

    class Chassis
    class ArduinoUno {
      +readSensors()
      +calculateAction()
      +sendMotorCommands()
    }

    class MotorDriver {
      +driveLeftMotor()
      +driveRightMotor()
      +setSpeed()
    }

    class LeftMotor
    class RightMotor
    class LeftIRSensor {
      +detectLine()
    }
    class RightIRSensor {
      +detectLine()
    }
    class UltrasonicSensor {
      +measureDistance()
    }
    class Battery {
      +supplyPower()
    }
    class Switch {
      +turnPowerOnOff()
    }
    class Breadboard
    class MotorHolder
    class IRSensorHolder
    class UltrasonicHolder
    class BatteryBreadboardHolder

    AutonomousVehicle *-- Chassis
    AutonomousVehicle *-- ArduinoUno
    AutonomousVehicle *-- MotorDriver
    AutonomousVehicle *-- LeftMotor
    AutonomousVehicle *-- RightMotor
    AutonomousVehicle *-- LeftIRSensor
    AutonomousVehicle *-- RightIRSensor
    AutonomousVehicle *-- UltrasonicSensor
    AutonomousVehicle *-- Battery
    AutonomousVehicle *-- Switch
    AutonomousVehicle *-- Breadboard
    AutonomousVehicle *-- MotorHolder
    AutonomousVehicle *-- IRSensorHolder
    AutonomousVehicle *-- UltrasonicHolder
    AutonomousVehicle *-- BatteryBreadboardHolder

    ArduinoUno --> LeftIRSensor : reads signal
    ArduinoUno --> RightIRSensor : reads signal
    ArduinoUno --> UltrasonicSensor : trig/echo
    ArduinoUno --> MotorDriver : control signals
    MotorDriver --> LeftMotor : power output
    MotorDriver --> RightMotor : power output
    Battery --> Switch : positive line
    Switch --> MotorDriver : switched power
    Battery --> ArduinoUno : power/GND
```


---

# Internal Block Diagram (IBD) – v1

```mermaid
flowchart LR
    Battery["Battery"]
    Switch["Switch"]
    Arduino["Arduino Uno"]
    Driver["Motor Driver"]
    LM["Left DC Motor"]
    RM["Right DC Motor"]
    LIR["Left IR Sensor"]
    RIR["Right IR Sensor"]
    US["Ultrasonic Sensor"]
    GND["Common Ground"]

    Battery -- "+ power" --> Switch
    Switch -- "switched + power" --> Driver
    Battery -- "GND" --> GND
    GND --> Driver
    GND --> Arduino
    GND --> LIR
    GND --> RIR
    GND --> US

    Arduino -- "5V" --> LIR
    Arduino -- "5V" --> RIR
    Arduino -- "5V" --> US

    LIR -- "digital input" --> Arduino
    RIR -- "digital input" --> Arduino
    Arduino -- "TRIG" --> US
    US -- "ECHO" --> Arduino

    Arduino -- "IN1, IN2, ENA" --> Driver
    Arduino -- "IN3, IN4, ENB" --> Driver

    Driver -- "OUT1/OUT2" --> LM
    Driver -- "OUT3/OUT4" --> RM
```


---

# Use Case Diagram – v1

```mermaid
flowchart LR
    Student["Actor: Student / Team Member"]
    Track["Actor: Track / Line"]
    Obstacle["Actor: Obstacle"]
    Vehicle["System: Autonomous Vehicle"]

    UC1(("Follow line"))
    UC2(("Detect obstacle"))
    UC3(("Stop vehicle"))
    UC4(("Avoid obstacle"))
    UC5(("Control motors"))
    UC6(("Read IR sensors"))
    UC7(("Read ultrasonic sensor"))
    UC8(("Simulate in Tinkercad"))
    UC9(("Test prototype"))
    UC10(("Document results in GitHub"))

    Student --> UC8
    Student --> UC9
    Student --> UC10

    Track --> UC1
    Obstacle --> UC2

    Vehicle --> UC1
    Vehicle --> UC2
    Vehicle --> UC3
    Vehicle --> UC4
    Vehicle --> UC5
    Vehicle --> UC6
    Vehicle --> UC7

    UC1 -.includes.-> UC6
    UC1 -.includes.-> UC5
    UC2 -.includes.-> UC7
    UC2 -.extends.-> UC3
    UC2 -.extends.-> UC4
```


---

# Sequence Diagram – v1

```mermaid
sequenceDiagram
    participant User as Student
    participant Arduino as Arduino Uno
    participant IR as IR Sensors
    participant US as Ultrasonic Sensor
    participant Driver as Motor Driver
    participant Motors as DC Motors

    User->>Arduino: Start vehicle / upload code
    loop Main control loop
        Arduino->>IR: Read left and right IR values
        IR-->>Arduino: Line position data

        Arduino->>US: Send TRIG signal
        US-->>Arduino: Return ECHO time / distance

        alt Obstacle distance below threshold
            Arduino->>Driver: Send stop command
            Driver->>Motors: Stop both motors
        else No obstacle detected
            Arduino->>Arduino: Calculate line-following action
            alt Line centered
                Arduino->>Driver: Move forward
                Driver->>Motors: Drive both motors forward
            else Line detected left
                Arduino->>Driver: Turn left
                Driver->>Motors: Adjust motor speeds
            else Line detected right
                Arduino->>Driver: Turn right
                Driver->>Motors: Adjust motor speeds
            else Line lost
                Arduino->>Driver: Stop or search line
                Driver->>Motors: Stop / slow movement
            end
        end
    end
```


---

# Activity Diagram – v1

```mermaid
flowchart TD
    A([Start])
    B[Initialize Arduino pins and variables]
    C[Read left and right IR sensors]
    D[Read ultrasonic distance]
    E{Obstacle detected?}
    F[Stop vehicle]
    G[Optional: avoid obstacle]
    H{Line position?}
    I[Move forward]
    J[Turn left]
    K[Turn right]
    L[Stop or search for line]
    M[Repeat control loop]

    A --> B
    B --> C
    C --> D
    D --> E

    E -- Yes --> F
    F --> G
    G --> M

    E -- No --> H
    H -- Centered --> I
    H -- Line left --> J
    H -- Line right --> K
    H -- Line lost --> L

    I --> M
    J --> M
    K --> M
    L --> M
    M --> C
```


---

# Package Diagram – v1

```mermaid
flowchart TD
    P0["Vehicle Project Model"]

    P1["Requirements Package"]
    P2["Structure Package"]
    P3["Behavior Package"]
    P4["Simulation Package"]
    P5["Prototype Package"]
    P6["Documentation Package"]

    R["Requirement Diagram"]
    BDD["Block Definition Diagram"]
    IBD["Internal Block Diagram"]
    UC["Use Case Diagram"]
    SEQ["Sequence Diagram"]
    ACT["Activity Diagram"]
    TIN["Tinkercad Design + Code"]
    CIR["Circuit Design"]
    ASM["Vehicle Assembly + Picture"]
    GIT["GitHub README + Notes"]

    P0 --> P1
    P0 --> P2
    P0 --> P3
    P0 --> P4
    P0 --> P5
    P0 --> P6

    P1 --> R
    P2 --> BDD
    P2 --> IBD
    P3 --> UC
    P3 --> SEQ
    P3 --> ACT
    P4 --> TIN
    P4 --> CIR
    P5 --> ASM
    P6 --> GIT

    P1 -.trace.-> P2
    P2 -.supports.-> P3
    P3 -.implemented in.-> P4
    P4 -.verified by.-> P5
```
