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


## practice configuration

    config ter
    interface range Et0/0-3,Et1/0-3, Et2/0-3, Et3/0-3
    shutdown
    exit
    interface range Et0/1, Et2/0-1
    switchport trunk encapsulation dot1q
    switchport mode trunk 
    no shutdown
    exit
    vlan 2
    name SecondVlan
    exit
    interface vlan 1
    ip address 10.0.0.2 255.0.0.0
    no shutdown
    exit

    config ter 
    hostname A1
    spanning-tree mode pvst
    line con 0
    exec-timeout 0 0
    logging synchronous
    exit
    interface range Et0/0-3,Et1/0-3, Et2/0-3, Et3/0-3
    shutdown
    exit
    interface range Et1/0-1, Et2/0-1
    switchport trunk encapsulation dot1q
    switchport mode trunk 
    no shutdown
    exit
    vlan 2
    name SecondVlan
    exit
    interface vlan 1
    ip address 10.0.0.3 255.0.0.0
    no shut
    exit