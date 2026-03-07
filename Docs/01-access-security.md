#  01: Configuración de Acceso y Seguridad Inicial

El primer paso en cualquier infraestructura profesional es asegurar que el acceso sea restringido y cifrado. En esta fase, eliminamos el uso de contraseñas para la autenticación remota.

##  Generación de llaves SSH (Ed25519)
Se optó por el algoritmo **Ed25519** en lugar de RSA debido a su mayor seguridad y mejor rendimiento computacional.

**Comando ejecutado en la máquina local (Host):**
```bash
ssh-keygen -t ed25519 -C "tu-email@ejemplo.com"
```
## Transferencia de Llave Pública
Para autorizar la conexión, se copió la llave pública al servidor:
```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub usuario@ip-del-servidor
```
**Nota para usuarios de Windows:** > El comando `ssh-copy-id` no está disponible nativamente en PowerShell. Se utilizó el siguiente comando alternativo para transferir la llave:
 ```powershell
 type $env:USERPROFILE\.ssh\id_ed25519.pub | ssh usuario@ip "cat >> .ssh/authorized_keys"
 ```

##  Creación de Usuario de Gestión
Se evitó el uso directo de la cuenta root creando un usuario con privilegios escalables vía sudo:
 ```bash
sudo adduser nombredetuusuario
sudo usermod -aG sudo nombredetuusuario
```
