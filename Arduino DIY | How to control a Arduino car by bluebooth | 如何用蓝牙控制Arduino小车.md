
## 1.Have a look at my achievement first

 ![](https://steemitimages.com/DQmYbgCVJHCuCMbs28gbfLSNct37WqR8SoK8mGJ36vCoLyB/%E5%9B%BE%E7%89%87.png)

![](https://steemitimages.com/DQmdGJwRZT3j6X7YCnDovhcPa33ZvFcHEzRoKRvrLswsZR7/%E5%9B%BE%E7%89%87.png)

## 2.Module compoents

- Arduino UNO minichip  * 1 

  ![](https://steemitimages.com/DQmZsDNzBVZpwgZZjVsaUZyXpcagKUaVzM7UsMCGQ8oeaFt/%E5%9B%BE%E7%89%87.png)

- L298N  Motor driven module * 1

  ![](https://steemitimages.com/DQmZdxyBKPcLBKG6ZgE4YynofTBA8mXTy5kTYYar218WPah/%E5%9B%BE%E7%89%87.png)

- Arduino Bluetooth Bee module * 1

  ![](https://steemitimages.com/DQmZ2awipHignRpdPFqKGxKDJHfFP8zjZeYdVMdjbsrfpvQ/%E5%9B%BE%E7%89%87.png)

-  Arduino Bluetooth  expansion board * 1

- Car chassis * 1

  ![](https://steemitimages.com/DQmYrzybKYyqp52wsKfcEiGGbGeH9GctDxRLv1WFtfLaTCQ/%E5%9B%BE%E7%89%87.png)

- Power supply (7~9V)  * 1

- Wires  * several

> INSTRUCTIONS:
>
> - My car is two wheels driven so I choose the L298N motor driven module.I recommend it to you if you also want to make a two wheels driven car.
> - The Bee bluetooth module I choose  is really convient for me ,which can directly insert the Bluetooth into the expansion board,then connect the bluetooth expansion board with Arduino.
> - The 7-9V power can drive two motors better.If you want more than two motors , you should use a more poweful battery.

------

## 3.How to connect modules with Arduino

![](https://steemitimages.com/DQmNs7ShH6mQ8pQ4kbtngkA7p5iTrA2pZjCYPk9o8EgUn3m/%E5%9B%BE%E7%89%87.png)

|      L298N      | Arduino  OR   Battery |
| :-------------: | :-------------------: |
|       GND       |      Arduino GND      |
|       VCC       |       Battery +       |
|       5V        |      Arduino 5V       |
|       IN1       |      Arduino D4       |
|       IN2       |      Arduino D5       |
|       IN3       |      Arduino D6       |
|       IN4       |      Arduino D7       |
| MOTOR1   OUTPUT |        MOTOR1         |
| MOTOR2   OUTPUT |        MOTOR2         |
---
| ENA  | IN1  | IN2  |  Status  |
| :--: | :--: | :--: | :------: |
|  0   |  X   |  X   |   STOP   |
|  1   |  0   |  0   |  BRAKE   |
|  1   |  0   |  1   | FORWARD  |
|  1   |  1   |  0   | BACKWARD |
|  1   |  1   |  1   |  BRAKE   |

IN1 and IN2 control motor 1.  IN3 and IN4 control motor2 

The form above  is the same for the motor1 as well as the motor2 

----

## 4.Run the motors

- First I simply designed a program to implete running the left wheel forwardly for 2 seconds ,then  stopping  for 2seconds , and running backwardly for 2 seconds ,then stopping for 2 seconds.

  ```
  #define IN1 4 
  #define IN2 5     
  void setup()
  {
    pinMode(IN1,OUTPUT);
    pinMode(IN2,OUTPUT);
  }
  void loop()
  {
    digitalWrite(IN1,HIGH);      
    digitalWrite(IN2,LOW);         //forward
    delay(2000);
    digitalWrite(IN1,LOW);      
    digitalWrite(IN2,LOW);         //stop
    delay(2000);
    digitalWrite(IN1,LOW);      
    digitalWrite(IN2,HIGH);         //backward
    delay(2000);
    digitalWrite(IN1,LOW);      
    digitalWrite(IN2,LOW);         //stop
    delay(2000);
  }
  ```

- Second I simply designed a program to implete running the right wheel forwardly for 2 seconds ,then  stopping  for 2seconds , and running backwardly for 2 seconds ,then stopping for 2 seconds.

  ```
  #define IN3 6 
  #define IN4 7     
  void setup()
  {
    pinMode(IN3,OUTPUT);
    pinMode(IN4,OUTPUT);
  }
  void loop()
  {
    digitalWrite(IN3,HIGH);      
    digitalWrite(IN4,LOW);         //forward
    delay(2000);
    digitalWrite(IN3,LOW);      
    digitalWrite(IN4,LOW);         //stop
    delay(2000);
    digitalWrite(IN3,LOW);      
    digitalWrite(IN4,HIGH);         //backward
    delay(2000);
    digitalWrite(IN3,LOW);      
    digitalWrite(IN4,LOW);         //stop
    delay(2000);
  }
  ```

  ​

***REMEMBER:***

***Before uploading the code ,you should disconnect Arduino with the bluetooth module.***



---

## 5. Simple motion of the car

| WHEEL | FORWARD | BACKWARD | STOP |
| ----- | ------- | -------- | ---- |
| LEFT  | FORWARD | BACKWARD | STOP |
| RIGHT | FORWARD | BACKWARD | STOP |

-----

| WHEEL | TURN LEFT FORWARDLY | TURN LEFT BACKWARDLY | TURN LEFT ON THE SPOT |
| ----- | ------------------- | -------------------- | --------------------- |
| LEFT  | STOP                | STOP                 | BACKWARD              |
| RIGHT | FORWARD             | BACKWARD             | FORWARD               |

---

| WHEEL | TURN RIGHT FORWARDLY | TURN RIGHT BACKWARDLY | TURN RIGHT ON THE SPOT |
| ----- | -------------------- | --------------------- | ---------------------- |
| LEFT  | FORWARD              | BACKWARD              | FORWARD                |
| RIGHT | STOP                 | STOP                  | BACKWARD               |

According to the form above ,we can write the code easily

``` 
#define IN1 4 
#define IN2 5     
#define IN3 6 
#define IN4 7    
void setup()
{
  pinMode(IN1,OUTPUT);
  pinMode(IN2,OUTPUT);
  pinMode(IN3,OUTPUT);
  pinMode(IN4,OUTPUT);

}
void loop()
{
  digitalWrite(IN1,HIGH);      
  digitalWrite(IN2,LOW);         //forward left wheel 
  digitalWrite(IN3,HIGH);      
  digitalWrite(IN4,LOW);         //forward right wheel 
  delay(2000);
  digitalWrite(IN1,LOW);      
  digitalWrite(IN2,LOW);         //stop left wheel 
  digitalWrite(IN3,LOW);      
  digitalWrite(IN4,LOW);         //stop right wheel 
  delay(1000);
  digitalWrite(IN1,LOW);      
  digitalWrite(IN2,HIGH);        //backward left wheel 
  digitalWrite(IN3,LOW);      
  digitalWrite(IN4,HIGH);        //backward right wheel
  delay(2000);
  digitalWrite(IN1,LOW);      
  digitalWrite(IN2,LOW);         //stop left wheel 
  digitalWrite(IN3,LOW);      
  digitalWrite(IN4,LOW);         //stop right wheel 
  delay(3000);
}
```

-----

## 6. Function writing

```
void forward( )
{
  digitalWrite(IN1,HIGH);      
  digitalWrite(IN2,LOW);         
  digitalWrite(IN3,HIGH);      
  digitalWrite(IN4,LOW);         
}

void back( )
{
  digitalWrite(IN1,LOW);      
  digitalWrite(IN2,HIGH);        
  digitalWrite(IN3,LOW);      
  digitalWrite(IN4,HIGH);       
}


void turnLeft( )
{
  digitalWrite(IN1,LOW);      
  digitalWrite(IN2,LOW);         
  digitalWrite(IN3,HIGH);      
  digitalWrite(IN4,LOW);         
}


void turnbackLeft( )
{
  digitalWrite(IN1,LOW);      
  digitalWrite(IN2,LOW);         
  digitalWrite(IN3,LOW);      
  digitalWrite(IN4,HIGH);        
}

void turnRight( )
{
  digitalWrite(IN1,HIGH);      
  digitalWrite(IN2,LOW);         
  digitalWrite(IN3,LOW);      
  digitalWrite(IN4,LOW);         
}

void turnbackRight( )
{
  digitalWrite(IN1,LOW);      
  digitalWrite(IN2,HIGH);        
  digitalWrite(IN3,LOW);      
  digitalWrite(IN4,LOW);        
}

void turnLeftOrigin( )
{
  digitalWrite(IN1,LOW);      
  digitalWrite(IN2,HIGH);        
  digitalWrite(IN3,HIGH);      
  digitalWrite(IN4,LOW);        
}

void turnRightOrigin( )
{
  digitalWrite(IN1,HIGH);      
  digitalWrite(IN2,LOW);        
  digitalWrite(IN3,LOW);      
  digitalWrite(IN4,HIGH);      
}


void _stop()
{
  digitalWrite(IN1,LOW);      
  digitalWrite(IN2,LOW);        
  digitalWrite(IN3,LOW);      
  digitalWrite(IN4,LOW);        
}
```

---

## 7. Function using

> ***Movement process***: 
>
> forward for 2S  ------>  backward for  2S ------>  forwardly left turn 2S ------> backwardly left turn for 2S ------> forwardly Turn right turn for  2S ------>  backwardly turn right for 2S ------>  turn left on the spot for 2S ------>  turn right on the spot for 2S  ------>  stop 3S

```
#define IN1 4 
#define IN2 5    
#define IN3 6 
#define IN4 7     

void forward( );
void back( );
void turnLeft( );
void turnRight( );
void turnbackLeft( );
void turnbackRight( );
void turnLeftOrigin( );
void turnRightOrigin( );
void _stop();          

void setup()
{
  pinMode(IN1,OUTPUT);
  pinMode(IN2,OUTPUT);
  pinMode(IN3,OUTPUT);
  pinMode(IN4,OUTPUT);
}

void loop()
{
    forward( );
    delay(2000);
    back( );
    delay(2000);
    turnLeft( );
    delay(2000);
    turnbackLeft( );
    delay(2000);
    turnRight( );
    delay(2000);
    turnbackRight( );
    delay(2000);
    turnLeftOrigin( );
    delay(2000);
    turnRightOrigin( );
    delay(2000);
    _stop();
    delay(3000);    
}

void forward( )
{
  digitalWrite(IN1,HIGH);      
  digitalWrite(IN2,LOW);         
  digitalWrite(IN3,HIGH);      
  digitalWrite(IN4,LOW);         
}

void back( )
{
  digitalWrite(IN1,LOW);      
  digitalWrite(IN2,HIGH);        
  digitalWrite(IN3,LOW);      
  digitalWrite(IN4,HIGH);       
}


void turnLeft( )
{
  digitalWrite(IN1,LOW);      
  digitalWrite(IN2,LOW);         
  digitalWrite(IN3,HIGH);      
  digitalWrite(IN4,LOW);         
}


void turnbackLeft( )
{
  digitalWrite(IN1,LOW);      
  digitalWrite(IN2,LOW);         
  digitalWrite(IN3,LOW);      
  digitalWrite(IN4,HIGH);        
}

void turnRight( )
{
  digitalWrite(IN1,HIGH);      
  digitalWrite(IN2,LOW);         
  digitalWrite(IN3,LOW);      
  digitalWrite(IN4,LOW);         
}

void turnbackRight( )
{
  digitalWrite(IN1,LOW);      
  digitalWrite(IN2,HIGH);        
  digitalWrite(IN3,LOW);      
  digitalWrite(IN4,LOW);        
}

void turnLeftOrigin( )
{
  digitalWrite(IN1,LOW);      
  digitalWrite(IN2,HIGH);        
  digitalWrite(IN3,HIGH);      
  digitalWrite(IN4,LOW);        
}

void turnRightOrigin( )
{
  digitalWrite(IN1,HIGH);      
  digitalWrite(IN2,LOW);        
  digitalWrite(IN3,LOW);      
  digitalWrite(IN4,HIGH);      
}


void _stop()
{
  digitalWrite(IN1,LOW);      
  digitalWrite(IN2,LOW);        
  digitalWrite(IN3,LOW);      
  digitalWrite(IN4,LOW);        
}

```

---

## 8. Using the bluetooth module 

> This step is the key of our DIY project.
>
> There are several points about that:
>
> - We can use 'Serial.read（）' to read the Bluetooth order  .
> - All the order will be conversed to ASCII code
> - When we receive the order ,we can use ASCII code or single quotation mark to judge what the order is.For example If the mobile terminal sends a "1", then we receive  ASCII code of '1' on the side, so when we want to judge which number we receive  we need to use case '1' or case49 to decide .(Note: ASCII code of 1 is 49).

- Test the bluetooth demo 

   whenever receiving the number '1', LED on Arduino D13 will power on. 

  ```
  void setup()
  {
    pinMode(13,OUTPUT);
    Serial.begin(9600);
  }
  int i;
  void loop()
  {
    if(Serial.available()>0)
    {
        i=Serial.read();
        switch(i)
        {
          case'1':
              {digitalWrite(13,HIGH);break;}
          case'2':
              {digitalWrite(13,LOW);break;}
        }
    }
  }
  ```

  ​

- Use the number one to nine to control the car movement.

  | ORDER |    MOVEMENT    |
  | :---: | :------------: |
  |   1   |    turnLeft    |
  |   2   |    forward     |
  |   3   |   turnRight    |
  |   4   | turnLeftOrigin |
  |   5   |   ***NONE***   |
  |   6   | turnLeftOrigin |
  |   7   |  turnbackLeft  |
  |   8   |      back      |
  |   9   | turnbackRight  |

  ​

```
#define IN1 4 
#define IN2 5    
#define IN3 6 
#define IN4 7    

void forward( );
void back( );
void turnLeft( );
void turnRight( );
void turnbackLeft( );
void turnbackRight( );
void turnLeftOrigin( );
void turnRightOrigin( );
void _stop();          

void setup()
{
  pinMode(IN1,OUTPUT);
  pinMode(IN2,OUTPUT);
  pinMode(IN3,OUTPUT);
  pinMode(IN4,OUTPUT);
  Serial.begin(9600);
}
int i;
void loop()
{
    if(Serial.available())
    {
      i=Serial.read();
      
      switch(i)
      {
        case'1':
          {turnLeft( );  break; }
        case'2':
          {forward( );   break;}
        case'3':
          {turnRight( );   break;}
        case'4':
          {turnLeftOrigin( );   break;}
        case'5':
          {_stop();   break;}
        case'6':
          {turnRightOrigin( );   break;}
        case'7':
          {turnbackLeft( );   break;}
        case'8':
          {back( );   break;}
        case'9':
          {turnbackRight( );   break;}
      }
    }    
}
void forward( )
{
  digitalWrite(IN1,HIGH);      
  digitalWrite(IN2,LOW);         
  digitalWrite(IN3,HIGH);      
  digitalWrite(IN4,LOW);         
}

void back( )
{
  digitalWrite(IN1,LOW);      
  digitalWrite(IN2,HIGH);        
  digitalWrite(IN3,LOW);      
  digitalWrite(IN4,HIGH);       
}


void turnLeft( )
{
  digitalWrite(IN1,LOW);      
  digitalWrite(IN2,LOW);         
  digitalWrite(IN3,HIGH);      
  digitalWrite(IN4,LOW);         
}


void turnbackLeft( )
{
  digitalWrite(IN1,LOW);      
  digitalWrite(IN2,LOW);         
  digitalWrite(IN3,LOW);      
  digitalWrite(IN4,HIGH);        
}

void turnRight( )
{
  digitalWrite(IN1,HIGH);      
  digitalWrite(IN2,LOW);         
  digitalWrite(IN3,LOW);      
  digitalWrite(IN4,LOW);         
}

void turnbackRight( )
{
  digitalWrite(IN1,LOW);      
  digitalWrite(IN2,HIGH);        
  digitalWrite(IN3,LOW);      
  digitalWrite(IN4,LOW);        
}

void turnLeftOrigin( )
{
  digitalWrite(IN1,LOW);      
  digitalWrite(IN2,HIGH);        
  digitalWrite(IN3,HIGH);      
  digitalWrite(IN4,LOW);        
}

void turnRightOrigin( )
{
  digitalWrite(IN1,HIGH);      
  digitalWrite(IN2,LOW);        
  digitalWrite(IN3,LOW);      
  digitalWrite(IN4,HIGH);      
}


void _stop()
{
  digitalWrite(IN1,LOW);      
  digitalWrite(IN2,LOW);        
  digitalWrite(IN3,LOW);      
  digitalWrite(IN4,LOW);        
}
```

![](https://steemitimages.com/DQmSu9wVVqoS76kaMVrm4npbxZx136qr6LFJnJHMvf6RBPL/%E5%9B%BE%E7%89%87.png)

---

## That is all .Thanks for visiting and reading patiently.
