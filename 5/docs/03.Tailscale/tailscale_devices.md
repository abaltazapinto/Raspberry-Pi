# Tailscale — Dispositivos

**Snapshot:** 2026-09-16

| Tailscale IP | Hostname | Sistema / Local | Estado atual | Notas |
|---|---|---|---|---|
| `100.92.44.113` | `baltazar` | Debian — Porto | online / local | PC Debian usado no Porto |
| `100.114.244.36` | `a56-de-andre` | Android | offline | last seen ~1 dia |
| `100.121.73.87` | `abaltaza-thinkstation-p520-1` | ThinkStation P520 — Linux 6.8 | offline | PC estava ligado, mas hoje não foi possível ligar remotamente; last seen ~11 h |
| `100.75.251.16` | `abaltaza-thinkstation-p520` | ThinkStation P520 — entrada antiga | offline | last seen ~15 dias; manter como entrada histórica até confirmar |
| `100.116.30.48` | `raspberrypi` | Raspberry Pi 5 | active | ligação direta observada: `10.42.0.54:41641` |

## Dispositivo ainda em falta

### Ubuntu — Porto

O Ubuntu do Porto **não aparece** no `tailscale status` deste snapshot.

Estado:

```text
Ubuntu Porto
Tailscale IP: por confirmar
Hostname: por confirmar
Estado: não listado no snapshot de 2026-09-16
```

## Comando de referência

```bash
tailscale status
```
