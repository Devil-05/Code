# Code
#include <Servo.h>
Servo myservo;

#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#define SCL A5
#define SDA A4
LiquidCrystal_I2C lcd(0x27,16,2);

#include <HX711.h>
#define DOUT  12
#define CLK  11
HX711 scale(DOUT, CLK); 



int ENA = A2;
int IN1 = A0;
int IN2 = A1;

const byte ledPin1 = 3;
const byte ledPin2 = 4;
const byte ledPin3 = 5;
//const byte ledPin4 = ; not connected
const byte ledPin5 = 6;

const byte buttonPin1 = 7;
const byte buttonPin2 = 8;
const byte buttonPin3 = 9;
//const byte buttonPin4 = ; 2kgs button -not connected
const byte buttonPin5 = 10;
const byte buttonPin6 = 2;

boolean buttonPress5 = false;
volatile boolean buttonPress6 = true;

void setup() {
  Serial.begin(9600);
  lcd.init ();
  lcd.backlight();
  lcd.cursor();
  //lcd.blink();
  scale.set_scale(218);
  scale.tare();
  
  lcd.print("Let's do it!");
  delay (1000);
 // Serial.println("Readings:");

 myservo.attach (A3); //(5, 500, 2000);
  //myservo.write(90);
  myservo.write (5);
  
//led output
  pinMode (ledPin1, OUTPUT);
  pinMode (ledPin2, OUTPUT);
  pinMode (ledPin3, OUTPUT);
  //pinMode (ledPin4, OUTPUT);
  pinMode (ledPin5, OUTPUT);
  
 //setup button pins 
  pinMode (buttonPin1, INPUT);
  pinMode (buttonPin2, INPUT);
  pinMode (buttonPin3, INPUT);
 // pinMode (buttonPin4, INPUT);
  pinMode (buttonPin5, INPUT);
  pinMode (buttonPin6, INPUT);
  
  pinMode(ENA, OUTPUT);
  pinMode(IN1, OUTPUT);
  pinMode (IN2, OUTPUT);
  
  digitalWrite (IN1, LOW);
  digitalWrite (IN2, LOW);
  attachInterrupt(digitalPinToInterrupt(buttonPin6),stopButton,CHANGE);
}

void loop () {
 
 boolean buttonPress1 = digitalRead(buttonPin1);
 boolean buttonPress2 = digitalRead(buttonPin2); 
 boolean buttonPress3 = digitalRead(buttonPin3);
// boolean buttonPress4 = digitalRead(buttonPin4);
 boolean buttonPress5 = digitalRead(buttonPin5);
 boolean buttonPress6 = digitalRead(buttonPin6);

//Variables set on the condition to stop the motor when desired weight is achieved

 int ledState_1 = digitalRead(ledPin1);
 int ledState_2 = digitalRead(ledPin2);
 int ledState_3 = digitalRead(ledPin3);
// int ledState_4 = digitalRead(ledPin4);

  byte Open_Position = 5;
  byte close_Position = 90;

 long actual_Weight = (scale.get_units());
 lcd.clear();
 lcd.setCursor (2,0);
 lcd.print ("Readings Grm: ");
 lcd.setCursor (6,1);
 lcd.print(actual_Weight);
 //Serial.print(actual_Weight);
 //Serial.println(" g");
 Serial.println ("Sta  ");
 Serial.print (ledState_3); 
 

// 250 grams fill weight

 if (buttonPress1 == true){
    digitalWrite (ledPin1, HIGH); 
 }
  if (buttonPress5 != false){
    myservo.write(Open_Position);
    delay(1000);
    digitalWrite (ledPin5, HIGH);
    digitalWrite (IN1, HIGH);
    digitalWrite (IN2, LOW);
    analogWrite (ENA, 250);
    
 }
 
 if ((actual_Weight >= 250)&& (ledState_1 == HIGH)) {
    myservo.write(close_Position);
    digitalWrite (IN1, LOW);
    digitalWrite (IN2, LOW);  
    
 }

// 500 grams fill weight

    if (buttonPress2 == true){
    digitalWrite (ledPin2, HIGH); 
    }
    if (buttonPress5 != false){
    digitalWrite (IN1, HIGH);
    digitalWrite (IN2, LOW);
    analogWrite (ENA, 250);
    }
   if ((actual_Weight >= 500)&& (ledState_2 == HIGH)) {
    digitalWrite (IN1, LOW);
    digitalWrite (IN2, LOW);   
 } 

 // 1000 GRAMS
 
 if (buttonPress3 == true){
    digitalWrite (ledPin3, HIGH); 
 }
if (buttonPress5 != false){
    digitalWrite (IN1, HIGH);
    digitalWrite (IN2, LOW);
    analogWrite (ENA, 250);
    }
 if ((actual_Weight >= 1000)&&(ledState_3 == HIGH)) {
    digitalWrite (IN1, LOW);
    digitalWrite (IN2, LOW);   
 } 

//2000 GRAMS (not connected)

//if (buttonPress4 == true){
   // digitalWrite (ledPin4, HIGH); 
// }
//if (buttonPress5 != false){
  //  digitalWrite (IN1, HIGH);
  //  digitalWrite (IN2, LOW);
//    analogWrite (ENA, 250);
  //  }
// if ((actual_Weight >= 2000)&& (ledState_4 == HIGH)) {
  //  digitalWrite (IN1, LOW);
  //  digitalWrite (IN2, LOW);   
 //}   

}//close loop

void stopButton (void){
  buttonPress6 != true;
  digitalWrite(ledPin1, LOW);
  digitalWrite(ledPin2, LOW);
  digitalWrite(ledPin3, LOW);
//  digitalWrite(ledPin4, LOW);
  digitalWrite(ledPin5, LOW);
  
  digitalWrite (IN1, LOW);
  digitalWrite (IN2, LOW);
  }
