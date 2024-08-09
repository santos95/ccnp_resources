
### ROUTER 1
### BASIC CONFIG - PORTS AND INITIAL CONFIG EXAMPLE
hostname R1
no ip domain lookup
ipv6 unicast-routing
banner motd # THIS IS R1, ENARSI EXAM - SANTOS ORTIZ #
enable secret santos12345
username admin privilege 15 algorithm-type scrypt secret santos12344
line con 0
logging synchronous
exec-timeout 0 0
exit
line vty 0 4
login local
transport input telnet
exec-timeout 5 0
exit
clock timezone UTC -6
interface e0/0
ip address 209.165.200.1 255.255.255.0
ipv6 address fe80::1:1 link-local
ipv6 address 2001:db8:200::1/64
no shut
exit
interface e0/1
ip address 10.165.249.1 255.255.255.0
ipv6 address fe80::1:2 link-local
ipv6 address 2001:db8:249::1/64
no shut
exit
interface s2/0
ip address 209.165.202.1 255.255.255.0
ipv6 address fe80::1:3 link-local
ipv6 address 2001:db8:202::1/64 
no shut
exit
interface s2/1
ip address 209.165.203.1 255.255.255.0
ipv6 address fe80::1:4 link-local
ipv6 address 2001:db8:203::1/64
no shut
exit
interface loopback 0
ip address 10.0.0.1 255.255.255.0
ipv6 address fe80::1:5 link-local
ipv6 address 2001:db8:10::1/64
no shut
exit
interface loopback 1
ip address 10.165.248.1 255.255.255.0
ipv6 address fe80::1:6 link-local
ipv6 address 2001:db8:248::1/64
no shut 
exit
end

### R2
hostname R2
no ip domain lookup
ipv6 unicast-routing
banner motd # THIS IS R2, ENARSI EXAM - SANTOS ORTIZ #
enable secret santos12345
username admin privilege 15 algorithm-type scrypt secret santos12344
line con 0
logging synchronous
exec-timeout 0 0
exit
line vty 0 4
login local
transport input telnet
exec-timeout 5 0
exit
clock timezone UTC -6
interface e0/0
ip address 209.165.200.2 255.255.255.0
ipv6 address fe80::2:1 link-local
ipv6 address 2001:db8:200::2/64
no shut
exit
interface e0/1
ip address 209.165.201.2 255.255.255.0
ipv6 address fe80::2:2 link-local
ipv6 address 2001:db8:201::2/64
no shut
exit
interface loopback 0
ip address 172.16.0.1 255.255.255.0
ipv6 address fe80::2:3 link-local
ipv6 address 2001:db8:172::1/64
no shut
exit
interface loopback 1
ip address 209.165.224.1 255.255.255.0
ipv6 address fe80::2:4 link-local
ipv6 address 2001:db8:224::1/64
no shut
exit

### R3
hostname R3
no ip domain lookup
ipv6 unicast-routing
banner motd # THIS IS R3, ENARSI EXAM - SANTOS ORTIZ #
enable secret santos12345
username admin privilege 15 algorithm-type scrypt secret santos12344
line con 0
logging synchronous
exec-timeout 0 0
exit
line vty 0 4
login local
transport input telnet
exec-timeout 5 0
exit
clock timezone UTC -6
interface e0/0
ip address 209.165.201.1 255.255.255.0
ipv6 address fe80::3:1 link-local
ipv6 address 2001:db8:201::1/64
no shut
exit
interface e0/1
ip address 192.168.241.1 255.255.255.0
ipv6 address fe80::3:2 link-local
ipv6 address 2001:db8:241::1/64
no shut
exit
interface s2/0
ip address 209.165.202.2 255.255.255.0
ipv6 address fe80::3:3 link-local
ipv6 address 2001:db8:202::2/64
no shut
exit
interface s2/1
ip address 209.165.203.2 255.255.255.0
ipv6 address fe80::3:4 link-local
ipv6 address 2001:db8:203::2/64
no shut
exit
interface loopback 0
ip address 192.168.0.1 255.255.255.0
ipv6 address fe80::3:5 link-local
ipv6 address 2001:db8:192::1/64
no shut
exit
interface loopback 1
ip address 192.168.240.1 255.255.255.0
ipv6 address fe80::3:6 link-local
ipv6 address 2001:db8:240::1/64
no shut
exit

### D1
hostname D1
no ip domain lookup
ip routing 
ipv6 unicast-routing
banner motd # THIS IS D1, ENARSI EXAM - SANTOS ORTIZ #
enable secret santos12345
username admin privilege 15 secret santos12345
line con 0
logging synchronous
exec-timeout 0 0
exit
line vty 0 4
login local
transport input telnet
exec-timeout 5 0
exit

clock timezone UTC -6

interface range e0/0-3,e1/0-3,e2/0-3,e3/0-3
switchport mode access
shut
exit

vlan 250
name Users
exit
vlan 251
name Servers
exit

interface e1/0
no switchport
ip address 10.165.249.2 255.255.255.0
ipv6 address fe80::d1:1 link-local
ipv6 address 2001:db8:249::2/64
no shut
exit

interface e3/1
switchport mode access
spanning-tree portfast
switchport access vlan 250
no shut
exit

interface vlan 250
ip address 10.165.250.1 255.255.255.0
ipv6 address fe80::d1:2 link-local
ipv6 address 2001:db8:24a::1/64
no shut
exit

interface vlan 251
ip address 10.165.251.1 255.255.255.0
ipv6 address fe80::d1:3 link-local
ipv6 address 2001:db8:24b::1/64
no shut
exit

interface range e2/0-1
switchport trunk encapsulation dot1q
switchport mode trunk
channel-group 1 mode active 
no shut
exit

ip dhcp excluded-address 10.165.250.1 10.165.250.5 
ip dhcp pool VLAN250DHCP
network 10.165.250.0 255.255.255.0
default-router 10.165.250.1
exit

ip dhcp excluded-address 10.165.251.1 10.165.251.5
ip dhcp pool VLAN251DHCP
network 10.165.251.0 255.255.255.0
default-router 10.165.251.1
exit

exit

### D2
hostname D2
no ip domain lookup
ip routing 
ipv6 unicast-routing
banner motd # THIS IS D2, ENARSI EXAM - SANTOS ORTIZ #
enable secret santos12345
username admin privilege 15 secret santos12345
line con 0
logging synchronous
exec-timeout 0 0
exit
line vty 0 4
login local
transport input telnet
exec-timeout 5 0
exit

clock timezone UTC -6

vlan 242
name Users
exit

vlan 243
name Servers
exit


interface range e0/0-3,e1/0-3,e2/0-3,e3/0-3
switchport mode access
shut
exit

interface e1/0
no switchport
ip address 192.168.241.2 255.255.255.0
ipv6 address fe80::d2:1 link-local
ipv6 address 2001:db8:241::2/64
no shut
exit

interface e3/1
switchport mode access
spanning-tree portfast
switchport access vlan 242
no shut
exit

interface e3/2
switchport mode access
spanning-tree portfast
switchport access vlan 243
no shut
exit

interface vlan 242
ip address 192.168.242.1 255.255.255.0
ipv6 address fe80::d2:2 link-local
ipv6 address 2001:db8:242::1/64
no shut
exit

interface vlan 243
ip address 192.168.243.1 255.255.255.0
ipv6 address fe80::d2:3 link-local
ipv6 address 2001:db8:243::1/64
no shut
exit

ip dhcp excluded-address 192.168.242.1 192.168.242.5
ip dhcp pool VLAN242DHCP
network 192.168.242.0 255.255.255.0
default-router 192.168.242.1 
exit

ip dhcp excluded-address 192.168.243.1 192.168.243.5
ip dhcp pool VLAN243DHCP
network 192.168.243.0 255.255.255.0
default-router 192.168.243.1 
exit


### A1
hostname A1
no ip domain lookup
banner motd # THIS IS A1, ENARSI EXAM - SANTOS ORTIZ #
enable secret santos12345
username admin privilege 15 secret santos12344
line con 0
logging synchronous
exec-timeout 0 0
exit
line vty 0 4
login local
transport input telnet
exec-timeout 5 0
exit

clock timezone UTC -6

vlan 251
name  Users
exit

vlan 250
name Servers
exit

interface range e0/0-3,e1/0-3,e2/0-3,e3/0-3
switchport mode access
shut
exit

interface e3/1
switchport mode access
spanning-tree portfast
switchport access vlan 251
no shut
exit

interface e3/2
switchport mode access
spanning-tree portfast
switchport access vlan 250
exit

interface vlan 250
ip address 10.165.250.2 255.255.255.0
ipv6 address fe80::a1:1 link-local
ipv6 address 2001:db8:24a::2/64
no shut
exit

ip default-gateway 10.165.250.1

interface range e2/0-1
switchport trunk encapsulation dot1q
switchport mode trunk
channel-group 1 mode active
 no shutdown
 exit

### end basic configuration

### CONFIGURE EIGRP ON R1 AND D1
#### R1
router eigrp ENARSI-SA
address-family ipv4 unicast autonomous-system 1
eigrp router-id 0.4.10.1
network 10.0.0.0
network 10.165.248.0
network 10.165.249.0
exit-address-family
address-family ipv6 unicast autonomous-system 1
eigrp router-id 0.6.10.1
af-interface e0/0
shutdown
exit-af-interface
exit-address-family
exit

#### END R1
#### D1 
router eigrp ENARSI-SA
 address-family ipv4 unicast autonomous-system 1
  eigrp router-id 0.4.10.2
  network 10.165.249.0
  network 10.165.250.0
  network 10.165.251.0
af-interface vlan 250
   passive-interface
   exit
af-interface vlan 251
   passive-interface
   exit
  exit-address-family
 address-family ipv6 unicast autonomous-system 1
  eigrp router-id 0.6.10.2
  af-interface vlan 250
   passive-interface
   exit
  af-interface vlan 251
   passive-interface
   exit
  exit-address-family
 exit
##### END D1

### CONCIGURE BGP IN R1 R2 Y R3
#### R1
router bgp 10
 no bgp default ipv4-unicast
 bgp router-id 4.6.10.1
 neighbor 209.165.200.2 remote-as 172
 neighbor 209.165.202.2 remote-as 192
 neighbor 209.165.203.2 remote-as 192
 neighbor 2001:db8:200::2 remote-as 172
 neighbor 2001:db8:202::2 remote-as 192
 neighbor 2001:db8:203::2 remote-as 192
address-family ipv4 unicast
neighbor 209.165.200.2 activate
neighbor 209.165.202.2 activate
neighbor 209.165.203.2 activate
network 10.165.248.0 mask 255.255.255.0
network 10.165.249.0 mask 255.255.255.0
network 10.165.250.0 mask 255.255.255.0
network 10.165.251.0 mask 255.255.255.0
network 10.0.0.0 mask 255.255.255.0
exit
address-family ipv6 unicast
  neighbor 2001:db8:200::2 activate
  neighbor 2001:db8:202::2 activate
  neighbor 2001:db8:203::2 activate
  network 2001:db8:248::/64
  network 2001:db8:249::/64
  network 2001:db8:24a::/64
  network 2001:db8:24b::/64
  network 2001:db8:10::/64
  exit
 exit

###### redistribuite bgp into eigrp
router eigrp ENARSI-SA
address-family ipv4 unicast autonomous-system 1
topology base
redistribute bgp 10 metric 1000000 10 255 1 1500
exit
exit-address-family
address-family ipv6 unicast autonomous-system 1
topology base
redistribute bgp 10 metric 1000000 10 255 1 1500
exit
exit
exit
#### END R1

#### R2
ip route 0.0.0.0 0.0.0.0 null0
ipv6 route ::/0 null0
router bgp 172
no bgp default ipv4-unicast
bgp router-id 4.6.172.2
neighbor 209.165.200.1 remote-as 10
neighbor 209.165.201.1 remote-as 192
neighbor 2001:db8:200::1 remote-as 10
neighbor 2001:db8:201::1 remote-as 192
address-family ipv4 unicast
neighbor 209.165.200.1 activate
neighbor 209.165.201.1 activate
network 172.16.0.0 mask 255.255.255.0
network 209.165.224.0
network 0.0.0.0 mask 0.0.0.0
exit
address-family ipv6 unicast
  neighbor 2001:db8:200::1 activate
  neighbor 2001:db8:201::1 activate
  network 2001:db8:172::/64
  network 2001:db8:224::/64
  network ::/0
  exit
exit

#### END R2

#### R3
router bgp 192
no bgp default ipv4-unicast
bgp router-id 4.6.192.3
neighbor 209.165.201.2 remote-as 172
neighbor 209.165.202.1 remote-as 10
neighbor 209.165.203.1 remote-as 10
neighbor 2001:db8:201::2 remote-as 172
neighbor 2001:db8:202::1 remote-as 10
neighbor 2001:db8:203::1 remote-as 10
address-family ipv4 unicast
neighbor 209.165.201.2 activate
neighbor 209.165.202.1 activate
neighbor 209.165.203.1 activate
network 192.168.240.0
network 192.168.241.0
network 192.168.242.0
network 192.168.243.0
network 192.168.0.0
exit
 address-family ipv6 unicast
  neighbor 2001:db8:201::2 activate
  neighbor 2001:db8:202::1 activate
  neighbor 2001:db8:203::1 activate
  network 2001:db8:240::/64
  network 2001:db8:241::/64
  network 2001:db8:242::/64
  network 2001:db8:243::/64
  network 2001:db8:192::/64
  exit
 exit


#### END R3
### END BGP CONFIGURATION

#### CONFIGURE OSPFV3 R3 AND D2

##### R3

router ospfv3 1
router-id 0.0.192.3
address-family ipv4 unicast
passive-interface default
no passive-interface e0/1
redistribute bgp 192
exit
address-family ipv6 unicast
  passive-interface default
  no passive-interface e0/1
  redistribute bgp 192
  exit
 exit
interface e0/1
 ospfv3 1 ipv4 area 0
 ospfv3 1 ipv6 area 0
 exit
interface loopback 0
 ip ospf network point-to-point
 ipv6 ospf network point-to-point
 ospfv3 1 ipv4 area 0
 ospfv3 1 ipv6 area 0
 exit
interface loopback 1
 ip ospf network point-to-point
 ipv6 ospf network point-to-point
 ospfv3 1 ipv4 area 0
 ospfv3 1 ipv6 area 0
 exit

##### END R3

##### D2
router ospfv3 1
router-id 0.0.192.2
address-family ipv4 unicast
    passive-interface default
    no passive-interface e1/0
exit
address-family ipv6 unicast
  passive-interface default
  no passive-interface e1/0
exit
exit
interface e1/0
 ospfv3 1 ipv4 area 0
 ospfv3 1 ipv6 area 0
exit
interface vlan 242
 ospfv3 1 ipv4 area 0
 ospfv3 1 ipv6 area 0
exit
interface vlan 243
 ospfv3 1 ipv4 area 0
 ospfv3 1 ipv6 area 0
 exit
end


##### END D2



 

