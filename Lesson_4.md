```cpp
#include <Servo.h>
Servo myservo;        // Создать объект для управления

 // Привязать сервопривод к пину 9

 // Повернуть вал на 90 градусов

```cpp
int potPin = A0;   // Потенциометр на A0
int servPin = 9;    // Светодиод на пине с ШИМ (~9)
int potValue = 0;  // Для хранения значения с потенциометра
int angle = 0;// Для хранения яркости

void setup(){
    pinMode(4, OUTPUT);
    pinMode(A0, INPUT);
  myservo.attach(servPin);
}

void loop(){
  potValue = analogRead(potPin);        // Читаем (0-1023)
  brightness = map(potValue, 0, 1023, 0, 255); // Конвертируем
  myservo.write(angle);
  delay(10); // Небольшая задержка для стабильности
}

```
