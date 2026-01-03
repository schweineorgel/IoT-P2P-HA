[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
![Status](https://img.shields.io/badge/status-stable-green)

Demostraciones reales del sistema disponibles en video (ver sección “Demostraciones en video”).

# Sistema IoT descentralizado con ESP32 y ESP-NOW

Sistema de automatización doméstica diseñado para operar de forma autónoma,
segura y tolerante a fallos, utilizando comunicación peer-to-peer entre nodos
ESP32 mediante ESP-NOW, sin dependencia de red Wi-Fi ni broker central.

## Alcance del repositorio y estructura de diseño

Este repositorio documenta el proceso completo de diseño, prototipado e
iteración de un sistema IoT descentralizado con ESP32 y ESP-NOW.

Los módulos incluidos pueden encontrarse en distintos estados de abstracción:
- Versiones finales con esquemáticos y diseños PCB.
- Placas de interconexión (solo PCB) orientadas a exponer pines físicos.
- Versiones preliminares o simplificadas utilizadas durante el proceso de
  validación experimental.

Cada módulo se documenta en su propio directorio dentro de `hardware/`,
incluyendo:
- El propósito funcional.
- Nivel de abstracción (esquemático completo, solo PCB, placa de interconexión).
- Relación entre diseño y la implementación física real.
- Decisiones de diseño relevantes y limitaciones conocidas.

La estructura general del directorio `hardware/` es la siguiente:

```
hardware/
├── nodemcu_sensor_pcb
├── nodemcu_ssr_pcb
├── sensor_pcb
└── ssr_pcb
```
Cada uno de estos directorios incluye su propio README detallado.

## Cómo leer este repositorio

Este repositorio no está orientado a la reproducción directa del sistema,
sino a la comprensión del proceso de diseño y validación.

Se recomienda el siguiente orden de lectura:

1. README principal (este archivo), para entender la arquitectura general.
2. Directorio `hardware/`, donde cada módulo cuenta con un README específico.
3. Directorios de firmware y configuración (ESPHome / Home Assistant), según interés.

Cada módulo de hardware debe interpretarse como una unidad independiente,
con su propio contexto, alcance y nivel de madurez.

## Arquitectura general

El sistema está compuesto por nodos ESP32 que toman decisiones localmente y
se comunican entre sí mediante ESP-NOW. La supervisión mediante Home Assistant
es opcional y no afecta la operación del sistema ante caídas de red o servidor.

## Flujo de funcionamiento

1. El nodo sensor detecta presencia mediante radar mmWave.
2. Se evalúa la luminosidad ambiente.
3. Si se cumplen las condiciones, se envía un token de acción vía ESP-NOW.
4. El nodo actuador conmuta la carga y confirma la acción localmente.

## Notas de diseño e iteraciones

Durante el desarrollo del nodo actuador se realizaron varias iteraciones en el
diseño del SSR, motivadas por pruebas prácticas y validación experimental del
comportamiento del circuito.

![SSR (Placa del Relé de Estado Sólido)](assets/ssr_top_bottom.jpeg)

> En la imagen se observan dos PCBs del SSR diseñado en KiCad: una vista frontal
> y una vista posterior.

En la cara posterior se aprecia la etapa de control basada en un transistor,
cuya resistencia en la base permite regular la amplificación de corriente en el
colector, junto a un LED indicador conectado en paralelo para señalización de
activación.
El emisor del transistor se conecta a tierra mediante un bypass implementado
con una resistencia de 0 Ω.

Esta configuración permite limitar la corriente suministrada al optoacoplador
(MOC), evitando la sobrecarga de los pines GPIO del ESP32 y manteniendo corrientes
de activación seguras, cercanas a los 9 mA por cada SSR activo.

### Desarrollo del SSR

El diseño inicial del SSR se basó en ingeniería inversa de un módulo comercial
capaz de operar con tensiones de control en un rango amplio (aprox. 3 V a 32 V).
Si bien el principio de funcionamiento era correcto, la reproducción directa
del diseño no resultó funcional al implementarse en una PCB propia, presentando
fallas de activación consistentes durante las pruebas.

Ante este comportamiento, se optó por una solución experimental que permitiera
controlar de forma más precisa la corriente de excitación del optoacoplador (MOC).
Para ello, se utilizó el LED indicador, que opera con corrientes del orden de
microamperes, junto con un transistor conectado en paralelo, actuando como una
etapa de control de corriente hacia el MOC, equivalente a una resistencia
variable, aprovechando las características de ganancia (hFE) del transistor.

Esta solución fue implementada y validada en tres SSR independientes, obteniendo
corrientes de activación muy similares entre unidades, lo que permitió confirmar
la reproducibilidad y estabilidad del enfoque.

Una vez validado el comportamiento del SSR en condiciones reales de carga, el
diseño fue posteriormente simplificado en la versión final, eliminando la etapa
activa cuando fue posible y ajustando la limitación de corriente mediante
resistencias, manteniendo un consumo seguro para los GPIO del ESP32.

### Elección del protocolo de comunicación

Inicialmente se evaluó el uso de MQTT como protocolo de comunicación entre nodos.
Sin embargo, debido a la latencia introducida por el broker y a la dependencia
de red, se optó por migrar a ESP-NOW.

El uso de comunicación peer-to-peer permitió reducir la latencia, eliminar
dependencias externas y garantizar la operación local del sistema ante caídas
de red o del servidor de supervisión.

## Fabricación de prototipos

Los prototipos de las placas PCB fueron diseñados en KiCad y posteriormente
fabricados mediante fresado CNC a partir de G-code generado con FlatCAM.

Este enfoque permitió iterar sobre el diseño del SSR, validar
cambios eléctricos en hardware real y ajustar parámetros de corriente y
activación sin depender de servicios externos de fabricación.

## Hardware – Implementación real

### Implementación de los nodos
![Nodos (Sensores & Actuador)](assets/sensor_actuator_nodes.jpeg)

El sistema está compuesto por dos ESP32 independientes que se comunican entre sí mediante ESP-NOW.
Uno de ellos actúa como nodo sensor, equipado con radar mmWave para detección de presencia y un sensor de luminosidad.
El segundo funciona como nodo actuador e integra un SSR diseñado en KiCad para el control seguro de carga eléctrica.

## Demostraciones en video

A continuación se incluyen videos demostrativos del funcionamiento real del
sistema en condiciones de uso, con el hardware y firmware descritos en este
repositorio.

### Control manual (override)

En este video se muestra el funcionamiento del nodo actuador cuando el sistema
se encuentra en modo *override*, permitiendo la conmutación manual de los SSR
desde Home Assistant.

El control manual no interfiere con la lógica distribuida del sistema y puede
habilitarse o deshabilitarse sin afectar la comunicación ESP-NOW entre nodos.

[![Control manual del nodo actuador](https://img.youtube.com/vi/FyrNp5YLqkk/0.jpg)](https://youtu.be/FyrNp5YLqkk)

### Detección de presencia y activación automática

Este video muestra la detección de presencia mediante radar mmWave en el nodo
sensor y la posterior activación automática del nodo actuador bajo condiciones
de baja luminosidad.

La decisión se toma de forma local en el nodo sensor y se comunica al nodo
actuador mediante ESP-NOW, sin dependencia de red Wi-Fi ni servidor central.

[![Detección de presencia y activación automática](https://img.youtube.com/vi/x1b7EXLiUZI/0.jpg)](https://youtu.be/x1b7EXLiUZI)

### Home Assistant
![Home Assistant dashboard](assets/homeassistant_dashboard.jpeg)

> Las capturas de pantalla corresponden al servidor de Home Assistant
> ejecutándose en Docker sobre un MacBook Air A1465 utilizado como host de pruebas.

Home Assistant se utiliza como capa de supervisión y control manual (override),
sin actuar como punto único de fallo del sistema.

En el dashboard, el nodo **esp32-a** corresponde al nodo sensor, donde se
visualiza el estado de presencia detectada y el nivel de luminosidad ambiental
medido en lux.

El nodo **esp32-b** corresponde al nodo actuador. Desde este se permite el
control manual de hasta cuatro SSR independientes cuando el modo *override*
está habilitado. En modo automático, los SSR se activan de forma simultánea
cuando el nodo sensor (**esp32-a**) envía un token de activación al detectar
presencia bajo condiciones de baja luminosidad.

Los SSR permanecen activos mientras se mantenga la detección de presencia en
la habitación, desactivándose automáticamente al cesar dicha condición.

### ESPHome
![ESPHome nodes](assets/esphome_nodes.jpeg)

ESPHome se emplea para la configuración y el monitoreo de los nodos ESP32.

## Estado del proyecto

El sistema se encuentra en un estado funcional y estable a nivel experimental.
No se contemplan nuevas funcionalidades mayores en el corto plazo, aunque
podrían realizarse mejoras menores o ajustes documentales.

El repositorio tiene como objetivo principal servir como referencia técnica
del proceso de diseño, prototipado e iteración de un sistema IoT descentralizado.

## Tecnologías aplicadas
ESP32 | ESPHome | ESP-NOW | Home Assistant | Docker | Debian 12 | KiCad | YAML
