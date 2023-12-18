# TelegramESPBot
TelegramESPBot - a program for the ESP8266 board to control the WS2812B addressable LED strip remotely or at a distance, using a separate telegram bot.

![image](https://github.com/VlaVi21/TelegramESPBot/assets/87720270/ed704109-a036-4d2b-aeb4-8953865f8973
#Project code

/*Used libraries/tutorial :
FastBot.h - https://github.com/GyverLibs/FastBot
FastLED.h - https://github.com/FastLED/FastLED 
*/

//Customize the data//
#define WIFI_SSID "Host name" 
#define WIFI_PASS "Password" 
#define BOT_TOKEN "Bot token"
#define CHAT_ID "chat id, there may be several"

//Specify the foams//
#define LED_PIN D5 //Pin tapes
#define LED_NUM 90 //Number of LEDs

#include <FastBot.h>
FastBot bot(BOT_TOKEN);

//Library for managing addressable LED strip//
#include "FastLED.h"
CRGB leds[LED_NUM];

//Brightness of the tape//
byte brightnessLED = 240;

void setup() {
  //Initialize the connection to Wi-Fi, chat, and initialize the feed//
  connectWiFi();
  bot.attach(newMsg);
  
  bot.setChatID(CHAT_ID);

  FastLED.addLeds<WS2812, LED_PIN, GRB>(leds, LED_NUM);
  FastLED.setBrightness(brightnessLED);
}
//work with a bot//
void newMsg(FB_msg& msg) {
     
    //Basic command menu interface//
    //*Open the menu bar*//
    if (msg.text == "/open_menu") {
      bot.showMenu(" open menu \n /color_Red \t /color_Blue \n /color_Green \t /color_Aqua \t /color_Yellow \n /color_White \n /color_Off \n /slava_Ukraine \n /close_menu");
    }

    //Menu commands and their actions//
    if (msg.text == "/close_menu") {
      bot.closeMenu(); 
   }
   

      if (msg.text == "/color_Red") {
      bot.sendMessage("Червоний колір на стрічці", msg.chatID);
      FastLED.showColor(CRGB::Red);
   }

    if (msg.text == "/color_Blue") {
      bot.sendMessage("Синій колір на стрічці", msg.chatID);
      FastLED.showColor(CRGB::Blue);
   }
    
    if (msg.text == "/color_Green") {
      bot.sendMessage("Зелений колір на стрічці", msg.chatID);
      FastLED.showColor(CRGB::DarkGreen);
   }

    if (msg.text == "/color_Aqua") {
      bot.sendMessage("Бірюзовий колір на стрічці", msg.chatID);
      FastLED.showColor(CRGB::Aqua);
   }
 
   if (msg.text == "/color_Yellow") {
      bot.sendMessage("Жовий колір на стрічці", msg.chatID);
      FastLED.showColor(CRGB::Yellow);
   }
   
   if (msg.text == "/color_White") {
      bot.sendMessage("Білий колір на стрічці", msg.chatID);
      FastLED.showColor(CRGB::Snow);
   }


    if (msg.text == "/color_Off") {
      bot.sendMessage("Стрічка ввимкнена", msg.chatID);
      FastLED.showColor(CRGB::Black);
   }
   
   //*Animation of the flag of Ukraine, corresponding response from the bot*//
   if (msg.text == "/slava_Ukraine") {
    bot.sendMessage("Героям Слава 🇺🇦", msg.chatID);
    for (int i = 0; i < 22; i++) {
    leds[i] = CRGB::Yellow; 
    FastLED.show(); 
    }
    for (int i = 22; i < 45; i++) {
    leds[i] = CRGB::Blue; 
    FastLED.show(); 
    }
   }

   /*
   open_menu - Відкрити меню
   command_no - команди неіснує
   slava_Ukraine - Слава Україні
   close_menu - Закрити меню
   */

}

void loop() {
  //Activating the bot//
  bot.tick();
}
//Connecting to Wi-Fi//
void connectWiFi() {
  delay(2000);
  Serial.begin(115200);
  Serial.println();
  WiFi.begin(WIFI_SSID, WIFI_PASS);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
    if (millis() > 15000) ESP.restart();
  }
  Serial.println("Connected");
}
