define LDR pin A0 // pin of the arduino board 1.e. Al where LDR sensor and resistor are connected
#include <Wire.h>
#include <LiquidCrystal 120.h>
#include <OneMire.h>
#include <DallasTemperature.h>
//Data wire is conntec to the Arduino digital pin 4
 #define ONE WIRE BUS 2
#include “DHT.h”
#define DHTPIN 8
#define DHTTYPE  DHT11
DHT dht (DHTPIN, DHTTYPE);
// Setup a onewire instance to communicate with any OneWire devices
OneWire oneWire (ONE_ WIRE_ BUS);
// Pass our one Wire reference to Dallas Temperature sensor 
DallasTemperature sensors (&oneWire);
float sg= 0,den= 0,x=0;
float tc=o, tf=0,tf=7;
 int inp=0;//
Set the LCD address to 0x27 for a 16 chars and 2 line display  

