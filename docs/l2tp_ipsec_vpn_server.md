# L2TP IPsec VPN Server

## Add firewall rules

### Ports and Protocols
* L2TP 1701 udp
* IKE 500 udp
* ESP 50 protocol
* NAT-T 4500 udp

### configure
* ssh into the client and go into configure mode

```sh
configure
```

## Configure VPN

Set WAN interface for IPSec (eth0 = WAN)
```sh
set vpn ipsec ipsec-interfaces interface eth0
```

2. 

## Sources
* [EdgeRouter - L2TP IPsec VPN Server](https://help.ubnt.com/hc/en-us/articles/204950294-EdgeRouter-L2TP-IPsec-VPN-Server#3)
* [Edgemax- L2TP Server Setup For Client Use](https://community.ubnt.com/t5/EdgeRouter/Edgemax-L2TP-Server-Setup-For-Client-Use/td-p/891812)
