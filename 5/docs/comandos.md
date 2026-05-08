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

# Recuperar dpkg

	sudo dpkg --configure -a
	sudo apt update

# instalar docker 
	sudo apt install -y docker.io

