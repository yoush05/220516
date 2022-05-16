# 220516

코드설명..

#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <PMsensor.h>
헤더파일.

PMsensor PM;
미세먼지센서

LiquidCrystal_I2C lcd(0x27,16,2);
엘씨디

 int redPin = 5;      
 int greenPin = 6;       
 int bluePin =9;
핀을 pwm핀으로 꽂았다. 

void setup() {

  Serial.begin(9600);
  
 lcd.init();    
 
  /////(infrared LED pin, sensor pin)  /////
  
  PM.init(4, A0);
  
 }


void loop() {

   lcd.backlight();
   
  Serial.println("=================================");
  
  Serial.println("Read PM2.5");

  float filter_Data = PM.read(0.1);
  
  float noFilter_Data = PM.read();

  Serial.print("Filter : ");
  
  Serial.println(filter_Data);
  
  Serial.print("noFilter : ");
  
  Serial.println(noFilter_Data);

  lcd.setCursor(0,0);
  lcd.print("Filter : ");
  lcd.print(filter_Data);
엘씨디 스크린에 필터 데이터를 출력한다. 이 데이터는 처음에 음수값을 가지다가 나중에는 양수값을 가진다. 
나는 처음에 계속 음수값만 나와서 당황했었다. 어떤식으로 고쳐졌는지는 모르겠는데 어쩄든 고쳐졌다. 

if (filter_Data <= 30)

{Serial.print("green ");

  setColor(0, 255, 0);}
  
if (filter_Data <80 &&filter_Data>30)

여기서 &&를 써야한다. < ~< 이런식으로 쓰면 오류가 난다. 
{Serial.print("red ");

  setColor(225,0,0);}
  
if (filter_Data>= 80)

{

  Serial.print("blue ");
  
  setColor(0,0,255);}
  
필터가 측정한 값에 따라 색이 달라진다

핀을 이상하게 꽂아서 색이 원래 의도했던 내용이랑 뒤죽박죽으로 나왔다. 

  delay(1000);
  
}

void setColor(int red, int green, int blue)

     {
     
       analogWrite(redPin, red);
       
       analogWrite(greenPin, green);
       
       analogWrite(bluePin, blue);
       
     }
     
     여기는 rgb를 위한 코드이다. 
