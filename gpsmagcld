#include <Wire.h>

#include <Adafruit_Sensor.h>
#include <Adafruit_HMC5883_U.h>
#include <TinyGPS++.h>
#include <SoftwareSerial.h>
#include <LiquidCrystal.h>

LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

static const int RXPin = 7, TXPin = 6;
static const uint32_t GPSBaud = 4800;

TinyGPSPlus gps;

SoftwareSerial ss(RXPin, TXPin);

Adafruit_HMC5883_Unified mag = Adafruit_HMC5883_Unified(12345);


void displaySensorDetails(void)
{
  sensor_t sensor;
  mag.getSensor(&sensor);
  Serial.println("------------------------------------");
  Serial.print  ("Sensor:       "); Serial.println(sensor.name);
  Serial.print  ("Driver Ver:   "); Serial.println(sensor.version);
  Serial.print  ("Unique ID:    "); Serial.println(sensor.sensor_id);
  Serial.print  ("Max Value:    "); Serial.print(sensor.max_value); Serial.println(" uT");
  Serial.print  ("Min Value:    "); Serial.print(sensor.min_value); Serial.println(" uT");
  Serial.print  ("Resolution:   "); Serial.print(sensor.resolution); Serial.println(" uT");  
  Serial.println("------------------------------------");
  Serial.println("");
  delay(500);
}

void setup()
{
  Serial.begin(115200);
  ss.begin(GPSBaud);
  lcd.begin(16, 2);

  Serial.println(F("DeviceExample.ino"));
  Serial.println(F("A simple demonstration of TinyGPS++ with an attached GPS module"));
  Serial.print(F("Testing TinyGPS++ library v. ")); Serial.println(TinyGPSPlus::libraryVersion());
  Serial.println(F("by Mikal Hart"));
  Serial.println();
  
    /* Initialise the sensor */
  if(!mag.begin())
  {
    /* There was a problem detecting the HMC5883 ... check your connections */
    Serial.println("Ooops, no HMC5883 detected ... Check your wiring!");
    while(1);
  }
  
  /* Display some basic information on this sensor */
  displaySensorDetails();
}

void loop()
{
  // This sketch displays information every time a new sentence is correctly encoded.
  while (ss.available() > 0)
    if (gps.encode(ss.read()))
      displayInfo();
      

  if (millis() > 5000 && gps.charsProcessed() < 10)
  {
    Serial.println(F("No GPS detected: check wiring."));
    while(true);
  }
  
  
  /* Get a new sensor event */ 
  sensors_event_t event; 
  mag.getEvent(&event);
 
  /* Display the results (magnetic vector values are in micro-Tesla (uT)) */
  Serial.print("X: "); Serial.print(event.magnetic.x); Serial.print("  ");
  Serial.print("Y: "); Serial.print(event.magnetic.y); Serial.print("  ");
  Serial.print("Z: "); Serial.print(event.magnetic.z); Serial.print("  ");Serial.println("uT");

  // Hold the module so that Z is pointing 'up' and you can measure the heading with x&y
  // Calculate heading when the magnetometer is level, then correct for signs of axis.
  float heading = atan2(event.magnetic.y, event.magnetic.x);
  
  // Once you have your heading, you must then add your 'Declination Angle', which is the 'Error' of the magnetic field in your location.
  // Find yours here: http://www.magnetic-declination.com/
  // Mine is: -13* 2' W, which is ~13 Degrees, or (which we need) 0.22 radians
  // If you cannot find your Declination, comment out these two lines, your compass will be slightly off.
  float declinationAngle = 0.26;
  heading += declinationAngle;
  
  // Correct for when signs are reversed.
  if(heading < 0)
    heading += 2*PI;
    
  // Check for wrap due to addition of declination.
  if(heading > 2*PI)
    heading -= 2*PI;
   
  // Convert radians to degrees for readability.
  float headingDegrees = heading * 180/M_PI; 
  
  Serial.print("Heading (degrees): "); Serial.println(headingDegrees);
    lcd.setCursor(0,0);
    lcd.print(gps.location.lat(), 5);
    lcd.print("  ");
    lcd.print(headingDegrees);
    lcd.setCursor(0, 1);
    lcd.print(gps.location.lng(), 5);
    lcd.print("  ");
    if((headingDegrees > 348.75) && (headingDegrees <= 360))
{
//  lcd.setCursor(0,1);
  lcd.print(" N ");
}
else if((headingDegrees >= 0) && (headingDegrees <= 11.25))
{
//  lcd.setCursor(0,1);
  lcd.print(" N ");
}
else if((headingDegrees > 11.25) && (headingDegrees <= 33.75))
{
//  lcd.setCursor(0,1);
  lcd.print("NNE");
}
else if((headingDegrees > 33.75) && (headingDegrees <= 56.25))
{
//  lcd.setCursor(0,1);
  lcd.print("NE ");
}
else if((headingDegrees > 56.25) && (headingDegrees <= 78.25))
{
//  lcd.setCursor(0,1);
  lcd.print("ENE");
}
else if((headingDegrees > 78.25) && (headingDegrees <= 101.25))
{
//  lcd.setCursor(0,1);
  lcd.print(" E ");
}
else if((headingDegrees > 101.25) && (headingDegrees <= 123.75))
{
//  lcd.setCursor(0,1);
  lcd.print("ESE");
}
else if((headingDegrees > 123.75) && (headingDegrees <= 146.25))
{
//  lcd.setCursor(0,1);
  lcd.print("SE ");
}
else if((headingDegrees > 146.25) && (headingDegrees <= 168.75))
{
//  lcd.setCursor(0,1);
  lcd.print("SSE");
}
else if((headingDegrees > 168.75) && (headingDegrees <= 191.25))
{
//  lcd.setCursor(0,1);
  lcd.print(" S ");
}
else if((headingDegrees > 191.25) && (headingDegrees <= 213.75))
{
//  lcd.setCursor(0,1);
  lcd.print("SSW");
}
else if((headingDegrees > 213.75) && (headingDegrees <= 236.25))
{
//  lcd.setCursor(0,1);
  lcd.print("SW ");
}
else if((headingDegrees > 236.25) && (headingDegrees <= 258.75))
{
//  lcd.setCursor(0,1);
  lcd.print("WSW");
}
else if((headingDegrees > 258.75) && (headingDegrees <= 281.25))
{
//  lcd.setCursor(0,1);
  lcd.print(" W ");
}
else if((headingDegrees > 281.25) && (headingDegrees <= 303.75))
{
//  lcd.setCursor(0,1);
  lcd.print("WNW");
}
else if((headingDegrees > 303.75) && (headingDegrees <= 326.25))
{
//  lcd.setCursor(0,1);
  lcd.print("NW ");
}
else if((headingDegrees > 326.25) && (headingDegrees <= 348.75))
{
//  lcd.setCursor(0,1);
  lcd.print("NNW");
}
  
  delay(500);
}

void displayInfo(void)
{
  Serial.print(F("Location: ")); 
  if (gps.location.isValid())
  {
    Serial.print(gps.location.lat(), 6);
    Serial.print(F(","));
    Serial.print(gps.location.lng(), 6);

  }
  else
  {
    Serial.print(F("INVALID"));
  }

  Serial.print(F("  Date/Time: "));
  if (gps.date.isValid())
  {
    Serial.print(gps.date.month());
    Serial.print(F("/"));
    Serial.print(gps.date.day());
    Serial.print(F("/"));
    Serial.print(gps.date.year());
  }
  else
  {
    Serial.print(F("INVALID"));
  }

  Serial.print(F(" "));
  if (gps.time.isValid())
  {
    if (gps.time.hour() < 10) Serial.print(F("0"));
    Serial.print(gps.time.hour());
    Serial.print(F(":"));
    if (gps.time.minute() < 10) Serial.print(F("0"));
    Serial.print(gps.time.minute());
    Serial.print(F(":"));
    if (gps.time.second() < 10) Serial.print(F("0"));
    Serial.print(gps.time.second());
    Serial.print(F("."));
    if (gps.time.centisecond() < 10) Serial.print(F("0"));
    Serial.print(gps.time.centisecond());
  }
  else
  {
    Serial.print(F("INVALID"));
  }

  Serial.println();
}
