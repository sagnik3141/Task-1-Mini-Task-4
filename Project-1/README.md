# Smartphone Accelerometer Controlled Car - Debugging
## Components
* Smartphone with Bluetooth
* ESP32 (Bluetooth functionality)
* L298 Motor Driver
* Motors
## Debugging
Debugging this project can be divided into three parts:
### Software + Bluetooth Connectivity
To verify if the Blutooth connection between the smartphone and the ESP32 Board has been established properly, we can print the values sent over Bluetooth for the corresponding buttons pressed on the app. This can be done by uploading the following after establishing Bluetooth connection.
```
#include <SoftwareSerial.h>

SoftwareSerial BT(10, 11); //TX, RX respetively
String readdata;

void setup() {
 BT.begin(9600);
 Serial.begin(9600);
  pinMode(3, OUTPUT); 
  pinMode(4, OUTPUT); 
  pinMode(5, OUTPUT); 
  pinMode(6, OUTPUT); 

}

void loop() {
  while (BT.available()){ 
  delay(10);
  char c = BT.read();
  readdata += c; 
  } 
  println(readdata);
  readdata="";}}
```
If it does not work as expected, the most likely cause is that the Bluetooth pins are connected incorrectly. Check SoftwareSerial BT() command and match it with the physical connections.
### Motors
Connect all the motors to the pins of the ESP32 and set them at HIGH one by one and see if they work.
### L298 Motor Driver
Make the necessary connections between the L298 Motor Driver and the Motors and also with the ESP32 Board.

Make sure pins 3 is front right, 5 is front left, 4 is back left and 6 is back right.

To check if the car turns as expected, upload the fowllowing:
```
#include <SoftwareSerial.h>

SoftwareSerial BT(10, 11); //TX, RX respetively
String readdata;

void setup() {
 BT.begin(9600);
 Serial.begin(9600);
  pinMode(3, OUTPUT); 
  pinMode(4, OUTPUT); 
  pinMode(5, OUTPUT); 
  pinMode(6, OUTPUT); 

}

void loop() {
  while (BT.available()){ 
  delay(10);
  char c = BT.read();
  readdata += c; 
  } 
  digitalWrite(3, HIGH);
    digitalWrite (4, LOW);
    digitalWrite(5,HIGH);
    digitalWrite(6,LOW);
    delay(100);
  readdata="";}}
```
Uploading this should drive the car forward.

Replace the 5 lines before the last line with the following and see if it functions as expected.
```
digitalWrite(3, LOW);
    digitalWrite(4, HIGH);
    digitalWrite(5, LOW);
    digitalWrite(6,HIGH);
    delay(100);
```
This should drive the car backwards.
```
    digitalWrite (3,HIGH);
    digitalWrite (4,LOW);
    digitalWrite (5,LOW);
    digitalWrite (6,HIGH);
    delay (100);
```
This should turn the car left.
```
   digitalWrite (3, LOW);
   digitalWrite (4, HIGH);
   digitalWrite (5, HIGH);
   digitalWrite (6, LOW);
   delay (100);
```
This should turn the car right.
```
   digitalWrite (3, LOW);
   digitalWrite (4, LOW);
   digitalWrite (5, LOW);
   digitalWrite (6, LOW);
   delay (100);
```
The car should remain stationary.
