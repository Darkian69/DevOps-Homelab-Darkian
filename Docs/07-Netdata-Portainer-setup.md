##Capa de Observabilidad y Gestion

Para garantizar la estabilidad del servidor Intel Celeron, se han implementado herramientas de monitoreo en tiempo real y administracion visual de contenedores.

### Netdata: Monitoreo de Rendimiento
Netdata se utiliza para la supervision exhaustiva del hardware y el comportamiento de los microservicios.
* **Justificacion**: Su arquitectura en C permite obtener metricas segundo a segundo con un impacto minimo en la CPU. Es vital para monitorear la temperatura y el uso de RAM durante las tareas de Machine Learning de Immich.
* **Funcionalidad**: Proporciona graficas de latencia de disco (I/O), uso de red y salud de los contenedores Docker, permitiendo identificar cuellos de botella de forma inmediata.

### Portainer: Administracion de Contenedores
Portainer CE actua como la interfaz de gestion centralizada para todo el stack de Docker.
* **Justificacion**: Facilita la inspeccion de logs, la gestion de volumenes y el reinicio de servicios sin necesidad de interactuar directamente con la CLI, reduciendo el tiempo de mantenimiento.
* **Funcionalidad**: Permite visualizar el estado de salud de Nextcloud e Immich, actualizar imagenes de forma sencilla y monitorizar el consumo de recursos especifico por cada contenedor.

### Resumen de Implementacion en Ansible
La automatizacion realizo el aprovisionamiento de volumenes persistentes para ambas herramientas y la configuracion de las reglas de firewall (UFW) correspondientes:
* **Puerto 19999**: Acceso al panel de control de Netdata.
* **Puerto 9443**: Acceso seguro (HTTPS) al panel de Portainer.

