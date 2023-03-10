# MH Electronics driver and L298 MotorDriver with Arduino

![image](https://user-images.githubusercontent.com/14288989/218312073-0a10213f-58cf-47e1-a1a2-9c6531d9cd30.png)


https://lastminuteengineers.com/l298n-dc-stepper-driver-arduino-tutorial/

![image](https://user-images.githubusercontent.com/14288989/218312115-49764728-9ee0-4c35-b69f-47b4f651fe6d.png)

If the driver does not work.

- All the 3 pins on the motor driver are connected ( 4 and 5 for direction, and 3 for speed control (
- Check that the enA is coded to send a value between  0-255, for speed control
- Check that the stub is removed on enA and enB.
- If the motor is rated 12v then you need to give 14v, but I gave 12v and the motor got 10v so it ran slow.
- If motor1 does not work, that means that part of the circuit is broke, so use motor2 side.

Mine did not work as it had one side of the circuit broken.

- I used 12v power adapter 1A.
- Changed from one side of the circuit to another as one side of the circuit was broken

Used this code and it works.  5 and 4 for turning CW or CCW, with 3 controlling the speed.


```
int enA = 9;
int in1 = 8;
int in2 = 7;
// Motor B connections
int enB = 3;
int in3 = 5;
int in4 = 4;

void setup() {
  // Set all the motor control pins to outputs
  pinMode(enA, OUTPUT);
  pinMode(enB, OUTPUT);
  pinMode(in1, OUTPUT);
  pinMode(in2, OUTPUT);
  pinMode(in3, OUTPUT);
  pinMode(in4, OUTPUT);
  
  // Turn off motors - Initial state
  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);
}

void loop() {
  directionControl();
  delay(1000);
  speedControl();
  delay(1000);
}

// This function lets you control spinning direction of motors
void directionControl() {
  // Set motors to maximum speed
  // For PWM maximum possible values are 0 to 255
  analogWrite(enA, 255);
  analogWrite(enB, 255);

  // Turn on motor A & B
  digitalWrite(in1, HIGH);
  digitalWrite(in2, LOW);
  digitalWrite(in3, HIGH);
  digitalWrite(in4, LOW);
  delay(2000);
  
  // Now change motor directions
  digitalWrite(in1, LOW);
  digitalWrite(in2, HIGH);
  digitalWrite(in3, LOW);
  digitalWrite(in4, HIGH);
  delay(2000);
  
  // Turn off motors
  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);
}

// This function lets you control speed of the motors
void speedControl() {
  // Turn on motors
  digitalWrite(in1, LOW);
  digitalWrite(in2, HIGH);
  digitalWrite(in3, LOW);
  digitalWrite(in4, HIGH);
  
  // Accelerate from zero to maximum speed
  for (int i = 0; i < 256; i++) {
    analogWrite(enA, i);
    analogWrite(enB, i);
    delay(20);
  }
  
  // Decelerate from maximum speed to zero
  for (int i = 255; i >= 0; --i) {
    analogWrite(enA, i);
    analogWrite(enB, i);
    delay(20);
  }
  
  // Now turn off motors
  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);
}

```



### Mh electronics. Motor driver


![image](https://user-images.githubusercontent.com/14288989/219933653-1ad4b41a-49a3-45a8-9be3-d94bc8d25bbe.png)


Compiling


![image](https://user-images.githubusercontent.com/14288989/219933661-69a95418-0406-4599-9dc2-5ad1b812295f.png)


DC motor connections


![image](https://user-images.githubusercontent.com/14288989/219933720-e7aaeefd-8eb8-4407-a2e6-7a0473eeec6f.png)


Key point in the Arduino sketch:

![image](https://user-images.githubusercontent.com/14288989/219933698-a6e3b1ad-4bb6-4470-bde9-42254aa783f9.png)



```
// Adafruit Motor shield library
// copyright Adafruit Industries LLC, 2009
// this code is public domain, enjoy!

#include <AFMotor.h>

AF_DCMotor motor(1);

void setup() {
  Serial.begin(9600);           // set up Serial library at 9600 bps
  Serial.println("Motor test!");

  // turn on motor
  motor.setSpeed(200);
 
  motor.run(RELEASE);
}

void loop() {
  uint8_t i;
  
  Serial.print("tick");
  
  motor.run(FORWARD);
  for (i=0; i<255; i++) {
    motor.setSpeed(i);  
    delay(10);
 }
 
  for (i=255; i!=0; i--) {
    motor.setSpeed(i);  
    delay(10);
 }
  
  Serial.print("tock");

  motor.run(BACKWARD);
  for (i=0; i<255; i++) {
    motor.setSpeed(i);  
    delay(10);
 }
 
  for (i=255; i!=0; i--) {
    motor.setSpeed(i);  
    delay(10);
 }
  

  Serial.print("tech");
  motor.run(RELEASE);
  delay(1000);
}

```
