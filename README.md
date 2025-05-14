# Interior light fade for your car
# Вежливый свет для твоего авто


### Простые пункты для счастья:
1. Сделай плату
2. Залей прошивку
3. Запаяй
4. Веселись ))

<i>Переменный резистор задумывался как регулировка скорости затухания, но это не имеет значения</i>

:warning:<b>ВАЖНО: использовать транзистор с напряжением отприная 5В или ниже(Gate Threshold Voltage) в данном случае IRL2505</b>:warning:
#### Активация происходит по низкому уровню на контакте IN
![23](https://github.com/user-attachments/assets/ca59c78b-84d3-41d7-a8bb-544bf5248063)




### Код супер простой и легкий

```cpp
#include <avr/io.h>
#include <avr/interrupt.h>
#include <avr/sleep.h>
#include <avr/wdt.h>



void setup() {
  DDRB = 1;
  PORTB = 4;
  TCCR0A = (1 << COM0A1) | (1 << WGM01) | (1 << WGM00); // Fast PWM
  TCCR0B = (1 << CS00);
  OCR0A = 0;
  wdt_enable(WDTO_15MS);
  sei();
}


ISR(WDT_vect) {

}

byte brightness = 0;

void loop() {
  wdt_reset();
  if (PINB & (1 << PB2)) {
    if (brightness != 0) {
      brightness -= 1;
      OCR0A = brightness;
      _delay_ms(8);
    }
  }
  else {
    if (brightness < 255) {
      brightness += 1;
      OCR0A = brightness;
      _delay_ms(10);
    }
  }
}
```

