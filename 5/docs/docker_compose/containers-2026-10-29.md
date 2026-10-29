# Raspberry Pi 5 — inventário de containers Docker

**Data da verificação:** 29-10-2026  
**Host:** `raspberrypi`  
**Comando utilizado:**

```bash
sudo docker ps --format "table {{.Names}}\t{{.Ports}}\t{{.Status}}"
```

## Estado observado

| Container | Serviço | Portas apresentadas por Docker | Estado |
| --- | --- | --- | --- |
| `nc_clamav` | ClamAV | `3310/tcp, 7357/tcp` (não publicadas no host) | Up 7 weeks (healthy) |
| `navidrome` | Navidrome | `0.0.0.0:4533->4533/tcp`, `:::4533->4533/tcp` | Up 3 months |
| `nc_app` | Nextcloud | `0.0.0.0:8080->80/tcp`, `:::8080->80/tcp` | Up 3 months |
| `nc_db` | MariaDB | `3306/tcp` (interna) | Up 3 months |
| `nc_redis` | Redis | `6379/tcp` (interna) | Up 3 months |
| `node-red` | Node-RED | **1880/tcp planeada** | **Ainda não instalado** |

**Nota:** a porta 1880 não aparece publicada por nenhum dos containers listados. Confirmar também que não existe outro processo a escutar nessa porta antes da instalação (por exemplo, `sudo ss -ltnp | grep ':1880'`). `docker ps` mostra containers em execução; não prova que toda a porta esteja livre.

## Próximo trabalho — Node-RED

- Instalar Node-RED via Docker Compose **separado** dos serviços existentes.
- Usar porta `1880` se disponível, volume persistente e `restart: unless-stopped`.
- Importar flows MQTT do protótipo COMCS PL6, instalar FlowFuse Dashboard 2 e reconfigurar credenciais fora do Git.
- Consultar dashboard remotamente via Tailscale, sem expor o editor à Internet pública.
- Verificar que Nextcloud, Navidrome, ClamAV, MariaDB e Redis continuam ativos depois da instalação.

## Comandos úteis

```bash
sudo docker ps --format "table {{.Names}}\t{{.Ports}}\t{{.Status}}"
sudo docker compose ls
sudo ss -ltnp | grep ':1880' || true
```

Não guardar passwords, tokens ou outros segredos neste repositório.
