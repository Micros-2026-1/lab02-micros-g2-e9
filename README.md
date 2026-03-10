[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/KzqfxGd5)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=22940028&assignment_repo_type=AssignmentRepo)
# Lab02 - Caracterización de osciladores (externo vs. interno)


## 1. Integrantes

* [Johan Sebastian Leon Villabon](https://github.com/johanleon96)
* [Juan David Riaño Gutierrez](https://github.com/DONTFISRT)
* [Jairo Alberto Hernandez Avila](https://github.com/jairoaha)

## 2. Documentación

# Laboratorio 2 - Microprocesadores / Caracterización de osciladores con microcontrolador PIC 

## Indice
#### 1.1 [Resumen y Marco Teorico]

#### 1.2 [Herramientas y materiales usados]

#### 2.1 Descripción del laboratorio

#### 2.2 Explicación del código implementado

#### 2.3 Análisis y comparación

#### 3 [Evidencias de implementación]

## 1.1 Resumen

En esta práctica hemos realizado el analisis, modelación, relacionamiento y montaje de  dos circuitos operados por un **PIC18F45K22**  que les brindará una salida digital mediante un código de programación hecho en lenguaje **C** por medio de un compilador llamado **MPLAB X IDE**.  Así, con esto, empezar a comprender un poco más el funcionamiento de los microprocesadores en un entorno relacionado a osciladores, condensadores y medidas de señales.

## * Marco Teorico

**PIC18F45K22:** Es un microcontrolador de alto desempeño, con arquitectura optimizada basado en lenguaje ```C compiler``` en encapsulado tipo ```DIP``` de 18 pines, este es un componente de bajo consumo con memoria SRAM de 1536 bytes.

**Lenguaje C:** Es un lenguaje de programación de propósito general​ originalmente desarrollado por Dennis Ritchie entre 1969 y 1972. Es muy eficiente y ofrece la posibilidad de manejar todos los aspectos de las instrucciones del ```CPU```. Su código ofrece una estructura clara y, por tanto, facilita la creación de aplicaciones de una forma rápida y potente.

**MPLAB X IDE:** Funciona como una interfaz unificada para herramientas de desarrollo de software y hardware adicionales de ```Microchip``` y de terceros. Es un  entorno de desarrollo integrado (IDE) gratuito y multiplataforma de Microchip para programar microcontroladores PIC® y AVR®, basado en NetBeans. 

**Oscilador:** Los osciladores son componentes esenciales que producen una señal electrónica periódica, típicamente una onda senoidal o cuadrada. Los osciladores convierten la señal de CC en señales de CA periódicas que pueden ser utilizadas para establecer la frecuencia, ser usadas en aplicaciones de audio, o utilizadas como una señal de reloj. Todos los microcontroladores y microprocesadores requieren de un oscilador para establecer la señal de reloj para poder funcionar.

**Osciloscopio:** Es un instrumento que muestra gráficamente señales eléctricas y cómo estas cambian con el tiempo. Mide estas señales conectándose a un sensor, un dispositivo que genera una señal eléctrica en respuesta a estímulos físicos como el sonido, la luz y el calor. 

## 1.2 Herramientas y materiales

* Microcontrolador ```PIC18F45K22``` 

* LEDS.

* Resistencias 330Ω y 1kΩ.

* Programador (PICkit $3$, PICkit $4$).

* Fuente de alimentación de $5$ V → El PICkit $3$ o $4$ para suministrar tensión directamente al circuito (típicamente $5$ V o $3.3$ V, según se configure en MPLAB X)

* Entorno de programación ```MPLAB X IDE``` con compilador ```XC8```.

* Condensadores electrolíticos de 21pF y 15pF.
  
* Oscilador de Cuarzo de 16 MHz
  
* Osciloscopio
  
* Computador con un SO relacionable

* Protoboard y cables de conexión.

### 2.1 Descripción del laboratorio

Realiazamos la operación de configurar y programar el  **PIC18F45K22**  para operar con condensadores, con su oscilador interno y externo basado en un cristal de cuarzo. Con el **oscilador interno ```(INTOSC)```** y con el **oscilador externo (cristal)** se va a verificar la frecuencia real del integrado y el circuito generado en una señal de referencia del pin de salida y medir dicha señal con un osciloscopio para verificar su estado de trabajo, señal y frecuencia relacionada. 

Acto seguido, se compara los modos de operación entre ```(Cuarzo cristal vs INTOSC vs RC)``` en cuanto a:

- Precisión de frecuencia, estabilidad de la señal, efectos de cambio de medida de acuerdo a sus manipulaciones y afectaciones térmicas de acuerdo a su medida de frecuencia.

### 2.2 Explicación del código implementado

```
// El xc.h permite usar los registros del microcontrolador.
// stdint.h permite usar tipos de datos como uint16_t.

#include <xc.h>    
#include <stdint.h>

// ========================== CONFIGURACIÓN GENERAL ========================
// Configuraciones que no dependen del modo de oscilador
#pragma config WDTEN = OFF      
#pragma config LVP = OFF        
#pragma config PBADEN = OFF     
#pragma config CP0 = OFF, CP1 = OFF, CP2 = OFF, CP3 = OFF  
#pragma config BOREN = OFF      
#pragma config FCMEN = OFF      
#pragma config IESO = OFF       

// ========================== MODO DE OSCILADOR ==========================
// 1 = INTOSC interno
// 2 = Cristal externo HS
// 3 = RC externo

#define MODE 1
// Define para el microcontrolador el pscilador que el usario dese usar  ======
#if MODE == 1
    #pragma config FOSC = INTIO67   // Oscilador interno
    #define USE_PLL 0
#elif MODE == 2
    #pragma config FOSC = HSHP     // Cristal HS
    #define USE_PLL 0
#elif MODE == 3
    #pragma config FOSC = RC       // RC externo
    #define USE_PLL 0
#else
    #error "Modo de oscilador inválido"
#endif


// ========================== FRECUENCIA DEL OSCILADOR =====================
// Indica que el microcontrolador trabaja a tal frecuencia 
#if MODE == 1 || MODE == 2
    #if USE_PLL
        #define _XTAL_FREQ 64000000UL
    #else
        #define _XTAL_FREQ 16000000UL
    #endif
#else
    #define _XTAL_FREQ 16000000UL // Ajustar según resistencia + condensador
#endif

// ========================== FUNCIONES ==========================
// Este comienza comienza la ejecución del programa.
void delay_ms(uint16_t ms) {
    while(ms--) {
        __delay_ms(1);
    }
}

void init_pins(void) {
    // RC0 salida blinker
    TRISCbits.TRISC0 = 0;
    LATCbits.LATC0 = 0;

    // RA6 salida CLKO solo si modo lo permite
    if(MODE == 1 || (MODE == 2 && USE_PLL)) {
        TRISAbits.TRISA6 = 0;
        LATAbits.LATA6 = 0;
    }
}

void init_oscillator(void) {
#if USE_PLL
    OSCCONbits.SPLLEN = 1;  // habilita PLL
#endif
}

// ========================== PROGRAMA PRINCIPAL ==========================
void main(void) {
    init_pins();
    init_oscillator();

    while(1) {
        // RC0 toggle ≈ 500 Hz
        LATCbits.LATC0 = 1;
        delay_ms(1);
        LATCbits.LATC0 = 0;
        delay_ms(1);
    }
}

```

### 2.3 Análisis y comparación

#### Tabla 1: Medición en frío

| Modo de oscilador | Freq. teórica Fosc | RA6 medible (CLKO)? | Freq. medida RA6 (Hz) | Freq. teórica RC0 (Hz)| Freq. medida RC0 (Hz) | Error RC0 (%) |  
|------------------|------------------|---------------------|---------------|---------------------|---------------|---------------|
| INTOSC (interno) | 16,000,000       | Sí                 | 31.4             |   500     |    31         |    93 %| |
| HS (cristal externo 16 MHz) | 16,000,000 | No |     NA      |   500                 | 496.1             |   0.8 %            |
| RC externo       | ~16,000,000*     | No      |       N/A        | 500                 |    501.6      |    0.32 %         | |

#### Tabla 2: Medición con calor

| Modo de oscilador | Freq. teórica Fosc | RA6 medible (CLKO)? | Freq. medida RA6 (Hz) | Freq. teórica RC0 (Hz)| Freq. medida RC0 (Hz) | Error RC0 (%) |  
|------------------|------------------|---------------------|---------------|---------------------|---------------|---------------|
| INTOSC (interno) | 16,000,000       | Sí    |  NO HAY SEÑAL      |  500           |     30.67     |    93.86 %       | |
| HS (cristal externo 16 MHz) | 16,000,000 | No |     NA      |    500       |          501.63     |      0.326 %         |
| RC externo       | ~16,000,000*     | No          |       N/A        | 500             |  419.7    |      16.06 %         | |

#### Tabla 3: Deriva

| Modo de oscilador |RC0 deriva (Hz) |
|------------------|--------------------|
| INTOSC (interno) |      468.6           |                
| HS (cristal externo 16 MHz) |    3.9     |                |
| RC externo       |         1.6        |                


<!-- Agregar tablas para valores usando PLL -->

<!-- Complemente con análisis de lo registrado en tablas -->

## 2.4 Diagramas

![pic](/proteus.jpg)
### 1. Diagrama simulación ```PROTEUS```

![pic](/pinesref.jpg)
### 2. Diagrama pines de referencia Pickit y Microcontrolador ```PIC18F45K22``` 

## 2.5 Formas de onda
![pic](/Simulacion.png)
### INTOSC (interno) 
![pic](/ondaintcalor.png)
### HS
![pic](/ondacalorcristal.png)
## RC
![pic](/ondacalorRC0.png)
## 3. Evidencias de implementación
![pic](/montajeosc.jpeg)
## 4. Preguntas

* ¿En qué modo se obtuvo la medición más cercana a la frecuencia teórica?

* ¿Fue posible evidenciar el fenómeno de deriva? ¿Qué factores podrían explicar la variación de frecuencia al calentar el PIC?

* ¿Cuál es más preciso en cuanto a frecuencia teórica vs. medida?


* Explique cómo usar RC0 para estimar la frecuencia del oscilador cuando RA6 no está disponible.

* Si se quisiera duplicar la frecuencia del PIC usando PLL, ¿en qué modos se podría aplicar?

* Enliste ventajas y desventajas de cada modo.

## 5. Referencias
