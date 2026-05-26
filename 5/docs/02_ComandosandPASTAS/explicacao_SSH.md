## Ação — 1 passo

**Corrige o modelo mental: com Tailscale, o teu telemóvel e o Raspberry ficam numa “rede virtual privada” própria, diferente da tua LAN física de casa.**

Ou seja, quando fazes SSH por Tailscale, normalmente **não estás a chegar ao Raspberry pelo IP local `192.168.x.x`**, mas sim pelo **IP Tailscale**, geralmente algo como:

```text
100.x.y.z
```

ou por nome MagicDNS, por exemplo:

```bash
ssh abaltaza@raspberrypi
```

---

## Objetivo

Clarificar as camadas no teu caso real:

```text
ConnectBot / SSH
↓
TCP porta 22
↓
IP virtual da Tailscale
↓
Internet
↓
WireGuard/Tailscale
↓
Raspberry Pi
```

Portanto, o SSH continua a ser **TCP**, mas o caminho IP não é simplesmente:

```text
telemóvel → router de casa → Raspberry
```

É mais parecido com:

```text
telemóvel
→ Internet móvel/Wi-Fi
→ túnel Tailscale
→ Raspberry em casa
```

---

## Como pensar

Tens **duas redes diferentes** envolvidas:

| Rede        |  Exemplo de IP | O que significa                                        |
| ----------- | -------------: | ------------------------------------------------------ |
| LAN de casa | `192.168.1.20` | IP privado dentro da tua rede doméstica                |
| Tailscale   |    `100.x.y.z` | IP virtual acessível pelos teus dispositivos Tailscale |

Se estás fora de casa, o teu telemóvel **não consegue ver diretamente** `192.168.1.20`, porque esse IP é privado e só existe dentro da tua LAN.

Com Tailscale ativo, o telemóvel passa a ter uma interface virtual. O Raspberry também. Então o SSH usa essa rede virtual.

### Exemplo provável

```bash
ssh abaltaza@100.x.y.z
```

ou:

```bash
ssh abaltaza@raspberrypi
```

não necessariamente:

```bash
ssh abaltaza@192.168.1.20
```

---

## Diagnóstico por camadas

| Teste                            | O que prova                             |
| -------------------------------- | --------------------------------------- |
| Tailscale ligado no telemóvel    | Tens entrada na rede virtual            |
| Tailscale ligado no Raspberry    | O destino existe na tailnet             |
| Internet no telemóvel            | Tens transporte até à Tailscale         |
| Internet no Raspberry/desktop    | O nó consegue manter o túnel            |
| `ping 100.x.y.z`                 | IP virtual funciona                     |
| `ssh abaltaza@100.x.y.z`         | TCP/22 + serviço SSH funcionam          |
| `ping 192.168.1.20` fora de casa | Normalmente falha, salvo subnet routing |

---

## O ponto importante ⚡

Quando dizes:

> “só funciona o SSH se tiver tudo conectado”

Correto.

Porque precisas de três coisas:

```text
1. Cliente com Internet
2. Raspberry online e com Tailscale ativo
3. Serviço SSH ativo no Raspberry
```

Se qualquer uma falhar:

```text
Sem Internet → não chega ao túnel
Sem Tailscale → não há rede virtual
Sem sshd → há IP, mas não há serviço SSH
```

---

## Pitfalls & troubleshooting

1. **Confundir IP local com IP Tailscale**
   `192.168.1.20` só funciona na LAN, salvo configuração especial.

2. **Ping funcionar mas SSH falhar**
   IP está OK, mas pode haver problema em `sshd`, firewall ou porta 22.

3. **SSH funcionar em casa mas não fora**
   Provavelmente estás a usar IP local em vez do IP Tailscale.

4. **Desktop precisa de Internet**
   Sim: se o desktop participa na tailnet, precisa de Internet para manter o túnel Tailscale.

---

## Alternativas / tradeoffs

| Abordagem                      | Vantagem                           | Desvantagem                       |
| ------------------------------ | ---------------------------------- | --------------------------------- |
| SSH por IP local `192.168.x.x` | Simples dentro de casa             | Não funciona fora da LAN          |
| SSH por Tailscale `100.x.x.x`  | Seguro, sem abrir portas no router | Requer Tailscale ativo e Internet |
| Port forwarding no router      | Acesso direto externo              | Mais exposição e risco            |
| Tailscale subnet router        | Permite aceder à LAN toda          | Configuração mais avançada        |

---

## Pergunta de decisão

Quando te ligas pelo ConnectBot, estás a usar no campo host um IP do tipo **`192.168.x.x`** ou um IP do tipo **`100.x.x.x`** da Tailscale?
