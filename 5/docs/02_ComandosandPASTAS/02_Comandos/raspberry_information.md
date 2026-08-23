# Raspberry Pi 5 — Estado Atual e Plano de Evolução

## 1. Identificação do Sistema

Este documento descreve o estado atual do meu Raspberry Pi 5 e o que já tenho funcional.

O Raspberry Pi 5 está a ser usado como um pequeno servidor pessoal, com foco em:

- Nextcloud
- SSH remoto
- Tailscale VPN
- Docker
- monitorização básica
- aprendizagem de Embedded Linux
- administração Linux
- redes
- armazenamento
- serviços persistentes

---

## 2. Hardware

### Board

- Raspberry Pi 5
- CPU: ARM 64-bit
- RAM observada: cerca de 8 GB
- Sistema operativo: Linux / Raspberry Pi OS
- Uso principal: servidor pessoal e laboratório Embedded Linux

### Armazenamento

- Sistema com armazenamento montado em `/`
- Nextcloud a usar armazenamento persistente através de Docker volumes / diretórios locais
- Existe preocupação com espaço livre e integridade dos dados

### Temperatura atual observada

```bash
vcgencmd measure_temp


## Ação — 1 passo

No ficheiro onde estás a guardar os comandos, cola isto:

````md
# Raspberry Pi 5 — Comandos de Acesso

## Dados atuais

```text
IP atual: 10.42.0.53
User: abaltaza
Host: raspberrypi
````

---

## Entrar por SSH usando IP

```bash
ssh abaltaza@10.42.0.53
```

---

## Entrar por SSH usando hostname local

Usar quando o IP mudar:

```bash
ssh abaltaza@raspberrypi.local
```

---

## Ver IPs do Raspberry Pi

Dentro do Raspberry Pi:

```bash
ip a
```

ou mais simples:

```bash
hostname -I
```

---

## Ver IP da Tailscale

```bash
tailscale ip -4
```

---

## Ver estado da Tailscale

```bash
tailscale status
```

---

## Testar se o Raspberry responde na rede local

A partir do portátil ou desktop:

```bash
ping raspberrypi.local
```

ou:

```bash
ping 10.42.0.53
```

---

## Testar porta SSH

```bash
nc -vz 10.42.0.53 22
```

ou, se o IP mudar:

```bash
nc -vz raspberrypi.local 22
```

---

## Nota importante

O IP `10.42.0.53` pode mudar se o router/DHCP mudar.

Por isso, o comando mais robusto na rede local é:

```bash
ssh abaltaza@raspberrypi.local
```

Para acesso fora de casa, usar o IP da Tailscale:

```bash
ssh abaltaza@<IP_TAILSCALE>
```

```

## Objetivo

Ficas com os comandos certos para entrar no Raspberry Pi mesmo que o IP local mude.

## Pitfalls & troubleshooting

- `10.42.0.53` só funciona na mesma rede local.
- `raspberrypi.local` depende de mDNS/Avahi funcionar.
- Fora de casa, usa Tailscale, não o IP `10.42.0.53`.
- Se `ssh` falhar, testa primeiro com `ping`.

## Pergunta de decisão

Queres guardar também uma secção só com **comandos Nextcloud/Docker**?
```
