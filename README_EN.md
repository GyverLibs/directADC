This is an automatic translation, may be incorrect in some places. See sources and examples!

# Directadc
Library for expanded manual control of ADC and comparator Atmega328
- The functions of the library allow access to all capabilities and operating modes with ADC
- Nothing is cut and not simplified, all the functionality described in the Datite is available

## compatibility
Atmega328 and other from this generation

### Documentation
There is [expanded documentation] to the library (https://alexgyver.ru/directadc/)

## Content
- [installation] (# Install)
- [initialization] (#init)
- [use] (#usage)
- [Example] (# Example)
- [versions] (#varsions)
- [bugs and feedback] (#fedback)

<a id="install"> </a>
## Installation
- The library can be found by the name ** Directadc ** and installed through the library manager in:
    - Arduino ide
    - Arduino ide v2
    - Platformio
- [download the library] (https://github.com/gyverlibs/directadc/archive/refs/heads/main.zip) .Zip archive for manual installation:
    - unpack and put in * C: \ Program Files (X86) \ Arduino \ Libraries * (Windows X64)
    - unpack and put in * C: \ Program Files \ Arduino \ Libraries * (Windows X32)
    - unpack and put in *documents/arduino/libraries/ *
    - (Arduino id) Automatic installation from. Zip: * sketch/connect the library/add .Zip library ... * and specify downloaded archive
- Read more detailed instructions for installing libraries [here] (https://alexgyver.ru/arduino-first/#%D0%A3%D1%81%D1%82%D0%B0%BD%D0%BE%BE%BE%BED0%B2%D0%BA%D0%B0_%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA)
### Update
- I recommend always updating the library: errors and bugs are corrected in the new versions, as well as optimization and new features are added
- through the IDE library manager: find the library how to install and click "update"
- Manually: ** remove the folder with the old version **, and then put a new one in its place.“Replacement” cannot be done: sometimes in new versions, files that remain when replacing are deleted and can lead to errors!


<a id="init"> </a>
## initialization
No

<a id="usage"> </a>
## Usage
`` `CPP
VOID SetanAlogmux (Adc_Modes Mux);// analog entrance (Adc_a0-adc_a7)/ Thermal attemptor (Adc_Sensor)/ 1.1V (Adc_1v1)/ Adc_Gnd (Default: ADC_A0)
VOID adc_enable (VOID);// Turn on the ADC
VOID Adc_Disable (VOID);// Turn off ADC (Default)
VOID Adc_SetPrescaler (Byte Prescl);// Choose a frequency divider of ADC (2, 4, 8, 16, 32, 64, 128) // (Default: 2)
VOID Adc_Setreference (Adc_Modes Ref);// Choose the source of the support voltage ADC (Adc_1v1, Adc_aref, Adc_vcc) // (Default: Adc_aref)
VOID Adc_autotriggerenable (Adc_Modes Trig);// Turn on the ADC autostart and select the event (Free_run, Analog_comp, Adc_int0, Timer0_compa, Timer0_ovf, Timer1_compb, Timer1_ovf)
Void adc_autotriggerdisable (VOID);// Turn off the autorol ADC // (Default)
Void adc_attachinterrapt (VOID (*isr) ());// enable an interruption of the readiness of the ADC and choose a function that will be performed
Void adc_detachinterrapt (VOID);// Turn off the interruption of the readiness of the ACP // (Default)
VOID Adc_StartConvert (VOID);// manual transformation launch
UNSIGNED in Adc_read (VOID);// Read the value of registerCranberries in ADC (the call to the end of the transformation will return the wrong result)
Boolean Adc_available (Void);// Check the readiness of the transformation of ADC
UNSIGNED intact_readwhenavailable (VOID);// wait for the end of the current transformation and return the result
VOID ACOMP_ATTACHINTERRRUPT (VOID (*isr) (), Adc_Modes Source);// Turn on the interruption of the comparator and choose at what event it will be caused (Falling_Trigger, Rising_Trigger, Change_Trigger)
VOID ACOMP_DETACHINTERRUPT (VOID);// Turn off the interruption of the comparator // (Default)
VOID ACOMP_ENABLE (VOID);// Turn on the comparator // (Default: included)
VOID ACOMP_DISBLE (VOID);// forcibly turn off the comparator
Boolean acomp_read (VOID);// Read the value at the comparator output
VOID ACOMP_SETPOSITIVEINPUT (Adc_Modes in);// Configure where is the subclaw +comparator entrance (Adc_1v1, Adc_ain0) (Default: Adc_ain0 - PIN 6)
VOID ACOMP_SETNEGATIVEINPUT (Adc_Modes in);// Configify where the sub -comparator is input (Adc_ain1, analog_mux) (Default: Adc_ain1 - Pin 7)
// divider 2: 3.04 μs (digitization frequency 329,000 kHz)
// divider 4: 4.72 μs (digitization frequency 210,000 kHz)
// divider 8: 8.04 μs (digitization frequency of 125,000 kHz)
// divider 16: 15.12 μs (digitization frequency 66 100 kHz)
// divider 32: 28.04 μs (digitization frequency 35 600 kHz)
// divider 64: 56.04 μs (digitization frequency 17 800 kHz)
// divider 128: 112 μs (digitization frequency 8 900 Hz)
`` `

<a id="EXAMPLE"> </a>
## Example
The rest of the examples look at ** Examples **!
`` `CPP
#include <directadc.h>
/ * an example of simple work with ADC */
VOID setup () {
  Serial.Begin (9600);
  Adc_enable ();// CALLY is necessarily
  Adc_SetPrescaler (64);// without call - divider 2
  Adc_Setreference (ADC_VCC);// without call - adc_aref
  setanAlogmux (adc_a4);// Select adc_a0-adc_a7 / adc_sensor - thermometer / adc_1v1 / adc_gnd // without call - adc_a0
}

VOID loop () {
  Adc_startConvert ();// manual start of transformation
  While (! Adc_available ());// So far, the transformation is not ready - we are waiting or doing something useful
  Serial.println (adc_read ());

  // adc_read ();- Direct gluing and reading, if the transformation has not yet ended, will return an erroneous value.
  // Adc_readwhenavailable ();- It will wait for the end of the transformation and will return the result if it is already ready, immediately.
}
`` `

<a id="versions"> </a>
## versions
- v1.1 - added reading functions 8 bits (thanks vitve4) https://github.com/alexgyver/gyverlibs/pull/43

<a id="feedback"> </a>
## bugs and feedback
Create ** Issue ** when you find the bugs, and better immediately write to the mail [alex@alexgyver.ru] (mailto: alex@alexgyver.ru)
The library is open for refinement and your ** pull Request ** 'ow!


When reporting about bugs or incorrect work of the library, it is necessary to indicate:
- The version of the library
- What is MK used
- SDK version (for ESP)
- version of Arduino ide
- whether the built -in examples work correctly, in which the functions and designs are used, leading to a bug in your code
- what code has been loaded, what work was expected from it and how it works in reality
- Ideally, attach the minimum code in which the bug is observed.Not a canvas of a thousand lines, but a minimum code