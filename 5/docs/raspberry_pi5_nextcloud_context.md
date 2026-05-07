# Raspberry Pi 5 — Embedded Linux + Nextcloud Context

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

### Networking
- DHCP vs static IP
- DNS
- reverse proxy
- HTTPS/TLS
- firewall
- SSH hardening

### Storage
- ext4
- mounts
- fstab
- external SSD
- backup strategy

### Services
- systemctl
- journald
- service dependencies
- startup sequence

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

