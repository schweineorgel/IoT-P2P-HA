## Hardware – Implementación real

A continuación se muestran los nodos del sistema en su implementación física actual.

### Implementación de los nodos
![Nodos (Sensores & Actuador)](assets/sensor_actuator_nodes.jpeg)

El sistema está compuesto por dos ESP32 independientes que se comunican entre sí mediante ESP-NOW.
Uno de ellos actúa como nodo sensor, equipado con radar mmWave para detección de presencia y un sensor de luminosidad.
El segundo funciona como nodo actuador e integra un SSR diseñado en KiCad para el control seguro de carga eléctrica.

### Home Assistant
![Home Assistant dashboard](assets/homeassistant_dashboard.jpeg)

Home Assistant se utiliza como capa de supervisión y control manual (override),
sin actuar como punto único de fallo del sistema.

> Las capturas de pantalla corresponden al servidor de Home Assistant
> ejecutándose en Docker sobre un MacBook Air A1465 utilizado como host de pruebas.

### ESPHome
![ESPHome nodes](assets/esphome_nodes.jpeg)

ESPHome se emplea para la configuración y el monitoreo de los nodos ESP32.

## Tecnologías aplicadas
ESP32 | ESPHome | ESP-NOW | Home Assistant | Docker | Debian 12 | KiCad | YAML
