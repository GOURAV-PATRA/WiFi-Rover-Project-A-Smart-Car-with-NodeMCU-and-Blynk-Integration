# WiFi Rover Project: A Smart Car with NodeMCU and Blynk Integration
![Screenshot 2024-08-18 190038](https://github.com/user-attachments/assets/fd63d095-45ec-4a5a-b619-4c05dd9b268e)

Welcome to the **WiFi Rover Project**, an exciting tutorial where we will guide you through the process of building a smart car controlled via WiFi using the NodeMCU ESP8266 board and the Blynk app. In this comprehensive guide, you’ll learn how to assemble the hardware components, set up the software, and integrate everything to create a fully functional WiFi-controlled rover. Whether you're a seasoned hobbyist or just starting with IoT projects, this guide will provide you with step-by-step instructions to achieve your goals.

## Project Overview

In this project, we’ll use a robotic car kit to build our rover. However, you can substitute it with any other type of vehicle, such as a tank or car model, based on your preference. This tutorial will cover all the necessary steps, from component identification to software setup, ensuring that you have a smooth and successful project experience.

## Components Required

1. **Robotic Car Kit** — Obtain this from various suppliers. Ensure it is compatible with the motor driver and NodeMCU board.
2. **NodeMCU ESP8266 Board** — Available at major electronics retailers or online platforms.
3. **L298N Motor Driver** — This is crucial for controlling the motors. Check electronics stores or online retailers for availability.
4. **Li-ion Batteries x 2** — These provide the necessary power for your rover. Purchase them from online shops or local electronics stores.
5. **Li-ion Battery Holder** — Essential for securely holding the batteries in place. Available from electronics suppliers or online.
6. **Breadboard** — Used for making connections between the NodeMCU and other components. You can find it at electronics stores or online.
7. **Jumper Wires** — These will help in making connections between different components. They are widely available from electronics suppliers or online.

## Step-by-Step Instructions

### Step 1: Component Identification

Begin by identifying and organizing all the components listed above. Ensure you have everything before starting the assembly to avoid interruptions during the process. Familiarize yourself with each component and its role in the project to streamline the setup.

### Step 2: Motor Driver Installation

Carefully attach the motor driver board to the top of the robotic car chassis. Make sure it is securely mounted and positioned for optimal performance. This step is crucial as the motor driver board controls the movement of the car.

### Step 3: Motor Connections
![circuit-transformed](https://github.com/user-attachments/assets/7d047395-d817-4fab-a5c3-08681070df55)

Connect the motors to the motor driver board. Follow the wiring diagram provided to ensure accurate connections. Proper wiring is essential for the correct operation of the motors and overall functionality of the rover.

### Step 4: Breadboard Setup

Attach the NodeMCU board to a breadboard or dot board. This will facilitate easy connections and modifications. Ensure that the NodeMCU is properly aligned and secured on the breadboard for stability.

### Step 5: Wiring Connections

Use jumper wires to connect the motor driver board to the NodeMCU board. Follow the circuit diagram closely to ensure correct wiring. Accurate connections are crucial for the proper communication between the components.

### Step 6: Battery Installation

Install the battery holder onto the car chassis. Connect the battery holder to the power connectors on the motor driver board. Ensure that the connections are secure and that the batteries are properly positioned in the holder.

### Step 7: NodeMCU Connection

Connect the NodeMCU board to your computer using a USB cable. This connection will allow you to upload the code and configure the settings for your rover.

### Step 8: Blynk App Setup

1. **Download and Install Blynk App**: Begin by downloading the Blynk app from your smartphone’s app store. Install it and open the application.
2. **Sign Up**: Create a new account or log in using your existing credentials.
3. **Create a New Project**: Click on the “New Project” button. Choose the device as NodeMCU and select the connection type as WiFi. Enter a name for your project and click “Create.” You will be directed to an empty project interface.
4. **Add Widgets**: Add four buttons and one slider to the interface. Customize the widgets to match your control needs. The widgets will control different functions of the rover.
5. **Configure Widgets**: Set up the widgets as follows:
   - Forward Button -> V0
   - Backward Button -> V1
   - Left Button -> V2
   - Right Button -> V3
   - Speed Slider -> V4 (range 0 – 255)

### Step 9: Arduino IDE Setup

Prepare the Arduino IDE for programming the NodeMCU board. Use the following code to control the rover:

```cpp
/* WiFi Rover Project: A Smart Car with NodeMCU and Blynk Integration
 * Code developed by Gourav Patra
 */

#define BLYNK_PRINT Serial
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>

// Motor PINs
#define ENA D0
#define IN1 D1
#define IN2 D2
#define IN3 D3
#define IN4 D4
#define ENB D5

bool forward = 0;
bool backward = 0;
bool left = 0;
bool right = 0;
int Speed;

char auth[] = ""; // Enter your Blynk application auth token here
char ssid[] = ""; // Enter your WiFi network name here
char pass[] = ""; // Enter your WiFi password here

void setup() {
  // Initialize serial communication for debugging
  Serial.begin(9600);
  // Set motor control pins as outputs
  pinMode(ENA, OUTPUT);
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  pinMode(ENB, OUTPUT);
  // Initialize Blynk communication
  Blynk.begin(auth, ssid, pass);
}

// Handle widget input
BLYNK_WRITE(V0) { forward = param.asInt(); }
BLYNK_WRITE(V1) { backward = param.asInt(); }
BLYNK_WRITE(V2) { left = param.asInt(); }
BLYNK_WRITE(V3) { right = param.asInt(); }
BLYNK_WRITE(V4) { Speed = param.asInt(); }

// Control rover movement based on widget inputs
void smartcar() {
  if (forward == 1) {
    Forward();
    Serial.println("Moving Forward");
  } else if (backward == 1) {
    Backward();
    Serial.println("Moving Backward");
  } else if (left == 1) {
    Left();
    Serial.println("Turning Left");
  } else if (right == 1) {
    Right();
    Serial.println("Turning Right");
  } else {
    Stop();
    Serial.println("Stopping");
  }
}

void loop() {
  Blynk.run(); // Run Blynk to handle widget communication
  smartcar();  // Update rover movement based on widget inputs
}

// Motor control functions
void Forward() {
  analogWrite(ENA, Speed);
  analogWrite(ENB, Speed);
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}
void Backward() {
  analogWrite(ENA, Speed);
  analogWrite(ENB, Speed);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}
void Left() {
  analogWrite(ENA, Speed);
  analogWrite(ENB, Speed);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}
void Right() {
  analogWrite(ENA, Speed);
  analogWrite(ENB, Speed);
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}
void Stop() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}
```

### Step 10: Upload the Code

Connect the NodeMCU board to your computer, select the appropriate board and port in the Arduino IDE, and upload the code. Ensure the upload process completes without errors.

### Step 11: Test Your Rover

Once the code is uploaded, insert the batteries into the holder and power on the rover. Open the Blynk app on your smartphone and use the interface to control your rover. Test all functionalities including forward, backward, left, right, and speed adjustments to ensure everything is working as expected.

### Step 12: Troubleshooting and Final Adjustments

If you encounter any issues during testing, review your connections, check the code for errors, and ensure that the Blynk app is correctly configured. Make any necessary adjustments to fine-tune the rover's performance.

## Conclusion

Congratulations on completing the WiFi Rover Project! You’ve successfully built a smart car controlled via WiFi using the NodeMCU and Blynk. This project demonstrates the power of IoT and how you can leverage technology to create interactive and intelligent systems. Keep experimenting with different features and improvements to enhance your project further.
