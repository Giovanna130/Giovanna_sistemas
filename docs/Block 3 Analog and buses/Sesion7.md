# Sesion 7 — ADC
**Goal:** _Program the Raspberry Pi to read and measure the signals from a light sensor, a temperature sensor, a microphone, and a potentiometer, using the analog-to-digital converter (ADC) to obtain their values.._
**Prediction:** _The Raspberry Pi is expected to record different numerical values ​​according to variations in light, temperature, sound, and potentiometer position, allowing observation of how each sensor responds to different stimuli._

## Temperature sensor

### Set Up 
We connected the temperature sensor to the Raspberry Pi, first its power and ground terminals to the corresponding pins on the board, while the signal output was connected to an analog ADC input so that the Raspberry Pi could read the voltage variations.

### What I did
During the experiment, we connected the temperature sensor to the Raspberry Pi using pin GP26, corresponding to channel ADC0, to read the analog signal it generated. We then programmed the board to initialize the analog-to-digital converter (ADC), select the input channel, and obtain the sensor readings. The program converted the digital values, ranging from 0 to 4095, to a voltage between 0 and 3.3 V and subsequently calculated a temperature from that voltage. Finally, the digital readings were displayed on the computer every 500 milliseconds, allowing us to observe how the signals recorded by the sensor changed.

### Code
``` 
#include <stdio.h>
#include "pico/stdlib.h"
#include "hardware/adc.h"
 
// Configure the ADC channel to use
#define ADC_INPUT 0 // channel 0
#define ADC      3.3f      
 
int main() {
    stdio_init_all();
    adc_init();
    // Configure the corresponding GPIO pin as an ADC input
    adc_gpio_init(26); // GPIO26 usually maps to ADC0 on the Pico 2
    // Select the channel
    adc_select_input(ADC_INPUT);
 
    // Optional: adjust the clock divider if you need to limit Fs
    // adc_set_clkdiv(div);
 
    while (true) {
        uint16_t adc = adc_read(); // 12 bits aligned to 0..4095
        float v = (adc * ADC) / 4095.0f;
        float temp = (v * 1.0f) / 0.01f;
        printf("%u\n", adc);
        sleep_ms(500);
    }
}
``` 
### What went wrong
I reversed the temperature sensor connection; due to this incorrect connection, the sensor burned out and it was not possible to continue working with it.

## Light Sensor
### Set Up 
### What I did
### Evidence 
### Code
``` 
#include <stdio.h>
#include "pico/stdlib.h"
#include "hardware/adc.h"
 
// Configure the ADC channel to use
#define ADC_INPUT 0 // channel 0
#define ADC 3.3f    
#define min 0.0f
#define max 165.0f
 
int main() {
    stdio_init_all();
    adc_init();
    // Configure the corresponding GPIO pin as an ADC input
    adc_gpio_init(26); // GPIO26 usually maps to ADC0 on the Pico 2
    // Select the channel
    adc_select_input(ADC_INPUT);
 
    // Optional: adjust the clock divider if you need to limit Fs
    // adc_set_clkdiv(div);
 
    while (true) {
        uint16_t adc = adc_read(); // 12 bits aligned to 0..4095
        double luz = ((adc) * 100.0f) / (165.0f);
        //float luz = ( ) ;
        printf("%u\n", adc);
        printf("%f\n", luz);
        sleep_ms(500);
    }
``` 
### What went wrong

## Potentiometer
### Set Up 
### What I did
### Evidence 
### Code
``` 
#include <stdio.h>
#include "pico/stdlib.h"
#include "hardware/adc.h"
 
// Configure the ADC channel to use
#define ADC_INPUT 0 // channel 0
#define ADC 3.3f  
 
int main() {
    stdio_init_all();
    adc_init();
    // Configure the corresponding GPIO pin as an ADC input
    adc_gpio_init(26); // GPIO26 usually maps to ADC0 on the Pico 2
    // Select the channel
    adc_select_input(ADC_INPUT);
 
    // Optional: adjust the clock divider if you need to limit Fs
    // adc_set_clkdiv(div);
 
    while (true) {
        uint16_t adc = adc_read(); // 12 bits aligned to 0..4095
        float porcentaje = ((adc - 1.0f) * 180.0f) / (3374.0f - 1.0f);
        //float luz = ( ) ;
        printf("%u\n", adc);
        printf("%.1f\n", porcentaje);
        sleep_ms(500);
    }
}
``` 
### What went wrong

## Microphone
### Set Up 
### What I did
### Evidence 
### Code
### What went wrong