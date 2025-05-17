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
## Install infinitive:
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

## Code for boot file:
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
```
systemctl enable infinitive
```
```
systemctl start infinitive
```
```
systemctl status infinitive
```

