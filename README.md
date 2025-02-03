# water-tank-monitering

#include <LiquidCrystal.h>

#define TRIG_PIN 7
#define ECHO_PIN 6
#define RELAY_PIN 8
#define BUZZER_PIN 9

LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

void setup() {
    pinMode(TRIG_PIN, OUTPUT);
    pinMode(ECHO_PIN, INPUT);
    pinMode(RELAY_PIN, OUTPUT);
    pinMode(BUZZER_PIN, OUTPUT);
    lcd.begin(16, 2);
    lcd.print("Water Level:");
    delay(1000);
}

long getDistance() {
    digitalWrite(TRIG_PIN, LOW);
    delayMicroseconds(2);
    digitalWrite(TRIG_PIN, HIGH);
    delayMicroseconds(10);
    digitalWrite(TRIG_PIN, LOW);
    
    long duration = pulseIn(ECHO_PIN, HIGH);
    return duration * 0.034 / 2;  // Convert to cm
}

void loop() {
    long distance = getDistance();
    int tankHeight = 30;  // Example tank height in cm
    int waterLevel = tankHeight - distance;
    
    lcd.setCursor(0, 1);
    lcd.print("Level: ");
    lcd.print(waterLevel);
    lcd.print(" cm   ");
    
    if (waterLevel < 5) {  // Low water level
        digitalWrite(RELAY_PIN, HIGH);
        digitalWrite(BUZZER_PIN, HIGH);
    } 
    else if (waterLevel > 25) {  // Tank full
        digitalWrite(RELAY_PIN, LOW);
        digitalWrite(BUZZER_PIN, HIGH);
        delay(2000);
        digitalWrite(BUZZER_PIN, LOW);
    } 
    else {
        digitalWrite(RELAY_PIN, LOW);
        digitalWrite(BUZZER_PIN, LOW);
    }
    
    delay(1000);
}
