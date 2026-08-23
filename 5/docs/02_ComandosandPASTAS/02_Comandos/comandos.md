# ativar 

	ssh abaltaza@10.42.0.53
	ssh abaltaza@raspberrypi.local
---
#desktop ALDEIA


ssh abaltaza@100.70.175.106


#ativar telemovel
	abaltaza@raspberrypi.local
	

# comandos para saber identificar 

	uname -a
	hostname
	whoami

# Desativar screen blanking temporariamente (sessão atual)
	xset s off
	xset -dpms
	xset s noblank

# Reativar monitor sem reboot
	DISPLAY=:0 xset dpms force on
ctrl+ alt + F7 ou F2
 
# Alternar entre terminal e ambiente gráfico
	CTRL + ALT + F2   # terminal TTY
	CTRL + ALT + F1   # voltar GUI
	CTRL + ALT + F7   # alternativa GUI

# Persistente no Raspberry Pi OS (LXDE)
	sudo nano /etc/xdg/lxsession/LXDE-pi/autostart
- Adicionar
		
	@xset s off
	@xset -dpms
	@xset s noblank

# Desativar blank do console kernel
	sudo nano /boot/firmware/cmdline.txt

- Acionar ao fim da linha
	consoleblank = 0


# Reiniciar apenas ambiente gráfico
	sudo systemctl restart lightdm

# Verificar se Linux está vivo
	uname -a

# Verificar undervoltage/throttling (muito importante no Pi 5)
	vcgencmd get_throttled

# Fonte recomendada Raspberry Pi 5
	5V / 5A USB-C PD

# Saber a temperatura 

	watch -n 1 vcgencmd measure_temp
	watch -n 1 vcgencmd get_throttled
# Recuperar dpkg

	sudo dpkg --configure -a
	sudo apt update

# instalar docker 
	sudo apt install -y docker.io

# docker 
	docker compose config
	sudo docker compose up -d
	sudo compose down (depois de fazer alteracoes a docker-compose.yml)

# GERAIS

	cat > raspberry_ssh_nextcloud_cheatsheet.md <<'EOF'
# Raspberry Pi 5 — SSH + Nextcloud + Docker Cheatsheet

## Identidade do Raspberry

```bash
hostname
whoami
uname -a

# ver se raspberry esta vivo

	ping raspberrypi.local

ou

	ping 10.42.0.53

# ver container docker activos

	sudo docker ps
	sudo docker ps -a
# ver logs nextcloud app

	sudo docker logs nc_app --tail=50

# ver logs da base de dados

	sudo docker logs nc_db --tail=50

#ver logs do redis

	sudo docker logs nc_redis --tail=50

# Reiniciar containers Nextcloud

- Usar so se houver problemas

	sudo docker restart nc_app nc_db nc_redis

# ver temperatura 

	watch -n 1 vcgencmd measure_temp

# ver throttling / problemas energia | temperatura

	watch -n 1 vcgencmd get_throttled

# ver uso de disco
	
	df -h 

# ver discos ligados

	lsblk

# ver memoria e carga

	free -h
	uptime

# Ver processos ativos

	top

#para sir do top

	q
# ver servicos systemd importantes

	systemctl status ssh

	systemctl status docker

# shutfown limpo

	sudo shutdown now

# Reboot 

	sudo reboot
