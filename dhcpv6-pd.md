# DHCPv6-PD
Set up DHCPv6 Prefix Delegation for Fiber7 as ISP.

## Config
* Go inti configure mode
```sh
configure
```

* This lines need to be repeated for every interface/vlan that was previously configured. Every interface, if it's in another ip range, the prefix-id needs to be adjusted.
```sh
set interfaces ethernet eth1 dhcpv6-pd pd 0 prefix-length /48
set interfaces ethernet eth1 dhcpv6-pd pd 0 interface eth0 service slaac
set interfaces ethernet eth1 dhcpv6-pd pd 0 interface eth0 prefix-id :0
```

* Save and exit
```sh
commit
save
exit
reboot
```

## Sources
* [Configuring a Ubiquiti EdgeRouter Lite (Erlite-3) for Fiber7](https://michael.stapelberg.de/posts/2014-08-11-fiber7_ubnt_erlite/)
