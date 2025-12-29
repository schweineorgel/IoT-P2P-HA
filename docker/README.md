# IoT Descentralizado - Docker Setup
Este repositorio contiene la configuración de contenedores Docker para la integración de Home Assistant y ESPHome en un sistema IoT autónomo basado en ESP32 y ESP-NOW.

## Contenedores incluidos
- **Home Assistant**: Orquestado en Debian 12 mediante Docker, para monitoreo y override manual.
- **ESPHome**: Orquestado en Debian 12 mediante Docker, para flasheo y gestión de ESP32.

## Requisitos
- Debian 12 o equivalente
- Docker y Docker Compose instalados
- Red local (solo para Home Assistant; los ESP32 funcionan peer-to-peer vía ESP-NOW)

## Instalación

1. Clonar el repositorio:
```bash
git clone <repo_url>
cd <repo_folder>
```

2. Iniciar contenedores:

`docker-compose up -d`

Acceder a Home Assistant en:
http://host:8123

Acceder a ESPHome en:
http://host:6052 (si se expone el dashboard web)

## Estructura
```
docker/
├── docker-compose.yml
└── README.md
```