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

### SHOW BLOCKED PORTS  
    show spanning-tree blockedports 

## DEBUG MODE TO SHOW LOGS FOR SPANNIG TREE TOPOLOGY ADJUSTMENT - TOPOLOGY CHANGES 
    debug spanning-tree events

## SPANING TREE - COMMON SPANNIG TREE MODE
    spanning-tree mode pvst

## RAPID SPANNING-TREE MODE
    spanning-tree mode rapid-pvst

## STP TOPOLOGY

### SET THE ROOT BRIDGE THROUG PRIORITY - Priority - value 0 - 61440 - INCREMENTS OF 4096
    spanning-tree vlan 10 priority 0

### primary - 24576 -- Secundary 28672
    spanning-tree vlan 10 root {primary | secundary}

#### default priority value 32,768 + external id - change the value to 24576 + external id
    spanning-tree vlan 10 root primary 

### modify the ports by cost  - Modify the cost  - path cost
    spannig-tree cost 10 - Modify the cost of all vlans

    spannig-tree vlan 10 cost 10 - Modify the cost of a specific vlan

### STP ports priority 
    spanning-tree port-priority 10 - all vlans

    spanning-tree vlan 10 port-priority 10 - specific vlan
### ROOT GUARD - TO AVOID A DESIGNATED PORT TO TURN INTO A ROOT PORT - FOR PORTS THAT CONECT TO A SWITHCES THAT MUST NOT BE ROOT BRIDGE - IF THE PORT IDENTIFY A BDPU -> TURN INTO A ERRDISABLE STATE 
    spanning-tree guard root - In a specific interface

### PORTFAST PORTS - PORT CONFIGURATION FOR ACCESS PORTS THAT ENABLES PORT TO AVOID LEARNING AND LISTENING STATES - DIRECTLY TO FORWARDING, ALSO AVOID THE GENERATION OF TCN BPDUS, IF IDENTIFY A BPDU THIS FUNCIONALITY GONE

    spanning-tree portfast -- for a specific port 

    spanning-tree portfast default -- for all the access port

    spanning-tree portfast disable - disable portfast for a specific port

    spanning-tree portfast trunk

    Example: 
    config ter 
    interface Et0/0
    switchport mode access
    switchport access vlan 10 
    spanning-tree portfast

### BPDU GUARD - CONFIGURATION ORIENTED TO POSTFAST PORTS THAT DISABLE THIS PORT WHEN IDENTIFY A BPDU - THIS BPDU GUARD IS ORIENTED TO PORTFAST PORTS ORIENTED TO THE HOST 
    spanning-tree portfast bpduguard default - to all the portfast STP ports

    spanning-tree portfast bpduguard enable/disable - to a spefic portfast STP port

### TO CHECK THE PORTFAST AND BPDU GUARD WITH THE COMMAND:
    show spanning-tree interface Et0/0 detail

### IF A PORT GET A BPDU - STATE TO errdisable - to check this: 
    show interface status -> we can see the err-disable status of the port

### BPDUGUARD ERROR RECOVERY - SERVICE THAT ENABLES DISABLE PORTS PRODUCT OF THE BPDUGUARD CONFIGURATION - PORTS ERR-DISABLE BY THE BPDUGUARD DO NOT RECORY AUTOMATICALY

    errdisable recovery cause bpduguard - recovery a port disable by the bpdu guard

    errdisable recovery interval 10 - set the time or period where the error recovery checks the ports

### BPDU FILTER - CONFIGURE THE PORTS IN A WAY THAT AVOIDS THEM TO SEND BPDU OUTSIDE THE PORT
    sppaning-tree portfast bpdufilter default - activate bpdu filter globaly

    spanning-tree bpdufilter enable - activate bpdu filter in a speficic port

### STP LOOP GUARD - WHEN A ROOT PORT DO NOT GET BPDUS, GENERALLY TURN INTO A DESIGNATED PORT AND ANOTHER PORT TURN INTO ROOT AND THIS GENERATE A LOOP - WHEN OPTIC FIB - FOR THIS THE STP LOOP GUARD TURN THE ROOT PORT INTO A ERR-DISABLE AND AVOID THAT ALT AND ROOT PORT TURN INTO DESIGNATED PORT - THIS IS NOT COMPATIBLE WITH PORTFAST PORTS 
    spanning-tree loopguard default - global 

    spanning-tree guard loop - specific interface

    show spanning-tree inconsistent-ports

### UDLD - UNIDIRECTIONAL LINK DETECTION - ENABLES BIDIRECTIONAL MONITORIZATION OF OPTICAL FIB - TWO TYPES : NORMAL - SI NO SE RECONOCE UNA TRAMA, EL ENLACE SE CONSIDERA INDETERMINADO Y EL PUERTO PERMANECE ACTIVO - AGRESIVE - SI NO SE RECONOCE UNA TRAMA, SE ENVIAN 8 PAQUETES EN UN INTERVALO DE 1 SEGUNDO, SI NO SE RECONOCEN LOS PAQUETES - PORT ERR-DISABLE
    udld enable - global command to enable udld
    udld enable aggressive - global command to enable aggressive udld
    udld port [aggressive] - enable udld in a speficic interface
    udld port disable - desable udld in a specific port/interface
    udld recovery [interval time] - enables udld recovery - default time 5 seg
    show udld neighbors - shows udld speed state
    show udld interface-id - shows detailed udld informatioin










