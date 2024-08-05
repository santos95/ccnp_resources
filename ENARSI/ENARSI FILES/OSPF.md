### to CHECK DETAILED VIEW OF THE STAGES WHEN ADJECENCY IS PROCESSED
    debug ip ospf adj 


### router ospf process-id defines and initializes the OSPF process
    router ospf process-id

### The network statements select and enable OSPF on the interfaces. The selection of interfaces within the OSPF process is accomplished by using the command 
    network ip-address wildcard-mask area area-id.

### The command passive interface-id under the OSPF process makes the interface passive, and the command passive interface default makes all interfaces passive.
    passive interface-id
    passive interface default 

### Set authentication two types for ospf plain text and md5
    R1
    interface e0/0
        ip address 10.12.1.1 255.255.255.0
        ip ospf authentication
        ip ospf authentication-key CISCO

    router ospf 1
        network 10.12.1.0 0.0.0.255 area 12
    
    R2
    interface e0/0
        ip address 10.12.1.2 255.255.255.0
        ip ospf authentication-key CISCO
    
    interface e0/1
        ip address 10.23.1.2 255.255.255.0
        ip ospf message-digest-key 1 md5 CISCO
    
    router ospf 1 
        area 0 authentication message-digest
        area 12 authentication 
        network 10.12.1.0 0.0.0.255 area 12
        network 10.23.1.0 0.0.0.255 area 0

    R3
    interface e0/1
        ip address 10.23.1.3 255.255.255.0
        ip ospf authentication message-digest
        ip ospf message-digest-key 1 md5 CISCO

    router ospf1
        network 10.23.1.0 0.0.0.255 area 0


### DR AND BDR
    1 - PRIORITY - DEFAULT VALUE 1 1 - 255
    2 - RID MAS ALTO

### check 
    show ip protocols
    show ip route ospf
    show ip ospf neighbor
    show ip ospf interface brief
    show ip ospf neighbor
    show ip ospf database
    show ip ospf topology-info

### check ospf process show even the passive interfaces
show ip protocols | section ospf

### TO SET THE priority in the interface    
    ip ospf priority 0-255

### COMMANDS TO CHECK OSPF

#### check timers for ospf
    show ip ospf interface | i Timer|line

#### the check the network type for the ospf
    show ip ospf interface Loopback 0 | include Type 

#### the check the authentication settings
    show ip ospf interface | include line|authentication|key
    show ip route ospf



#### TROUBLESHOOT OSPF
#### neighbor relationship
##### To verify the ospfv2 neighbor relationship 
    show ip ospf neighbor
##### we can verify the neighbor id (router id of the neighbor), priority, state (dr, bdr, drother), deadtime (default 40 seconds wait for hellopackets to mark the relationship as down), address (neighbor ip interface address from which the hello is sent), interface, the local interfae used to reach that neighbor

#### reasons to do not form adjencency:
    interface is down
    interface is not running ospf process and is not sending ospf hello packages
    mismatched timers (hello and dead timers must match between neigtbors)
    mismatched area numbres - the two end points of a link must be in the same ospf area
    mismateched area types - stub area or not so stubby area. 
    different subnets
    passive interface - this allow the interface to be advertised but do not send/receive hello packets
    mismatch authentication info
    acls
    mtu mismatch
    duplicate routers id
    mismatched network types

#### basic configuration erros 
    verify who should be a neighbor: show cdp neighbors
    verify routers ospf configuration and status:
        show ip interface brief and show ip protocols:
            router interface must be up 
            check passive interface
            network ip wildcar area areaid or ip ospf processid area areaid configured on the wrong interfaces or wrong area 
    
    Mismatch timers
        standar - 10 seg for hellpackeges in broadcast and ptp network and 30 for not broadcast and point to multipoint network - dead timers default 40 for broadcast and p to p and 120 for not broad and point to multipoint

        check timers: show ip ospf interface interface_type
        debug ip ospf hello - to reveal mismatch 

    Mismatch area numbers:
        show ip ospf interface interfaetype interfacenumber
        show ip ospf interface brief
        debug ip ospf adj
        show ip ospf



