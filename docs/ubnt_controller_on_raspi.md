# Install Ubiquiti controller on Raspberry Pi

## Install
```sh
sudo apt-get install unifi oracle-java8-jdk -y
```
```sh
update-java-alternatives -s jdk-8-oracle-arm32-vfp-hflt
```
```sh
sudo systemctl disable mongodb
sudo systemctl stop mongodb
```

## Sources
* [UniFi Controller 5.5 on Raspberry Pi](https://community.ubnt.com/t5/UniFi-Wireless/UniFi-Controller-5-5-on-Raspberry-Pi/td-p/2045751)
