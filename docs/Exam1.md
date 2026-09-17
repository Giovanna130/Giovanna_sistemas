# EXAM 1 

**Goal::** _To develop a code that solves the challenge posed by the professor, using the knowledge learned during the first block of the course_ 

**Prediction:** _We expect to be able to solve the challenge on time and in the correct manner, applying the tools we have learned_

 ## Set Up 
We assembled the circuit using a Raspberry Pi Pico 2W, 10 buttons, and a 9-LED matrix. All components were connected to the Raspberry Pi's GPIO pins to control and detect input and output signals.

![Circuito del examen](recursos/imgs/circuitoexamen.jpeg)

### What I did 
For this exam, we had to solve a puzzle by turning off all the LEDs in a 3x3 matrix using nine buttons. Upon completing the puzzle, the LEDs would flash three times to indicate that we had won. There was also a reset button that changed the position of the LEDs and allowed us to start a new game.

### Evidence 
<iframe width="560" height="315" src="https://www.youtube.com/embed/_JvYY5MS-6Q?si=flXAW5DqeKDQPghC" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### Code
```
#include "pico/stdlib.h"
#include "hardware/gpio.h"
#include "hardware/irq.h"
#include "hardware/structs/sio.h"
 
#define L1 13
#define L2 12
#define L3 11
#define L4 10
#define L5 9
#define L6 8
#define L7 7
#define L8 6
#define L9 5
#define B1 16
#define B2 17
#define B3 18
#define B4 19
#define B5 20
#define B6 21
#define B7 22
#define B8 0
#define B9 1
#define BR 2
 
// Tiempo de espera para que termine el rebote del botón
#define antir_ms 20
// Parámetros yupi yey
#define yupiyey 3
#define vel 200
 
const uint32_t leds = (1u << L1) | (1u << L2) | (1u << L3) | (1u << L4) | (1u << L5) | (1u << L6) | (1u << L7) | (1u << L8) | (1u << L9);
const uint32_t botones = (1u << B1) | (1u << B2) | (1u << B3) | (1u << B4) | (1u << B5) | (1u << B6) | (1u << B7) | (1u << B8) | (1u << B9) | (1u << BR);
static const uint32_t enemigosmortales[9] = {   // lo que va a prender
    (1u << L1) | (1u << L2) | (1u << L4), (1u << L2) | (1u << L1) | (1u << L3) | (1u << L5),
    (1u << L3) | (1u << L2) | (1u << L6), (1u << L4) | (1u << L1) | (1u << L5) | (1u << L7),
    (1u << L5) | (1u << L2) | (1u << L4) | (1u << L6) | (1u << L8), (1u << L6) | (1u << L3) | (1u << L5) | (1u << L9),
    (1u << L7) | (1u << L4) | (1u << L8), (1u << L8) | (1u << L5) | (1u << L7) | (1u << L9),
    (1u << L9) | (1u << L6) | (1u << L8)
};
volatile uint32_t pendientes = 0; // Guarda  botones que fueron presionados
static uint32_t rng_estado;
static void rng_semilla(void) {
    rng_estado = (uint32_t)time_us_64();
    if (rng_estado == 0) rng_estado = 0xA5A5A5A5; // no puede arrancar en 0
}
static uint32_t mi_rand32(void) { //num. aleatorio
    uint32_t x = rng_estado;
    x ^= x << 13;
    x ^= x >> 17;
    x ^= x << 5;
    rng_estado = x;
    return x;
}
static void generar_tablero(void) // patrón aleatorio de LEDs prendidos
{
    uint32_t patron;
    do {
        patron = mi_rand32() & leds;
    }
    while (patron == 0);
    sio_hw->gpio_clr = leds;  // apaga todos los leds
    sio_hw->gpio_set = patron;  // enciende los leds del patrón
}
static void isr_botones(uint pin, uint32_t event_mask)
{
    gpio_acknowledge_irq(pin, event_mask);  // confirma que la interrupción fue atendida
    if (event_mask & GPIO_IRQ_EDGE_FALL) // comprueba si ocurrió un flanco de bajada
    {
        pendientes |= (1u << pin); // guarda botón fue presionado
    }
}
int main(void)
        {//principal
    gpio_init_mask(leds | botones);   // inicia los GPIO
    sio_hw->gpio_oe_set = leds;  
    sio_hw->gpio_oe_clr = botones;
    gpio_pull_up(B1); // Activa el pull-up de cada botón
    gpio_pull_up(B2);
    gpio_pull_up(B3);
    gpio_pull_up(B4);
    gpio_pull_up(B5);
    gpio_pull_up(B6);
    gpio_pull_up(B7);
    gpio_pull_up(B8);
    gpio_pull_up(B9);
    gpio_pull_up(BR); //boton reinicio
 
    gpio_set_irq_enabled_with_callback(BR, GPIO_IRQ_EDGE_FALL,true, &isr_botones); // registra la función de int. para el botón de reinicio.
    gpio_set_irq_enabled(B1, GPIO_IRQ_EDGE_FALL, true);   // activa la int. de cada botón
    gpio_set_irq_enabled(B2, GPIO_IRQ_EDGE_FALL, true);
    gpio_set_irq_enabled(B3, GPIO_IRQ_EDGE_FALL, true);
    gpio_set_irq_enabled(B4, GPIO_IRQ_EDGE_FALL, true);
    gpio_set_irq_enabled(B5, GPIO_IRQ_EDGE_FALL, true);
    gpio_set_irq_enabled(B6, GPIO_IRQ_EDGE_FALL, true);
    gpio_set_irq_enabled(B7, GPIO_IRQ_EDGE_FALL, true);
    gpio_set_irq_enabled(B8, GPIO_IRQ_EDGE_FALL, true);
    gpio_set_irq_enabled(B9, GPIO_IRQ_EDGE_FALL, true);
 
    irq_set_priority(IO_IRQ_BANK0, 0xC0); // configura prioridad
    irq_set_enabled(IO_IRQ_BANK0, true);  // da la interrupción GPIO
    rng_semilla(); // inicia el generador aleatorio
    generar_tablero();   // Genera el primer tablero
    while (true) //bucle
    {
        if (pendientes)     // comprobamos si hay botón pendiente
        {
            sleep_ms(antir_ms);
            irq_set_enabled(IO_IRQ_BANK0, false);  // Desactiva int.
            uint32_t eventos = pendientes;  // Guarda los  pendientes
            pendientes = 0;   // Limpia los pendientes
            irq_set_enabled(IO_IRQ_BANK0, true); // Vuelve a activar int.
            uint32_t presionados =
                eventos & ~sio_hw->gpio_in;  // Lee botones presionados
            if (presionados & (1u << B1)) {
                sio_hw->gpio_togl = enemigosmortales[0];
            }
            if (presionados & (1u << B2)) {
                sio_hw->gpio_togl = enemigosmortales[1];
            }
            if (presionados & (1u << B3)) {
                sio_hw->gpio_togl = enemigosmortales[2];
            }
            if (presionados & (1u << B4)) {
                sio_hw->gpio_togl = enemigosmortales[3];
            }
            if (presionados & (1u << B5)) {
                sio_hw->gpio_togl = enemigosmortales[4];
            }
            if (presionados & (1u << B6)) {
                sio_hw->gpio_togl = enemigosmortales[5];
            }
            if (presionados & (1u << B7)) {
                sio_hw->gpio_togl = enemigosmortales[6];
            }
            if (presionados & (1u << B8)) {
                sio_hw->gpio_togl = enemigosmortales[7];
            }
            if (presionados & (1u << B9)) {
                sio_hw->gpio_togl = enemigosmortales[8];
            }
            if (presionados & (1u << BR)){
                generar_tablero();
            }
             else if ((presionados & ~(1u << BR)) &&    // leds parpadean al mismo tiempo
                     (sio_hw->gpio_out & leds) == 0) {
                for (int i = 0; i < yupiyey ; i++) {
                    sio_hw->gpio_set = leds;
                    sleep_ms(vel);
                    sio_hw->gpio_clr = leds;
                    sleep_ms(vel);  //nueva partida
                }
            }
        }
        tight_loop_contents();
    }
}
```





