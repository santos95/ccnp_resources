# CHAPTER - PACKET FORWADING

## SWITCH

### CREATE VLAN - GLOBAL CONFIG 
    config term

    vlan 50

    name VlanName

    vlan 60

    name VlanName

#### CHECK VLAN AND PORT ASSIGNMENT

    show vlan brief 

    show vlan vlanName

    show vlan Group60

    show vlan id

    show vlan 60

### ACCESS PORT

#### DEFINE A PORT AS ACCESS PORT 
    interface interfaceName

    interface Eth1/1
    
    switchport mode access

### ASSIGN A VLAN TO AN SPECIFC ACCESS PORT
    switchport access vlan {id | name}

    switchport access vlan 99

###  CONFIGURE A SWITCH TO SUPPORT IP ROUTING AND IPV6 UNICAST ROUTING
    ip routing 

    ipv6 unicast-routing

### CREATE A VLAN AND NAME THEM AS SPECIFIC 
    vlan 50

    name Group50

    exit

    vlan 60
    name Group60
    exit

### ASSIGN A SPECIFIC INTERFACE TO A VLAN - in a switch 
    interface Et0/1
    switchport mode access
    switchport access vlan 50
    no shutdown
    exit

    interface Et0/2
    switchport mode access
    switchport access vlan 60
    no shutdown
    exit

### CREATE A SWITCHED VIRTUAL INTERFACE (SVI) THAT SUPPORT A VLAN
    interface vlan 50
    ip address 10.2.50.1 255.255.255.0
    ipv6 address fe80::d1:2 link-local
    ipv6 address 2001:db8:acad:1050::d1/64
    no shutdown
    exit

### CHECK MAC ADDRESSS TABLE 
    show mac addresss-table dynamic 

### ADD A STATIC MAC-ADDRESS    
    mac address-table static mac-address vlan vlanId

### CLEAR THE MAC ADDRESS DYNAMIC TABLE
    clear mac address-table dynamic

### CONFIGURE A ROUTED PORT AND DEFAULT ROUTES TOWARD A ROUTER - NO SWITCHPORT CHANGE THE L2 PORT TO A L3 PORT -> ROUTED PORT
    interface Et0/0
    no switchport
    ip address 10.1.13.13 255.255.255.0
    ipv6 address fe80::d1:1 link-local
    ipv6 address 2001:db8:acad:10d1::d1/64
    no shutdown
    exit

### VERIFY AN INTERFACE 
    show vlan brief 
    show vlan brief | i Et0/0

### CONFIGURE STATUC DEFAULT ROUTES FOR IPV4 AND 6 THAT POINT TOWARD THE INFERFACE ADDRESS AT ROUTER - in a switch
    ip route 0.0.0.0 0.0.0.0 10.1.13.1
    ipv6 route ::/0 2001:db8:acad:10d1::1

### CONFIGURE INTERFACE ADDRESSING AND STATIC ROUTING
#### CONFIGURE ROUTER TO SUPPORT IPV6 UNICAST ROUTING
    ipv6 inicast-routing

#### CONFIGURE INTERFACES IN THE ROUTER WITH THE ADSRESSES 
    interface Et0/0
    ip address 10.1.13.1 255.255.255.0
    ipv6 address fe80::1:1 link-local
    ipv6 address 2001:db8:acad:10d1::1/64
    no shutdown
    exit

    interface S0/0
    ip address 10.1.3.1 255.255.255.0
    ipv6 address fe80::1:2 link-local
    ipv6 address 2001:db8:acad:1013::1/64
    no shutdown
    exit
    

### CONFIGURE ROUTING ON THE ROUTER - CONFIGURE STATIC ROUTES SUPPORTED BY THE SWITCH AND ANYTHING ELSE TO ANOTHER ROUTER
    ip route 10.2.0.0 255.255.0.0 10.1.13.13
    ipv6 route 2001:db8:acad:1050::/64 2001:db8:acad:10d1::d1
    ipv6 route 2001:db8:acad:1060::/64 2001:db8:acad:10d1::d1
    ip route 0.0.0.0 0.0.0.0 10.1.3.3
    ipv6 route ::/0 2001:db8:acad:1913::3


### SAFE CONFIGURATION
    copy running-config startup-config

### TO VERIFY IP ADDRESSES
    show ip interface breaf

### WHEN WE WANT TO SET A PORT AS TRUNK AND WE GET THE ERROR An interface whose trunk encapsulation is "Auto"
    switchport trunk encapsulation dot1q
### then we can configure the port interface as trunk and set a native vlan
    switchport mode trunk -- set as trunk
    show interfaces trunk - show info about trunk interfaces
    swithport trunk native vlan 999 

### TO specify which vlan can be forwaded throug a link
    switchport trunk allowed vlan 10,20,30

### VLAN NATIVE 
    swithport trunk native vlan vlan-id

### SHOW ARP TABLE
    show ip arp

###  CREATE SUBINTERFACES AND ASSIGN A VLAN TO IT   
    interface Et0/0.75
    encapsulation dot1Q 75

    interface Et0/0.85
    encapsulation dot1Q 85

    interface Et0/0.999
    encapsulation dot1Q 999 native

### show vrf interfaces
    show ip vrf interfaces
    

    


