## Diseño PCB

![PCB - Vista PCB](screenshots/esp32_sensor.png)

Este diseño corresponde al NodeMCU del nodo sensor y, al igual que el PCB del
nodo actuador, fue desarrollado únicamente a nivel de layout PCB, sin un
esquemático formal asociado.

El diseño se realizó considerando las dimensiones físicas y disposición de
pines del NodeMCU (ESP32), actuando como una placa de interconexión entre el
microcontrolador y la placa de sensores externa.

Se expusieron los siguientes pines del ESP32 para la integración de sensores:

- **SDA / SCL**: bus I²C utilizado para la comunicación con el sensor de
  luminosidad VEML7700.
- **GPIO25 (TX) / GPIO26 (RX)**: interfaz UART empleada para la configuración y
  comunicación con el radar mmWave C4001, permitiendo su operación en modo
  detección de presencia.
- **GPIO27**: entrada digital conectada al pin OUT del sensor mmWave,
  utilizada para indicar detección de presencia.
- **3.3 V y GND**: alimentación de la placa de sensores directamente desde el
  ESP32.