# SPANNING TREE COMMANDS

## SWITCH PORT TYPES:  P2P PORTS  VS P2P EDGE 
Port type that connects one device with another device in the network (PC or switch)

P2P Edge - Especify that portfast is enabled in that port



## CHECK ROOT ID AND ROUTE PORTS 
    show spanning-tree root

## CHECK THE COST AND ROOT PORTS
    show spanning-tree vlan 1

## CHECK THE CONFIGURATION OF A PORT / CHECH THE INFORMATION OF A TRUNK PORT
    show spanning-tree interface Et1/1

## TO CHECK TOPOLOGY CHANGES
    show spanning-tree vlan 10 detail

## NORMAL SPANNING TREE PORT STATES
### Disable - Shutdown
### Blocked - Do not sent traffic and BPDUs
### Listening - Only BPDUs traffic
### Learning - BPDUs traffic and modify mac trable
### Forwarding - Normal Traffic / Modify mac table
### Broken - Discart packages

## RAPID SPANNING TREE port states
### Discarting - Disable / Shutdown
### Learning - BPDU traffic / Modify MAC table
### Forwarding  - 

### GET DHCP IP
    ip dhcp

### COMMANDS EXAMPLES 1
### BASIC SETTINGS ROUTER 1
    hostname h1
    ipv6 unicast-routing
    no ip domain lookup
    banner motd # R1 #
    line con 0
    exec-timeout 0 0
    logging synchronous
    exit
### SET INTERFACES FOR ROUTER 1
    interface e0/0
    ip address 209.165.200.225 255.255.255.224
    ipv6 address fe80::1:1 link-local
    ipv6 address 2001:db8:200::1/64
    no shutdown
    exit
    interface e0/1
    ip address 10.0.10.1 255.255.255.0
    ipv6 address fe80::1:2 link-local
    ipv6 address 2001:db8:100:1010::1/64
    no shutdown
    exit
    interface s2/0
    ip address 10.0.13.1 255.255.255.0
    ipv6 address fe80::1:3 link-local
    ipv6 address 2001:db8:100:1013::1/64
    no shutdown
    exit

### BASIC SETTINGS ROUTER 2
    hostname R2
    ipv6 unicast-routing
    no ip domain lookup
    banner motd # R2 #
    line con 0
    exec-timeout 0 0
    logging synchronous
    exit
    interface e0/0
    ip address 209.165.200.226 255.255.255.224
    ipv6 address fe80::2:1 link-local
    ipv6 address 2001:db8:200::2/64
    no shutdown
    exit
    interface Loopback 0
    ip address 2.2.2.2 255.255.255.255
    ipv6 address fe80::2:3 link-local
    ipv6 address 2001:db8:2222::1/128
    no shutdown
    exit

### BASIC SETTINGS ROUTER 3
    hostname R3
    ipv6 unicast-routing
    no ip domain lookup
    banner motd # R3, ENCOR Skills Assessment, Scenario 1 #
    line con 0
    exec-timeout 0 0
    logging synchronous
    exit
    interface e0/1
    ip address 10.0.11.1 255.255.255.0
    ipv6 address fe80::3:2 link-local
    ipv6 address 2001:db8:100:1011::1/64
    no shutdown
    exit
    interface s2/0
    ip address 10.0.13.3 255.255.255.0
    ipv6 address fe80::3:3 link-local
    ipv6 address 2001:db8:100:1010::2/64
    no shutdown
    exit


### BASIC SETTINGS SWITCH D1
    hostname D1
    ip routing
    ipv6 unicast-routing
    no ip domain lookup
    banner motd # D1  #
    line con 0
    exec-timeout 0 0
    logging synchronous
    exit
    vlan 100
    name Management
    exit
    vlan 101
    name UserGroupA
    exit
    vlan 102
    name UserGroupB
    exit
    vlan 999
    name NATIVE
    exit
    interface e1/0
    no switchport
    ip address 10.0.10.2 255.255.255.0
    ipv6 address fe80::d1:1 link-local
    ipv6 address 2001:db8:100:1010::2/64
    no shutdown
    exit
    interface vlan 100
    ip address 10.0.100.1 255.255.255.0
    ipv6 address fe80::d1:2 link-local
    ipv6 address 2001:db8:100:100::1/64
    no shutdown
    exit
    interface vlan 101
    ip address 10.0.101.1 255.255.255.0
    ipv6 address fe80::d1:3 link-local
    ipv6 address 2001:db8:100:101::1/64
    no shutdown
    exit
    interface vlan 102
    ip address 10.0.102.1 255.255.255.0
    ipv6 address fe80::d1:4 link-local
    ipv6 address 2001:db8:100:102::1/64
    no shutdown
    exit
    ip dhcp excluded-address 10.0.101.1 10.0.101.109
    ip dhcp excluded-address 10.0.101.141 10.0.101.254
    ip dhcp excluded-address 10.0.102.1 10.0.102.109
    ip dhcp excluded-address 10.0.102.141 10.0.102.254
    ip dhcp pool VLAN-101
    network 10.0.101.0 255.255.255.0
    default-router 10.0.101.254
    exit
    ip dhcp pool VLAN-102
    network 10.0.102.0 255.255.255.0
    default-router 10.0.102.254
    exit
    interface range e0/1-3, e1/1-3, e2/2-3
    shutdown
    exit

### BASIC SETTINGS SWITCH D2
    hostname D2
    ip routing
    ipv6 unicast-routing
    no ip domain lookup
    banner motd # D2 #
    line con 0
    exec-timeout 0 0
    logging synchronous
    exit
    vlan 100
    name Management
    exit
    vlan 101
    name UserGroupA
    exit
    vlan 102
    name UserGroupB
    exit 
    vlan 999
    name NATIVE
    exit
    interface e1/0
    no switchport
    ip address 10.0.11.2 255.255.255.0
    ipv6 address fe80::d1:1 link-local
    ipv6 address 2001:db8:100:1011::2/64
    no shutdown
    exit
    interface vlan 100
    ip address 10.0.100.2 255.255.255.0
    ipv6 address fe80::d2:2 link-local
    ipv6 address 2001:db8:100:100::2/64
    no shutdown
    exit
    interface vlan 101
    ip address 10.0.101.2 255.255.255.0
    ipv6 address fe80::d2:3 link-local
    ipv6 address 2001:db8:100:101::2/64
    no shutdown
    exit
    interface vlan 102
    ip address 10.0.102.2 255.255.255.0
    ipv6 address fe80::d2:4 link-local
    ipv6 address 2001:db8:100:102::2/64
    no shutdown
    exit
    ip dhcp excluded-address 10.0.101.1 10.0.101.209
    ip dhcp excluded-address 10.0.101.241 10.0.101.254
    ip dhcp excluded-address 10.0.102.1 10.0.102.209
    ip dhcp excluded-address 10.0.102.241 10.0.102.254
    ip dhcp pool VLAN-101
    network 10.0.101.0 255.255.255.0
    default-router 10.0.101.254
    exit
    ip dhcp pool VLAN-102
    network 10.0.102.0 255.255.255.0
    default-router 10.0.102.254
    exit
    interface range e0/1-3, e1/1-3, e2/0-1
    shutdown
    exit

### BASIC SETTINGS SWITCH A1
    hostname A1
    no ip domain lookup
    banner motd # A1, ENCOR Skills Assessment, Scenario 1 #
    line con 0
    exec-timeout 0 0
    logging synchronous
    exit
    vlan 100
    name Management
    exit
    vlan 101
    name UserGroupA
    exit
    vlan 102
    name UserGroupB
    exit
    vlan 999
    name NATIVE
    exit
    interface vlan 100
    ip address 10.0.100.3 255.255.255.0
    ipv6 address fe80::a1:1 link-local
    ipv6 address 2001:db8:100:100::3/64
    no shutdown
    exit
    interface range e0/2-3,e1/0-3,e3/0-3
    shutdown
    exit


### SWITCH ENABLE TRUNK LINKS  
### D1  P2
### TRUNK LINK 1
    interface range e3/0-3
    switchport mode trunk
### SET TO NATIVE VLAN IN THE TRUNK LINK 2  
    switchport trunk native vlan 999
### ETHERNETCHANNELS
    channel-group 12 mode active
    no shutdown
    exit
### TRUNK LINK 1 SET TO NATIVE VLAN IN THE TRUNK LINK 2 ETHERNETCHANNELS D1
    interface range e2/0-1
    switchport mode trunk
    switchport trunk native vlan 999
    channel-group 1 mode active
    no shutdown
    exit

### ALLOW RAPID SPANNNG-TREE D1 - general config 3 4 
    spanning-tree mode rapid-pvst
    spanning-tree vlan 100,102 root primary
    spanning-tree vlan 101 root secondary

### CONFIGURE ACCESS PORT TO DEVICES LIKE PCS 
    interface e0/0
    switchport mode access
    switchport access vlan 100
    spanning-tree portfast
    no shutdown
    exit
    end

### D2
    interface range e3/0-3
    switchport mode trunk
    switchport trunk native vlan 999
    channel-group 12 mode active
    no shutdown
    exit
    interface range e2/2-3
    switchport mode trunk
    switchport trunk native vlan 999
    channel-group 2 mode active
    no shutdown
    exit
    !
    spanning-tree mode rapid-pvst
    spanning-tree vlan 101 root primary
    spanning-tree vlan 100,102 root secondary
    !
    interface e0/0
    switchport mode access
    switchport access vlan 102
    spanning-tree portfast
    no shutdown
    exit
    end

### A1
    spanning-tree mode rapid-pvst
    interface range e2/0-1
    switchport mode trunk
    switchport trunk native vlan 999
    channel-group 1 mode active
    no shutdown
    exit
    interface range e3/2-3
    switchport mode trunk
    switchport trunk native vlan 999
    channel-group 2 mode active
    no shutdown
    exit
### CONFIGURE ACCESS PORT TO DEVICES LIKE PCS 
    interface e0/0
    switchport mode access
    switchport access vlan 101
    spanning-tree portfast
    no shutdown
    exit
    interface e0/1
    switchport mode access
    switchport access vlan 100
    spanning-tree portfast
    no shutdown
    exit
    end
