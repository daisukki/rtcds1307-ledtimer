# rtcds1307-ledtimer

#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <RTClib.h>

RTC_DS1307 rtc;
LiquidCrystal_I2C lcd(0x27, 16, 2);  // endereço i2c do lcd
const int ledPin = 13;

void setup() {
  pinMode(ledPin, OUTPUT);
  digitalWrite(ledPin, LOW);
  Wire.begin();
  rtc.begin();

  lcd.init();                      
  lcd.backlight();                 
  lcd.setCursor(0, 0);
  lcd.print("Hora: ");
}

void loop() {
  DateTime now = rtc.now();

  DateTime targetTime(2024, 12, 25, 12, 0, 0); // natal de 2024 o led acende
  DateTime endTime = targetTime + TimeSpan(0, 0, 30, 0); 

  if (now >= targetTime && now < endTime) {
    digitalWrite(ledPin, HIGH); 
  } else {
    digitalWrite(ledPin, LOW);  
  }

  lcd.setCursor(7, 0);
  lcd.print(now.hour(), DEC);
  lcd.print(':');
  lcd.print(now.minute(), DEC);
  lcd.print(':');
  lcd.print(now.second(), DEC);

  lcd.setCursor(0, 1);
  lcd.print(now.day(), DEC);
  lcd.print('/');
  lcd.print(now.month(), DEC);
  lcd.print('/');
  lcd.print(now.year(), DEC);
  
  delay(1000); 
}
