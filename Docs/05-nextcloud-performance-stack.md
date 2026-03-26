# Despliegue de Nextcloud High-Performance Stack

Se implementó  una instancia de Nextcloud optimizada para hardware de recursos limitados (CPU Intel Celeron y 4GB de RAM). Se ha evitado el uso de imágenes monolíticas en favor de una arquitectura de microservicios distribuida para maximizar la eficiencia del sistema.

## Arquitectura del Sistema

Para garantizar un rendimiento fluido en un entorno de bajos recursos, se ha implementado una arquitectura basada en cuatro componentes principales:

1. **Nextcloud (fpm-alpine):** Utiliza la variante FastCGI Process Manager (FPM) sobre Alpine Linux. Esta imagen es significativamente más ligera que la estándar, ya que no incluye un servidor web integrado, delegando esa tarea a un binario especializado.
2. **Nginx (alpine):** Actúa como servidor web frontal y proxy inverso. Se encarga de servir archivos estáticos (imágenes, CSS, JS) de forma nativa y comunica las peticiones dinámicas al contenedor de Nextcloud mediante el protocolo FastCGI.
3. **MariaDB 10.11:** Motor de base de datos relacional. Se ha seleccionado esta versión por su estabilidad y compatibilidad con las optimizaciones de transacciones requeridas por Nextcloud.
4. **Redis (alpine):** Almacenamiento en memoria para el manejo de caché de archivos (File Locking) y sesiones. Su implementación reduce drásticamente las operaciones de lectura/escritura en el disco duro, eliminando cuellos de botella en el I/O del sistema.

## Estrategia de Persistencia: Bind Mounts

Se ha optado por el uso de Bind Mounts en lugar de volúmenes gestionados por Docker para las carpetas de datos (`nextcloud_data`) y base de datos (`nextcloud_db`).

### Justificación Técnica:
* **Gestión de Backups:** Facilita la realización de copias de seguridad incrementales directamente desde el sistema de archivos del host sin necesidad de herramientas de exportación.
* **Transparencia de Datos:** Permite el acceso directo a los archivos para tareas de mantenimiento y auditoría desde la terminal del servidor Celeron.
* **Control de Propiedad:** Garantiza que los permisos de los archivos se mantengan consistentes con el usuario del sistema operativo, evitando errores de acceso denegado comunes en volúmenes virtuales.

## Automatización mediante Ansible

El despliegue completo se orquestó a través del playbook `05_deploy_nextcloud.yml`, el cual automatiza el ciclo de vida del servicio.

### Fases del Playbook:
1. **Aprovisionamiento de Directorios:** Creación de la jerarquía de carpetas en el host con permisos restrictivos (0755) para asegurar la persistencia antes de iniciar los contenedores.
2. **Despliegue de Configuración:** Transferencia del archivo `nginx.conf` optimizado para Nextcloud-FPM desde el nodo de control (WSL) hacia el servidor remoto.
3. **Gestión de Redes y Seguridad:** Apertura programática del puerto 8080 en el firewall UFW, limitando la exposición del servicio a los parámetros definidos.
4. **Orquestación con Docker Compose:** Ejecución del stack completo utilizando la colección `community.docker`.

## Optimizaciones de Software Especializadas

El stack ha sido diseñado para soportar un flujo de trabajo centrado en la fotografía mediante dos herramientas críticas:

* **Memories:** Aplicación de alto rendimiento que sustituye a la galería convencional, optimizada para la carga rápida de metadatos y navegación fluida.
* **Preview Generator:** Configuración de tareas programadas para la generación de miniaturas en segundo plano, evitando picos de consumo de CPU cuando el usuario accede a la interfaz desde dispositivos móviles.
