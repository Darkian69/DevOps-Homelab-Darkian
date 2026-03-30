# Despliegue de Servicios: Nextcloud & Immich

Este documento detalla la configuracion final y funcional para el despliegue de una infraestructura de nube privada (Nextcloud) y gestion de fotografias (Immich) sobre un servidor con recursos limitados (Intel Celeron, 4GB RAM).

## 1. Arquitectura del Sistema

El sistema se divide en dos stacks independientes gestionados mediante Docker Compose y automatizados con Ansible.

### Componentes Core:
- **Nextcloud Hub 9**: Gestion de archivos, documentos y backups de escritorio.
- **Immich**: Clon de Google Photos con reconocimiento facial y busquedas por Machine Learning.
- **Nginx**: Servidor web y proxy inverso para la entrega de activos estaticos para Nextcloud.
- **PostgreSQL / MariaDB**: Motores de base de datos optimizados - PostgreSQL para Immich y MariaDB para Nextcloud.
- **Redis**: Cache en memoria para mejorar el rendimiento de Immich.

---

## 2. Configuracion de Nextcloud (Solo Archivos)

Se optimizo Nextcloud para actuar exclusivamente como gestor de archivos, desactivando modulos pesados para liberar CPU.

### Optimizaciones Aplicadas:
- **Client Max Body Size**: Configurado a 10G en Nginx para permitir subidas de archivos pesados.
- **MIME Types**: Soporte explicito para modulos JS (.mjs) y estilos CSS para evitar errores de bloqueo en el navegador.
- **Database Indices**: Aplicacion de indices faltantes en MariaDB mediante `occ db:add-missing-indices`.
- **Background Jobs**: Configuracion de Cron del sistema para ejecutar tareas de mantenimiento cada 5 minutos.

---

## 3. Configuracion de Immich (Gestion de Fotos)

Immich utiliza una base de datos especializada para el reconocimiento facial.

### Especificaciones del Stack:
- **Database**: Imagen `tensorchord/pgvecto-rs:pg14-v0.2.0`. Es obligatoria para el soporte de la extension pgvector necesaria para los algoritmos de busqueda visual.
- **Machine Learning**: Contenedor dedicado para el procesamiento de caras y objetos.
- **Mapeo de Puertos**: Acceso via puerto 2283.


---

## 4. Automatización mediante Ansible

El despliegue completo se orquestó a través del playbook [`03_deploy_nextcloud.yml`](Ansible/03_deploy_nextcloud.yml) y [`04_deploy_immich.yml`](Ansible/04_deploy_immich.yml), el cual automatiza el ciclo de vida del servicio.

### Estructura del Playbook: `03_deploy_nextcloud.yml` 
1. **Aprovisionamiento de Directorios:** Creación de la jerarquía de carpetas en el host con permisos restrictivos (0755) para asegurar la persistencia antes de iniciar los contenedores.
2. **Despliegue de Configuración:** Transferencia del archivo `nginx.conf` optimizado para Nextcloud-FPM desde el nodo de control (WSL) hacia el servidor remoto.
3. **Gestión de Redes y Seguridad:** Apertura programática del puerto 8080 en el firewall UFW, limitando la exposición del servicio a los parámetros definidos.
4. **Orquestación con Docker Compose:** Ejecución del stack completo utilizando la colección `community.docker`.



### Estructura del Playbook: `04_deploy_immich.yml`
1. **Provisionamiento de Directorios**: Crea las carpetas `upload` (fotos), `postgres` (base de datos) y `model-cache` (almacenamiento de modelos de Machine Learning) con los permisos adecuados para el usuario root de Docker.
2. **Sincronizacion de Configuracion**: Copia el archivo `docker-compose.yml` y el archivo de variables de entorno `.env` desde el entorno de control (WSL) hacia el servidor host (Celeron).
3. **Configuracion de Red (UFW)**: Abre el puerto `2283/tcp` en el Firewall del sistema para permitir la comunicacion de la API y el acceso web desde dispositivos externos.
4. **Despliegue de Contenedores**: Utiliza el modulo `community.docker.docker_compose_v2` para levantar los servicios de Server, Machine Learning, Postgres y Redis.

