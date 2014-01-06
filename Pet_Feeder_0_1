/*      Pet Feeder 0.1
        1/5/13

        Coding By: Jeffrey Dorfman
        Arduino Forum Contact: crashoverride68
        Debouncing code By: Giuseppe Di Cillo


        The MIT License (MIT)

        Copyright (c) 2014 Jeffrey P Dorfman
        
        Permission is hereby granted, free of charge, to any person obtaining a copy
        of this software and associated documentation files (the "Software"), to deal
        in the Software without restriction, including without limitation the rights
        to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
        copies of the Software, and to permit persons to whom the Software is
        furnished to do so, subject to the following conditions:
        
        The above copyright notice and this permission notice shall be included in all
        copies or substantial portions of the Software.
        
        THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
        IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
        FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
        AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
        LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
        OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
        SOFTWARE.


        Hardware Used;
        
         Arduino Uno R2
         Adafruit 16x2 LCD
         Adafruit DS1307 RTC
         Futaba Micro Servo
         3 Momentary Switches with 10k pull down resistor
         3.7v Lithium Battery (back-up Battery)
         3v-5v Step-Up transformer


*/

#include <LiquidCrystal.h>  
#include <Wire.h>
#include <Servo.h>
#include <Time.h>
#include <TimeAlarms.h>
#include <DS1307RTC.h>

Servo myservo;

const int clockPower = 17;
const int clockGrd = 16;

const int buttonPinNext = 10;    
const int buttonPinEsc = 11;    
const int buttonPinEnter = 12;  

const int page = 1000;
const int pagebreak = 2000;

int pos = 0;

int lastButtonPushed = 0;

int lastButtonEnterState = LOW; 
int lastButtonEscState = LOW;  
int lastButtonNextState = LOW;

int breakfastHour = 4;
int breakfastMinute = 30;
int breakfastSecond = 0;

int dinnerHour = 23;
int dinnerMinute = 9;
int dinnerSecond = 30;

long lastEnterDebounceTime = 0; 
long lastEscDebounceTime = 0;  
long lastNextDebounceTime = 0;  
long debounceDelay = 50;    

long previousMillis = 0;          //For RTC delay using millis
long interval = 1000;

typedef enum {  MAIN,              //Define Possible states
                SKIP_MEAL,
                FEED_NOW,
                SET_MEALS,
                SET_BREAKFAST,
                SET_DINNER,
                SET_CLOCK,
                DATA  
              } states;
             
states state = MAIN;

LiquidCrystal lcd(2, 3, 4, 5, 6, 7);


void setup() {
  
  pinMode(buttonPinNext, INPUT);
  pinMode(buttonPinEnter, INPUT);
  pinMode(buttonPinEsc, INPUT);
  
  pinMode(clockPower, OUTPUT);          //Power for clock module
  pinMode(clockGrd, OUTPUT);
  digitalWrite(clockPower, HIGH); 
  digitalWrite(clockGrd, LOW);    
  
  setSyncProvider(RTC.get);   // the function to get the time from the RTC
  
  Alarm.alarmRepeat(breakfastHour,breakfastMinute,breakfastSecond, breakfastAlarm);    //Alarm set for breakfast every day
  Alarm.alarmRepeat(dinnerHour,dinnerMinute,dinnerSecond, dinnerAlarm);                //Alarm set for dinner every day
  
  lcd.begin(16, 2);
  
  myservo.attach(9);

}


void nextMeal() {           //Determines whether next meal is Breakfast or Dinner

}


void skipMeal() {           //Skips next Meal, is seperated from nextMeal so that time to "Next Meal" can be displayed
}
  
  
void feedNow() {            //Dispenses Food

  lcd.setCursor(0,1);
  lcd.print("Feeding Now     ");
  delay(3000);
}
  
  
void setBreakfast() {       //Allows user to change time of Breakfast
}
  
  
void setDinner() {          //Allows user to change time of Dinner
}
  

void setClock() {          //Allows user to set time/date of DS1307 Module
}
  
  
void scrollData() {        //Scrolls data vertically for the User
}


void breakfastAlarm() {

  feedNow();
  
}


void dinnerAlarm() {

  feedNow();
  
}


void pushedNext() {
  
  switch(state) {
    case MAIN:
      break;            //ignore
      
    case SKIP_MEAL:
      state = FEED_NOW;
      break;
      
    case FEED_NOW:
      state = SET_MEALS;
      break;
      
    case SET_MEALS:
      state = SET_CLOCK;
      break;
      
    case SET_BREAKFAST:
      state = SET_DINNER;
      break;
      
    case SET_DINNER:
      state = SET_BREAKFAST;
      break;
      
    case SET_CLOCK:
      state = DATA;
      break;
      
    case DATA:
      state = SKIP_MEAL;
      break;
  }
  
}


void pushedEnter() {
  
  switch(state) {
    case MAIN:
      state = SKIP_MEAL;
      break;
      
    case SKIP_MEAL:
      nextMeal;
      skipMeal;
      state = MAIN;
      break;
      
    case FEED_NOW:
      nextMeal;
      skipMeal;
      feedNow;
      state = MAIN;
      break;
      
    case SET_MEALS:
      state = SET_BREAKFAST;
      break;
      
    case SET_BREAKFAST:
      setBreakfast;
      state = MAIN;
      break;
      
    case SET_DINNER:
      setDinner;
      state = MAIN;
      break;
      
    case SET_CLOCK:
      setClock;
      state = MAIN;
      break;
      
    case DATA:
      scrollData;
      state = MAIN;
      break;
  }
  
}


void pushedEsc() {
  
  switch(state) {
    case MAIN:
      break;           //ignore
      
    case SKIP_MEAL:
    case FEED_NOW:
    case SET_MEALS:
    case SET_BREAKFAST:
    case SET_DINNER:
    case SET_CLOCK:
    case DATA:
      state = MAIN;
      break;
  }
  
}


void  readButtons() {  
  
  int reading;
  int buttonEnterState=LOW;            
  int buttonEscState=LOW;             
  int buttonNextState=LOW;             
    
  //Enter button
                
                  reading = digitalRead(buttonPinEnter);

                  if (reading != lastButtonEnterState) {
                    lastEnterDebounceTime = millis();
                  } 
                  
                  if ((millis() - lastEnterDebounceTime) > debounceDelay) {
                    buttonEnterState=reading;
                    lastEnterDebounceTime=millis();
                  }

                  lastButtonEnterState = reading;
                  
    //Esc button               
                 
                  reading = digitalRead(buttonPinEsc);

                  if (reading != lastButtonEscState) {
                    lastEscDebounceTime = millis();
                  } 
                  
                  if ((millis() - lastEscDebounceTime) > debounceDelay) {
                    buttonEscState = reading;
                    lastEscDebounceTime=millis();
                  }

                  lastButtonEscState = reading;
                     
   //Next button               

                  reading = digitalRead(buttonPinNext);

                  if (reading != lastButtonNextState) {
                    // reset the debouncing timer
                    lastNextDebounceTime = millis();
                  } 
                  
                  if ((millis() - lastNextDebounceTime) > debounceDelay) {
                    buttonNextState = reading;
                   lastNextDebounceTime =millis();
                  }

                  lastButtonNextState = reading;                  
                 
  if (buttonEnterState==HIGH) {
    lastButtonPushed=buttonPinEnter;
  }
  else if(buttonEscState==HIGH) {
    lastButtonPushed=buttonPinEsc;
  }
  else if(buttonNextState==HIGH) {
    lastButtonPushed=buttonPinNext;
  }
  else {
    lastButtonPushed=0;
  }          
  
}

void processInput() {        //hopefully navigate the menus
  
  readButtons();
  lcd.setCursor(0,1);
  
  if (state == MAIN) {
    lcd.print("Press to Start  ");
  }
  else if(state == SKIP_MEAL) {
    lcd.print("Skip Next Meal  ");
  }
  else if(state == FEED_NOW) {
    lcd.print("Feed Now        ");
  }
  else if(state == SET_MEALS) {
    lcd.print("Set Mealtimes   ");
  }
  else if(state == SET_BREAKFAST) {
    lcd.print("Set Breakfast   ");
  }
  else if(state == SET_DINNER) {
    lcd.print("Set Dinner      ");
  }
  else if(state == SET_CLOCK) {
    lcd.print("Set Date/Time   ");
  }
  else if(state == DATA) {
    lcd.print("View Data       ");
  }
  
  switch(lastButtonPushed) {
    case buttonPinEnter:
      pushedEnter;
      break;
      
    case buttonPinEsc:
      pushedEsc;
      break;
      
    case buttonPinNext:
      pushedNext;
      break;
      
    default:
      state = MAIN;
      break;
  }
    
}


void loop() {
  
  processInput();
  digitalClockDisplay();
  
}


void digitalClockDisplay() {
  
  unsigned long currentMillis = millis();
  
  if (timeStatus() != timeSet) {
    lcd.setCursor(0,1);
    lcd.println("The time not set");
    delay(4000);
  }
  
  else if(currentMillis - previousMillis > interval) {
    previousMillis = currentMillis;
    lcd.setCursor(10, 0);
    printDigits(minute());
    printDigits(second());
    lcd.println();
  }

}

void printDigits(int digits)
{
  lcd.print(":");
  if(digits < 10)
    lcd.print('0');
  lcd.print(digits);
}

