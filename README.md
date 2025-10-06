# 🦾 Experimenting with the 28BYJ-48 Stepper Motor using Arduino

This project demonstrates how to control a **28BYJ-48 stepper motor** using an **Arduino Uno** and a **ULN2003 motor driver** via the `Stepper.h` library.  
The Arduino receives rotation commands via the Serial Monitor (e.g., `rotate 90` or `rotate -180`) and moves the motor with **precise angular control** — a fundamental concept for robotic arms and automation.

---

## 🌟 Project Overview

Stepper motors are widely used in robotics and automation for **precise position and speed control**.  
In this project, you will learn how to:

- Interface the 28BYJ-48 stepper motor with Arduino  
- Convert **angle input (degrees)** into **motor steps**  
- Control rotation direction (clockwise & counterclockwise)  
- Understand the role of **kinematics** in motor motion

This experiment is a foundation for projects such as **robotic arms, CNC machines, and 3D printers**.

---

## 🧩 Components Required

| Component | Quantity | Description |
|-----------|----------|-------------|
| Arduino Uno | 1 | Microcontroller board |
| 28BYJ-48 Stepper Motor | 1 | 5V unipolar stepper motor |
| ULN2003 Driver Board | 1 | Motor driver interface |
| Jumper Wires | ~6 | For connections |
| USB Cable | 1 | To upload code & Serial communication |

---

## ⚙️ Hardware Connections

| ULN2003 Pin | Arduino Pin | Description |
|-------------|------------|-------------|
| IN1 | D8 | Control Coil 1 |
| IN2 | D9 | Control Coil 2 |
| IN3 | D10 | Control Coil 3 |
| IN4 | D11 | Control Coil 4 |
| VCC | 5V | Motor power |
| GND | GND | Common ground |

> ⚠️ **Tip:** Ensure Arduino GND and ULN2003 GND are connected. Without a common ground, the motor will not work properly.

---

## 🧠 Understanding the 28BYJ-48 Stepper Motor

- **Type:** Unipolar Stepper Motor  
- **Operating Voltage:** 5V DC  
- **Gear Ratio:** ~64:1  
- **Step Angle:** 5.625°/64 ≈ 0.0879° per step  
- **Total Steps per Revolution:** 2048 steps

### 🔢 Step Calculation Formula

```text
steps = (angle / 360) * 2048
Example:

90° → (90 / 360) * 2048 = 512 steps

180° → (180 / 360) * 2048 = 1024 steps
```

## 💻 Arduino Code
Below is the complete Arduino sketch. The code reads commands from the Serial Monitor and moves the motor accordingly.

```cpp
Copy code
#include <Stepper.h>

const int stepsPerRevolution = 2048;  // 28BYJ-48 with gearbox
const int in1Pin = 8;
const int in2Pin = 9;
const int in3Pin = 10;
const int in4Pin = 11;

// Step sequence: IN1, IN3, IN2, IN4
Stepper myStepper(stepsPerRevolution, in1Pin, in3Pin, in2Pin, in4Pin);

String inputCommand = "";

void setup() {
  Serial.begin(9600);
  myStepper.setSpeed(15);  // RPM
  Serial.println("Stepper Motor Ready!");
  Serial.println("Type: rotate <angle>");
  Serial.println("Example: rotate 90  or  rotate -180");
}

void loop() {
  if (Serial.available() > 0) {
    inputCommand = Serial.readStringUntil('\n');
    inputCommand.trim();

    if (inputCommand.startsWith("rotate")) {
      int spaceIndex = inputCommand.indexOf(' ');
      if (spaceIndex > 0) {
        String angleStr = inputCommand.substring(spaceIndex + 1);
        float angle = angleStr.toFloat();

        int steps = (int)(angle / 360.0 * stepsPerRevolution);

        Serial.print("Rotating ");
        Serial.print(angle);
        Serial.println(" degrees...");
        
        myStepper.step(steps);

        Serial.println("Done.");
      } else {
        Serial.println("Error: use format 'rotate <angle>'");
      }
    } else {
      Serial.println("Unknown command. Use: rotate <angle>");
    }
  }
}
```
## ▶️ How to Run
Open Arduino IDE (v2.3.6 or newer)

Connect the Arduino Uno to your computer

Copy and paste the code above into the IDE

Select:

Board: Arduino Uno

Port: Your Arduino port

Click Upload

Open the Serial Monitor (baud rate 9600)

Type commands such as:

text
Copy code
rotate 90
rotate -180
rotate 360
The motor will rotate according to the command.

📷 Example Output
text
Copy code
Stepper Motor Ready!
Type: rotate <angle>
Example: rotate 90  or  rotate -180
> rotate 90
Rotating 90 degrees...
Done.
Positive angles → clockwise rotation

Negative angles → counterclockwise rotation

🧮 Kinematics in Context
Kinematics relates joint movements (motor steps) to mechanical position.
By converting degrees into steps, we can precisely control the motor and track positions — critical for robotic arms and automation projects.

🔧 Optional Enhancements
🏠 Add a home function using a limit switch

📈 Track the current motor position

⚡ Implement acceleration/deceleration with the AccelStepper library

🤖 Control multiple motors for a robotic arm

🔄 Integrate encoders for movement verification

🧰 Troubleshooting
Issue	Cause	Solution
Motor not moving	Wrong wiring	Check pin sequence (IN1, IN3, IN2, IN4)
Motor jitter	Speed too high	Reduce RPM in setSpeed()
Upload error	Port/permissions	Reconnect USB or run sudo usermod -aG dialout $USER
No Serial response	Baud mismatch	Set Serial Monitor to 9600

📂 Repository Structure
bash
Copy code
28BYJ48-StepperMotor-Experiment/
│
├── stepper_control.ino  # Arduino sketch
├── README.md            # Project documentation
└── images/              # Wiring diagrams & photos
📜 License
This project is licensed under the MIT License — free for personal, educational, and research use.

👩‍💻 Author
Uwayo Pascaline
Experiment: Controlling the 28BYJ-48 Stepper Motor using Arduino Uno & ULN2003
📧 Email: pascuwayo@gmail.com
📍 Kigali, Rwanda

🌐 GitHub Repository
https://github.com/upascalin3/28BYJ48-StepperMotor-Experiment





