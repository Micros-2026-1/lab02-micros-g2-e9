[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/KzqfxGd5)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=22940028&assignment_repo_type=AssignmentRepo)
# Lab02 - Caracterización de osciladores (externo vs. interno)


## 1. Integrantes

* [Johan Sebastian Leon Villabon](https://github.com/johanleon96)
* [Juan David Riaño Gutierrez](https://github.com/DONTFISRT)
* [Jairo Alberto Hernandez Avila](https://github.com/jairoaha)

## 2. Documentación

# Laboratorio 1 - Microprocesadores / Caracterización de osciladores con microcontrolador PIC 

## Indice
#### 1.1 [Resumen y Marco Teorico]

#### 1.2 [Herramientas y materiales usados]

#### 1.3 [Procedimiento y montajes realizados]

### 2.1 Descripción del laboratorio

### 2.2 Explicación del código implementado

### 2.3 Análisis y comparación

#### Tabla 1: Medición en frío

| Modo de oscilador | Freq. teórica Fosc | RA6 medible (CLKO)? | Freq. medida RA6 (Hz) | Freq. teórica RC0 (Hz)| Freq. medida RC0 (Hz) | Error RC0 (%) |  
|------------------|------------------|---------------------|---------------|---------------------|---------------|---------------|
| INTOSC (interno) | 16,000,000       | Sí                 | 31.4               |             500                 |    31         |               | |
| HS (cristal externo 16 MHz) | 16,000,000 | No |     NA      |               500                 |               |               |
| RC externo       | ~16,000,000*     | No                                    |       N/A        | 500                 | 496.1           |               | |

#### Tabla 2: Medición con calor

| Modo de oscilador | Freq. teórica Fosc | RA6 medible (CLKO)? | Freq. medida RA6 (Hz) | Freq. teórica RC0 (Hz)| Freq. medida RC0 (Hz) | Error RC0 (%) |  
|------------------|------------------|---------------------|---------------|---------------------|---------------|---------------|
| INTOSC (interno) | 16,000,000       | Sí                 |                     |                500                 |               |               | |
| HS (cristal externo 16 MHz) | 16,000,000 | No |     NA      |               500                 |               |               |
| RC externo       | ~16,000,000*     | No                                    |       N/A        | 500                 |               |               | |

#### Tabla 3: Deriva

| Modo de oscilador |RC0 deriva (Hz) |
|------------------|--------------------|
| INTOSC (interno) |                    |                
| HS (cristal externo 16 MHz) |                |                |
| RC externo       |                 |                


<!-- Agregar tablas para valores usando PLL -->

<!-- Complemente con análisis de lo registrado en tablas -->

## 2.4 Diagramas

## 2.5 Formas de onda

### INTOSC (interno) 


### HS

## RC

## 3. Evidencias de implementación

## 4. Preguntas

* ¿En qué modo se obtuvo la medición más cercana a la frecuencia teórica?

* ¿Fue posible evidenciar el fenómeno de deriva? ¿Qué factores podrían explicar la variación de frecuencia al calentar el PIC?

* ¿Cuál es más preciso en cuanto a frecuencia teórica vs. medida?


* Explique cómo usar RC0 para estimar la frecuencia del oscilador cuando RA6 no está disponible.

* Si se quisiera duplicar la frecuencia del PIC usando PLL, ¿en qué modos se podría aplicar?

* Enliste ventajas y desventajas de cada modo.

## 5. Referencias
