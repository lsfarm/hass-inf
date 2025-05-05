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
git clone https://github.com/lurgh/infinitive.git
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
cd home and make sure it runs:
```
cd && ./infinitive -httpport=8080 -serial=/dev/ttyUSB0
```
