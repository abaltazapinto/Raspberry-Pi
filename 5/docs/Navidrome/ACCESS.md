## Ação — 1 passo 📝

No **Raspberry Pi**, cria um ficheiro de documentação do serviço:

```bash
nano /srv/navidrome/ACCESS.md
```

E cola isto lá dentro:

````md
# PrivateSpotify — Navidrome Access

## Servidor

Machine: Raspberry Pi 5  
Host: raspberrypi  
User: abaltaza  
Music folder: /srv/music  
Navidrome config: /srv/navidrome  
Docker compose file: /srv/navidrome/docker-compose.yml  

## Acesso local / VPN

Navidrome Web:

http://raspberrypi:4533

Ou por IP:

http://100.70.175.106:4533

## Gestão do serviço

Entrar no Raspberry Pi:

```bash
ssh abaltaza@raspberrypi
````

Ver containers:

```bash
sudo docker ps
```

Arrancar Navidrome:

```bash
cd /srv/navidrome && sudo docker compose up -d
```

Parar Navidrome:

```bash
cd /srv/navidrome && sudo docker compose down
```

Ver logs:

```bash
cd /srv/navidrome && sudo docker compose logs -f
```

## Pastas

Músicas reais:

```bash
/srv/music
```

Configuração e base de dados:

```bash
/srv/navidrome/data
```

## Clientes

Desktop Samorinha:

[http://raspberrypi:4533](http://raspberrypi:4533)

Laptop ZFury:

[http://raspberrypi:4533](http://raspberrypi:4533)

Telemóvel:

Usar app compatível com Subsonic/Navidrome.

Exemplos:

* Android: Symfonium, Ultrasonic, Subtracks
* iOS: play:Sub, Amperfy

## Nota

Não expor diretamente à internet sem VPN, Tailscale ou HTTPS.

````

Para gravar no `nano`:

```text
CTRL + O
ENTER
CTRL + X
````

## Objetivo

Criar documentação mínima para não dependeres da memória quando fores aceder pelo desktop, laptop, telemóvel ou outro PC.

## Como pensar

Isto é engenharia de sistemas: o serviço não está “feito” quando arranca; está feito quando consegues:

```text
instalar
aceder
parar
reiniciar
debuggar
repetir
documentar
```

## Pitfalls

* O IP `100.70.175.106` pode mudar se for VPN/Tailscale ou rede diferente.
* `raspberrypi` pode não resolver em todos os PCs.
* Telemóvel fora de casa provavelmente precisa de VPN/Tailscale.
* Ainda falta criar o primeiro user/admin no Navidrome pelo browser.

## Pergunta de decisão

Queres que o próximo passo seja **abrir o Navidrome no browser e criar o utilizador admin**, ou **configurar primeiro acesso seguro por Tailscale/VPN**?
