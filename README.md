1. In use: [ACDrepo](https://github.com/acd/infinitive)
2. [UpdatedRepo HA Intergration](https://github.com/gogades/hass-infinitive/tree/master)
  [OriginalRepo](https://github.com/mww012/ha_customcomponents) [Will1604Repo](https://github.com/Will1604/infinitive)
  [MQTTRepo](https://github.com/lurgh/infinitive)
  [HAThread](https://community.home-assistant.io/t/carrier-bryant-infinitive-integration/119578/22)
3. [Installing Go](https://www.e-tinkers.com/2019/06/better-way-to-install-golang-go-on-raspberry-pi/)
4. [get 485 hat to work](https://forum.openmarine.net/showthread.php?tid=4534)  ```# Enable UART
enable_uart=1 ```
```
sudo nano /boot/firmware/config.txt
```
```
wget -O infinitive https://github.com/acd/infinitive/releases/download/v0.2/infinitive.arm
```
```
chmod +x infinitive
```
```
ls -lha infinitive
```
```
./infinitive -httpport=8080 -serial=/dev/ttyACM0     ttyS0 
```
4/22/25 - install rpi4 on westFH its at [10.10.15.99:8080](10.10.15.99:8080) also localDNS set to [hvactest.church](hvactest.church:8080)
4/23/25 - Runs great until terminal is closed. Closing terminal causes [HUD](https://forums.raspberrypi.com/viewtopic.php?t=34073)
[Trying this](https://www.dexterindustries.com/howto/run-a-program-on-your-raspberry-pi-at-startup/)


# Code for boot file:
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
ExecStart=/home/pi/infinitive -httpport=8080 -serial=/dev/ttyS0
[Install]
WantedBy=multi-user.target
```
```
sudo chmod 644 /lib/systemd/system/infinitive.service
```
