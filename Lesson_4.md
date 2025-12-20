```cpp
#include <Servo.h>
Servo myservo;        // Создать объект для управления

int potPin = A0;   // Потенциометр на A0
int servPin = 9;    // Светодиод на пине с ШИМ (~9)
int potValue = 0;  // Для хранения значения с потенциометра
int angle = 0;// Для хранения яркости

void setup(){
  pinMode(A0, INPUT);
  myservo.attach(servPin);
}

void loop(){
  potValue = analogRead(potPin);        // Читаем (0-1023)
  angle = map(potValue, 0, 1023, 0, 180); // Конвертируем
  myservo.write(angle);
  delay(10); // Небольшая задержка для стабильности
}

```
