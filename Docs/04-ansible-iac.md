# 04: Infraestructura como Código (IaC) con Ansible

## El Cambio de Paradigma: De Manual a Automatizado
Tras completar la configuración inicial del servidor de forma manual para comprender los fundamentos del sistema, se procedió a implementar un enfoque de Infraestructura como Código (IaC). El uso de Ansible permite que la configuración del servidor sea reproducible, versionable y libre de errores humanos.

Al ser una herramienta "agentless", Ansible utiliza el protocolo SSH (previamente configurado) para gestionar el nodo sin añadir carga extra de procesamiento al hardware limitado del Celeron.

## Estructura del Proyecto de Automatización
Se ha diseñado una estructura de control basada en:
- **Inventario:** Definición de los hosts y variables de conexión.
- **Playbooks:** Archivos YAML que describen el "estado deseado" del servidor.
- **Módulos:** Uso de herramientas nativas de Ansible para la gestión de paquetes, usuarios y servicios.

## Implementación de la Configuración Base
El primer Playbook de aprovisionamiento automatiza las tareas críticas realizadas en las fases anteriores:

1. **Gestión de Paquetes:** Actualización del sistema y limpieza de dependencias.
2. **Seguridad:** Refuerzo de las reglas de UFW y políticas de SSH.
3. **Runtime de Contenedores:** Verificación y mantenimiento del motor Docker.

```bash
# Ejemplo de ejecución del Playbook de configuración
ansible-playbook -i inventory.ini setup_server.yml
```
## Mi primer Playbook: Automatización del Sistema Base
Se implementó un Playbook (`01_setup_system.yml`) para estandarizar el estado inicial del servidor. Este script automatiza:
- La actualización de repositorios (`apt update`).
- La instalación de dependencias esenciales de administración (htop, curl, git).
- La garantía de persistencia de las reglas del Firewall para el servicio SSH.

### Concepto clave: Idempotencia
Una de las mayores ventajas de Ansible aplicadas en este paso es la **idempotencia**. Si ejecuto el Playbook múltiples veces, Ansible detecta qué tareas ya fueron realizadas y solo aplica los cambios necesarios, evitando inconsistencias en el servidor.
