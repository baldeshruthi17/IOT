// ✅ PERFECT WIRING - Final Production Code
#define PIR_PIN 4   // D2 ✓
#define BUZZER_PIN 5 // D1 ✓

bool alarmActive = false;
unsigned long alarmStart = 0;
const unsigned long ALARM_TIME = 10000; // 10 seconds

void setup() {
  Serial.begin(115200);
  pinMode(PIR_PIN, INPUT);
  pinMode(BUZZER_PIN, OUTPUT);
  digitalWrite(BUZZER_PIN, LOW);
  
  Serial.println("🔒 Security System - Wiring PERFECT!");
  Serial.println("Calibrating PIR (30s)... STAY STILL!");
  
  // Startup beep
  for(int i = 0; i < 3; i++) {
    digitalWrite(BUZZER_PIN, HIGH);
    delay(100);
    digitalWrite(BUZZER_PIN, LOW);
    delay(200);
  }
  
  delay(30000); // PIR calibration
  Serial.println("✅ SYSTEM ARMED - Ready for intruders!");
}

void loop() {
  bool pirState = digitalRead(PIR_PIN);
  
  if (pirState == HIGH && !alarmActive) {
    // MOTION DETECTED!
    alarmActive = true;
    alarmStart = millis();
    Serial.println("🚨 INTRUDER DETECTED! 🚨");
    Serial.println("🔊 BUZZER ACTIVATED ON D1!");
  }
  
  // ALARM CONTROL
  if (alarmActive) {
    digitalWrite(BUZZER_PIN, HIGH);  // D1 HIGH = Buzzer ON
    
    if (millis() - alarmStart > ALARM_TIME) {
      digitalWrite(BUZZER_PIN, LOW);   // D1 LOW = Buzzer OFF
      alarmActive = false;
      Serial.println("✅ System reset - ARMED");
    }
  }
  
  delay(50);
}
