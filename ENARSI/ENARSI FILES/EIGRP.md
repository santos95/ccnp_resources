## contains information to identify loop-free backup routes. The topology table contains all the network prefixes advertised within an EIGRP autonomous system. 
    show ip eigrp topology 

## all-links keyword shows all paths received
    show ip eigrp topology  all-links

### show ip eigrp interfaces [{interface-id [detail] | detail}] shows active EIGRP interfaces
    show ip eigrp interfaces

### The command show ip eigrp neighbors [interface-id] displays the EIGRP neighbors for a router.
    show ip eigrp neighbors

### You can see EIGRP routes that are installed into the RIB by using the command show ip route eigrp, D routes from the originate autonomous system, rd - 90 EX - External - for external eigrp routes rd - 170
    show ip route eigrp
    [rd/metric]

### The router ID (RID) is a 32-bit number that uniquely identifies an EIGRP router and is used as a loop-prevention mechanism. The RID can be set dynamically, which is the default, or manually - dynamic - selected with the hig ip address for any loopback interface up, if any isn't up, with the highest ip address of any physical interface up
    router eigrp 100
    eigrp router-id 1.1.1.1

### define dhcp router server service
    ip dhcp pool HOSTS
    network 192.168.1.0 255.255.255.0
    default-router 192.168.1.1
    exit
    end

## To configure an EIGRP interface as passive, you use the command passive-interface interface-id under the EIGRP process for classic configuration
    router eigrp 100
    passive-interface e0/1

    router eigrp 100
    passive-interface default
    no passive-interface e0/0

## PASIVE INTERFACES FOR A NAMED MODE  - For a named mode configuration, you place the passive-interface state on af-interface default for all EIGRP interfaces or on a specific interface with the af-interface interface-id section
    config ter 
    router eigrp EIGRP-NAMED
    address-family ipv4 unicast autonomous-system 100
    af-interface e0/0
    passive-interface

    router eigrp EIGRP-NAMED
    address-family ipv4 unicast autonomous system 100
    af-interface default
    passive interface
    exit-af-interface
    af-interface e0/0
    no passive-interface
    exit-af-interface

### configure a classic EIGRP IPV4
    router eigrp 27
    eigrp router-id 2.2.2.2
    network 10.0.0.0 255.255.224.0
    end

### named configuration of eigrp
    router eigrp BASIC-EIGRP-LAB
    address-family ipv4 unicast autonomous-system 27
    eigrp router-id 132.132.132.132
    network 172.16.0.0 255.255.0.0
    end

### verify the interfaces involved in EIGRP
    show ip eigrp interfaces 

### examine the EIGRP entries in the IP routing table using the show ip route eigrp | begin Gateway command
    show ip route eigrp | begin Gateway

### e.	To see the Routing Information Base (RIB) Scale and Metric Scale values, as well as other protocol information, issue the show ip protocols | section eigrp command.
    show ip protocols | section eigrp

### f.	To examine details about a particular path, issue the show ip eigrp topology [address] command.
    show ip eigrp topology 192.168.3.0/24

### passive interface 
    router eigrp BASIC-EIGRP-LAB
    address-family ipv4 unicast autonomous-system 27
    af-interface E0/1.2
    passive-interface
    end

### the other aproch - passive all and then activate the required - classic eigrp
    router eigrp 27
    passive-interface default - set as passive all the interface
    no passive-interface e0/0
    no passive-interface e0/1

### the other aproch - passive all and then activate the required - named eigrp
    router eigrp BASIC-EIGRP-LAB
    address-family ipv4 unicast autonomous-system 27
    af-interface-default - to apply the command to all the interfaces
    passive-interface - passive the interface in this case all because after af-interface-default
    exit
    af-interface e0/0
    no passive-interface
    exit
    af-interface e0/1
    no passive-interface
    exit
    end

### c.	The output of the show ip protocols | section Passive command will give you a list of passive interfaces 

### set classic eigrp security with md5 
#### router 1 with named eigrp
    key chain EIGRP-AUTHEN-KEY
    key 1
    key-string $3cre7!!
    end

    router eigrp BASIC-EIGRP-LAB
    address-family ipv4 unicast autonomous-system 27
    af-interface e0/0
    authentication key-chain EIGRP-AUTHEN-KEY
    authentication mode md5
    end

### router 2 clasic eigrp config
    key chain EIGRP-AUTHEN-KEY
    key 1
    key-string $3cre7!!
    end

    interface e0/0
    ip authentication key-chain eigrp 27 EIGRP-AUTHEN-KEY
    ip authentication mode eigrp 27 md5
    exit
    interface e0/1
    ip authentication key-chain eigrp 27 EIGRP-AUTHEN-KEY
    ip authentication mode eigrp 27 md5
    end


### set named eigrp security with sha 256 
### in both routers configure key-string
#### in router 1 
    key chain EIGRP-AUTHEN-KEY
    key 1
    key-string $3cre7!!
    end

    router eigrp BASIC-EIGRP-LAB
    address-family ipv4 unicast autonomous-system 27
    af-interfae e0/1.1
    authentication mode hmac-sha-256 $3cre7!!
    end

#### router 2
    key chain EIGRP-AUTHEN-KEY
    key 1
    key-string $3cre7!!
    end

    router eigrp BASIC-EIGRP-LAB
    address-family ipv4 unicast autonomous-system 27
    af-interface e1/1
    authentication mode hmac-sha-256 $3cre7!!
    end

### CHECK KEY CHAIN
    show key chain 

### get data about an specific network and prefix with their metrics data
    show ip eigrp topology network/prefix-length.

### to get info for metrics for the interfaces 
    show interfaces interface id

### change the delay 
    config ter
    interface e0/0
    delay 100
    end
    show interface et0/0 | 1 DELAY
    

                  