# Home-Automation

#CODE FOR ARDUINO UNO

#include <SoftwareSerial.h>
int a1;
char data;
String response;
SoftwareSerial esp8266(2, 3);

void setup() 
{
 pinMode(4, OUTPUT);
 pinMode(5, OUTPUT);
 pinMode(6, OUTPUT);
 pinMode(7, OUTPUT);
 
 digitalWrite(4, LOW);
 digitalWrite(5, LOW);
 digitalWrite(6, LOW);
 digitalWrite(7, LOW);

 Serial.begin(9600);
 esp8266.begin(9600);
 Serial.println("Getting In..");
}

int ESPwait(String stopstr, int timeout_secs)
{
  response;
  bool found = false;
  char c;
  long timer_init;
  long timer;

  response="";
  timer_init = millis();
  while (!found) {
    timer = millis();
    if (((timer - timer_init) / 1000) > timeout_secs) { // Timeout?
      Serial.println("!Timeout!");
      return 0;  // timeout
    }
    if (esp8266.available()) {
      c = esp8266.read();
      //Serial.print(c);
      response += c;
      if (response.endsWith(stopstr)) {
        found = true;
        delay(10);
        esp8266.flush();
        Serial.flush();
        //Serial.println();
      }
    } // end Serial1_available()
  } // end while (!found)
  return 1;
}

void loop() 
{
char c;
  if(esp8266.available())
  {
  c=esp8266.read();
  if(c=='*')
   {
    if(ESPwait("#",3))
   {
  Serial.println(response);
 if(response.endsWith("1#"))
        {
 digitalWrite(4, HIGH);             // tell servo to go to position in variable 'pos'
        }
  if(response.endsWith("2#"))
        {
 digitalWrite(4, LOW);             // tell servo to go to position in variable 'pos'
        }
  if(response.endsWith("3#"))
        {
 digitalWrite(5, HIGH);             // tell servo to go to position in variable 'pos'
        }
  if(response.endsWith("4#"))
        {
 digitalWrite(5, LOW);             // tell servo to go to position in variable 'pos'
        }
  if(response.endsWith("5#"))
        {
 digitalWrite(6, HIGH);             // tell servo to go to position in variable 'pos'
        }
  if(response.endsWith("6#"))
        {
 digitalWrite(6, LOW);             // tell servo to go to position in variable 'pos'
        }
  if(response.endsWith("7#"))
        {
 digitalWrite(7, HIGH);             // tell servo to go to position in variable 'pos'
        }
  if(response.endsWith("8#"))
        {
 digitalWrite(7, LOW);             // tell servo to go to position in variable 'pos'
        }
        response="";  
        }
    }
 else
 Serial.print((String)(c));
 }
 }


#CODE FOR NODE MCU

/************************ Adafruit IO Config *******************************/

// visit io.adafruit.com if you need to create an account,
// or if you need your Adafruit IO key.
#define IO_USERNAME  "home1111"
#define IO_KEY       "3371d736c2604097a51eb976acbcf316"
                        
/******************************* WIFI **************************************/

// the AdafruitIO_WiFi client will work with the following boards:
//   - HUZZAH ESP8266 Breakout -> https://www.adafruit.com/products/2471
//   - Feather HUZZAH ESP8266 -> https://www.adafruit.com/products/2821
//   - Feather M0 WiFi -> https://www.adafruit.com/products/3010
//   - Feather WICED -> https://www.adafruit.com/products/3056

#define WIFI_SSID       "SSID"
#define WIFI_PASS       "PASS"

// comment out the following two lines if you are using fona or ethernet
#include "AdafruitIO_WiFi.h"
AdafruitIO_WiFi io(IO_USERNAME, IO_KEY, WIFI_SSID, WIFI_PASS);


/******************************* FONA **************************************/

// the AdafruitIO_FONA client will work with the following boards:
//   - Feather 32u4 FONA -> https://www.adafruit.com/product/3027

// uncomment the following two lines for 32u4 FONA,
// and comment out the AdafruitIO_WiFi client in the WIFI section
// #include "AdafruitIO_FONA.h"
// AdafruitIO_FONA io(IO_USERNAME, IO_KEY);


/**************************** ETHERNET ************************************/

// the AdafruitIO_Ethernet client will work with the following boards:
//   - Ethernet FeatherWing -> https://www.adafruit.com/products/3201

// uncomment the following two lines for ethernet,
// and comment out the AdafruitIO_WiFi client in the WIFI section
// #include "AdafruitIO_Ethernet.h"
// AdafruitIO_Ethernet io(IO_USERNAME, IO_KEY);
