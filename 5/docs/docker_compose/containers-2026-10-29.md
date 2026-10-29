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
| `node-red` | Node-RED 4.1.8 + FlowFuse Dashboard 2 1.34.1 | `0.0.0.0:1880->1880/tcp` | Up (healthy) após instalação |

**Atualização 29-10-2026:** Node-RED instalado posteriormente ao inventário inicial, com porta 1880 publicada. Os estados dos restantes serviços são os do levantamento inicial e não foram novamente verificados.

## Node-RED — instalação realizada

- Docker Compose independente em `~/docker/node-red`, imagem `nodered/node-red:4.1.8`, `restart: unless-stopped`, `TZ=Europe/Lisbon`, porta `1880:1880`.
- Volume Docker nomeado `node_red_data:/data` para persistência.
- FlowFuse Dashboard 2 (`@flowfuse/node-red-dashboard` 1.34.1) instalado.
- Flow académico COMCS PL6 importado, com MQTT sobre TLS (EMQX Cloud, porta 8883), subscrição `comcs/pl6/#` e gauges de temperatura e humidade.
- Dashboard acessível via Tailscale em `http://100.116.30.48:1880/dashboard/page1`; captura mostra 22,6 °C e 47% de humidade. Estes valores não comprovam, por si, receção em tempo real no Raspberry Pi.
- Credenciais MQTT introduzidas na configuração privada do Node-RED, não no repositório.

### Próximas verificações

- Confirmar estado `connected` do nó MQTT e receção de mensagens novas no Debug.
- Ativar autenticação `adminAuth` no editor Node-RED e restringir o acesso à porta 1880; atualmente o serviço usa HTTP sem autenticação confirmada. Não configurar port forwarding público.
- Verificar estado dos restantes containers após instalação.
- Considerar armazenamento histórico e deteção de sensores offline.


## Comandos úteis

```bash
sudo docker ps --format "table {{.Names}}\t{{.Ports}}\t{{.Status}}"
sudo docker compose ls
sudo ss -ltnp | grep ':1880' || true
```

Não guardar passwords, tokens ou outros segredos neste repositório.
