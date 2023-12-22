This is an automatic translation, may be incorrect in some places. See sources and examples!

# Microds3231
Light library for working with RTC DS3231 for Arduino
- Reading and recording time
- Conclusion in Char* and String
- Reading the sensor temperature

## compatibility
Compatible with all arduino platforms (used arduino functions)

## Content
- [installation] (# Install)
- [initialization] (#init)
- [use] (#usage)
- [Example] (# Example)
- [versions] (#varsions)
- [bugs and feedback] (#fedback)

<a id="install"> </a>
## Installation
- The library can be found by the name ** Microds3231 ** and installed through the library manager in:
    - Arduino ide
    - Arduino ide v2
    - Platformio
- [download library] (https://github.com/gyverlibs/microds3231/archive/refs/heads/main.zip). Zip archive for manual installation:
    - unpack and put in * C: \ Program Files (X86) \ Arduino \ Libraries * (Windows X64)
    - unpack and put in * C: \ Program Files \ Arduino \ Libraries * (Windows X32)
    - unpack and put in *documents/arduino/libraries/ *
    - (Arduino id) Automatic installation from. Zip: * sketch/connect the library/add .Zip library ... * and specify downloaded archive
- Read more detailed instructions for installing libraries [here] (https://alexgyver.ru/arduino-first/#%D0%A3%D1%81%D1%82%D0%B0%BD%D0%BE%BE%BE%BED0%B2%D0%BA%D0%B0_%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA)
### Update
- I recommend always updating the library: errors and bugs are corrected in the new versions, as well as optimization and new features are added
- through the IDE library manager: find the library how to install and click "update"
- Manually: ** remove the folder with the old version **, and then put a new one in its place.“Replacement” cannot be done: sometimes in new versions, files that remain when replacing are deleted and can lead to errors!


<a id="init"> </a>
## initialization
`` `CPP
Microds3231 RTC;// default address 0x68
Microds3231 RTC (address);// specify your address
`` `

<a id="usage"> </a>
## Usage
`` `CPP
Bool Begin ();// initialization, will return True if RTC is found
VOID settime (uint8_t param);// Installation of time == compilation time
VOID settime (Datetime Time);// Install from the DateTime structure
VOID settime (Int8_t Seconds, Int8_t Minutes, Int8_T Hours, Int8_T DATE, Int8_T MONTH, Int16_T YEAR);// Installation of time
VOID sethmsdmy (int8_t hours, int8_t minates, int8_t seconds, int8_t date, int8_t month, int16_t year);// Time 2 installation 2
    
// DateTime structure
uint8_t second;
uint8_t minute;
Uint8_t Hour;
uint8_t day;// Day of the week (Mon .. BC = 1 .. 7)
uint8_t date;
uint8_t months;
uint16_t year;

Datetime Gettime ();// Get in the DATETIME structure
uint8_t getSeconds ();// Get seconds
uint8_t getminutes ();// Get minutes
uint8_t gethours ();// Get a clock
uint8_t getday ();// Get the day of the week
uint8_t getdate ();// Get the number
uint16_t getyear ();// Get a year
uint8_t getmonth ();// Get a month
uint32_t getunix (int16_t GMT);// Get unix time (specify your hourly belt in hours or minutes)

String gettimestring ();// Get the time as a line of the type hh: mm: ssCranberry
String getdatestring ();// get a date as a line of the type DD.mm.yyy
VOID GettimeChar (Char* Array);// Get the time as Char Array [8] species hh: mm: ss
VOID getdateChar (Char* Array);// get a date as a char array [10] species dd.mm.yyyyy
    
Bool Lostpower ();// Checking for power discharge
Float gettemperaturefloat ();// get the temperature float
int Gettemperature ();// get the temperature int
`` `

<a id="EXAMPLE"> </a>
## Example
The rest of the examples look at ** Examples **!
`` `CPP
// Demo of the library capabilities
#include <micrrods3231.h>
Microds3231 RTC;

VOID setup () {
  Serial.Begin (9600);
  
  // Checking the presence of a module on the line i2c
  // Call rtc.begin () is not obligatory for work
  if (rtc.begin ()) {
    Serial.println ("DS3231 Not Found");
    for (;;);
  }
  
  // ======== Installation of time automatically ===========
  // rtc.settime (compile_time);// set time == compilation time
  
  // Visually cumbersome, but more "light" from the point of view of memory of the way to set the compilation time
  RTC.Settime (Build_Sec, Build_min, Build_hour, Build_day, Build_Month, Build_year);
    
  if (rtc.lostpower ()) {// will be performed when dumping the battery
    Serial.println ("Lost Power!");
    // Here you can once set the time == compilation time
  }
  
  // ======== Establishing time manually ============
  // You can manually set the time in two ways (substitute real numbers)
  //rtc.settime(Sec, min, hor, day, month, year);
  //rtc.sethmsdmy(Hour, min, sec, day, month, year);
  
  // You can also set the time through Datetime
  /*
  Datetime Now;
  Now.Second = 0;
  Now.minute = 10;
  Now.hour = 50;
  Now.Date = 2;
  Now.month = 9;
  Now.year = 2021;
  
  RTC.Settime (Now);// Download to RTC
  */
}

VOID loop () {
  // Receiving and withdrawal of each component
  Serial.print (rtc.gethours ());
  Serial.print (":");
  Serial.print (rtc.getminutes ());
  Serial.print (":");
  Serial.print (rtc.getseconds ());
  Serial.print ("");
  Serial.print (rtc.getday ());
  Serial.print ("");
  Serial.print (rtc.getdate ());
  Serial.print ("/");
  Serial.print (rtc.getmonth ());
  Serial.print ("/");
  Serial.println (rtc.getyear ());
  
  /*
  // you can via datetime (more optimal):
  DateTime Now = rtc.gettime ();
  Serial.print (Now.hour);
  Serial.print (":");
  Serial.print (Now.minute);
  Serial.print (":");
  Serial.print (Now.Second);
  Serial.print ("");
  Serial.print (Now.day);
  Serial.print ("");
  Serial.print (Now.Date);
  Serial.print ("/");
  Serial.print (Now.month);
  Serial.print ("/");
  Serial.println (Now.year);
  */
  
  // Chip temperature output
  Serial.println (rtc.gettemperaturefloat ());
  //Serial.println (rtc.gettemperature ());
  
  // output of time finished string string
  Serial.println (rtc.gettimestring ());
  
  // Date output of the finished line string
  Serial.println (rtc.getdatestestring ());

  // Time output through Charray
  Char Time [8];
  rtc.gettimechar (Time);
  Serial.println (Time);
  
  // Conclusion of the date through Charray
  char date [10];
  RTC.getDateChar (Date);
  Serial.println (Date);
  
  Serial.println ();
  Delay (500);
}
`` `

<a id="versions"> </a>
## versions
- V1.2 - Added restrictions on the numbers entered in Settime.You can’t enter on February 29, alas =)
- v1.3 - freezing is fifted when the module is disconnected but interviewed
- V1.4 - Minor Fix
- V2.0 - new opportunities, optimization and relief
- V2.1 - added temperature output, output to String and Char
- V2.2- Fixed days of the week (Mon-Sun 1-7)
- V2.3 - small corrections, optimization, the procedure for output of the date is changed
- V2.4 - Fixed setting the compilation time
- v2.5 - added begin to check the presence of a module on the line
- v2.6 - negative temperatures are fixed
- V2.7 - Added conclusion unix

<a id="feedback"> </a>
## bugs and feedback
Create ** Issue ** when you find the bugs, and better immediately write to the mail [alex@alexgyver.ru] (mailto: alex@alexgyver.ru)
The library is open for refinement and your ** pull Request ** 'ow!Cranberry


When reporting about bugs or incorrect work of the library, it is necessary to indicate:
- The version of the library
- What is MK used
- SDK version (for ESP)
- version of Arduino ide
- whether the built -in examples work correctly, in which the functions and designs are used, leading to a bug in your code
- what code has been loaded, what work was expected from it and how it works in reality
- Ideally, attach the minimum code in which the bug is observed.Not a canvas of a thousand lines, but a minimum code