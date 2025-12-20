```cpp
void setup(){
    pinMode(4, OUTPUT);
    pinMode(A0, INPUT);
}

void loop(){
  potValue = analogRead(potPin);        // Читаем (0-1023)
  brightness = map(potValue, 0, 1023, 0, 255); // Конвертируем
  analogWrite(ledPin, brightness);      // Управляем яркостью
  delay(10); // Небольшая задержка для стабильности
}

```cpp
