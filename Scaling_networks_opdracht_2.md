# Opdracht 2 Scaling Networks

## Verbind een router (Fysiek) met een mikrotik router

### Benodigdheden

* Fysieke (GNS3) cisco router (2901)
* Mikrotik router
* Ethernetkabels (Obvious)

## uitwerking

* Begin met lege routers
* Eerst beginnen we met IP-adressen toe te kennen aan de interfaces van de routers
    * 10.0.0.1/24 &rarr;  Cisco router
    * 10.0.0.2/24 &rarr;  Mikrotik router
    * = gemeenschappelijk netwerk tussen de routers
* Hierna kunnen we de routers gaan instellen.
* De mikrotik hebben we geconfiguereerd aan de hand van een voorbeeld op internet (Zie bijlagen)
* De cisco router hebben we geconfiguereerd met de cusus van Cisco netacad

## Configuratie Cisco router

```
Building configuration...

Current configuration : 1737 bytes
!
! Last configuration change at 13:53:32 UTC Tue May 7 2019
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname Router
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
!
!
!         
!
!
!
!
!
!
ip dhcp pool home
 network 10.0.10.0 255.255.255.0
 domain-name home.local
 dns-server 9.9.9.9 
 default-router 10.0.10.1 
 lease 7
!
!
!
ip cef
no ipv6 cef
multilink bundle-name authenticated
!
!
cts logging verbose
!
!         
license udi pid CISCO2901/K9 sn FCZ18519544
!
!
!
redundancy
!
!
!
!
!
!
interface Loopback0
 no ip address
!
interface Loopback8
 ip address 8.8.8.8 255.255.255.0
!
interface Embedded-Service-Engine0/0
 no ip address
 shutdown
!
interface GigabitEthernet0/0
 ip address 10.0.0.1 255.255.255.0
 duplex auto
 speed auto
!
interface GigabitEthernet0/1
 ip address 10.0.20.1 255.255.255.0
 duplex auto
 speed auto
!
interface Serial0/0/0
 no ip address
 shutdown
 clock rate 2000000
!
interface Serial0/0/1
 no ip address
 shutdown
 clock rate 2000000
!
interface Serial0/1/0
 no ip address
 shutdown
 clock rate 2000000
!         
interface Serial0/1/1
 no ip address
 shutdown
 clock rate 2000000
!
router ospf 1
 router-id 10.0.0.1
 passive-interface GigabitEthernet0/1
 network 8.8.8.0 0.0.0.255 area 0
 network 10.0.0.0 0.0.0.255 area 0
 network 10.0.20.0 0.0.0.255 area 0
!
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
!
!
!
!
control-plane
!         
!
!
line con 0
 logging synchronous
line aux 0
line 2
 no activation-character
 no exec
 transport preferred none
 transport output pad telnet rlogin lapb-ta mop udptn v120 ssh
 stopbits 1
line vty 0 4
 login
 transport input none
!
scheduler allocate 20000 1000
!
end
```

## Configuratie Mikrotik router

``` configuration
# jan/02/1970 00:06:10 by RouterOS 6.42.7
# software id = WRJC-HL9U
#
# model = 960PGS
# serial number = AD8A09FAA190
/interface bridge
add admin-mac=B8:69:F4:C8:41:E3 auto-mac=no comment=defconf name=bridge
add name=looback
/interface list
add comment=defconf name=WAN
add comment=defconf name=LAN
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip pool
add name=default-dhcp ranges=192.168.88.10-192.168.88.254
/ip dhcp-server
add address-pool=default-dhcp disabled=no interface=bridge name=defconf
/routing ospf instance
set [ find default=yes ] router-id=10.0.0.2
/interface bridge port
add bridge=bridge comment=defconf interface=ether2
add bridge=bridge comment=defconf interface=ether3
add bridge=bridge comment=defconf interface=ether4
add bridge=bridge comment=defconf interface=ether5
add bridge=bridge comment=defconf interface=sfp1
/ip neighbor discovery-settings
set discover-interface-list=LAN
/interface list member
add comment=defconf interface=bridge list=LAN
add comment=defconf interface=ether1 list=WAN
/ip address
add address=192.168.88.1/24 comment=defconf interface=bridge network=\
    192.168.88.0
add address=10.0.0.2/24 interface=ether3 network=10.0.0.0
add address=10.0.10.1/24 interface=ether4 network=10.0.10.0
add address=10.0.0.2/24 interface=looback network=10.0.0.0
add address=10.0.30.1/24 interface=ether5 network=10.0.30.0
/ip dhcp-client
add comment=defconf dhcp-options=hostname,clientid disabled=no interface=\
    ether1
/ip dhcp-server network
add address=192.168.88.0/24 comment=defconf gateway=192.168.88.1
/ip dns
set allow-remote-requests=yes
/ip dns static
add address=192.168.88.1 name=router.lan
/ip firewall filter
add action=accept chain=input comment=\
    "defconf: accept established,related,untracked" connection-state=\
    established,related,untracked
add action=drop chain=input comment="defconf: drop invalid" connection-state=\
    invalid
add action=accept chain=input comment="defconf: accept ICMP" protocol=icmp
add action=drop chain=input comment="defconf: drop all not coming from LAN" \
    in-interface-list=!LAN
add action=accept chain=forward comment="defconf: accept in ipsec policy" \
    ipsec-policy=in,ipsec
add action=accept chain=forward comment="defconf: accept out ipsec policy" \
    ipsec-policy=out,ipsec
add action=fasttrack-connection chain=forward comment="defconf: fasttrack" \
    connection-state=established,related
add action=accept chain=forward comment=\
    "defconf: accept established,related, untracked" connection-state=\
    established,related,untracked
add action=drop chain=forward comment="defconf: drop invalid" \
    connection-state=invalid
add action=drop chain=forward comment=\
    "defconf:  drop all from WAN not DSTNATed" connection-nat-state=!dstnat \
    connection-state=new in-interface-list=WAN
/ip firewall nat
add action=masquerade chain=srcnat comment="defconf: masquerade" \
    ipsec-policy=out,none out-interface-list=WAN
/routing ospf network
add area=backbone network=10.0.10.0/24
add area=backbone network=10.0.0.0/24
add area=backbone network=10.0.30.0/24
/system routerboard settings
set silent-boot=no
/tool mac-server
set allowed-interface-list=LAN
/tool mac-server mac-winbox
set allowed-interface-list=LAN
```

## Bijlagen

[https://wiki.mikrotik.com/wiki/Manual:OSPF-examples](https://wiki.mikrotik.com/wiki/Manual:OSPF-examples)
