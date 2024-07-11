### to CHECK DETAILED VIEW OF THE STAGES WHEN ADJECENCY IS PROCESSED
    debug ip ospf adj 


### router ospf process-id defines and initializes the OSPF process
    router ospf process-id

### The network statements select and enable OSPF on the interfaces. The selection of interfaces within the OSPF process is accomplished by using the command 
    network ip-address wildcard-mask area area-id.

### The command passive interface-id under the OSPF process makes the interface passive, and the command passive interface default makes all interfaces passive.
    passive interface-id
    passive interface default 

