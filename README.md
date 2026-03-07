

# DevOps HomeLab: De Celeron a Infraestructura Profesional

Este repositorio documenta el proceso de diseño, configuración y automatización de mi servidor doméstico (HomeLab), aplicando principios de **DevOps** y **Cloud Engineering**. 

El objetivo es transformar un hardware limitado (Celeron con 4GB RAM) en una infraestructura robusta, segura y escalable para desplegar servicios personales.

##  Arquitectura del Sistema
* **SO:** Ubuntu Server 24.04 LTS (Minimal)
* **Provisionamiento:** Ansible (IaC)
* **Contenedores:** Docker & Docker Compose
* **Red y VPN:** Tailscale & UFW
* **Observabilidad:** Netdata / Prometheus + Grafana
* **CI/CD:** GitHub Actions (Self-hosted runner)

##  Estructura del Proyecto
* `/docs`: Bitácoras detalladas paso a paso.
* `/ansible`: Playbooks para configuración automática.
* `/docker`: Archivos Compose para los servicios (Immich, Nextcloud, etc).
* `/scripts`: Automatización en Bash y Python.

## 🛠️ Bitácora de Implementación
1. [Configuración de Acceso Seguro (SSH)](Docs/01-access-security.md)
2. [Hardening de Linux y Firewall (UFW)](docs/02-hardening.md) - *Próximamente*
3. [Instalación y Optimización de Docker](docs/03-docker-setup.md) - *Próximamente*
