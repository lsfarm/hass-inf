1. In use: [ACDrepo](https://github.com/acd/infinitive)
2. [UpdatedRepo HA Intergration](https://github.com/gogades/hass-infinitive/tree/master)
  [OriginalRepo](https://github.com/mww012/ha_customcomponents)
  [MQTTRepo](https://github.com/lurgh/infinitive)
  [HAThread](https://community.home-assistant.io/t/carrier-bryant-infinitive-integration/119578/22)

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
./infinitive -httpport=8080 -serial=/dev/ttyACM0
```
4/22/25 - install rpi4 on westFH its at [10.10.15.99:8080](10.10.15.99:8080) also localDNS set to [hvactest.church](hvactest.church:8080)
4/23/25 - Runs great until terminal is closed. Closing terminal causes [HUD](https://forums.raspberrypi.com/viewtopic.php?t=34073)
[Trying this](https://www.dexterindustries.com/howto/run-a-program-on-your-raspberry-pi-at-startup/)

Code for boot file:

# /etc/systemd/system/myapp.service
[Unit]
Description=My Go App Service
After=network.target

[Service]
ExecStart=/home/pi/myapp -httpport=8080 -serial=/dev/ttyACM0
WorkingDirectory=/home/pi
Restart=always
User=pi
Environment=GO_ENV=production
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target

..

pi@hvactest4:~ $ sudo nano /lib/systemd/system/bootinf.service
pi@hvactest4:~ $ sudo chmod 644 /lib/systemd/system/bootinf.service
