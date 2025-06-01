In use:  [MQTTRepo](https://github.com/lurgh/infinitive) \
[ACDrepo](https://github.com/acd/infinitive) - [UpdatedRepo HA Intergration](https://github.com/gogades/hass-infinitive/tree/master) - [OriginalRepo](https://github.com/mww012/ha_customcomponents) - [Will1604Repo](https://github.com/Will1604/infinitive) - [Installing Go](https://www.e-tinkers.com/2019/06/better-way-to-install-golang-go-on-raspberry-pi/) - [HAThread](https://community.home-assistant.io/t/carrier-bryant-infinitive-integration/119578/22) - [get 485 hat to work](https://forum.openmarine.net/showthread.php?tid=4534) \
4/22/25 - install rpi4 on westFH its at [10.10.15.99:8080](10.10.15.99:8080) also localDNS set to [hvactest.church](hvactest.church:8080)  
4/23/25 - Runs great until terminal is closed. Closing terminal causes [HUD](https://forums.raspberrypi.com/viewtopic.php?t=34073)
[Trying this](https://www.dexterindustries.com/howto/run-a-program-on-your-raspberry-pi-at-startup/)   \
[Schedular Card Repo](https://github.com/nielsfaber/scheduler-card)  \
MQTT listener:
```
mosquitto_sub -h 192.168.15.15 -u MQTT.user -P 5678.1234. -t "#" -v
```
## Fresh SD to running infinitive:
### Update: git first?
```
sudo apt install git
```
``` 
sudo apt update
```
``` 
sudo apt upgrade
```
TimeZone set:
```
sudo raspi-config
```
### Enable RS485 Hat:
```
sudo nano /boot/firmware/config.txt
```
```
# Enable UART
enable_uart=1
```
```
sudo reboot
```
should have ttyS0 in /dev now 
### Install Go:
```
nano go_installer.sh
```

```
export GOLANG="$(curl -s https://go.dev/dl/ | awk -F[\>\<] '/linux-arm64/ && !/beta/ {print $5;exit}')"
wget https://golang.org/dl/$GOLANG
sudo tar -C /usr/local -xzf $GOLANG
rm $GOLANG
unset GOLANG
```
```
sudo chmod +x go_installer.sh
```
```
./go_installer.sh
```
```
nano ~/.profile
```
```
PATH=$PATH:/usr/local/go/bin
GOPATH=$HOME/golang
```
```
source ~/.profile
```
```
go version
```
### Install Infinitive:
```
sudo mkdir src && cd src
```
Original:
```
sudo git clone https://github.com/lurgh/infinitive.git
```
MQQT remove uneeded topics:
```
sudo git clone -b mqtt-remove https://github.com/lsfarm/infinitive.git
```
```
cd infinitive
```
```
go build -o ~/infinitive
```
how to fix VCS?
```
go build -buildvcs=false -o ~/infinitive
```
cd home and make sure it runs: !!careful launching here will dump 38 entities into HA that aren't named right!!
```
cd && ./infinitive -httpport=8080 -serial=/dev/ttyUSB0
```
## Code for boot file to run as daemon:  !!Name Instance!!
```
sudo nano /lib/systemd/system/infinitive.service
```
```
[Unit]
Description=Infinitive Service
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
RestartSec=1
User=root
Environment="MQTTPASS=5678.1234."
ExecStart=/home/pi/infinitive -httpport=8080 -serial=/dev/ttyS0 -instance=Hall_East -mqtt=tcp://MQTT.user@192.168.15.15:1883

[Install]
WantedBy=multi-user.target
```
```
sudo chmod 644 /lib/systemd/system/infinitive.service
```
```
sudo systemctl enable infinitive
```
```
sudo systemctl start infinitive
```
```
sudo systemctl stop infinitive
```
```
systemctl status infinitive
```
