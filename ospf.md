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