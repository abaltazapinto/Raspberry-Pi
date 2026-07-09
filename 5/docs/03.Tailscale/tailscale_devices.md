# Tailscale — Inventário de IPs

> Registo rápido dos devices no tailnet para consulta no Git.

## Estado atual

Data observada: `2026-07-09`

| Tailscale IP | Hostname / Device | Tipo | Estado observado | Notas |
|---|---|---|---|---|
| `100.70.175.106` | `raspberrypi` | Linux / Raspberry Pi | ativo | Servidor Pi-hole / Nextcloud / SSH |
| `100.104.253.113` | `a56-de-andre` | Android | offline, visto há ~9h | Telemóvel Samsung A56 |
| `100.99.35.94` | `abalta...thinkstation-p520` | Linux | offline, visto há ~12d | Desktop / ThinkStation |
| `100.123.108.17` | `andre-hp-zbook-fury-15-6-inch-g8-mobile-workstation-pc` | Linux | ativo | Portátil HP ZBook — apareceu no Query Log do Pi-hole |
| `100.113.176.35` | `desktop-net1nbd` | Windows | offline, visto há ~34d | Desktop Windows |

## Comandos úteis

### Ver devices Tailscale

```bash
tailscale status
```

### Ver IP Tailscale do device atual

```bash
tailscale ip -4
```

### Testar conectividade para o Raspberry Pi

```bash
ping 100.70.175.106
```

### Entrar por SSH no Raspberry Pi

```bash
ssh abaltazar@raspberrypi.local
```

ou, via Tailscale IP:

```bash
ssh abaltazar@100.70.175.106
```

## Interpretação Pi-hole

Quando no Pi-hole aparecer um client com IP `100.x.x.x`, isso normalmente é tráfego vindo da rede Tailscale.

Exemplo observado:

```text
100.123.108.17 → andre-hp-zbook-fury-15-6-inch-g8-mobile-workstation-pc
```

Logo, o bloqueio visto no Query Log do Pi-hole veio do portátil HP ZBook, não do telemóvel.

## Nota de segurança

Estes IPs são privados dentro do tailnet Tailscale, mas continuam a identificar os teus devices. Evitar publicar este ficheiro em repositórios públicos.

## Próximos passos possíveis

- Confirmar se o Android aparece como `active` no `tailscale status`.
- Configurar o Pi-hole como DNS do tailnet.
- Criar aliases SSH para os hosts principais.
- Documentar serviços por device: SSH, Nextcloud, Pi-hole, Docker.
