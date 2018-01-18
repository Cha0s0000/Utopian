#### What Will I Learn?
- How to connect  4*4 matrix keyboard with Arduino 
- Common  functions of using 4*4 matrix keyboard 

---

#### Requirements
- Arduino Uno board;

- 4*4 matrix keyboard

  ![image.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516251154/i41zbdiw4kvhkbo9hsol.png)

- 8 wire

---

#### Difficulty
- Basic

---

#### Tutorial Contents
##### 1.Connection

- Keypad Pin R1 –> Arduino Pin 2

- Keypad Pin R2 –> Arduino Pin 3

- Keypad Pin R3 –> Arduino Pin 4

- Keypad Pin R4 –> Arduino Pin 5

- Keypad Pin C1 –> Arduino Pin 6

- Keypad Pin C2 –> Arduino Pin 7

- Keypad Pin C3 –> Arduino Pin 8

- Keypad Pin C4 –> Arduino Pin 9

  ![image.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516251417/aecno6pulkwrgx9d7yuw.png)

  ![image.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516251465/zbizhev2olvicgsdjed0.png)


##### **2.Using 4*4 matrix keyboard **

- **Download library**

  ​	In the Arduino IDE 1.6.2 or later, click  project - > Load Library - > in the library management, search Keypad and then install it.You can also download the library [here](http://playground.arduino.cc/uploads/Code/keypad.zip), and add it manually to the IDE.

- **Initialization settings**

  > keypad(makeKeymap(userKeymap), row[], col[], rows, cols)

  ```
  const byte rows = 4; //four rows
  const byte cols = 3; //three columns
  char keys[rows][cols] = {
    {'1','2','3'},
    {'4','5','6'},
    {'7','8','9'},
    {'#','0','*'}
  };
  byte rowPins[rows] = {5, 4, 3, 2}; //connect to the row pinouts of the keypad
  byte colPins[cols] = {8, 7, 6}; //connect to the column pinouts of the keypad
  Keypad keypad = Keypad( makeKeymap(keys), rowPins, colPins, rows, cols );
  ```

- **Common function**

  - Initializes the internal keymap to be equal to userKeymap

    [See File -> Examples -> Keypad -> Examples -> CustomKeypad]

    > ## void begin(makeKeymap(userKeymap))

  - This function will wait forever until someone presses a key. Warning: It blocks all other code until a key is pressed. That means no blinking LED's, no LCD screen updates, no nothing with the exception of interrupt routines.

    > ## char waitForKey()

  - This function will returns the key that is pressed, if any. This function is non-blocking.

    > ## char getKey()

  - This function will returns the current state of any of the keys.
    The four states are IDLE, PRESSED, RELEASED and HOLD.

    > ## KeyState getState()

  - This function will  let you know when the key has changed from one state to another. For example, instead of just testing for a valid key you can test for when a key was pressed.

    > ## boolean keyStateChanged()

  - This function will set the amount of milliseconds the user will have to hold a button until the HOLD state is triggered.

    > ## setHoldTime(unsigned int time)

  - This function will set the amount of milliseconds the keypad will wait until it accepts a new keypress/keyEvent. This is the "time delay" debounce method.

    > ## setDebounceTime(unsigned int time)

  - This function will  trigger an event if the keypad is used. You can load an example in the Arduino IDE.

    > ## addEventListener(keypadEvent)

- **Example**

  ```
  #include <Keypad.h>

  const byte ROWS = 4; //four rows
  const byte COLS = 3; //three columns
  char keys[ROWS][COLS] = {
    {'1','2','3'},
    {'4','5','6'},
    {'7','8','9'},
    {'#','0','*'}
  };
  byte rowPins[ROWS] = {5, 4, 3, 2}; //connect to the row pinouts of the keypad
  byte colPins[COLS] = {8, 7, 6}; //connect to the column pinouts of the keypad

  Keypad keypad = Keypad( makeKeymap(keys), rowPins, colPins, ROWS, COLS );

  void setup(){
    Serial.begin(9600);
  }

  void loop(){
    char key = keypad.getKey();

    if (key != NO_KEY){
      Serial.println(key);
    }
  }
  ```

##  

---

#### Curriculum

- [[Arduino basics tutorials] use MQ135 air quality detecting module](https://utopian.io/utopian-io/@cha0s0000/arduino-basics-tutorials-use-mq135-air-quality-detecting-module)


- [[Arduino basics tutorials] use LCD 1602 display module](https://utopian.io/utopian-io/@cha0s0000/arduino-basics-tutorials-use-lcd-1602-display-module)
- [Test esp8266 by AT command](https://utopian.io/utopian-io/@cha0s0000/test-esp8266-by-at-command)
- [Arduino DIY | How to control a Arduino car by bluebooth |](https://utopian.io/utopian-io/@cha0s0000/arduino-diy-or-how-to-control-a-arduino-car-by-bluebooth-or-arduino)