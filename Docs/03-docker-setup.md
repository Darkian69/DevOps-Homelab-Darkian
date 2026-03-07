# 03: Instalación y Optimización de Docker Engine

## Introducción a la Contenedorización
Docker es la herramienta estándar en la cultura DevOps para el empaquetado y despliegue de aplicaciones. A diferencia de las máquinas virtuales tradicionales, Docker comparte el kernel del sistema operativo host, lo que lo hace extremadamente ligero y eficiente para hardware con recursos limitados como este procesador Celeron.

La adopción de Docker en este proyecto permite garantizar la portabilidad de los servicios y una gestión simplificada de dependencias.

## Metodología de Instalación
Se ha evitado el uso de paquetes genéricos de los repositorios de la distribución, optando por el repositorio oficial de Docker para asegurar estabilidad, parches de seguridad actualizados y acceso a las últimas características de Docker Compose.

### Pasos ejecutados:
1. **Configuración de Seguridad:** Instalación de certificados CA y llaves GPG oficiales para validar la integridad de los binarios.
2. **Provisionamiento del Repositorio:** Configuración del canal `stable` específico para la arquitectura del sistema.
3. **Instalación de Componentes Core:**
   - `docker-ce`: El motor de ejecución.
   - `docker-compose-plugin`: Herramienta de orquestación local para despliegues multi-contenedor.

## Configuración de Privilegios (Post-Install)
Para facilitar los flujos de trabajo de CI/CD y automatización, se realizó una gestión de grupos en Linux:

```bash
sudo usermod -aG docker ${USER}
```
