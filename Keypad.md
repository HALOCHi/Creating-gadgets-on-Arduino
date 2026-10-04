# Keypad with OLED

```cpp
#include <Keypad.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

// Настройки дисплея OLED
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET    -1
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

// Настройки матричной клавиатуры 4х4
const int ROWS = 4;
const int COLS = 4;

String result = ""; // Строка для накопления цифр, которые мы нажимаем
long value1 = 0;    // Первое число для математического действия
long value2 = 0;    // Второе число для математического действия
char op = ' ';      // Знак операции (+, -, * или /)

char keys[ROWS][COLS] = {
  {'1', '2', '3', 'A'},
  {'4', '5', '6', 'B'},
  {'7', '8', '9', 'C'},
  {'*', '0', '#', 'D'}
};

byte rowPins[ROWS] = {9, 8, 7, 6};
byte colPins[COLS] = {5, 4, 3, 2};

Keypad pad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

void setup() {
  // Запускаем дисплей
  if(!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    for(;;); // Если дисплей не подключен, программа остановится тут
  }
  
  display.clearDisplay();     // Очищаем экран при старте
  display.setTextSize(2);      // Ставим крупный, понятный шрифт
  display.setTextColor(WHITE); // Белый цвет текста
  display.setCursor(0, 10);    // Сдвигаем курсор
  display.println("Calc ready"); // Пишем, что калькулятор готов
  display.display();           // Выводим текст на экран
  delay(1500);
}

void loop() {
  char key = pad.getKey(); // Проверяем, нажата ли какая-то кнопка
  
  if (key) {
    
    // 1. Если нажата цифра от '0' до '9'
    if (key >= '0' && key <= '9') {
      result += key; // Дописываем цифру в конец нашей строки
    }
    
    // 2. Если нажата буква (выбор математического действия)
    else if (key == 'A' || key == 'B' || key == 'C' || key == 'D') {
      if (result != "") {
        value1 = result.toInt(); // Переводим накопленные символы в первое число
        result = ""; // Очищаем строку, чтобы начать копить второе число
        
        if (key == 'A') op = '+';
        if (key == 'B') op = '-';
        if (key == 'C') op = '*';
        if (key == 'D') op = '/';
      }
    }
    
    // 3. Если нажата кнопка "Равно" (#)
    else if (key == '#') {
      if (result != "" && op != ' ') {
        value2 = result.toInt(); // Переводим накопленные символы во второе число
        long finalResult = 0;

        // Вместо switch-case используем обычные условия if
        if (op == '+') {
          finalResult = value1 + value2;
        }
        else if (op == '-') {
          finalResult = value1 - value2;
        }
        else if (op == '*') {
          finalResult = value1 * value2;
        }
        else if (op == '/') {
          finalResult = value1 / value2;
        }

        // Очищаем экран и показываем итоговый ответ
        display.clearDisplay();
        display.setCursor(0, 10);
        display.print("Ans: ");
        display.println(finalResult);
        display.display();
        
        // ВНИМАНИЕ: Оставляем результат на экране, но переменные пока не сбрасываем!
      }
    }
    
    // 4. Если нажата кнопка Очистки (*)
    else if (key == '*') {
      result = "";
      value1 = 0;
      value2 = 0;
      op = ' ';
    }

    // Блок обновления экрана: показываем то, что вводит пользователь прямо сейчас
    // Но только если мы не нажали кнопку равенства '#'
    if (key != '#') {
      display.clearDisplay();
      display.setCursor(0, 10);
      
      // Показываем первое число и операцию, если они уже заданы
      if (op != ' ') {
        display.print(value1);
        display.print(op);
      }
      // Показываем текущую накапливаемую строку
      display.print(result);
      display.display();
    }
  }
}

```
