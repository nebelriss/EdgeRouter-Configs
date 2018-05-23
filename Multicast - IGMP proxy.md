# Multicast - IGMP proxy for TV7

## Overview
These are the steps to get [TV7](https://www.init7.net/de/tv/angebot/) from Fiber7 running.

You need to configure IGMP proxy and then add rules to the Firewall to allow it.

* [VLC Playlist (XSPF)](https://api.init7.net/tvchannels)
* [M3U Playlist](https://api.init7.net/tvchannels.m3u)

## Configuration

First ssh into your router.

### IGMP proxy
* Assign an interface upstream and downstream.

```sh
ubnt@rtr1:~$ configure
[edit]
ubnt@rtr1# edit protocols igmp-proxy
[edit protocols igmp-proxy]
ubnt@rtr1# set interface eth0 role upstream
[edit protocols igmp-proxy]
ubnt@rtr1# set interface eth1 role downstream
[edit protocols igmp-proxy]
```

* Select subnets to proxy.
```sh
ubnt@rtr1# set interface eth0 alt-subnet 0.0.0.0/0
[edit protocols igmp-proxy]
ubnt@rtr1# set interface eth1 alt-subnet 0.0.0.0/0
```

* Configure a threshold for each interface.
```sh
ubnt@rtr1# set interface eth0 threshold 1
[edit protocols igmp-proxy]
ubnt@rtr1# set interface eth1 threshold 1
[edit protocols igmp-proxy]
```

* Save and display the current configuration.
```sh
ubnt@rtr1# commit
ubnt@rtr1# top
[edit]
```
```sh
ubnt@rtr1# show protocols igmp-proxy 
 interface eth0 {
     alt-subnet 0.0.0.0/0
     role upstream
     threshold 1
 }
 interface eth1 {
     alt-subnet 0.0.0.0/0
     role downstream
     threshold 1
 }
[edit]
```
```sh
ubnt@rtr1# save
Saving configuration to '/config/config.boot'...
Done
[edit]
ubnt@rtr1# exit
```

### Firewall

> These rules need to be the first.

* WAN_IN - Multicast UDP
```sh
set firewall name WAN_IN rule 1 action accept
set firewall name WAN_IN rule 1 description "Allow IPTV Multicast UDP"
set firewall name WAN_IN rule 1 protocol udp
set firewall name WAN_IN rule 1 log disable
set firewall name WAN_IN rule 1 destination address 239.0.0.0/8
set firewall name WAN_IN rule 1 source address 0.0.0.0/0
```

* WAN_IN - IGMP
```sh
set firewall name WAN_IN rule 2 action accept
set firewall name WAN_IN rule 2 description "Allow IGMP"
set firewall name WAN_IN rule 2 protocol igmp
set firewall name WAN_IN rule 2 log disable
```

* WAN_LOCAL - Multicast UDP
```sh
set firewall name WAN_LOCAL rule 1 action accept
set firewall name WAN_LOCAL rule 1 description "Allow IPTV Multicast UDP"
set firewall name WAN_LOCAL rule 1 protocol udp
set firewall name WAN_LOCAL rule 1 log disable
set firewall name WAN_LOCAL rule 1 destination address 239.0.0.0/8
set firewall name WAN_LOCAL rule 1 source address 0.0.0.0/0
```

* WAN_LOCAL - IGMP
```sh
set firewall name WAN_LOCAL rule 2 action accept
set firewall name WAN_LOCAL rule 2 description "Allow IGMP"
set firewall name WAN_LOCAL rule 2 protocol igmp
set firewall name WAN_LOCAL rule 2 log disable
```


## Test
### Statistics
```bash
ubnt@rtr1:~$ show ip multicast mfc
Group           Origin           In     Out           Pkts         Bytes  Wrong
224.1.1.1       172.16.1.6       eth0   eth1        120875      154.93MB      0
```
```bash
ubnt@rtr1:~$ show ip multicast interfaces 
Intf        BytesIn        PktsIn      BytesOut       PktsOut            Local
eth0       173.47MB        135343         0.00b             0       172.16.1.1
eth1          0.00b             0      173.47MB        135343       172.16.2.1
```

### VLC Test

## Watch

### Apple TV
> not tested by myself

Install the TV7 app from the Apple Store and run it.

### VLC
[VLC Playlist (XSPF)](https://api.init7.net/tvchannels)

1. Run VLC
2. Open -> Network
3. Add https://api.init7.net/tvchannels.m3u as Source

### Kodi
[M3U Playlist](https://api.init7.net/tvchannels.m3u)

## Sources
* [EdgeRouter - Set up IGMP proxy and statistics](https://help.ubnt.com/hc/en-us/articles/204961854-EdgeRouter-Set-up-IGMP-proxy-and-statistics)
* [IPTV/IGMP/Multicast Solution for Edgemax Router](https://community.ubnt.com/t5/EdgeRouter/IPTV-IGMP-Multicast-Solution-for-Edgemax-Router/td-p/1253350)
* [Kann ich TV7 auch auf anderen Geräten als Apple TV anschauen?](https://www.init7.net/de/support/faq/TV-andere-Geraete/)
* [EdgeRouter setup for Swiss FTTH providers](https://community.ubnt.com/t5/EdgeRouter/EdgeRouter-setup-for-Swiss-FTTH-providers/td-p/1658929)
