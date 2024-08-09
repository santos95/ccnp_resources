
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








 

