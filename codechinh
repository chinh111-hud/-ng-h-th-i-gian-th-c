#include <Wire.h>
#include <RTClib.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET -1
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

// Khởi tạo RTC
RTC_DS1307 rtc;

char buffer[20];
String clockString;
String dateString;

void setup() {
  Serial.begin(115200);

  if (!rtc.begin()) {
    Serial.println("Không tìm thấy RTC!");
    while (1);
  }

  if (!rtc.isrunning()) {
    Serial.println("RTC chưa chạy, thiết lập thời gian mặc định...");
    rtc.adjust(DateTime(F(__DATE__), F(__TIME__)));  // Thiết lập thời gian theo thời gian biên dịch
  }

  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println(F("Không tìm thấy màn hình OLED"));
    while (1);
  }

  display.clearDisplay();
  display.display();
}

void loop() {
  DateTime now = rtc.now();  // Lấy thời gian từ DS1307

  sprintf(buffer, "%02d:%02d:%02d", now.hour(), now.minute(), now.second());
  clockString = String(buffer);

  dateString = dayOfWeekToString(now.dayOfTheWeek());
  sprintf(buffer, " %02d/%02d/%04d", now.day(), now.month(), now.year());
  dateString += String(buffer);

  Serial.println("Giờ: " + clockString + " | Ngày: " + dateString);

  display.clearDisplay();
  display.setTextColor(WHITE);

  display.setTextSize(2);
  display.setCursor(0, 30);
  display.print(clockString);

  display.setTextSize(1);
  int16_t x1, y1;
  uint16_t w, h;
  display.getTextBounds(dateString, 0, 0, &x1, &y1, &w, &h);
  int x = (SCREEN_WIDTH - w) / 2;
  display.setCursor(x, 60);
  display.print(dateString);

  display.display();

  delay(1000);
}

String dayOfWeekToString(uint8_t dayOfWeek) {
  switch (dayOfWeek) {
    case 0: return "CN";
    case 1: return "T2";
    case 2: return "T3";
    case 3: return "T4";
    case 4: return "T5";
    case 5: return "T6";
    case 6: return "T7";
    default: return "??";
  }
}
