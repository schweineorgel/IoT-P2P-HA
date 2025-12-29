# IoT Descentralizado - ESPHome
Firmware para ESP32 en un sistema de automatización doméstica descentralizada usando ESP-NOW, diseñado para funcionar incluso sin conexión a la red Wi-Fi o servidor central.

## Dispositivos
- ESP32 Sensor
Sensores: radar mmWave (presencia) y sensor de luminosidad (lux)
Función: detecta presencia y luminosidad, y envía un token de acción al actuador mediante ESP-NOW si se cumplen ciertas condiciones.

- ESP32 Actuador
Control: carga eléctrica mediante SSR diseñado en KiCad
Función: recibe token del sensor y activa/desactiva la carga; mantiene sincronización local aunque Home Assistant esté desconectado.

## Comunicación
- Peer-to-peer vía ESP-NOW, independiente de Wi-Fi o servidor central.

>Inicialmente se probó MQTT, pero se descartó por su alta latencia y vulnerabilidad de seguridad.

## Instalación y flasheo

1. Conectar el ESP32 al computador.

2. Flashear usando dashboard de ESPHome.
- `actuator_node.yaml` 
- `sensor_node.yaml`

Verificar logs para confirmar conexión y estado.
(No olvidar ingresar las direcciones MAC de los ESP32 en el secrets.yaml)

## Estructura de archivos
```
esphome/
├── sensor_node.yaml
├── actuator_node.yaml
├── secrets.yaml.example
└── README.md
```