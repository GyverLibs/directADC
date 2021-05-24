![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)
![author](https://img.shields.io/badge/author-AlexGyver-informational.svg)
# directADC
Библиотека для расширенного ручного управления АЦП и компаратором ATmega328
- Функции библиотеки позволяют получить доступ ко всем возможностям и режимам работы с АЦП
- Ничего не урезано и не упрощено, доступен весь описанный в даташите функционал

### Совместимость
ATmega328 и прочие из этого поколения

### Документация
К библиотеке есть [расширенная документация](https://alexgyver.ru/directADC/)

## Содержание
- [Установка](#install)
- [Инициализация](#init)
- [Использование](#usage)
- [Пример](#example)
- [Версии](#versions)
- [Баги и обратная связь](#feedback)

<a id="install"></a>
## Установка
- Библиотеку можно найти по названию **directADC** и установить через менеджер библиотек в:
    - Arduino IDE
    - Arduino IDE v2
    - PlatformIO
- [Скачать библиотеку](https://github.com/GyverLibs/directADC/archive/refs/heads/main.zip) .zip архивом для ручной установки:
    - Распаковать и положить в *C:\Program Files (x86)\Arduino\libraries* (Windows x64)
    - Распаковать и положить в *C:\Program Files\Arduino\libraries* (Windows x32)
    - Распаковать и положить в *Документы/Arduino/libraries/*
    - (Arduino IDE) автоматическая установка из .zip: *Скетч/Подключить библиотеку/Добавить .ZIP библиотеку…* и указать скачанный архив
- Читай более подробную инструкцию по установке библиотек [здесь](https://alexgyver.ru/arduino-first/#%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0_%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA)

<a id="init"></a>
## Инициализация
Нет

<a id="usage"></a>
## Использование
```cpp
void setAnalogMux(ADC_modes mux);           // Аналоговый вход (ADC_A0-ADC_A7)/ термодатчик (ADC_SENSOR)/ 1.1V (ADC_1V1)/ ADC_GND (default: ADC_A0)
void ADC_enable(void);                      // Включить АЦП 
void ADC_disable(void);                     // Выключить АЦП (default)
void ADC_setPrescaler(byte prescl);         // Выбрать делитель частоты АЦП (2, 4, 8, 16, 32, 64, 128) // (default: 2)
void ADC_setReference(ADC_modes ref);       // Выбрать источник опорного напряжения АЦП (ADC_1V1, ADC_AREF, ADC_VCC) // (default: ADC_AREF)
void ADC_autoTriggerEnable(ADC_modes trig); // Включить автозапуск АЦП и выбрать событие (FREE_RUN, ANALOG_COMP, ADC_INT0, TIMER0_COMPA, TIMER0_OVF, TIMER1_COMPB, TIMER1_OVF)
void ADC_autoTriggerDisable(void);          // Выключить автозапуск АЦП // (default)
void ADC_attachInterrupt(void (*isr)());    // Включить прерывание готовности АЦП и выбрать функцию, которая будет при этом выполняться
void ADC_detachInterrupt(void);             // Выключить прерывание готовности АЦП // (default) 
void ADC_startConvert(void);                // Ручной запуск преобразования
unsigned int ADC_read(void);                // Прочитать значение регистров АЦП (Вызов до окончания преобразования вернет неверный результат)
boolean ADC_available(void);                // Проверить готовность преобразования АЦП
unsigned int ADC_readWhenAvailable(void);   // Дождаться окончания текущего преобразования и вернуть результат
void ACOMP_attachInterrupt(void (*isr)(), ADC_modes source);    // Включить прерывание компаратора и выбрать при каком событии оно будет вызвано (FALLING_TRIGGER, RISING_TRIGGER, CHANGE_TRIGGER)
void ACOMP_detachInterrupt(void);           // Выключить прерывание компаратора // (default)
void ACOMP_enable(void);                    // Включить компаратор // (default: Включен)
void ACOMP_disable(void);                   // Принудительно выключить компаратор
boolean ACOMP_read(void);                   // Прочитать значение на выходе компаратора 
void ACOMP_setPositiveInput(ADC_modes in);  // Настроить куда подкл +Вход компаратора (ADC_1V1, ADC_AIN0) (default: ADC_AIN0 - pin 6) 
void ACOMP_setNegativeInput(ADC_modes in);  // Настроить куда подкл -Вход компаратора (ADC_AIN1, ANALOG_MUX) (default: ADC_AIN1 - pin 7)
// делитель 2: 3.04 мкс (частота оцифровки 329 000 кГц)
// делитель 4: 4.72 мкс (частота оцифровки 210 000 кГц)
// делитель 8: 8.04 мкс (частота оцифровки 125 000 кГц)
// делитель 16: 15.12 мкс (частота оцифровки 66 100 кГц)
// делитель 32: 28.04 мкс (частота оцифровки 35 600 кГц)
// делитель 64: 56.04 мкс (частота оцифровки 17 800 кГц)                          
// делитель 128: 112 мкс (частота оцифровки 8 900 Гц)
```

<a id="example"></a>
## Пример
Остальные примеры смотри в **examples**!
```cpp
#include <directADC.h>
/* пример простой работы с ацп */
void setup() {
  Serial.begin(9600);
  ADC_enable(); // вызывается обязательно
  ADC_setPrescaler(64); // без вызова - делитель 2
  ADC_setReference(ADC_VCC); // без вызова - ADC_AREF
  setAnalogMux(ADC_A4); // выбрать ADC_A0-ADC_A7 / ADC_SENSOR - термометр / ADC_1V1 / ADC_GND // Без вызова - ADC_A0
}

void loop() {
  ADC_startConvert(); // ручной старт преобразования
  while (!ADC_available()); // пока преобразование не готово - ждем или делаем что то полезное
  Serial.println(ADC_read());

  // ADC_read(); - прямая склейка и чтение,если преобразование еще не закончилось - вернет ошибочное значение.
  // ADC_readWhenAvailable(); - дождется окончания преобразования и вернет результат, если уже готово-вернет сразу.
}
```

<a id="versions"></a>
## Версии
- v1.1 - добавлены функции чтения 8 бит (спасибо Vitve4) https://github.com/AlexGyver/GyverLibs/pull/43

<a id="feedback"></a>
## Баги и обратная связь
При нахождении багов создавайте **Issue**, а лучше сразу пишите на почту [alex@alexgyver.ru](mailto:alex@alexgyver.ru)  
Библиотека открыта для доработки и ваших **Pull Request**'ов!