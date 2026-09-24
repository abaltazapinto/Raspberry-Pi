# Raspberry Pi 5 — Remote Access Context

## Current Situation

* Raspberry Pi 5 located in Maia
* André currently in Bragança, when he talks about LeNovoDesktop
* Raspberry is running:

  * SSH
  * Tailscale
  * Nextcloud
* Goal:

  * remotely access files/services
  * upload photos remotely
  * use Raspberry as personal cloud server

---

## Current Architecture

Bragança Lenovo/Desktop
↓
Internet
↓
Tailscale VPN
↓
Raspberry Pi 5 (Maia)
↓
Nextcloud / SSH / Docker

---

## Services Running / Planned

* SSH remote terminal
* Nextcloud self-hosted cloud
* Docker containers
* Remote file upload
* Tailscale mesh VPN

---

## Important Concepts

### Remote Access

* SSH
* VPN
* Tailscale
* NAT traversal

### Storage

* SSD
* ext4
* backups
* cloud-first workflow

### Networking

* static IP
* DNS
* tailscale IP
* reverse proxy (future)

### Embedded Linux Learning

* systemd
* journald
* networking stack
* services
* observability
* remote debugging

---

## Important Commands

### SSH

```bash
ssh andre@100.x.x.x
```

### Temperature

```bash
vcgencmd measure_temp
```

### Throttling

```bash
vcgencmd get_throttled
```

### Docker containers

```bash
docker ps
```

### Services

```bash
systemctl status
```

### Logs

```bash
journalctl -xe
```

---

## Next Steps

* validate remote Nextcloud access
* configure secure uploads
* create backup strategy
* automate monitoring
* study service persistence
* study remote observability


# Raspberry Pi 5 — Remote Access Data

## Tailscale Devices

### Windows Desktop — Bragança
- Hostname: desktop-net1nbd
- Tailscale IP: 100.113.176.35
- OS: Windows

### Android Phone
- Hostname: a56-de-andre
- Tailscale IP: 100.104.253.113
- OS: Android

### Raspberry Pi 5 — Maia
- Hostname: raspberrypi
- Tailscale IP: 100.70.175.106
- OS: Linux

---

## SSH to Raspberry Pi

From Windows PowerShell:

```powershell
ssh abaltaza@100.70.175.106
