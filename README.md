#include <OneWire.h>           // Library for temperature sensor
#include <DallasTemperature.h> // Library for DS18B20
#include <LiquidCrystal.h>     // Library for LCD display

// Pin Definitions
#define ONE_WIRE_BUS 2       // DS18B20 data pin
#define PH_SENSOR_PIN A0     // pH sensor connected to Analog pin A0
#define TEMP_SENSOR_PIN 2    // Digital pin for temperature sensor
#define LCD_RS 7
#define LCD_EN 8
#define LCD_D4 9
#define LCD_D5 10
#define LCD_D6 11
#define LCD_D7 12

// LCD Initialization
LiquidCrystal lcd(LCD_RS, LCD_EN, LCD_D4, LCD_D5, LCD_D6, LCD_D7);

// Temperature Sensor Setup
OneWire oneWire(ONE_WIRE_BUS);
DallasTemperature tempSensor(&oneWire);

// Variables
float temperatureC = 0;
float pHValue = 0;
int analogValue = 0;

void setup() {
  Serial.begin(9600);         // Start Serial communication
  lcd.begin(16, 2);           // Initialize 16x2 LCD display
  tempSensor.begin();         // Start DS18B20 sensor
  lcd.print("Milk Quality");
  delay(2000);
  lcd.clear();
}

void loop() {
  // Get Temperature
  tempSensor.requestTemperatures();
  temperatureC = tempSensor.getTempCByIndex(0);
  
  // Read pH Sensor
  analogValue = analogRead(PH_SENSOR_PIN);
  pHValue = analogValue * (14.0 / 1023.0); // Scale to pH range (0-14)

  // Display on LCD
  lcd.setCursor(0, 0);
  lcd.print("Temp: ");
  lcd.print(temperatureC);
  lcd.print(" C");
  
  lcd.setCursor(0, 1);
  lcd.print("pH: ");
  lcd.print(pHValue);

  // Print to Serial Monitor
  Serial.print("Temperature (C): ");
  Serial.println(temperatureC);
  Serial.print("pH Value: ");
  Serial.println(pHValue);

  delay(1000); // 1-second delay
}

