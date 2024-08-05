## BGP - PATH VECTOR PROTOCOL,
#### IBGP (INTERNAL BGP) SESSION STABLISHED WITH AN IGBP ROUTER IN THE SAME AS
#### EBGP (EXTERNAL BGP) Session stablished with a bgp router in a different AS
#### ASPATH - TO DETECT LOOPS IF A ROUTER RECEIVES A ROUTE THAT CONTAINS THE ROUTER IN THE ASPATH
#### BGP PAs - path attributes - NETWORK LAYER REACHABILITY INFORMACION (NLRI)
#### ADDRESS FAMILY - HAS THEIR OWN DATABASE - FOR IPV4 AND IPV6, ALSO MORE GRANULARITY WITH UNICAST - MULTICAST
## PACKAGES - OPEN - SET UP AND STABLISHES BGP ADJECENCY
## UPDATE - ADVERTICES, UPDATES OR WITHDRAWS ROUTES
## NOTIFICATION - INDICATES AN ERROR TO BGP NEIGBORT
## KEEPALIVE ENSURES BGP NEIGHBORTS ARE STILL ALIVE

### CONNECTION SEQUENCE - BGP USE THE TCP PORT 179 TO STABLISH A TCP CONNECTION BEWTEEN A DIRECT CONNECTED ROUTER OR A REMOTE ROUTER HOPS AWAY.
#### IDLE - BGP DETECTS A START EVENT AND TRY TO INITIATE A CONNECTION, TCP CONNECTION TO A BGP PEER
#### CONNECTECT - BGP INIATES A TCP CONNECTION, USING A FLOATING PORT AS SOURCE WITH DESTINATION PORT 179. 
#### ACTIVE - tcp handsahek process, if is stablished - OPEN MESSAGE SENT -> TO openSent state
#### OPENSENT - sent open message and waite for the open message from the other, when is received - compare:
    BGP VERSION
    OPEN MESSAGE SOURVE IP MUST MATCH THE CONFIGURED IP NEIGBOR
    OPEN MESSAGE AS NUMBER MUST MATCH THE CONFIGURED NEIGHBOR AS
    BGP IDENTIFIERS (RIDS) MUST BE UNIQUES
    SECURITY PARAMETERS
#### OPENCONFIRM IF THE OPEN MESSAGE DO NOT HAVE ANY ERROR PASS FROM OPENSENT TO OPENCONFIRM - IF RECIEVE A NOTIFICACION PACKAGE PASS TO IDLE - IF RECEIVES A KEEPALIVE PASS TO ESTABLISHED
#### ESTABLISHED - THE BGP SESSION IS STABLISHED AND BGP NEIGHBORS EXCHANGE ROUTES THROUGH UPDATE PACKAGES 

### to check tcp session to check if there are atp session between routers:
    show tcp brief - in the actual router that we execute the command - local address -> remote router with a floating point - foreign address - pot 179

#### CONFIGURE BGP
    to stablish a communication between a remote bgp neighbor we need to set in bot sites: ASN of the BGP peer, authentication, keepalive timers, source and destination ip 
    Address family initialization - advertisement and summarization occurs under address family
    By default bgp set address-family ipv4, if whe set no bgp default ipv4-unicast we need to activate the address family
        router bgp 100
        no bgp default ipv4-unicast
        neighbor 10.2.1.1 remote as 200
        neighbor 10.12.1.1 timers 15 (keepalive) 50 (holdtime)
        address-family ipv4
        neighbor 10.12.1.1 activate - this referes to activation of the addres family to allow the communication with the bgp peer
    To initiate a BGP session with a peer we need activation of the address family 

#### BASIC BGP CONFIGURATION 
##### R1 - with the default option for address family - under the hood activate the address family to the peer - The bgp default ipv4-unicast command enables the automatic exchange of IPv4 address family prefixes. When this command is disabled using no bgp default ipv4-unicast, bgp neighbors must be activated within IPv4 address family (AF) configuration mode. BGP network commands must also be configured within IPv4 AF mode
    router bgp 1000
    bgp router-id 1.1.1.1
    neighbor 10.12.1.2 remote-as 200
    neighbor 10.12.1.2 password Cisco123
    neighbor 10.12.1.2 timers 10 40 
    network 192.168.1.0 mask 255.255.255.224 - to advertise this network

##### R2 - here, without the default behavior,  bgp neighbors must be activated within IPv4 address family (AF) configuration mode. BGP network commands must also be configured within IPv4 AF mode
    router bgp 200
    bgp router-id 2.2.2.2
    no bgp default ipv4-unicast
    neighbor 10.12.1.1 remote-as 1000
    neighbor 10.12.1.1 password Cisco123
    neighbor 10.12.1.1 timers 15 50
    address-family ipv4
    neighbor 10.12.1.1 activate 
    network 192.168.1.2 mask 255.255.255.224 - to advertise this network
    exit address-family

#### verifyin bgp neighbor relationship - to show the routing table
    show ip route bgp | begin Gateway

#### to check adjecency - we can check the timers, neighbors ip and router id, even the as, even the state of the connection edle, established, etc 
    show ip bgp neighbors

#### to check bgp running config
    show running-config | begin bgp

#### to verify bgp operation - * rechaable network *> prefered route - as path 0 - 300 - 1000 i, usually prefer the path with less hops when we have many routes to a single destinations. 0.0.0. referes to directly connected networks
    show ip bgp

#### to show all the paths for a specefic route - Paths: (2 available, best #2, table default) best tell which of the listed paths is taken as route to that prefix
    show ip bgp network 
    show ip bgp 192.168.1.0

#### to summarize a route - Summarizing prefixes conserves router resources and accelerates best-path calculation by reducing the size of the table - Although AS 1000 only has two prefixes 192.168.1.0/27 and 192.168.1.64/26, this customer has been allocated the entire 192.168.1.0/24 prefix.
    router bgp 1000
    bgp router-id 1.1.1.1
    neighbor 10.12.1.2 remote-as 200
    neighbor 10.12.1.2 password Cisco123
    neighbor 10.12.1.2 timers 10 40 
    network 192.168.1.0 mask 255.255.255.224 - to advertise this network
    network 192.168.1.94 mask 255.255.255.192 - to advertise this network
    aggregate-address 192.168.1.0 255.255.255.0 summary-only

    show ip route bgp | begin Gateway shows 

        192.168.1.0/24 is variably subnetted, 5 subnets, 4 masks
    B        192.168.1.0/24 [200/0], 00:27:47, Null0 - this null 0 means that is a summarize route to the same router to avoid loops.
        192.168.2.0/24 is variably subnetted, 2 subnets, 2 masks
    B        192.168.2.0/27 [20/0] via 10.1.2.2, 13:34:31
    B        192.168.2.64/26 [20/0] via 10.1.2.2, 13:34:31
    B     192.168.3.0/24 [20/0] via 10.1.3.3, 00:26:01

    R2# show ip route bgp | begin Gateway
    Gateway of last resort is not set

    B     192.168.1.0/24 [20/0] via 10.1.2.1, 00:33:53 -- only one route is instaled in other routers 
    B     192.168.3.0/24 [20/0] via 10.2.3.3, 00:32:08

    The bgp table shows the specific routes but are supressed (s)
    show ip bgp
    BGP table version is 69, local router ID is 1.1.1.1
    Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
    <output omitted>

        Network          Next Hop            Metric LocPrf Weight Path
    s>   192.168.1.0/27   0.0.0.0                  0         32768 i
    *>   192.168.1.0      0.0.0.0                            32768 i
    s>   192.168.1.64/26  0.0.0.0                  0         32768 i`

#### with bgp in other router we can summarize prefix received for another one, but if we do: 
    router bgp 500
    aggregate-address 192.168.1.0 255.255.255.0 summary-only - in the as path in other router we will see the ass path with show ip bgp - 0 500 i and not the origin router, to avoid that we can summarize like this: 
    aggregate-address 192.168.1.0 255.255.255.0 as-set summary-only

#### Configure default route advertisement on R2.
    R2(config)# router bgp 500
    R2(config-router)# neighbor 10.1.2.1 default-originate

#### shows adjecencsy for ipv4 summary
    show bgp ipv4 unicast summary
#### shows bgp neigbors iinformation 
    show bgp ipv6 neighbors
    show bgp ipv4 neighbors
    show bgp all neighbors
### show bgp afi safi - display the content of the bgp database
    The origin is a well-known mandatory BGP path attribute used in the BGP best-path algorithm. A value of i represents an IGP, e indicates EGP, and ? indicates a route that was redistributed into BGP.

            Network          Next Hop            Metric LocPrf Weight Path
        s>   192.168.1.0/27   0.0.0.0                  0         32768 i
        *>   192.168.1.0      0.0.0.0                            32768 i
        s>   192.168.1.64/26  0.0.0.0                  0         32768 i`

### ebgp - bgp session between routers in different as - ad - 20
### ibgp - bgp session between routers in the same as - ad - 200