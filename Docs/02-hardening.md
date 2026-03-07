# 02: Hardening del Sistema y Seguridad de Red

## Introducción al Hardening
El hardening es el conjunto de prácticas y configuraciones aplicadas a un sistema operativo para reducir su superficie de vulnerabilidad. El objetivo principal es eliminar riesgos potenciales mediante la desactivación de servicios innecesarios y la restricción de accesos, logrando que el sistema sea más resistente ante intentos de intrusión.

En este proyecto, el hardening es crítico debido a que el hardware se encuentra en una red doméstica y debe ser protegido antes de desplegar servicios públicos.

## Configuración Avanzada de SSH
Para fortalecer el acceso remoto, se modificó el servicio SSH para forzar el uso de criptografía de llave pública y mitigar ataques de fuerza bruta.

### Cambios realizados en /etc/ssh/sshd_config:
- **Desactivación de contraseñas:** Se cambió `PasswordAuthentication` a `no`. Esto garantiza que solo los dispositivos con una llave privada autorizada puedan establecer conexión.
- **Restricción de usuario Root:** Se configuró `PermitRootLogin` en `no`. Esta es una mejor práctica de la industria que obliga a los administradores a acceder mediante un usuario con privilegios limitados y escalar permisos mediante `sudo` solo cuando es necesario.

### Aplicación de cambios:
```bash
sudo sshd -t
sudo systemctl restart ssh
```

## Implementación de Firewall (UFW)

Se ha configurado Uncomplicated Firewall (UFW) como la primera línea de defensa a nivel de red para el host. La estrategia se basa en una política de "Deny by Default", donde todo el tráfico entrante es rechazado a menos que exista una regla explícita que lo permita.

### Configuración de políticas base
Se establecieron las reglas fundamentales para asegurar que el servidor no exponga servicios innecesarios:

```bash
# Establecer políticas por defecto
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Permitir acceso de gestión (SSH)
sudo ufw allow ssh

# Activar el firewall
sudo ufw enable
```
