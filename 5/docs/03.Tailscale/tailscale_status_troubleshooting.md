# Tailscale — Estado e Troubleshooting

**Última atualização:** 2026-09-16

## Estado conhecido da Tailnet

O snapshot atual foi obtido com:

```bash
tailscale status
```

Resultado relevante:

```text
100.92.44.113   baltazar                      linux    -
100.114.244.36  a56-de-andre                  android  offline, last seen 1d ago
100.121.73.87   abaltaza-thinkstation-p520-1  linux    offline, last seen 11h ago
100.75.251.16   abaltaza-thinkstation-p520    linux    offline, last seen 15d ago
100.116.30.48   raspberrypi                   linux    active; direct 10.42.0.54:41641
```

## Identificação correta

### `100.92.44.113` — `baltazar`

Este é o:

```text
Debian
Local: Porto
```

Não confundir com o Ubuntu do Porto.

### Ubuntu — Porto

O Ubuntu **não aparece neste snapshot**.

Por isso ainda falta confirmar:

```text
hostname
Tailscale IP
estado do tailscaled
```

Quando estiveres nessa máquina:

```bash
hostname
tailscale ip -4
tailscale status
```

---

## ThinkStation P520 — Linux 6.8

Entrada atual:

```text
100.121.73.87
abaltaza-thinkstation-p520-1
Linux 6.8
```

Estado observado:

```text
offline, last seen 11h ago
```

Hoje o PC estava fisicamente ligado, mas não foi possível estabelecer ligação remota.

Isto é importante porque:

```text
PC ligado != Tailscale online
```

Possíveis causas:

```text
Internet indisponível
tailscaled parado
máquina sem rota válida
DNS/rede com problema
suspensão parcial
interface de rede caída
Tailscale sem autenticação válida
```

### Diagnóstico quando voltares a ter acesso físico

Executar primeiro:

```bash
tailscale status
```

Depois:

```bash
systemctl status tailscaled
```

E:

```bash
ip -4 addr
ip route
ping -c 3 1.1.1.1
```

Se a Internet funcionar mas o Tailscale não:

```bash
sudo systemctl restart tailscaled
tailscale status
```

---

## Entrada antiga do ThinkStation

Existe ainda:

```text
100.75.251.16
abaltaza-thinkstation-p520
offline, last seen 15d ago
```

Por enquanto deve ser tratada como:

```text
entrada histórica / identidade anterior
```

Não usar este IP automaticamente para acesso remoto.

A entrada mais recente conhecida é:

```text
100.121.73.87
```

---

## Raspberry Pi 5

```text
100.116.30.48
raspberrypi
active
direct 10.42.0.54:41641
```

Testar:

```bash
tailscale ping 100.116.30.48
```

SSH:

```bash
ssh abaltaza@100.116.30.48
```

---

## Warning de DNS

O Tailscale mostrou:

```text
System DNS config not ideal. /etc/resolv.conf overwritten.
```

Não alterar configuração às cegas.

Diagnóstico:

```bash
ls -l /etc/resolv.conf
cat /etc/resolv.conf
resolvectl status
```

---

## Regra de troubleshooting

Quando um acesso remoto falha:

```text
1. tailscale status
2. peer online?
3. tailscale ping <IP>
4. só depois testar SSH / RDP / Nextcloud
```

Exemplo:

```bash
tailscale ping 100.121.73.87
```

Se falhar e o peer estiver `offline`, o problema está antes do RDP/SSH.

---

## Comandos principais

```bash
tailscale status
tailscale ip -4
tailscale ping <IP>
tailscale netcheck
systemctl status tailscaled
sudo systemctl restart tailscaled
```
