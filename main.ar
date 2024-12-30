#include <TinyGPS++.h>
#include <HardwareSerial.h>
#include <Wire.h>
#include<DHT.h>
#define Dpin 4
#define Dtype DHT11
DHT dht(Dpin, Dtype);
#define BLYNK_TEMPLATE_ID "TMPLPGXs-zaL"
#define BLYNK_DEVICE_NAME "IOT Internship"
#define BLYNK_AUTH_TOKEN "Lowqmg3c4QntpPZDHfDmyxakQzTThg2O"
#define BLYNK_PRINT Serial
#include <WiFi.h>
#include <WiFiClient.h>
#include <BlynkSimpleEsp32.h>
char auth[] = BLYNK_AUTH_TOKEN;
char ssid[] = "dlink";
char pass[] = "";
BlynkTimer timer;
BlynkTimer timer1;
TinyGPSPlus gps;
float latitude , longitude;
String  latitude_string , longitiude_string;
HardwareSerial SerialGPS(2);

void setup()
{
  Serial.begin(9600);
  dht.begin();
  Blynk.begin(auth, ssid, pass);
  timer.setInterval(2000L, myTimerEvent);
  timer1.setInterval(3000L, myTimerEvent1);
  SerialGPS.begin(9600, SERIAL_8N1, 16, 17);
  pinMode(35, INPUT); //led LDR
  pinMode(34, INPUT);//led IR
  pinMode(22, OUTPUT); //led
  pinMode(32, INPUT);//buzzer IR
  pinMode(19, OUTPUT);//buzzer
  pinMode(15, OUTPUT); //AirConditioner ON
  pinMode(23, OUTPUT); //Cabin Lights
}
int ldr, ir, val;

void loop()
{
  Blynk.run();
  timer.run();
  timer1.run();
  ir = analogRead(34); //distance IR
  ldr = analogRead(35); //Light intensity LDR
  val = analogRead(32); //distance IR buzzer

  if ( ir<300 and ldr>3000 and val <= 100)
  {
    digitalWrite(22, HIGH);
    digitalWrite(19, HIGH);
    delay(100);
    digitalWrite(19, LOW);
    delay(100);
  }
  else if (ir >= 300 and ldr <= 3000 and val <= 100)
  {
    digitalWrite(22, LOW);
    digitalWrite(19, HIGH);
    delay(100);
    digitalWrite(19, LOW);
    delay(100);
  }

  else if ( ir<300 and ldr>3000 and val > 100)
  {
    digitalWrite(22, HIGH);
    digitalWrite(19, LOW);
  }
  else if ( val <= 100)
  {
    digitalWrite(19, HIGH);
    delay(100);
    digitalWrite(19, LOW);
    delay(100);
  }
  else
  { digitalWrite(19, LOW);
    digitalWrite(22, LOW);

  }

  Serial.println(val);
  delay(1000);


}
float t, h;
void myTimerEvent()
{
  h = dht.readHumidity();
  t = dht.readTemperature();
  if (isnan(t) || isnan(h))
  {
    Serial.println("Check the Connection of DHT11");
    return;
  }

  Blynk.virtualWrite(V2, h);
  Blynk.virtualWrite(V1, t);


      Serial.println("");
      Serial.print("Temp :");
      Serial.print(t);
      Serial.println("");
      Serial.print("Humid :");
      Serial.print(h);
}

void myTimerEvent1()
{
  while (SerialGPS.available() > 0)
  {
    if (gps.encode(SerialGPS.read()))
    {
      if (gps.location.isValid())
      {
        latitude = gps.location.lat();
        latitude_string = String(latitude , 6);
        longitude = gps.location.lng();
        longitiude_string = String(longitude , 6);
        Serial.print("Latitude = ");
        Serial.println(latitude_string);
        Serial.print("Longitude = ");
        Serial.println(longitiude_string);
        Blynk.virtualWrite(V0, longitude);
        Blynk.virtualWrite(V3, latitude);

      }
      delay(1000);
      Serial.println();
    }
  }
}
BLYNK_WRITE(V4)
{
  int pinValue = param.asInt(); // Assigning incoming value from pin V4 to a variable
  if (pinValue == 1)
  {
    digitalWrite(15, LOW); // Turn Relay ON
    Serial.println("AC is on");
  }
  else if (pinValue == 0)
  {
    digitalWrite(15, HIGH); // Turn Relay OFF
    Serial.println("AC is OFF");

  }
}

BLYNK_WRITE(V5)
{ int pinValue = param.asInt(); // Assigning incoming value from pin V4 to a variable
  if (pinValue == 1)
  {
    digitalWrite(23, LOW); // Turn Relay ON
    Serial.println("LIGHT is on");

  }
  else if (pinValue == 0)
  {
    digitalWrite(23, HIGH); // Turn Relay OFF
    Serial.println("LIGHT is OFF");

  }
}
