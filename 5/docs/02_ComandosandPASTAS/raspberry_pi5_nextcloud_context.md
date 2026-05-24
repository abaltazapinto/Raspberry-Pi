# Raspberry Pi 5 — Embedded Linux + Nextcloud Context

IP: 10.42.0.53
User: abaltaza
Host: raspberrypi

# Activar

	ssh abaltaza@10.42.0.53	

---

# Se IP mudar 

	ssh abaltaza@raspberrypi.local

---

# IP do Raspberry Pi que aparece neste projecto e :
	10.42.0.53

---

Pasta a guardar fichieros no Desktop em Windows e:

Remote synchronization of 102GB Google Photos archive from Bragança desktop to Raspberry Pi 5 Nextcloud server in Maia through Tailscale VPN infrastructure.

Utilizador > Nextcloud2

# Para aceder remotamente1

ssh abaltaza@100.70.175.106


Próximo passo

Depois do Linux instalado, instalamos Tailscale no Linux e voltas a aceder ao Raspberry pela mesma rota:

100.70.175.106:8080

## Hardware
- Raspberry Pi 5
- Purpose: self-hosted Nextcloud server
- Focus:
  - Linux systems administration
  - networking
  - storage
  - services
  - security
  - embedded Linux mindset

---

## Main Learning Goals
- Understand Linux boot process
- Learn systemd deeply
- Understand networking stack
- Practice troubleshooting
- Learn service orchestration
- Understand filesystem hierarchy
- Develop observability/debugging habits
- Practice backup/recovery strategy

---
## Important Linux Areas
### Boot Process
- Boot ROM
- GPU firmware
- bootloader
- Linux kernel
- device tree
- init/systemd
- userspace
---
### Networking
- DHCP vs static IP
- DNS
- reverse proxy
- HTTPS/TLS
- firewall
- SSH hardening
---
### Storage
- ext4
- mounts
- fstab
- external SSD
- backup strategy
---
### Services
- systemctl
- journald
- service dependencies
- startup sequence
---
### Observability
- journalctl
- dmesg
- top/htop
- iostat
- vmstat
- ss/netstat
---
## Nextcloud Stack
Possible stack:
- Nextcloud
- PostgreSQL/MariaDB
- Nginx
- PHP-FPM
- Redis
---
Possible deployment:
- native host
- Docker containers
---
## Important Engineering Questions
- What happens during boot?
- How are services started?
- What fails if networking is unavailable?
- How is data recovered after power loss?
- How are logs inspected?
- How are services monitored?
- What are bottlenecks?
- How is storage integrity maintained?

---

## Future Expansion
- reverse proxy
- VPN access
- monitoring stack
- container orchestration
- automation scripts
- backup automation
- embedded observability

---

## Key Embedded Linux Topics
- Linux kernel
- device tree
- systemd
- filesystems
- process scheduling
- IPC
- networking
- security
- automation

