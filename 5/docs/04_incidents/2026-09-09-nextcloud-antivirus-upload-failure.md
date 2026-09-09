# Nextcloud Upload Failure — Antivirus Misconfiguration

Date: 2026-09-09

## Symptom

Nextcloud stopped receiving new files.

Observed effects:

- Android auto-upload stuck in "waiting to upload"
- Desktop files not updating
- Manual browser upload failed
- Old files were still visible
- Tailscale connectivity appeared suspicious initially

## Architecture

Client
→ Tailscale
→ Raspberry Pi
→ Nextcloud container
→ Antivirus scan
→ Storage

## Investigation

Tailscale peer was reachable:

```bash
tailscale status

Raspberry Pi Nextcloud HTTP endpoint responded:

curl -v http://100.116.30.48:8080/status.php

Result:

HTTP 200
installed: true
maintenance: false
Nextcloud 31.0.14

Manual upload through the browser produced:

No connection to anti virus.
Upload cannot be completed.

Docker inspection:

sudo docker ps -a

Only the Nextcloud container was running:

nc_app   Up   nextcloud:31-apache

Nextcloud antivirus configuration:

sudo docker exec -u www-data nc_app \
  php occ config:list files_antivirus

Result:

av_path = /usr/bin/clamscan
enabled = yes

Checked the binary:

sudo docker exec nc_app \
  sh -lc 'command -v clamscan; ls -l /usr/bin/clamscan'

Result:

/usr/bin/clamscan: No such file or directory
Root Cause

files_antivirus was enabled in Nextcloud and configured to execute:

/usr/bin/clamscan

but clamscan did not exist inside the Nextcloud container.

The antivirus integration therefore failed closed and blocked uploads.

Recovery

Temporarily disabled the antivirus app:

sudo docker exec -u www-data nc_app \
  php occ app:disable files_antivirus

Result:

files_antivirus 6.0.7 disabled

Manual browser upload immediately worked again.

Root Cause Summary
Nextcloud                OK
Tailscale                OK
Raspberry Pi             OK
Apache/PHP               OK
Storage path             OK

files_antivirus          ENABLED
/usr/bin/clamscan        MISSING
                         ↓
                  ALL UPLOADS BLOCKED
Engineering Lesson

Do not assume:

peer reachable == service healthy
service healthy == full application pipeline healthy

Test progressively:

Network
→ Port
→ HTTP service
→ Application
→ Real write/upload operation
→ Dependencies

A successful /status.php only proves that Nextcloud responds.
A real upload tests the complete application path.

Permanent Fix

Do not install ClamAV manually inside a running container.

Preferred architecture:

Nextcloud container
        ↓ TCP
ClamAV / clamd container

Configure both services persistently through Docker Compose.


### Porque isto é bom para o projeto

Este incidente mostra muito mais do que “tenho Nextcloud em Raspberry Pi”. Mostra **troubleshooting real de Linux, Docker, networking e service dependencies**.

Para o teu Raspberry Pi Home Lab, isto pode depois aparecer como:

> **Incident: Nextcloud uploads blocked by unavailable antivirus dependency — diagnosed from network layer through application dependency and recovered using `occ`.**

📓 A frase que eu guardaria mesmo no caderno:

```text
HTTP 200 proves liveness.
A real transaction proves functionality.
