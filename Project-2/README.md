# Space Race Game - Debugging
## Components
* ADXL345 Accelerometer Module
* ESP32 Board
* Nokia 5110 Display
## Debugging
### Accelerometer Module
To see if the Accelerometer Module is working properly, connect VCC, GND and the X and Y pins to any two Analog input pins. Upload the following:
```
#include <SoftwareSerial.h>
const int x=A0;
const int y=A1;
int xh, yh;
int xcord, ycord;
void setup()
{
  pinMode(x,INPUT);
  pinMode(y,INPUT);
  Serial.begin(9600);
}
void loop()
{
  xh=analogRead(x);
  yh=analogRead(y);
  xcord=map(xh,286,429,100,999);
  ycord=map(yh,282,427,100,800);
  Serial.print(xcord);
  Serial.print(ycord);
}
```
This should print X and Y coordinates on the Serial Monitor, if the coordinates do not match the physical movement, the Accelerometer Module is either not connected properly or faulty.
### Microcontroller and Display Module
To see if a ship object gets displayed on the screen (you can change the location of the object to see if it pixels light up at all locations):
```
#include <SPI.h>
#include <Adafruit_GFX.h>
#include <Adafruit_PCD8544.h>
static const unsigned char PROGMEM ship[] =
{
B00000000,B00000000,
B00000001,B00000000,
B00000011,B10000000,
B00000010,B10000000,
B00000010,B11000000,
B00000111,B11000000,
B00001101,B11100000,
B00011111,B11110000,
B00111111,B11111000,
B01111111,B11111100,
B01111111,B11111100,
B01111111,B11111100,
B00011111,B11110000,
B00000111,B11100000,
B00000000,B00000000,
};
void setup()   {
  Serial.begin(9600); //Serial Monitor for Debugging

  display.begin(); //Begin the LCD communication
  display.setContrast(30); //Set the contrast of the display
  display.clearDisplay();   // clears the screen and start new
}
void loop() {
  display.clearDisplay();
  display.drawBitmap(2, 32, ship, 15, 15, BLACK);
}
```
To see if texts gets displayed properly, add the following lines under void loop():
```
display.clearDisplay();  
display.setCursor(20,2);
display.println("HELLO WORLD");
```
This should display 'HELLO WORLD' on the display.

If any of the above does not work, check the connections, or else, it is a faulty display.
