#### What Will I Learn?
**Two ways to use MQ135 air quality detecting module**

> MQ135 is mainly used to detect the concentration of carbon dioxide, alcohol, benzene, NOx, ammonia and other gases in the air.

- Do not use library files
- Using a library document 

---

#### Requirements
- Arduino Uno  *1

- MQ135 air quality detecting module  *1

  ![图片.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516207323/s0ntzfeov5zdc7jq8p4n.png)

- wire  *3

- Bakery board  *1

---

#### Difficulty
- Basic

---

#### Tutorial Contents

**1.Do not use library files**

- Connection

  ![图片.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516207151/q3hblj0ofebta4x7zeap.png)

  | MQ135 |      | Arduino |
  | ----- | ---- | ------- |
  | VCC   | ->   | 5V      |
  | AOUT  | ->   | A0      |
  | GND   | ->   | GND     |

- Sample program

```
const int gasSensor =0;
void setup(){
  Serial.begin(9600);      // sets the serial port to 9600
}
void loop(){
  float voltage;
  voltage = getVoltage(gasSensor);
  
  Serial.println(voltage);
  delay(1000);
}
 
float getVoltage(int pin){
  return (analogRead(pin) * 0.004882814);
  // This equation converts the 0 to 1023 value that analogRead()
  // returns, into a 0.0 to 5.0 value that is the true voltage
  // being read at that pin.
}
```



**2. use the <MQ135.h> Library**

The required materials and wiring are the same as the way which does not use of the library files. The only difference is that you need to download the MQ135 library.

- Load library files

  Download the [MQ135 file]([https://codeload.github.com/GeorgK/MQ135/zip/master](https://link.jianshu.com?t=https://codeload.github.com/GeorgK/MQ135/zip/master)) .

  Open the  Arduino IDE, click the project - > Load Library - > add a.Zip library, then select the MQ135 file .

  ​

- Initialization settings

  Before you start using it, you need to energized it and preheat it for 12-24 hours. After that, perform the following procedures in the environment of 20 degree C/35% air temperature, and read the value of RZERO.

  ```
  #include "MQ135.h"
  const int ANALOGPIN=0;
  MQ135 gasSensor = MQ135(ANALOGPIN);
  void setup(){
    Serial.begin(9600);      // sets the serial port to 9600
  }
  void loop(){
    float rzero = gasSensor.getRZero();
    Serial.println(rzero);
    delay(1000);
  }
  ```

  Write the read value in the MQ135.h file in the library file.

  > ***Where to find the MQ135.h file***
  >
  > You can select the file > preferences in Arduino IDE, in the pop-up window, you can see the project folder location
  >
  >  eg: d:\Users\weiwe\Documents\Arduino, and then you find the folder inside the libraries->MQ135-master->MQ135.h file can be opened, the value of RZERO is to be filled in.

  ```
  #define RZERO 76.63
  ```

  ​

- Start detecting air quality

```
#include "MQ135.h"
const int ANALOGPIN=0;
MQ135 gasSensor = MQ135(ANALOGPIN);
void setup(){
  Serial.begin(9600);      // sets the serial port to 9600
}
void loop(){
  float ppm = gasSensor.getPPM();
  Serial.println(ppm);
  delay(1000);
}
```

---

## Tips

- Experiments show that MQ-135 can detect the above mentioned gases, but it does not distinguish them. If you want to test the content of a specific gas in the air, you may need to find other better sensors.

- MQ-135 uses a heating module to preheat the sensor, so it is suggested that a power supply with a larger capacity will not run out of electricity quickly.

- Indoor air quality control table

  ![图片.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516208012/wwerzjnpjde16lmiikkd.png)

---

## Curriculum

- [[Arduino basics tutorials] use LCD 1602 display module](https://utopian.io/utopian-io/@cha0s0000/arduino-basics-tutorials-use-lcd-1602-display-module)
- [Test esp8266 by AT command](https://utopian.io/utopian-io/@cha0s0000/test-esp8266-by-at-command)
- [Arduino DIY | How to control a Arduino car by bluebooth |](https://utopian.io/utopian-io/@cha0s0000/arduino-diy-or-how-to-control-a-arduino-car-by-bluebooth-or-arduino)



---

# Thanks for reading patiently!