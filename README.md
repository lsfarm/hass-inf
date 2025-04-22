1. In use: [ACDrepo](https://github.com/acd/infinitive)
2. [UpdatedRepo](https://github.com/gogades/hass-infinitive/tree/master)
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