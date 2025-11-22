**🛩️ Raspberry Pi Pico Drone Build Guide**

This project walks through building and programming a quadcopter flight controller from scratch using a Raspberry Pi Pico W. Unlike Betaflight-based controllers, you will write all flight-control logic yourself — including gyro reading, receiver interpretation, PID stabilization, and ESC PWM output.

**📘 Phase 1: System Overview**

Your Raspberry Pi Pico W acts as the flight controller. Since it lacks built-in flight firmware, your code must:

Read gyroscope data to determine drone orientation

Read receiver input to determine pilot commands

Use a PID loop to compute stabilization corrections

Output PWM signals to the ESCs to control motor speed

**🔌 Phase 2: Wiring Diagram**
1. Powering the System

The F450 frame includes a bottom plate that functions as a Power Distribution Board (PDB).

Battery → PDB

ESCs → PDB (+ and – pads)

Pico Power:
Most yellow 30A ESCs include a 5V BEC.

Connect 5V (red) from one ESC to VSYS on the Pico

Connect ground (black) to Pico GND

Remove the red wire from the other three ESCs to avoid multiple 5V sources

2. Motor Signal Wiring (PWM)
Motor	Location	Pico GPIO
1	Front Left	GPIO 0
2	Front Right	GPIO 1
3	Rear Right	GPIO 2
4	Rear Left	GPIO 3

Ensure all ESC grounds connect to Pico GND.

3. MPU-6050 (Gyro / Accelerometer)

I2C wiring:

VCC → 3.3V (Pico 3V3 OUT)

GND → GND

SCL → GPIO 5 (I2C0 SCL)

SDA → GPIO 4 (I2C0 SDA)

Mount the module flat, centered, and with the arrow pointing forward.

4. RC Receiver (FlySky FS-iA6B)

Use iBUS (digital, low-latency).

VCC → 5V (from ESC BEC or Pico VSYS)

GND → GND

iBUS Signal → GPIO 9 (UART1 RX)

**🛠️ Phase 3: Assembly Steps**
Frame & Power

Solder ESC power wires + XT60 connector to the PDB

Check for shorts with a multimeter before plugging in a battery

Motor Installation

Mount motors to arms

Do NOT install propellers yet

Connect motor wires to ESCs (swap any two wires to reverse direction later)

Pico Mounting

Use foam, rubber, or a vibration-isolated mount. Vibrations will degrade gyro performance.

**🔄 Phase 4: Motor Rotation & Propellers**

Quad X configuration:

Motor	Position	Rotation
1	Front Left	CW
2	Front Right	CCW
3	Rear Right	CW
4	Rear Left	CCW

Propellers: raised lettering faces upward.

🧠 Phase 5: Flight Software (PID Loop)

You will implement a 3-axis PID controller.

PID Terms

P (Proportional): Corrects the immediate error

I (Integral): Corrects long-term drift

D (Derivative): Dampens oscillations

Tuning Guide

Test with props OFF

Arm, throttle up slightly, and tilt the drone by hand

Motors should react against your movement

Adjust:

P: Increase until fast oscillation, then back off

D: Increase to reduce wobble

I: Increase for holding angle under wind or imbalance

⚠️ Safety Checklist

PROPS OFF anytime the drone is on USB or being programmed

Use a smoke stopper for first power-up

Set up a kill switch on the transmitter immediately
