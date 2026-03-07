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
