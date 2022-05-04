This is an automatic translation, may be incorrect in some places. See sources and examples!

#directADC
Library for advanced manual control of ADC and ATmega328 comparator
- Library functions allow you to access all the features and modes of operation with the ADC
- Nothing has been truncated or simplified, all the functionality described in the datasheet is available

### Compatibility
ATmega328 and others from this generation

### Documentation
The library has [extended documentation](https://alexgyver.ru/directADC/)

## Content
- [Install](#install)
- [Initialization](#init)
- [Usage](#usage)
- [Example](#example)
- [Versions](#versions)
- [Bugs and feedback](#feedback)

<a id="install"></a>
## Installation
- The library can be found by name **directADC** and installed via the library manager in:
    - Arduino IDE
    - Arduino IDE v2
    - PlatformIO
- [Download library](https://github.com/GyverLibs/directADC/archive/refs/heads/main.zip) .zip archive for manual installation:
    - Unzip and put in *C:\Program Files (x86)\Arduino\libraries* (Windows x64)
    - Unzip and put in *C:\Program Files\Arduino\libraries* (Windows x32)
    - Unpack and put in *Documents/Arduino/libraries/*
    - (Arduino IDE) automatic installation from .zip: *Sketch/Include library/Add .ZIP library…* and specify the downloaded archive
- Read more detailed instructions for installing libraries [here] (https://alexgyver.ru/arduino-first/#%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE% D0%B2%D0%BA%D0%B0_%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA)

<a id="init"></a>
## Initialization
Not

<a id="usage"></a>
## Usage
```cpp
void setAnalogMux(ADC_modes mux); // Analog input (ADC_A0-ADC_A7)/ thermal sensor (ADC_SENSOR)/ 1.1V (ADC_1V1)/ ADC_GND (default: ADC_A0)
void ADC_enable(void); // Enable ADC
void ADC_disable(void); // Disable ADC (default)
void ADC_setPrescaler(byte prescl); // Select ADC Divisor (2, 4, 8, 16, 32, 64, 128) // (default: 2)
void ADC_setReference(ADC_modes ref); // Select ADC reference voltage source (ADC_1V1, ADC_AREF, ADC_VCC) // (default: ADC_AREF)
void ADC_autoTriggerEnable(ADC_modes trig); // Enable ADC autostart and select an event (FREE_RUN, ANALOG_COMP, ADC_INT0, TIMER0_COMPA, TIMER0_OVF, TIMER1_COMPB, TIMER1_OVF)
void ADC_autoTriggerDisable(void); // Disable ADC autostart // (default)
void ADC_attachInterrupt(void (*isr)()); // Enable the ADC ready interrupt and select the function to be executed
void ADC_detachInterrupt(void); // Disable ADC ready interrupt // (default)
void ADC_startConvert(void); // Manual conversion start
unsigned int ADC_read(void); // Read the value of the ADC registers (Calling before the end of the conversion will return an incorrect result)
boolean ADC_available(void); // Check ADC conversion readiness
unsigned int ADC_readWhenAvailable(void); // Wait for the end of the current transformation and return the result
void ACOMP_attachInterrupt(void (*isr)(), ADC_modes source); // Enable comparator interrupt and choose which event triggers it (FALLING_TRIGGER, RISING_TRIGGER, CHANGE_TRIGGER)
void ACOMP_detachInterrupt(void); // Disable comparator interrupt // (default)
void ACOMP_enable(void); // Enable comparator // (default: Enabled)
void ACOMP_disable(void);// Force comparator off
boolean ACOMP_read(void); // Read the value at the output of the comparator
void ACOMP_setPositiveInput(ADC_modes in); // Set where to connect + Comparator input (ADC_1V1, ADC_AIN0) (default: ADC_AIN0 - pin 6)
void ACOMP_setNegativeInput(ADC_modes in); // Set where to connect - Comparator input (ADC_AIN1, ANALOG_MUX) (default: ADC_AIN1 - pin 7)
// divider 2: 3.04 µs (sampling rate 329,000 kHz)
// divider 4: 4.72 µs (sample rate 210,000 kHz)
// divider 8: 8.04 µs (sampling rate 125,000 kHz)
// divider 16: 15.12 µs (sampling rate 66 100 kHz)
// divider 32: 28.04 µs (sampling rate 35600 kHz)
// divider 64: 56.04 µs (sampling rate 17800 kHz)
// divider 128: 112 µs (sampling rate 8900 Hz)
```

<a id="example"></a>
## Example
See **examples** for other examples!
```cpp
#include <directADC.h>
/* example of simple work with ADC */
void setup() {
  Serial.begin(9600);
  ADC_enable(); // required to be called
  ADC_setPrescaler(64); // no call - divisor 2
  ADC_setReference(ADC_VCC); // no call - ADC_AREF
  setAnalogMux(ADC_A4); // select ADC_A0-ADC_A7 / ADC_SENSOR - thermometer / ADC_1V1 / ADC_GND // No call - ADC_A0
}

void loop() {
  ADC_startConvert(); // manual start of conversion
  while(!ADC_available()); // while the transformation is not ready - wait or do something useful
  Serial.println(ADC_read());

  // ADC_read(); - direct gluing and reading, if the transformation has not yet ended - will return an erroneous value.
  // ADC_readWhenAvailable(); - wait for the conversion to finish and return the result, if it is already ready, it will return immediately.
}
```

<a id="versions"></a>
## Versions
- v1.1 - added 8 bit read functions (thanks to Vitve4) https://github.com/AlexGyver/GyverLibs/pull/43

<a id="feedback"></a>
## Bugs and feedback
When you find bugs, create an **Issue**, or better yet, write to [alex@alexgyver.ru](mailto:alex@alexgyver.ru)
The library is open for revision and your **Pull Request**'s!