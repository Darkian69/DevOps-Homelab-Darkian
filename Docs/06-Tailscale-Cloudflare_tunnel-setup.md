Conectividad y Acceso Remoto
=============================================

1\. Acceso Externo mediante Cloudflare Zero Trust (Tunnels)
-----------------------------------------------------------

El uso de Cloudflare Tunnels permite exponer servicios locales a internet de forma segura sin necesidad de abrir puertos en el router (Port Forwarding).

### Justificacion y Utilidad

-   **Seguridad**: El trafico viaja a traves de un tunel cifrado de salida. El servidor permanece invisible para escaneos de puertos externos.

-   **Conectividad Transparente**: Permite que las aplicaciones moviles de Nextcloud e Immich funcionen de forma nativa mediante subdominios (ej. nube.tudominio.com).

-   **Gestion de Certificados**: Cloudflare gestiona el cifrado HTTPS, eliminando la complejidad de configurar SSL manualmente.

### Implementacion con Ansible

El playbook realiza las siguientes tareas:

-   Despliega el conector cloudflared mediante un contenedor ligero.

-   Vincula el servidor al panel de Cloudflare mediante un token de autenticacion.



* * * * *

2\. Red Privada Virtual con Tailscale (VPN Mesh)
------------------------------------------------

Tailscale se utiliza como una capa de administracion y redundancia para crear una red privada entre el servidor y los dispositivos autorizados.

### Justificacion y Utilidad

-   **Administracion Remota Segura**: Permite el acceso via SSH al servidor desde cualquier lugar como si estuviera en la misma red local, utilizando una IP privada unica.

-   **Puerta Trasera de Emergencia**: En caso de fallos en el tunel de Cloudflare, Tailscale proporciona acceso directo a la infraestructura interna.

-   **Simplicidad**: Utiliza el protocolo WireGuard para establecer conexiones seguras que atraviesan firewalls sin configuracion manual.

### Implementacion con Ansible

El playbook realiza las siguientes tareas:

-   Instala el binario oficial de Tailscale directamente en el sistema operativo.

-   Provisiona el servicio y activa la interfaz de red virtual.

-   Autentica el nodo en la red privada, permitiendo la resolucion de nombres mediante MagicDNS.
