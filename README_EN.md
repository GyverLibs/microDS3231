This is an automatic translation, may be incorrect in some places. See sources and examples!

#microDS3231
Lightweight RTC DS3231 library for Arduino
- Read and write time
- Output to char* and String
- Read temperature sensor

### Compatibility
Compatible with all Arduino platforms (using Arduino functions)

## Content
- [Install](#install)
- [Initialization](#init)
- [Usage](#usage)
- [Example](#example)
- [Versions](#versions)
- [Bugs and feedback](#feedback)

<a id="install"></a>
## Installation
- The library can be found under the name **microDS3231** and installed through the library manager in:
    - Arduino IDE
    - Arduino IDE v2
    - PlatformIO
- [Download library](https://github.com/GyverLibs/microDS3231/archive/refs/heads/main.zip) .zip archive for manual installation:
    - Unzip and put in *C:\Program Files (x86)\Arduino\libraries* (Windows x64)
    - Unzip and put in *C:\Program Files\Arduino\libraries* (Windows x32)
    - Unpack and put in *Documents/Arduino/libraries/*
    - (Arduino IDE) automatic installation from .zip: *Sketch/Include library/Add .ZIP library…* and specify the downloaded archive
- Read more detailed instructions for installing libraries [here] (https://alexgyver.ru/arduino-first/#%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE% D0%B2%D0%BA%D0%B0_%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA)

<a id="init"></a>
## Initialization
```cpp
MicroDS3231 rtc; // default address 0x68
MicroDS3231rcranberry tc(address); // specify your address
```

<a id="usage"></a>
## Usage
```cpp
boolbegin(); // initialization, will return true if RTC is found
void setTime(uint8_tparam); // set time == compile time
void setTime(DateTime); // set from DateTime structure
void setTime(int8_t seconds, int8_t minutes, int8_t hours, int8_t date, int8_t month, int16_t year); // set the time
void setHMSDMY(int8_t hours, int8_t minutes, int8_t seconds, int8_t date, int8_t month, int16_t year); // set time type 2
    
// DateTime structure
uint8_t second;
uint8_t minute;
uint8_t hour;
uint8_t day; // day of the week (Mon.. Sun - 1.. 7)
uint8_tdate;
uint8_tmonth;
uint16_t year;

DateTime getTime(); // get into a DateTime structure
uint8_t getSeconds(); // get seconds
uint8_t getMinutes(); // get minutes
uint8_t getHours(); // get clock
uint8_t getDay(); // get the day of the week
uint8_t getDate(); // get number
uint16_t getYear(); // get the year
uint8_t getMonth(); // get the month
    
String getTimeString(); // get the time as a string like HH:MM:SS
String getDateString(); // get the date as a string like DD.MM.YYYY
void getTimeChar(char*array); // get time as char array [8] like HH:MM:SS
void getDateChar(char*array); // get date as char array [10] like DD.MM.YYYY
    
bool lostPower(); // check for power reset
float getTemperatureFloat(); // get float temperature
int getTemperature(); // get temperature int
```

<a id="example"></a>
## Example
See **examples** for other examples!
```cpp
// demo of library features
#include <microDS3231.h>
MicroDS3231 rtc;

void setup() {
  Serial.begin(9600);
  
  // check if the module is on the i2c line
  // calling rtc.begin() is not required to work
  if (!rtc.begin()) {
    Serial.println("DS3231 not found");
    for(;;);
  }
  
  // ======== SETTING THE TIME AUTOMATICALLY ========
  // rtc.setTime(COMPILE_TIME);// set time == compile time
  
  // visually cumbersome, but more "memory-friendly" way to set compile time
  rtc.setTime(BUILD_SEC, BUILD_MIN, BUILD_HOUR, BUILD_DAY, BUILD_MONTH, BUILD_YEAR);
    
  if (rtc.lostPower()) { // executed when the battery is reset
    Serial.println("lost power!");
    // here you can set time == compile time once
  }
  
  // ======== SETTING THE TIME MANUALLY ========
  // you can set the time manually in two ways (substitute real numbers)
  //rtc.setTime(SEC, MIN, HOUR, DAY, MONTH, YEAR);
  //rtc.setHMSDMY(HOUR, MIN, SEC, DAY, MONTH, YEAR);
  
  // you can also set the time via DateTime
  /*
  DateTime now;
  now second = 0;
  now.minute = 10;
  now.hour = 50;
  now.date = 2;
  now.month = 9;
  now.year = 2021;
  
  rtc.setTime(now); // upload to RTC
  */
}

void loop() {
  // getting and outputting each component
  Serial.print(rtc.getHours());
  serial print(":");
  Serial.print(rtc.getMinutes());
  serial print(":");
  Serial.print(rtc.getSeconds());
  serial print(" ");
  Serial.print(rtc.getDay());
  serial print(" ");
  Serial.print(rtc.getDate());
  serial print("/");
  Serial.print(rtc.getMonth());
  serial print("/");
  Serial.println(rtc.getYear());
  
  /*
  // possible via DateTime (more optimal):
  DateTime now = rtc.getTime();
  Serial print(now hour);
  serial print(":");
  Serial.print(now.minute);
  serial print(":");
  Serial.print(now.second);
  serial print(" ");
  Serial.print(now.day);
  serial print(" ");
  Serial.print(now.date);
  serial print("/");
  Serial.print(now.month);
  serial print("/");
  serial.println(now.year);
  */
  
  // display chip temperature
  Serial.println(rtc.getTemperatureFloat());
  //Serial.println(rtc.getTemperature());
  
  // displaying the time as a ready-made string String
  Serial.println(rtc.getTimeString());
  
  // displaying the date as a ready-made string String
  Serial.println(rtc.getDateString());

  // output timecranberry eni via char array
  chartime[8];
  rtc.getTimeChar(time);
  Serial println(time);
  
  // output date via char array
  date[10];
  rtc.getDateChar(date);
  Serial println(date);
  
  Serial.println();
  delay(500);
}
```

<a id="versions"></a>
## Versions
- v1.2 - added restrictions on numbers entered in setTime. Also, you can’t enter February 29, alas =)
- v1.3 - fixed hang when module is disabled but polled
- v1.4 - minor fix
- v2.0 - new features, optimization and simplification
- v2.1 - added temperature output, output to String and char
- v2.2 - fixed days of the week (Mon-Sun 1-7)
- v2.3 - minor fixes, optimization, date display order changed
- v2.4 - fixed compilation time setting
- v2.5 - added begin to check if a module is on the line
- v2.6 - fixed negative temperatures
    
<a id="feedback"></a>
## Bugs and feedback
When you find bugs, create an **Issue**, or better, immediately write to the mail [alex@alexgyver.ru](mailto:alex@alexgyver.ru)
The library is open for revision and your **Pull Request**'s!