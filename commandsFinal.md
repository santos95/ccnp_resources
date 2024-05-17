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

## ROUTING 
    Routers allows to transmit packages bewteen networks segments

    Routers moves packages from one network to another

    Routers aprenden sobre redes no conectadas a traves de rutas estaticas y protocolos de enrutamiento dinamico, estos buscan seleccionar la mejor ruta sin bucles para la transmision de paquetes

    Routers interconectados y sistemas relacionados bajo una misma administracion - Sistema autonomos

## IGP INTERIOR GATEWAY PROTOCOLS - FOR AUTONOMOUS SYSTEMS ROUTING
    RIPv2
    EIGRP
    OSPF
    IS-IS

## BGP BORDER GATEWAY PROTOCOL

## VECTOR DISCANTACE - RIP
    RIP ANOUNCE ROUTES AS VECTORS 
    DISTANCE (COST) - HOP COUNT - METRIC COUNT OF HOPS TO REACH THE NEXT NETWORK OR ROUTER
    VECTOR - INTERFACE OR IP OF THE NEXT ROUTER

## ENHANCE DISTANCE VECTOR EGIRP
    Fasters convergence times 
    Send topology updates only when the topology changes
    Metrics - bandwith, delay, reliability, load and mtu size   
    Allow load balance
## link state
    IS-IS
    OSPF    
## Path vector -BGP

## PATH SELECTION - ROUTERS SELECT THE BEST PATH EVALUATING: 
    PREFIX LENGTH
    ADMINISTRATIVE DISTANCE
    METRICS

    the fib is calculated based on the rib  

## PREFIX LENGTH 
    Refers to the numbers of initialis bits on in the mask - prefers the longest match

## ADMINISTRATIVE DISTANCE 

    Routing Protocol​

    Default Administrative Distance​

    Command​

    0​

    Static​

    1​

    EIGRP summary route​

    5​

    External BGP (eBGP)​

    20​

    EIGRP (internal)​

    90​

    OSPF​

    110​

    IS-IS​

    115​

    RIP​

    120​

    EIGRP (external)​

    170​

    Internal BGP (iBGP)​

    200​



## SHOW THE RIP (ROUTER INFORMATION BASE)
    show ip route

## static routes - default AD = 1
    No requieren comunicacion con otros enrutadores por lo que no requieren ancho de banda
    Rutas que se utilizan cuando los routers tienen poca memoria

### directly attached static routed - Static route that use the next hop interface 
    ip route network subnetmask next-hop-interface-id
    ip route 10.22.22.0 255.255.255.0 Et0/0

## Recursive route - Specified the ip of the next hop - Works when the router search for the route in the rib and make referencia cruzada con la tabla adyacente
    ip route network subnet-mask next-hop-ip.
    ip route 10.22.22.0 255.255.255.0 10.12.1.2 ​

## fully specified static route - to avoid some problems of recursive routes - arp  
    ip route network subnet-mask interface-id next-hop-ip.​
    ip route 10.22.22.0 255.255.255.0 Et0/0 10.12.1.2

## floating static route - for backups purposes when a dinamic protocols routes fails. Has superior AD respecto to the mains routes
    ip route 10.22.22.0 255.255.255.0 10.12.1.2 10
    ip route 10.22.22.0 255.255.255.0 S0/1 210 - Floating static route

## null zero static route - to avoid internet traffic - avoid loops when something is happening in the net - common tec to avoid loops
    ip route 172.16.0.0 255.255.255.0 Null0

## VRF
    vrf definition vrf-name
    address-family {ipv4 | ipv6} para especificar la familia de direcciones adecuada
    se asocia a la interfaz con el comando vrf forwarding vrf-name

## Se requieren los siguientes pasos para crear un VRF y asignarlo a una interfaz: ​

    Paso 1. Cree una tabla de enrutamiento VRF multiprotocolo utilizando el comando vrf definition vrf-name. ​

    Paso 2. Inicialice la familia de direcciones (address family) adecuada utilizando el comando address-family {ipv4 | ipv6}. El address family puede ser IPv4, IPv6, o ambos. ​

    Paso 3. ingrese al submodo de configuración de la interfaz y especifique la interfaz que se asociará con la instancia de VRF mediante el comando interface interface-id.​

    Paso 4. Asocie la instancia de VRF a la interfaz o subinterfaz ingresando el comando vrf forwarding vrf-name,  en el submodo de configuración de la interfaz.​

    Paso 5. Configure una dirección IP (IPv4, IPv6 o ambas) en la interfaz o subinterfaz ingresando uno o ambos de los siguientes comandos:​

    IPv4 - ip address ip-address subnet-mask [secondary] ​

    IPv6 - ipv6 address ipv6-address/prefix-length

## EIGRP

### show topology, successors and feasable successors
    show ip eigrp topology

### BASIC CONFIG FOR ROUTER 
    hostname R1
    no ip domain lookup
    ipv6 unicast-routing -- TO ENABLE ipv6 routing 
    banner motd # R1, Investigate Static Routes #
    line con 0
    exec-timeout 0 0
    logging synchronous
    exit
    line vty 0 4
    privilege level 15
    password cisco123
    exec-timeout 0 0
    logging synchronous
    login
    exit

### configure a directly attached static route a. Create a directly attached static route for the 192.168.2.0/27 network using E0/0 as the exit interface.
    ip route network subnetmask next-hop-interface-id
    
    config ter
    ip route 192.168.2.0 255.255.255.224 e0/0 
    end

### CHECK FOR THE ROUTES - SHOW THE RIP
show ip route static | begin Gateway

### configure a recursive static route - configure a recursive static route for the 10.2.3.0/24 network using the next hop address 10.1.2.2.
    ip route network subnet-mask next-hop-ip
    ip route 10.2.3.0 255.255.255.0  10.1.2.2
### add a fully specified static route using interface E0/0 as the exit interface.
    ip route network subnet-mask interface-id next-hop-ip.
    ip route 10.2.3.0 255.255.255.0 e0/0 10.1.2.2

### Configure a fully specified static route to the 192.168.3.0/27 network using the exit interface S2/0 and the next hop 10.1.3.3. 
    ip route 10.22.22.0 255.255.255.0 Et0/0 10.12.1.2
    ip route 192.168.3.0 255.255.255.224 S2/0 10.1.3.3
    ip route 192.168.3.0 255.255.255.224 S3/0 10.1.3.130

###    floating static route -this time adding an AD of 7.
    ip route 192.168.3.0 255.255.255.224 S3/0 10.1.3.130 7

### configure a recursive static route. a.	Create a route to the 2001:db8:acad:1012::/64 network via 2001:db8:acad:1023::2
    ipv6 route 2001:db8:acad:1012::/64 2001:db8:acad:1023::2

    
### a.	What is the neighbor’s link local address? You can see this for Ethernet interfaces using the show ipv6 neighbor command. Neighbors do not appear on serial interfaces
    show ipv6 neighbors
  

## OSPF - OPEN SHORTEST PATH FIRST
    INTERIOR GATEWAY PROTOCOL (IGP) QUE SUPERA LAS DEFICIENCIAS DE LOS ALGORITMOS DE VECTOR DE DISTANCIA Y DISTRIBUYE INFORMACION DE ENRUTAMIENTO DENTRO DE UN DOMINIO DE ENRUTAMIENTO OSPF UNICO

###    LSAs, LSDB, and SPT
    LSA - LINK STATE ADVERTISEMENTS - ospf envia lsa a enrutadores vecinos - Contienen informacion del enlace y metricas
    LSDB - LINK-STATE DATABASE - Base de datos local que almacena los LSA - Proporciona la topologia de red
    SPT - Contiene todos los destinos dentro del dominio ospf

### OSPF ARQUITECTURE - LEVELS
    OSPF utiliza multiples areas ospf dentro del dominio de enrutamiento ospf 
    Areas - Arquitectura de dos niveles
    Area 0 -> especial, conocido como backbone al que se conectan todas las demas areas
    Area x -> Demas, areas no backbone anuncian rutas hacia la area 0, la 0 anuncia rutas hacia las demas areas

### OTHER CHARACTERISTICS
    OSPF use IPv4, en el puerto tcp 89 y el direcciones multicast 224.0.0.5 en todos los enrutadores y 224.0.0.6 en enrutadores DR

### PACKAGES 
    hello - descubrir y mantener vecinos - se envian periodicamente a todas las interfaces ospf para descubrir vecinos y verificar que se encuentren en linea
    Database descriptor (DBD O DDP) - Paquetes que se envian al formarse la adyacencia ospf, describen el contenido de la LSDB
    LINK-STATE REQUEST (LSR) - Solicitud que envian los enrutadores cuando creen que parte de su LSDB esta obsoleta - solicita parte de la base de datos 
    LINK-STATE Update  (LSU) - Se envian en respuesta de la LSR, siendo un LSA explicito para el enlace 
    LINK-STATE ACK 

### OSPF Configuration​
    router ospf process-id define e inicializa el proceso OSPF.
    
    OSPF se habilita en una interfaz mediante dos métodos:​
        Una declaración de network en OSPF​
        Configuración específica de la interfaz​

### ​OSPF Network Statement​ 
    selección de interfaces dentro del proceso OSPF se logra mediante el comando network ip-address wildcard-mask area area-id 
    
    La red conectada para la interfaz habilitada para OSPF se agrega a OSPF LSDB en el área OSPF correspondiente en la que participa la interfaz.​

    network ip-address wildcard-mask area area-id 

    router ospf 1

    network 10.0.0.0 0.0.0.255 area 0 

### Interface-Specific Configuration
    El segundo método para habilitar OSPF en una interfaz para IOS es configurarlo específicamente en una interfaz con el comando ip ospf process-id area area-id
    La configuracion especifica tiene prioridad sobre la network
    Puede aumentar la complejidad cuando se incrementa el numero de interfaces

    interface E0/0
    ip address 10.0.0.1 255.255.255.0   
    ip ospf 1 area 0

    El comando router-id router-id asigna estáticamente el RID de OSPF bajo el proceso OSPF. 
    El comando clear ip ospf process reinicia el proceso OSPF en un enrutador para que OSPF pueda usar el nuevo RID (al hacer el cambio manual).​


    El comando passive interface-id bajo el proceso OSPF hace que la interfaz sea pasiva, 
    y el comando passive interface default hace que todas las interfaces sean pasivas. 
    Para permitir que una interfaz procese paquetes OSPF, se utiliza el comando no passive interface-id.​


### REQUERIMENTS
    Los RID deben ser únicos entre los dos dispositivos.​

    Las interfaces deben compartir una subred común.​

    Las MTU de las interfaces deben coincidir.​

    El ID del área debe coincidir con el del segmento.​

    La habilitación de DR debe coincidir con el segmento.​

    Los temporizadores de hello y de dead timer de OSPF deben coincidir para el segmento.​

    El tipo de autenticación y las credenciales (si las hay) deben coincidir con el segmento.​

    Los tipo de área deben coincidir con las del segmento (por ejemplo, Stub, NSSA).​

### SHOW INFO 
    show ip ospf interface [brief | interface-id] muestra las interfaces habilitadas para OSPF.​
    show ip ospf neighbor
    show ip route ospf - muestra la tabla de enrutamiento con las rutas ospf en el router

### Default Route Advertisement​
    La ruta por defecto se anuncia mediante el comando default-information originate [always] [metric metric-value] [metric-type type-value]

### interface cost
    ip ospf cost 1–65535 - cambiar el costo en la interfaz 

### other commands
    show ip ospf interface - muestra las interfaces con el tiempo de paquetes hello y dead
    ip ospf hello-interval 1–65535.​
    ip ospf dead-interval 1–65535.​  cambiar el intervalo de los hello y dead

### PARA LA ELECCION DEL DR - prioridad mas alta
    prioridad default - 1 
    ip ospf priority 100 - cambiar la prioridad

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
    switchport trunk encapsulation dot1q
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
    switchport trunk encapsulation dot1q
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
    switchport trunk encapsulation dot1q
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
    
### P3 
### R1 routing configuration 
    router ospf 4
    router-id 0.0.4.1
    network 10.0.10.0 0.0.0.255 area 0
    network 10.0.13.0 0.0.0.255 area 0
    default-information originate
    exit
    ipv6 router ospf 6
    router-id 0.0.6.1
    default-information originate
    exit
    interface e0/1
    ipv6 ospf 6 area 0
    exit
    interface s2/0
    ipv6 ospf 6 area 0
    exit
    ip route 10.0.0.0 255.0.0.0 null0
    ipv6 route 2001:db8:100::/48 null0
   
### bgp R1
    router bgp 300
    bgp router-id 1.1.1.1
    neighbor 209.165.200.226 remote-as 500
    neighbor 2001:db8:200::2 remote-as 500
    address-family ipv4 unicast 
    neighbor 209.165.200.226 activate
    no neighbor 2001:db8:200::2 activate
    network 10.0.0.0 mask 255.0.0.0
    exit-address-family
    address-family ipv6 unicast
    no neighbor 209.165.200.226 activate
    neighbor 2001:db8:200::2 activate
    network 2001:db8:100::/48
    exit-address-family

### R2 routing configuration 
    ip route 0.0.0.0 0.0.0.0 loopback 0
    ipv6 route ::/0 loopback 0
    router bgp 500
    bgp router-id 2.2.2.2
    neighbor 209.165.200.225 remote-as 300
    neighbor 2001:db8:200::1 remote-as 300
    address-family ipv4
    neighbor 209.165.200.225 activate
    no neighbor 2001:db8:200::1 activate
    network 2.2.2.2 mask 255.255.255.255
    network 0.0.0.0
    exit-address-family
    address-family ipv6
    no neighbor 209.165.200.225 activate
    neighbor 2001:db8:200::1 activate
    network 2001:db8:2222::/128
    network ::/0
    exit-address-family

### R3 routing configuration 
    router ospf 4
    router-id 0.0.4.3
    network 10.0.11.0 0.0.0.255 area 0
    network 10.0.13.0 0.0.0.255 area 0
    exit

    ipv6 router ospf 6
    router-id 0.0.6.3
    exit
    interface e0/1
    ipv6 ospf 6 area 0
    exit
    interface s2/0
    ipv6 ospf 6 area 0
    exit
    end

### D1 ROUTING
    router ospf 4
    router-id 0.0.4.131
    network 10.0.100.0 0.0.0.255 area 0
    network 10.0.101.0 0.0.0.255 area 0
    network 10.0.102.0 0.0.0.255 area 0
    network 10.0.10.0 0.0.0.255 area 0
    passive-interface default
    no passive-interface e0/1
    exit

    ipv6 router ospf 6
    router-id 0.0.6.131
    passive-interface default
    no passive-interface e0/1
    exit
    interface e0/1
    ipv6 ospf 6 area 0
    exit
    interface vlan 100
    ipv6 ospf 6 area 0
    exit
    interface vlan 101
    ipv6 ospf 6 area 0
    exit
    interface vlan 102
    ipv6 ospf 6 area 0
    exit
    end

### D2 ROUTING
    router ospf 4
    router-id 0.0.4.132
    network 10.0.100.0 0.0.0.255 area 0
    network 10.0.101.0 0.0.0.255 area 0
    network 10.0.102.0 0.0.0.255 area 0
    network 10.0.11.0 0.0.0.255 area 0

### D2 ROUTING disable ospv advertisment for all int except e0/1
    passive-interface default
    no passive-interface e0/1
    exit

### D2 ROUTING ospf 6
    ipv6 router ospf 6
    router-id 0.0.6.132
    passive-interface default
    no passive-interface e0/1
    exit
    interface e0/1
    ipv6 ospf 6 area 0
    exit
    interface vlan 100
    ipv6 ospf 6 area 0
    exit
    interface vlan 101
    ipv6 ospf 6 area 0
    exit
    interface vlan 102
    ipv6 ospf 6 area 0
    exit
    end


