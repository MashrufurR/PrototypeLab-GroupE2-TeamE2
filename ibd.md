graph LR
    p["p\nBattery and Switch Assembly"]
    c["c\nArduino Uno Controller"]
    md["md\nMotor Driver L293D"]
    us_left["us_left\nDISTULTRASONIC_1 Left"]
    us_right["us_right\nDISTULTRASONIC_2 Right"]
    ir_left["ir_left\nUIR_1 Left IR"]
    ir_right["ir_right\nUIR_2 Right IR"]
    m_left(["m_left\nLeft DC Motor"])
    m_right(["m_right\nRight DC Motor"])

    %% ── 5V Logic Power Rails ─────────────────────────────────────────────
    p -->|"5V Power"| c
    p -->|"5V Power"| us_left
    p -->|"5V Power"| us_right
    p -->|"5V Power"| ir_left
    p -->|"5V Power"| ir_right
    p -->|"Switched Motor Power — Pin 8 Vcc2"| md

    %% ── IR Sensor OUT Signals ────────────────────────────────────────────
    ir_left  -->|"OUT Signal — D2"| c
    ir_right -->|"OUT Signal — D7"| c

    %% ── Left Ultrasonic Trig / Echo ──────────────────────────────────────
    c       -->|"D8 — Trig Pulse"| us_left
    us_left -->|"Echo Return — D9"| c

    %% ── Right Ultrasonic Trig / Echo ─────────────────────────────────────
    c        -->|"D10 — Trig Pulse"| us_right
    us_right -->|"Echo Return — D11"| c

    %% ── Motor Driver Control Inputs ──────────────────────────────────────
    c -->|"D5 — Control IN1"| md
    c -->|"D4 — Control IN2"| md
    c -->|"D2 — Control IN3"| md

    %% ── Drive Current to Wheels ──────────────────────────────────────────
    md -->|"Drive Current — OUT1/OUT2"| m_left
    md -->|"Drive Current — OUT3/OUT4"| m_right

    %% ── Block colour coding ──────────────────────────────────────────────
    classDef power  fill:#854F0B,stroke:#412402,color:#FAEEDA,font-weight:500
    classDef sensor fill:#185FA5,stroke:#042C53,color:#E6F1FB,font-weight:500
    classDef ctrl   fill:#0F6E56,stroke:#04342C,color:#E1F5EE,font-weight:500
    classDef driver fill:#993C1D,stroke:#4A1B0C,color:#FAECE7,font-weight:500
    classDef motor  fill:#3C3489,stroke:#26215C,color:#EEEDFE,font-weight:500

    class p power
    class us_left,us_right,ir_left,ir_right sensor
    class c ctrl
    class md driver
    class m_left,m_right motor
