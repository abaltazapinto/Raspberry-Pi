````md
# Raspberry Pi 5 — SSH + Nextcloud + Remote Access Summary

## Hardware
- Raspberry Pi 5
- SSD 256 GB
- Boot direto pelo SSD
- Nextcloud server
- Docker containers

---

# Estado atual do sistema

## Confirmado funcional
- Linux operacional
- SSH operacional
- Docker operacional
- Nextcloud operacional
- nc_app ativo
- nc_db ativo
- nc_redis ativo

---

# Acesso SSH local

## Pelo IP local

```bash
ssh abaltaza@10.42.0.53
````

## Pelo hostname local (melhor)

```bash
ssh abaltaza@raspberrypi.local
```

---

# Identidade do Raspberry

```bash
hostname
whoami
uname -a
```

Resultado esperado:

```text
raspberrypi
abaltaza
Linux raspberrypi ...
```

---

# Docker / Nextcloud

## Ver containers ativos

```bash
sudo docker ps
```

Containers esperados:

```text
nc_app
nc_db
nc_redis
```

## Ver todos os containers

```bash
sudo docker ps -a
```

## Reiniciar containers

```bash
sudo docker restart nc_app nc_db nc_redis
```

---

# Temperatura / estado do Raspberry

## Temperatura

```bash
vcgencmd measure_temp
```

## Ver throttling

```bash
vcgencmd get_throttled
```

Valor ideal:

```text
throttled=0x0
```

---

# Informação de sistema

## Ver discos

```bash
lsblk
```

## Espaço em disco

```bash
df -h
```

## RAM

```bash
free -h
```

## CPU/load

```bash
uptime
```

---

# Shutdown seguro

## Desligar corretamente

```bash
sudo shutdown now
```

## Reboot seguro

```bash
sudo reboot
```

Nunca remover energia diretamente enquanto houver:

* Docker ativo
* escrita no SSD
* sincronização Nextcloud

---

# SSH pelo telemóvel

## App gratuita usada

```text
ConnectBot
```

## Ligação usada

```text
abaltaza@raspberrypi.local
```

ou

```text
abaltaza@10.42.0.53
```

---

# Diagnóstico importante aprendido

## Ecrã preto ≠ Raspberry morto

Mesmo com HDMI/ecrã preto:

* SSH podia funcionar
* Docker podia funcionar
* Nextcloud podia funcionar

SSH tornou-se o método principal de administração.

---

# Rede local vs fora de casa

## Rede local

Funciona:

* 10.42.0.53
* raspberrypi.local

## Fora de casa

NÃO funciona:

* 10.42.0.53
* raspberrypi.local

Porque são locais/private network.

---

# Solução para acesso remoto global

## Tailscale

Objetivo:

* acesso seguro ao Raspberry
* sem abrir portas no router
* SSH remoto
* Nextcloud remoto

Instalação:

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

Depois:

```bash
sudo tailscale up
```

Após login:

* Raspberry recebe IP Tailscale
* normalmente tipo 100.x.y.z

Exemplo futuro:

```bash
ssh abaltaza@100.x.y.z
```

---

# Arquitectura confirmada

```bash
dpkg --print-architecture
```

Resultado:

```text
arm64
```

---

# Conceitos Linux / Embedded aprendidos

## SSH

* acesso remoto seguro
* terminal remoto

## Docker

* containers isolados
* serviços persistentes

## systemd

* gestão de serviços Linux

## Networking

* IP local
* mDNS
* portas
* sockets
* SSH daemon

## Observability

* verificar serviços
* verificar portas
* troubleshooting remoto

---

# Mentalidade importante

```text
PC / Telemóvel
        ↓
      SSH
        ↓
 Raspberry Pi
        ↓
 Docker / Nextcloud / Linux
```

Se SSH funciona:

* o Raspberry está vivo
* administração remota é possível
* monitor local deixa de ser crítico

---

# Próximos passos possíveis

* terminar setup Tailscale
* acesso remoto fora de casa
* HTTPS/reverse proxy
* backups automáticos
* monitorização
* logs Docker
* observabilidade Linux
* VPN architecture
* automação

```
```

