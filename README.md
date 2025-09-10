# ESP32 LCD 16x2 I2C Scrolling Text

แสดงข้อความคงที่ + ข้อความเลื่อน (Scrolling Text) บนจอ **LCD 16x2 I2C** ด้วย ESP32 และ Arduino IDE  
เหมาะสำหรับมือใหม่สาย Maker / IoT ที่อยากลองแสดงผลข้อความบนจอ LCD  

---

## 🔎 Features
- แสดงข้อความคงที่ (Static Text) และข้อความเลื่อน (Scrolling Text)
- ใช้เพียง 2 ขา (SDA, SCL) ของ ESP32 ผ่าน I2C
- ใช้ `millis()` แทน `delay()` เพื่อให้ข้อความเลื่อนต่อเนื่องโดยไม่ค้างบอร์ด
- รองรับจอ **16x2** และ **20x4** LCD I2C

---

## 🛠️ Hardware Requirements
- ESP32 DevKit V1 (หรือ Arduino UNO, NodeMCU ก็ใช้ได้)
- LCD 16x2 I2C Module (Address 0x27 หรือ 0x3F)
- สาย Jumper

---

## ⚡ Wiring ESP32 → LCD I2C

| ESP32 | LCD I2C |
|-------|---------|
| 3.3V  | VCC     |
| GND   | GND     |
| GPIO 21 (SDA) | SDA |
| GPIO 22 (SCL) | SCL |

> 📌 ค่า SDA/SCL อาจต่างกันตามบอร์ด เช่น NodeMCU: SDA=4, SCL=5  

---

## 📦 Installation
1. ติดตั้ง **Arduino IDE**  
2. เพิ่ม **ESP32 Board Manager URL**
https://dl.espressif.com/dl/package_esp32_index.json
3. ติดตั้ง Library `LiquidCrystal_I2C`  
- ไปที่ **Sketch → Include Library → Manage Libraries**  
- ค้นหา **LiquidCrystal I2C** และกด Install

---

## อ่านบทความเต็ม

👉 [การต่อใช้งานจอแสดงผล LCD DISPLAYS I2C ​​ขนาด 16×2  ร่วมกับ ESP32](https://devadiy.com/esp32-lcd-i2c-guide/)

👉 [รวมโครงงาน ESP32 และ Smart Farm](https://devadiy.com/)

## 📄 Example Code

```cpp
#include <LiquidCrystal_I2C.h>

int lcdColumns = 16;
int lcdRows = 2;

LiquidCrystal_I2C lcd(0x27, lcdColumns, lcdRows);

String stringStatic = "Deva DIY ";
String stringToScroll = "Test Scrolling 1 2 3 4 5 6 7 8 9 0";

void setup() {
lcd.init();
lcd.backlight();
}

void loop() {
lcd.setCursor(0, 0);
lcd.print(stringStatic);
scrollText(1, stringToScroll, 1000);
}

void scrollText(int row, String message, int Delaytime) {
static unsigned long lastSlide = 0;
static int pos = 0;
static int str = message.length();
message = " " + message + " " + message + " ";

if (millis() - lastSlide >= Delaytime) {
 lcd.setCursor(0, row);
 lcd.print(message.substring(pos, pos + lcdColumns));
 (pos <= str) ? pos++ : pos = 0;
 lastSlide = millis();
}
}

