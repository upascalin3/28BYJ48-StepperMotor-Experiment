# Experimenting with the 28BYJ-48 Stepper Motor using Arduino

This project demonstrates how to control a **28BYJ-48 stepper motor** with a **ULN2003 driver board** using an **Arduino Uno** and the built-in `Stepper.h` library.  
The Arduino receives rotation commands via the Serial Monitor and moves the motor with precise angular control.

---

## 🧩 Components Used
| Component | Description |
|------------|-------------|
| **Arduino Uno** | Microcontroller board |
| **28BYJ-48 Stepper Motor** | 5-V unipolar stepper motor |
| **ULN2003 Driver Board** | Interface between Arduino and motor |
| **Jumper Wires** | For connections |
| **USB Cable** | For uploading code and Serial communication |

---

## ⚙️ Wiring Connections

| ULN2003 Pin | Arduino Pin | Description |
|--------------|--------------|-------------|
| IN1 | D8 | Control Coil 1 |
| IN2 | D9 | Control Coil 2 |
| IN3 | D10 | Control Coil 3 |
| IN4 | D11 | Control Coil 4 |
| VCC | 5V | Motor power |
| GND | GND | Common ground |

---

## 💻 Arduino Code

```cpp
#include <Stepper.h>

const int stepsPerRevolution = 2048;  // 28BYJ-48 with gearbox
const int in1Pin = 8;
const int in2Pin = 9;
const int in3Pin = 10;
const int in4Pin = 11;

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
🧮 How It Works
The Arduino reads the Serial input from the user.

The entered angle (e.g., rotate 90) is converted into steps:

ini
Copy code
steps = (angle / 360) * 2048
The Stepper motor moves clockwise or counterclockwise depending on the sign of the angle.

🧠 Concepts Covered
Stepper motor control with ULN2003 driver

Degree-to-step conversion

Serial input handling in Arduino

Basics of motion kinematics in robotics

▶️ How to Run
Upload the sketch to Arduino Uno.

Open Serial Monitor at 9600 baud.

Type:

nginx
Copy code
rotate 90
rotate -180
Observe the motor rotating precisely to the commanded angles.

📷 Optional Enhancements
Add a “home” command to return the motor to zero position.

Track the motor’s current position.

Add multiple motors for a simple robotic arm.

📎 Author
Uwayo Pascaline
Experiment: 28BYJ-48 Stepper Motor with ULN2003 and Arduino Uno

yaml
Copy code

---

Would you like me to add a simple **diagram (image)** showing the **Arduino-to-ULN2003 wiring** for your README? I can generate one for you.






You said:
make the best readme file with full desciprion


ChatGPT said:
Absolutely! Here’s a professionally structured and detailed README.md for your
📘 “Experimenting with the 28BYJ-48 Stepper Motor using Arduino” repository.

It includes: project overview, components, setup, wiring diagram, theory, code explanation, demo instructions, and extension ideas — perfect for GitHub and presentations.

markdown
Copy code
# 🦾 Experimenting with the 28BYJ-48 Stepper Motor using Arduino

This project demonstrates how to control a **28BYJ-48 stepper motor** using an **Arduino Uno** and the **ULN2003 motor driver board** through the **Stepper.h** library.  
The system accepts **Serial commands** (like `rotate 90` or `rotate -180`) and moves the motor accordingly, showcasing **precise angular motion control** — a core concept used in robotic arms and automation systems.

---

## 🌟 Project Overview

Stepper motors are widely used in robotics and automation where **precise control of position and speed** is required.  
In this experiment, we explore how to:
- Interface the 28BYJ-48 stepper motor with Arduino.
- Convert **angle input (degrees)** into **motor steps**.
- Control rotation direction (clockwise & counterclockwise).
- Understand the role of **kinematics** in motor movement.

This experiment forms the foundation for more advanced applications such as **robotic arms, CNC machines, and 3D printers**.

---

## 🧩 Components Required

| Component | Quantity | Description |
|------------|-----------|-------------|
| **Arduino Uno** | 1 | Main microcontroller board |
| **28BYJ-48 Stepper Motor** | 1 | 5V unipolar stepper motor |
| **ULN2003 Driver Board** | 1 | Interface between Arduino and motor |
| **Jumper Wires** | ~6 | For electrical connections |
| **USB Cable** | 1 | To upload code & provide power |

---

## ⚙️ Hardware Connections

| ULN2003 Pin | Arduino Pin | Description |
|--------------|--------------|--------------|
| IN1 | D8 | Control coil 1 |
| IN2 | D9 | Control coil 2 |
| IN3 | D10 | Control coil 3 |
| IN4 | D11 | Control coil 4 |
| VCC | 5V | Motor power |
| GND | GND | Common ground |

> ⚠️ **Tip:** Make sure to connect Arduino GND and ULN2003 GND together. Without a common ground, the motor will not respond correctly.

---

## 🧠 Understanding the 28BYJ-48 Stepper Motor

- **Type:** Unipolar Stepper Motor  
- **Operating Voltage:** 5V DC  
- **Gear Ratio:** ~64:1  
- **Step Angle:** 5.625°/64 ≈ **0.0879° per step**  
- **Total Steps per Revolution:** 2048 steps (for one full 360° rotation)

### 🔢 Step Calculation Formula

steps = (angle / 360) * 2048

arduino
Copy code

For example:
- 90° → `(90 / 360) * 2048 = 512 steps`
- 180° → `(180 / 360) * 2048 = 1024 steps`

---

## 💻 Arduino Code (Stepper.h)

```cpp
#include <Stepper.h>

const int stepsPerRevolution = 2048;  // 28BYJ-48 with gearbox
const int in1Pin = 8;
const int in2Pin = 9;
const int in3Pin = 10;
const int in4Pin = 11;

// Step sequence order: IN1, IN3, IN2, IN4 for correct rotation
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
▶️ How to Run
Open Arduino IDE (v2.3.6 or newer).

Connect your Arduino Uno to your computer.

Copy and paste the above code into the IDE.

Select:

Board: Arduino Uno

Port: (your connected port)

Click Upload.

Open the Serial Monitor (set to 9600 baud).

Type:

nginx
Copy code
rotate 90
rotate -180
rotate 360
The motor will move precisely to the commanded angles!

📸 Example Output
makefile
Copy code
Stepper Motor Ready!
Type: rotate <angle>
Example: rotate 90  or  rotate -180
> rotate 90
Rotating 90 degrees...
Done.
The stepper motor rotates 90° clockwise.
Entering a negative angle (rotate -180) will rotate it counterclockwise.

🧮 Kinematics in Context
In robotics, kinematics relates joint movements (like the stepper motor's rotation) to the end-effector position.
By converting degrees to steps, we define a clear mathematical relationship between:

Input angle (human command)

Stepper motor motion (mechanical output)

This principle enables precise robotic arm control and position tracking.

🔧 Optional Enhancements
🏠 Home Function: Add a reference position using a limit switch.

📈 Position Tracking: Keep track of current angle or step count.

⚡ Acceleration Control: Use the AccelStepper library for smooth motion.

🤖 Multi-Motor Control: Extend to control multiple motors for a full robotic arm.

🔄 Feedback System: Integrate encoders to verify movement accuracy.

🧰 Troubleshooting
Issue	Possible Cause	Solution
Motor not rotating	Wrong wiring order	Check pin order (IN1, IN3, IN2, IN4)
Jittery movement	Speed too high	Reduce RPM using myStepper.setSpeed()
Upload error	Port or permissions issue	Reconnect USB, or run sudo usermod -aG dialout $USER
No Serial response	Baud rate mismatch	Ensure Serial Monitor is set to 9600

📂 Repository Structure
bash
Copy code
28BYJ48-StepperMotor-Experiment/
│
├── stepper_control.ino      # Main Arduino sketch
├── README.md                # Project documentation
└── images/                  # (Optional) Wiring diagram & project photos
📜 License
This project is released under the MIT License — free to use, modify, and share for educational and research purposes.

👩‍💻 Author
Uwayo Pascaline
Experiment: Controlling the 28BYJ-48 Stepper Motor using Arduino Uno and ULN2003
📧 Email: pascuwayo@gmail.com
📍 Kigali, Rwanda

🌐 GitHub Repository
👉 https://github.com/upascalin3/28BYJ48-StepperMotor-Experiment

