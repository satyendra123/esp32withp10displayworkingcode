use the Esp32 by espress version 1.0.6 or 2.0.14 and if you will use the version 3.0.5 then you will get the Timer error. and also arduino avr board version is 1.8.6 and also arduino esp32 board by arduino version will be 2.0.18-20240930 will be installed then it is working.



# esp32withp10displayworkingcode
esp32 with p10 display working code

this is the working code with the above setup

#include <DMD32.h>
#include "fonts/SystemFont5x7.h"
#include "fonts/Arial_black_16.h"

#define DISPLAYS_ACROSS 1
#define DISPLAYS_DOWN 1
DMD dmd(DISPLAYS_ACROSS, DISPLAYS_DOWN);

hw_timer_t * timer = NULL;

void IRAM_ATTR triggerScan() {
  dmd.scanDisplayBySPI();
}

void setup() {

  Serial.begin(9600);
  Serial.println();

  delay(500);

  Serial.println();
  Serial.println("return the clock speed of the CPU.");
  uint8_t cpuClock = ESP.getCpuFreqMHz();
  delay(500);

  Serial.println();
  Serial.println("Timer Begin");
  timer = timerBegin(0, cpuClock, true);
  delay(500);

  Serial.println();
  Serial.println("Attach triggerScan function to our timer.");
  timerAttachInterrupt(timer, &triggerScan, true);
  delay(500);

  Serial.println();
  Serial.println("Set alarm to call triggerScan function.");
  timerAlarmWrite(timer, 300, true);
  delay(500);

  Serial.println();
  Serial.println("Start an alarm.");
  timerAlarmEnable(timer);
  delay(500);
}

void loop() {
  dmd.selectFont(Arial_Black_16);
  String txt_1 = "ESP32 with P10 LED Display";
  char char_array_txt_1[txt_1.length() + 1];
  txt_1.toCharArray(char_array_txt_1, txt_1.length() + 1);

  dmd.clearScreen(true);
  delay(1000);
  dmd.drawMarquee(char_array_txt_1,txt_1.length(),(32*DISPLAYS_ACROSS)-1,0);
  long start_1=millis();
  long timer_1=start_1;
  boolean ret=false;
  while(!ret){
   if ((timer_1+30) < millis()) {
     ret=dmd.stepMarquee(-1,0);
     timer_1=millis();
   }
  }

  //.................Display Text.
  dmd.clearScreen(true);
  delay(1000);
  dmd.drawString(0,0,"DMD", 3, GRAPHICS_NORMAL);  //--> dmd.drawString(x, y, Text, Number of characters in text, GRAPHICS_NORMAL);
  delay(3000);
  dmd.selectFont(SystemFont5x7);

  dmd.clearScreen(true);
  delay(1000);
  dmd.drawString(0,0,"ESP32", 5, GRAPHICS_NORMAL);
  dmd.drawString(0,9,"P10", 3, GRAPHICS_NORMAL);
  delay(3000);

  String txt_2 = "ESP32 with P10 LED Display";
  char char_array_txt_2[txt_2.length() + 1];
  txt_2.toCharArray(char_array_txt_2, txt_2.length() + 1);
  int scrl_long = (txt_2.length()*6) + (32*DISPLAYS_ACROSS);

  dmd.clearScreen(true);
  delay(1000);
  
  dmd.drawString(4,0,"UTEH", 4, GRAPHICS_NORMAL);
  
  long start_2=millis();
  long timer_2=start_2;
  int i = 32*DISPLAYS_ACROSS;
  while(true){
    if ((timer_2+30) < millis()) {
      dmd.drawString(i, 9, char_array_txt_2, txt_2.length(), GRAPHICS_NORMAL);
      
      if (i > ~scrl_long) {
        i--;
      } else {
        break;
      }
    
      timer_2=millis();
    }
  }

  //.................Displays Text and Numbers.
  int T = 29;
  int H = 73;
  char char_array_T[String(T).length() + 1];
  char char_array_H[String(H).length() + 1];
  String(T).toCharArray(char_array_T, String(T).length() + 1);
  String(H).toCharArray(char_array_H, String(H).length() + 1);

  dmd.clearScreen(true);
  delay(1000);
  
  dmd.drawString(0, 0, "T:", 2, GRAPHICS_NORMAL);
  dmd.drawString(11, 0, char_array_T, String(T).length(), GRAPHICS_NORMAL);
  dmd.drawCircle(24, 1, 1, GRAPHICS_NORMAL);
  dmd.drawString(27, 0, "C", 1, GRAPHICS_NORMAL);
  
  dmd.drawString(0, 9, "H:", 2, GRAPHICS_NORMAL);
  dmd.drawString(11, 9, char_array_H, String(H).length(), GRAPHICS_NORMAL);
  dmd.drawString(27, 9, "%", 1, GRAPHICS_NORMAL);

  delay(3000);
}


