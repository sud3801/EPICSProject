#include "DHT.h"
//#include <LiquidCrystal.h>
#include <SoftwareSerial.h>
SoftwareSerial gsm(9, 10);
#include <LiquidCrystal_I2C.h>

#include <Wire.h>
LiquidCrystal_I2C lcd(0x27, 16, 2);
//LiquidCrystal lcd(A0, A1, A2, A3, A4, A5);
int us1,us2,y,kn=1,n,v,p;
#define DHTPIN 5     // what digital pin we're connected to
// Uncomment whatever type you're using!
#define DHTTYPE DHT11   // DHT 11
DHT dht(DHTPIN, DHTTYPE);
int moi=3,mot=13,fire=4;
String exString;
void setup() 
{
  lcd.init();// initialize the lcd 
  lcd.init();
  // Print a message to the LCD.
  lcd.backlight();
  // initialize serial communication:
  Serial.begin(9600);
  gsm.begin(9600);
  lcd.begin(16, 2);
  pinMode(moi,INPUT);
  pinMode(fire,INPUT);
  pinMode(mot,OUTPUT);
  lcd.setCursor(0, 0);
  lcd.print(" LINE FAULT  ");
  //lcd.setCursor(0, 1);
  //lcd.print("identification.");
  delay(2000);
  lcd.setCursor(0, 0);
  lcd.print("TEMP:    HUM:   ");
  lcd.setCursor(0, 1);
  lcd.print("s:  ");
  lcd.setCursor(7, 1);
  dht.begin();
  RecieveMessage();
  
}
void loop()
{
    ht();
    Serial.println(digitalRead(moi));
    if(digitalRead(moi)==1)
    {
      lcd.setCursor(4, 1);
      lcd.print("FLT");
       digitalWrite(mot,HIGH);
      SendMessage(3);
      
    }
    else
    {
      lcd.setCursor(4, 1);
      lcd.print("NOFLT.....");
     digitalWrite(mot,LOW);
    }
    if(digitalRead(fire)==0)
    {
      lcd.setCursor(10, 1);
      lcd.print("fire......");
     // digitalWrite(mot,HIGH);
      SendMessage(4);
    }
    else
    {
      lcd.setCursor(10, 1);
      lcd.print("nlF.....");
      //digitalWrite(mot,LOW);
    }
    if (gsm.available()>0)
{ 
  exString=gsm.readString();
  Serial.println(exString);
  Serial.println(exString.substring(12,22));
  Serial.println("--------");
  Serial.println(exString.substring(51,54));  
  Serial.println("--------");
  if(exString.substring(51,54)=="ONN")
  {
    
    Serial.println("ON--------ok");
  //gps('3');
  //digitalWrite(mot,HIGH);
  }
  else if(exString.substring(51,54)=="OFF")
  {
    
    Serial.println("OFF--------ok");
    //gps('3');
    //digitalWrite(mot,LOW);
  }
}
}
void ht()
{
  delay(2000);

  // Reading temperature or humidity takes about 250 milliseconds!
  // Sensor readings may also be up to 2 seconds 'old' (its a very slow sensor)
  int h = dht.readHumidity();
  // Read temperature as Celsius (the default)
  int t = dht.readTemperature();
  // Read temperature as Fahrenheit (isFahrenheit = true)
  float f = dht.readTemperature(true);
  int m = analogRead(A0);
  m=m/3;
  // Check if any reads failed and exit early (to try again).
  if (isnan(h) || isnan(t) || isnan(f)) {
    Serial.println("Failed to read from DHT sensor!");
    return;
  }
  // Compute heat index in Fahrenheit (the default)
  float hif = dht.computeHeatIndex(f, h);
  // Compute heat index in Celsius (isFahreheit = false)
  float hic = dht.computeHeatIndex(t, h, false);
  Serial.print("H0");
  Serial.print(String(h)+"*");
  Serial.print("T0");
  Serial.print(String(t)+"*");
  lcd.setCursor(5, 0);
  lcd.print(t);
  lcd.setCursor(13, 0);
  lcd.print(h);
  if(h>80)
  {
    SendMessage(2);
  }
  if(t>38)
  {
    SendMessage(1);
  }
}
 void SendMessage(int hh)
{
  if(hh==1)
  {
    gsm.println("AT+CMGF=1");    //Sets the GSM Module in Text Mode
    delay(1500);  // Delay of 1000 milli seconds or 1 second
    gsm.println("AT+CMGS=\"8121657249\"\r"); // Replace x with mobile number
    delay(1500);
    gsm.println("TEMPERATURE EXCEDDED TAKE ALERT..!");// The SMS text you want to send
    delay(1500);
     gsm.println((char)26);// ASCII code of CTRL+Z
    delay(1500);
  }
  else if(hh==2)
  {
    gsm.println("AT+CMGF=1");    //Sets the GSM Module in Text Mode
    delay(1500);  // Delay of 1000 milli seconds or 1 second
    gsm.println("AT+CMGS=\"8121657249\"\r"); // Replace x with mobile number
    delay(1500);
    gsm.println("HUMIDITY CLIMATE IS HIGH TAKE ALERT..!");// The SMS text you want to send
    delay(1500);
     gsm.println((char)26);// ASCII code of CTRL+Z
    delay(1500);
  }
  if(hh==3)
  {
    gsm.println("AT+CMGF=1");    //Sets the GSM Module in Text Mode
    delay(1500);  // Delay of 1000 milli seconds or 1 second
    gsm.println("AT+CMGS=\"8121657249\"\r"); // Replace x with mobile number
    delay(1500);
    gsm.println("FAULT OCCURED.....");// The SMS text you want to send
    gsm.println(" https://maps.app.goo.gl/qKxUMUU7ZeboQZZd7 ");// The SMS text you want to send
    delay(1500);
     gsm.println((char)26);// ASCII code of CTRL+Z
    delay(1500);
  }
  else if(hh==4)
  {
    gsm.println("AT+CMGF=1");    //Sets the GSM Module in Text Mode
    delay(1500);  // Delay of 1000 milli seconds or 1 second
    gsm.println("AT+CMGS=\"8121657249\"\r"); // Replace x with mobile number
    delay(1500);
    gsm.println("FIRE EXD..!");// The SMS text you want to send
    delay(1500);
     gsm.println((char)26);// ASCII code of CTRL+Z
    delay(1500);
  }

}
 void RecieveMessage()
{
  gsm.println("AT+CMGF=1");    //Sets the GSM Module in Text Mode
  gsm.println("AT+CNMI=2,2,0,0,0"); // AT Command to receive a live SMS
  delay(1000);
}
