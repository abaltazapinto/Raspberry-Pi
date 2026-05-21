# Ação — instalar Tailscale no Raspberry Pi

No SSH do Raspberry, corre:

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

Depois liga o Raspberry à tua conta Tailscale:

```bash
sudo tailscale up
```

Vai aparecer um link. Abre esse link no browser, faz login, e autoriza o Raspberry. O método oficial suporta Raspberry Pi OS e depois usa `sudo tailscale up` para autenticar. ([Tailscale][1])

## Objetivo

Dar ao Raspberry um IP privado Tailscale, tipo:

```text
100.x.y.z
```

Depois, fora de casa, vais usar:

```bash
ssh abaltaza@100.x.y.z
```


sudo tailscale status

# Confirmar o IP tailscale

	tailscale ip -4

# IP do raspberry

	100.70.175.106

