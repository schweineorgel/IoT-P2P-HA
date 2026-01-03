## Hardware – Implementación real

A continuación se muestran los nodos del sistema en su implementación física actual.

### Nodos
![Nodos (Sensores & Actuador)](assets/sensor_actuator_nodes.jpeg)

El sistema está compuesto por dos ESP32 independientes que se comunican entre sí mediante ESP-NOW.
Uno de ellos actúa como nodo sensor, equipado con radar mmWave para detección de presencia y un sensor de luminosidad.
El segundo funciona como nodo actuador e integra un SSR diseñado en KiCad para el control seguro de carga eléctrica.

## Tecnologías aplicadas
ESP32 | ESPHome | ESP-NOW | Home Assistant | Docker | Debian 12 | KiCad | YAML