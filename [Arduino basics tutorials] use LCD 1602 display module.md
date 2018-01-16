**Before reading:**
I write it on my PC typora ,but  the passage may go wrong when I copy
 the passage to Utopian post editor.So I upload to the Github. If the 
article layout  make you dazzled please read on the below url.Thanks.
[The more readable version](https://github.com/Cha0s0000/Utopian/blob/master/Test%20esp8266%20by%20AT%20command.md)

---

LCD display module      

> There are two ways to connect the LCD display module to the Arduino:

1. Directly connected to Arduino
2. Connect to Arduino by using I2C

### 1. Directly connected to Arduino

The advantage of connecting directly to Arduino is that you don't have to buy a switch board now, but the result is a large amount of using  Arduino IO. If your project doesn't have many sensors, it's OK, but if you need to connect many sensors or other accessories, your IO will be in urgent need.

#### Required materials

- 1x Arduino UNO
- 1x LCD 16x2
- 1x 10KΩ Rotating rheostat
- 1x Bakery board

#### Wiring 

![](https://steemitimages.com/DQmPQAKqXz3ij63z63NX4AErJ2L8XeAeWGyUWgGDYGMMEMR/%E5%9B%BE%E7%89%87.png)



| LCD  |      | Arduino |
| ---- | ---- | ------- |
| RS   | ->   | 12      |
| E    | ->   | 11      |
| D4   | ->   | 5       |
| D5   | ->   | 4       |
| D6   | ->   | 3       |
| D7   | ->   | 2       |
| VCC  | ->   | 5V      |
| R/W  | ->   | GND     |
| GND  | ->   | GND     |

| LCD  |      | Rotating rheostat |
| ---- | ---- | :---------------: |
| VCC  | ->   |   the left pin    |
| Vo   | ->   |  the middle pin   |
| R/W  | ->   |   the right pin   |
| GND  | ->   | another right pin |

#### Load library files

- In the Arduino IDE 1.6.2 or later, click project - > Load Library - > in the library management, search LiquidCrystal and then install it.

#### Sample code

```
// include the library code:
#include <LiquidCrystal.h>

// initialize the library with the numbers of the interface pins
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

void setup() {
  // set up the LCD's number of columns and rows: 
  lcd.begin(16, 2);
  // Print a message to the LCD.
  lcd.print("hello, world!");
}

void loop() {
  // set the cursor to column 0, line 1
  // (note: line 1 is the second row, since counting begins with 0):
  lcd.setCursor(0, 1);
  // print the number of seconds since reset:
  lcd.print(millis()/1000);
}

```



------

### 2.Connect to Arduino through the PCF8574T switch board      

In this way, you can save a lot of Arduino IO port, but  you have to buy a PCF8574T switch board.



#### Required materials

- 1x Arduino UNO
- 1x LCD 16x2
- 1x PCF8574T switch board
- Electric soldering iron, soldering tin, rosin, etc

#### Wiring 

First, weld the adapter board to the LCD display

| PCF8574T |      | Arduino |
| -------- | ---- | ------- |
| GND      | ->   | GND     |
| VCC      | ->   | 5V      |
| SDA      | ->   | A4      |
| SCL      | ->   | A5      |

> If your A4 and A5 ports have been occupied, you can also use the two top Arduino IO ports without marked characters, that is, the top two ports of the row of D0-D13.

- SCL -> the topside pin
- SDA -> the second top pin

#### Scan the I2C address

Use the following code in Arduino IDE and execute it. Click select tools - > serial port monitor,set  the lower right corner of the baud rate to 115200, you can read out the I2C address, as shown below.

```
// I2C Scanner
// Written by Nick Gammon
// Date: 20th April 2011
#include <Wire.h>
void setup() { 
    Serial.begin (115200); // Leonardo: wait for serial port to connect 
    while (!Serial) { } 
    Serial.println (); 
    Serial.println ("I2C scanner. Scanning ..."); 
    byte count = 0; 
    Wire.begin(); 
    for (byte i = 8; i < 120; i++) { 
        Wire.beginTransmission (i); 
        if (Wire.endTransmission () == 0) { 
          Serial.print ("Found address: "); 
          Serial.print (i, DEC); 
          Serial.print (" (0x"); 
          Serial.print (i, HEX); 
          Serial.println (")"); 
          count++; 
          delay (1); // maybe unneeded? 
        } // end of good response 
    } // end of for loop 
    Serial.println ("Done."); 
    Serial.print ("Found "); 
    Serial.print (count, DEC); 
    Serial.println (" device(s).");
} // end of setup
void loop() {}

```

![图片.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516121120/v9nnnhoxvreioudn3fgp.png)

The address displayed in the serial monitor is ：0x3F

#### Load library files

Download the latest New LiquidCrystal and add it to your Arduino IDE manually.

（ps:Remember to modify your I2C address and change the LiquidCrystal_I2C LCD (0x3F, 2,1,0,4,5,6,7) to your real address.）

#### Sample code

```
/* Demonstration sketch for PCF8574T I2C LCD Backpack Uses library from https://bitbucket.org/fmalpartida/new-liquidcrystal/downloads GNU General Public License, version 3 (GPL-3.0) */ 
#include <Wire.h> 
#include <LCD.h> 
#include <LiquidCrystal_I2C.h> 
LiquidCrystal_I2C lcd(0x3F,2,1,0,4,5,6,7); // 0x27 is the I2C bus address for an unmodified backpack 
void setup() { // activate LCD module 
  lcd.begin (16,2); // for 16 x 2 LCD module 
  lcd.setBacklightPin(3,POSITIVE); 
  lcd.setBacklight(HIGH); 
} 
void loop() { 
  lcd.home (); // set cursor to 0,0 
  lcd.print(" tronixlabs.com"); 
  lcd.setCursor (0,1); // go to start of 2nd line 
  lcd.print(millis()); 
  delay(1000); 
  lcd.setBacklight(LOW); // Backlight off delay(250);        
  lcd.setBacklight(HIGH); // Backlight on delay(1000); 
}

```

---

### Conclusion

1. Remember to modify your I2C address to your real adress
2. If there is  any wrong while using the library , delete`NewliquidCrystal_1.3.4.zip`library and download again.

---

## Thanks for reading patiently.