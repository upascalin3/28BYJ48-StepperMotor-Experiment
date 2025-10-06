🦾 Controlling the 28BYJ-48 Stepper Motor with Arduino
This project demonstrates how to control a 28BYJ-48 stepper motor using an Arduino Uno and a ULN2003 driver board with the Stepper.h library. The Arduino accepts Serial commands (e.g., rotate 90 or rotate -180) to move the motor to precise angular positions, making it ideal for learning about stepper motor control, kinematics, and robotic automation.

🌟 Project Overview
The 28BYJ-48 stepper motor is a low-cost, unipolar stepper motor commonly used in robotics and automation projects. This experiment shows how to:

Interface the motor with an Arduino via the ULN2003 driver.
Convert user-input angles (in degrees) to motor steps.
Control rotation direction (clockwise or counterclockwise).
Apply kinematic principles for precise motion control.

This project serves as a foundation for applications like robotic arms, CNC machines, or camera gimbals.

🧩 Components Required



Component
Quantity
Description



Arduino Uno
1
Microcontroller board for logic and control


28BYJ-48 Stepper Motor
1
5V unipolar stepper motor with 2048 steps/revolution


ULN2003 Driver Board
1
Interface to drive the motor's coils


Jumper Wires
6-8
For connecting Arduino to ULN2003


USB Cable
1
For code upload and Serial communication


Optional: External 5V Power
1
For high-torque applications (ensure common GND)



⚙️ Wiring Connections
The ULN2003 driver board simplifies motor control by handling the high-current requirements of the stepper motor's coils.



ULN2003 Pin
Arduino Pin
Description



IN1
D8
Controls Coil 1


IN2
D9
Controls Coil 2


IN3
D10
Controls Coil 3


IN4
D11
Controls Coil 4


VCC
5V
Powers the motor


GND
GND
Common ground with Arduino



⚠️ Important: Ensure the Arduino GND is connected to the ULN2003 GND to avoid erratic behavior.

📷 Wiring Diagram
Below is a simplified representation of the wiring setup. For a visual diagram, refer to images/wiring_diagram.png in the repository (or generate one using tools like Fritzing).
[Arduino Uno]          [ULN2003 Driver Board]          [28BYJ-48 Motor]
   D8 ----------------- IN1
   D9 ----------------- IN2
   D10 ---------------- IN3
   D11 ---------------- IN4
   5V ----------------- VCC
   GND ---------------- GND ------------------- Motor GND


ℹ️ Note: The motor's 5-pin connector plugs directly into the ULN2003 board. Ensure the correct orientation (check the board's pin labels).


🧠 Understanding the 28BYJ-48 Stepper Motor

Type: Unipolar stepper motor
Voltage: 5V DC
Gear Ratio: ~64:1
Step Angle: 5.625° / 64 ≈ 0.0879° per step
Steps per Revolution: 2048 steps (360° rotation)

🔢 Angle-to-Step Conversion
To rotate the motor to a specific angle, we use the formula:
steps = (angle / 360) * 2048

Examples:

90° → (90 / 360) * 2048 = 512 steps
180° → (180 / 360) * 2048 = 1024 steps
-90° → Negative steps for counterclockwise rotation


💻 Arduino Code
The following Arduino sketch uses the Stepper.h library to control the motor based on Serial input commands.
#include <Stepper.h>

// Motor configuration
const int stepsPerRevolution = 2048;  // Steps for one full rotation
const int in1Pin = 8;   // ULN2003 IN1 to Arduino D8
const int in2Pin = 9;   // ULN2003 IN2 to Arduino D9
const int in3Pin = 10;  // ULN2003 IN3 to Arduino D10
const int in4Pin = 11;  // ULN2003 IN4 to Arduino D11

// Initialize Stepper library (note the specific pin order for correct rotation)
Stepper myStepper(stepsPerRevolution, in1Pin, in3Pin, in2Pin, in4Pin);

String inputCommand = "";

void setup() {
  // Initialize Serial communication
  Serial.begin(9600);
  
  // Set motor speed (RPM)
  myStepper.setSpeed(15);  // Recommended: 10-15 RPM for smooth operation
  
  // Print instructions
  Serial.println("🦾 Stepper Motor Control");
  Serial.println("Enter: rotate <angle> (e.g., rotate 90 or rotate -180)");
}

void loop() {
  if (Serial.available() > 0) {
    inputCommand = Serial.readStringUntil('\n');
    inputCommand.trim();  // Remove whitespace

    if (inputCommand.startsWith("rotate")) {
      int spaceIndex = inputCommand.indexOf(' ');
      if (spaceIndex > 0) {
        String angleStr = inputCommand.substring(spaceIndex + 1);
        float angle = angleStr.toFloat();
        
        // Calculate steps from angle
        int steps = (int)(angle / 360.0 * stepsPerRevolution);

        Serial.print("Rotating ");
        Serial.print(angle);
        Serial.println(" degrees...");
        
        // Perform the rotation
        myStepper.step(steps);
        
        Serial.println("Done.");
      } else {
        Serial.println("❌ Error: Use format 'rotate <angle>'");
      }
    } else {
      Serial.println("❌ Unknown command. Use: rotate <angle>");
    }
  }
}

🧮 Code Explanation

Library: The Stepper.h library simplifies motor control by handling step sequences.
Pin Order: The sequence IN1, IN3, IN2, IN4 ensures correct coil activation for smooth rotation.
Serial Input: Commands like rotate 90 are parsed to extract the angle and convert it to steps.
Speed Control: setSpeed(15) sets the motor to 15 RPM, balancing smoothness and torque.
Error Handling: Invalid commands trigger helpful error messages.


▶️ How to Run

Set Up Hardware:

Connect the ULN2003 driver to the Arduino as per the wiring table.
Plug the 28BYJ-48 motor into the ULN2003 board.
Connect the Arduino to your computer via USB.


Install Arduino IDE:

Download and install Arduino IDE (v2.3.6 or newer).


Upload the Code:

Copy the Arduino code above into a new sketch.
In the IDE, select:
Board: Arduino Uno
Port: Your Arduino's connected port


Click Upload.


Test the Motor:

Open the Serial Monitor (Ctrl+Shift+M) in the Arduino IDE.
Set the baud rate to 9600.
Type commands like:rotate 90
rotate -180
rotate 360


Observe the motor rotating to the specified angles.



📸 Example Serial Output
🦾 Stepper Motor Control
Enter: rotate <angle> (e.g., rotate 90 or rotate -180)
> rotate 90
Rotating 90 degrees...
Done.
> rotate -180
Rotating -180 degrees...
Done.


🧮 Robotics & Kinematics Context
This project introduces forward kinematics, where the motor's rotation (joint angle) directly maps to a mechanical output. The angle-to-step conversion ensures precise positioning, a key concept in:

Robotic arms: Controlling joint angles for specific end-effector positions.
CNC machines: Precise movement along axes.
Automation systems: Synchronized motor control.

By mastering this, you can extend the project to multi-axis systems or closed-loop control with feedback.

🔧 Optional Enhancements

🏠 Home Position Command:

Add a home command to reset the motor to a reference position using a limit switch.
Example: if (inputCommand == "home") { myStepper.step(-currentSteps); }


📈 Position Tracking:

Maintain a global variable to track the motor's current angle or step count.
Example: float currentAngle += angle;


⚡ Smooth Acceleration:

Replace Stepper.h with the AccelStepper library for smoother motion and acceleration control.


🤖 Multi-Motor System:

Add more stepper motors to simulate a robotic arm or XY table.


🔄 Feedback Mechanism:

Integrate an encoder or hall-effect sensor to verify the motor's actual position.




🧰 Troubleshooting



Issue
Possible Cause
Solution



Motor not rotating
Incorrect wiring or pin order
Verify IN1-IN4 connections (D8-D11) and GND


Jittery or erratic movement
Speed too high or low power
Reduce RPM with myStepper.setSpeed(10) or use external 5V power


No Serial response
Wrong baud rate or port
Set Serial Monitor to 9600 baud and check port


Upload error
Port conflict or permissions
Reconnect USB, restart IDE, or run sudo usermod -aG dialout $USER (Linux)



📂 Repository Structure
28BYJ48-StepperMotor-Experiment/
│
├── stepper_control.ino      # Arduino sketch for motor control
├── README.md               # Project documentation
└── images/                 # Wiring diagrams and project photos
    └── wiring_diagram.png  # Simplified wiring diagram


📜 License
This project is licensed under the MIT License. Feel free to use, modify, and share for educational or personal projects.

👩‍💻 Author
Uwayo Pascaline📧 Email: pascuwayo@gmail.com📍 Location: Kigali, Rwanda🌐 GitHub: upascalin3/28BYJ48-StepperMotor-Experiment

📷 Wiring Diagram (Optional)
To visualize the connections, you can create a wiring diagram using tools like Fritzing or Tinkercad Circuits. A sample diagram (wiring_diagram.png) is included in the images/ folder, showing:

Arduino Uno connected to ULN2003 pins (IN1-IN4, VCC, GND).
28BYJ-48 motor plugged into the ULN2003 driver.
Optional external 5V power supply for high-torque applications.

Would you like me to generate a specific diagram file (e.g., in SVG or PNG format)? Let me know!
