## 32 bit version of this 
### Go Install:
```
nano go_installer.sh
```
```
export GOLANG="$(curl -s https://go.dev/dl/ | awk -F[\>\<] '/linux-armv6l/ && !/beta/ {print $5;exit}')"
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
```
sudo git clone https://github.com/lurgh/infinitive.git
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
ExecStart=/home/pi/infinitive -httpport=8080 -serial=/dev/ttyUSB0 -instance=Hall_East -mqtt=tcp://MQTT.user@192.168.15.15:1883

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
systemctl status infinitive
```

