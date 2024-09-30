# DocumentacionLab2

## Miembros: Tomas Barrios Guevara, Valentina Ruiz Torres y Darek Aljuri Martínez

## 1. Introducción
En este laboratorio, se busca afianzar los conceptos fundamentales de redes, tales como LAN, WLAN, VLAN, IPv4 y Subneteo, a través de la construcción y configuración de una red SOHO en un entorno emulado con Cisco Packet Tracer. Se implementarán y configurarán servicios esenciales como DHCP, DNS y NAT, asegurando una eficiente asignación de direcciones IP. Asimismo, se establecerán protocolos de enrutamiento EIGRP u OSPF en las interfaces requeridas, permitiendo el correcto enrutamiento de paquetes IP a través de la red WAN. Además, se configurará un servidor HTTP para alojar contenidos web accesibles desde cualquier dispositivo dentro de la red. Durante el desarrollo del laboratorio, se simulará el flujo de datos y se analizará su comportamiento, garantizando que todos los servicios funcionen correctamente.
## 2. Topología de red
![](imagenesWiki/topologiaredfuncionando.png)
La topología mostrada es una red SOHO (Small Office Home Office) con interconexión a Internet y servidores remotos, los componentes principales de esta red son 

### Zona SOHO (en verde):
Incluye dispositivos de una oficina o hogar como PCs, laptops, tablets, impresoras, y un servidor local.
Hay varios PCs conectados por cable Ethernet, así como dispositivos móviles conectados a la red inalámbrica.
Está compuesta por switches (2960-24TT) y un Wireless LAN Controller (WLC) con un Access Point (LAP) para la red inalámbrica.

* Switches 2960-24TT: Según la página oficial de Cisco los Switches Cisco Catalyst 2960-24TT forman parte de la serie Cisco Catalyst 2960. Esta serie es una familia de switches de capa 2 diseñados para implementaciones en redes de pequeñas y medianas empresas, además cuentan con "24 y 48 puertos de conectividad Gigabit Ethernet (GbE) de escritorio 10/100/1000 " [1].
  
* Wireless LAN Controller: "Un controlador WLAN gestiona los puntos de acceso de red inalámbrica que permiten a los dispositivos inalámbricos conectarse a la red." [2]. Otra de sus funciones es centralizar la administración de la red Wi-Fi, lo que permite a los administradores gestionar configuraciones, políticas de seguridad y monitoreo de rendimiento de manera más eficiente.
  
*  Access Point (LAP): Un punto de acceso, es un dispositivo de red que permite la conexión de dispositivos inalámbricos a una red cableada. Actúa como un puente entre los dispositivos inalámbricos (como teléfonos, laptops y tabletas) y la red local (LAN). En este caso se usó un Lightweight Access Point (LAP) que es un punto de acceso (AP) que está diseñado para ser conectado a un controlador de red inalámbrica (WLC)." [3] Es decir que a diferencia de los Access point tradicionales los LAPs dependen de un controlador para gestionar su configuración, seguridad y operación.

### Interconexión WAN (en amarillo):

La topología muestra una conexión a Internet mediante routers (Cisco 2811) que representan diferentes puntos de conexión: ISP_BOG, ISP_NET, e ISP_TX.
Estos routers permiten la comunicación entre la red SOHO y servidores externos.

* El Cisco 2811 es un router de la serie 2800 de Cisco, diseñado principalmente para pequeñas y medianas empresas. Ofrece una combinación de funcionalidades de red y seguridad

### Zona de servidores (en azul):

Aquí se encuentran un servidor DNS y un servidor web, conectados a un switch (2960-24TT), que ofrece servicios externos.

## 3. Síntesis de la metodología y resultados de configuración: 
* **Montaje de la topología:** Utilizando el cableado estructurado y los modelos de dispositivos indicados (Switches Cisco 2960, Router Cisco 2811 (Routers a los cuales se les agrego el modulo WIC-2T para permitir conexiones seriales, es decir para poder conectar multiples routers entre si para poder similar la WAN del internet), WLC 3504 y LAP 3702i), se recreó la topología de red en Cisco Packet Tracer.
  
  ![.](imagenesWiki/CableadoEstructuradoFoto1Nuevo.png)
  
 El cableado estructurado realizado se hizo siguiendo las indicaciones en clase y las lecturas. Todos los computadores fueron conectados a los patch panels A y B, para esto se conectaron los   punchdowns A a el patch panel A y los punchdowns B y el patch panel B, esto por las normas impuestas dentro de las lecturas, siendo las A para la red_unisabana y las B para la red SOHO que se montó para este laboratorio. Se montaron dispositivos extras en nuevos racks y mesas para poder cumplir lo que pide el laboratorio y se utilizaron los 3 primeros switches del cableado para la topologia del SOHO.

![.](imagenesWiki/CableadoEstructuradoFoto2.png)

 Los patch panels de interconexión se conectaron por la parte de atrás para poder conectar dispositivos que se encontraban en distintos racks, un set de ocho para cada interconexión, se conectan en la parte de atrás para no tener cables entre racks en la parte de enfrente. 

![.](imagenesWiki/CableadoEstructuradoFoto3.png)

 Se puede evidenciar el proceso del cableado realizado en la sala de computación. Solo 4 computadores están funcionando debido a que en la topologiá pedida para el laboratorio solo disponía de 4 computadores y como se reutilizo el primer switch se desconectaron los otros computadores a este para que no impidiera en conseguir la topologiá deseada.
  
* **Esquema de direccionamiento IPv4:** Se diseñó un esquema de direccionamiento basado en los requerimientos de las VLAN, asignando rangos de direcciones según la cantidad de dispositivos. Se realizó el Subneteo y se construyó una tabla de direccionamiento para toda la topología, teniendo en cuenta que las VLANs 20 y 40 requieren 1022 clientes cada una, mientras que las VLANs 99 y 55, requieren 254 clientes cada una y que la  zona de servidores  requiere 10 hosts

| ![.](imagenesWiki/subneteoInternet.jpg)| ![.](imagenesWiki/subneteosoho.jpg) |![.](imagenesWiki/subneteoServidores.jpg) |
|:----------------------------------------------:|:---------------------------------------------------------:|:---------------------------------------------------------:|

* **Configuración de VLANs:** Se crearon y configuraron las 4 VLANs del SOHO y la VLAN de la zona de servidores según el esquema de direccionamiento diseñado. Se verificó la correcta creación y funcionamiento de estas mediante los siguientes comandos:
  * Para mostrar la configuración de VLANs en el switch:
      - ***show vlan brief***: Este comando muestra un resumen de todas las VLANs configuradas en el switch, incluyendo sus IDs y los puertos asignados.
        
| Switch red soho  | Descripción                                                                                                                                                                                                                                                                                                             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ![.](imagenesWiki/vlansoho.jpg) | **VLAN Name**: Muestra el nombre de cada VLAN. Las VLANs identificadas son: <br>1: default, <br>20: Guest,<br> 40: Internal, <br>55: Staff,<br> 99: Native.<br> <br> **Status**: Muestra el estado de cada VLAN, que en todos los casos aparece como <br> "active", lo que significa que están operativas.<br> <br> **Ports**: Indica los puertos asignados a cada VLAN. Por ejemplo:<br> VLAN 1 (default) tiene asignados los puertos Fa0/6 a Fa0/9, Fa0/10 a Fa0/13, <br> Fa0/14 a Fa0/17, y otros hasta Gig0/2.<br> VLAN 20 (Guest) tiene asignado el puerto Fa0/4.<br> VLAN 40 (Internal) tiene asignado el puerto Fa0/3.<br> VLAN 55 (Staff) tiene asignado el puerto Fa0/5.<br> VLAN 99 (Native) no tiene puertos visibles asignados.<br> También se observan otras VLANs predeterminadas como 1002 (fddi-default),<br> 1003 (token-ring-default), <br>1004 (fddinet-default), <br>y 1005 (trnet-default), todas en estado activo pero sin puertos asignados en este caso. |

| Switch red Servers  | Descripción                                                                                                                                                                                                                                                                                                             |
|-----------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <img src="imagenesWiki/vlanserver.jpg" width="2900px"> | **VLAN Name**:VLAN Name: Muestra los nombres de las VLANs. En este caso, se observa la VLAN por defecto (VLAN 1) y otras VLANs predeterminadas (fddi-default, token-ring-default, fddinet-default, trnet-default), las cuales están activas aunque no suelen utilizarse en configuraciones actuales<br> <br> Status: Indica el estado de la VLAN. Todas las VLANs en la salida están en estado "active". <br> <br> Ports: Lista los puertos asociados a cada VLAN. Para la VLAN 1 (default), todos los puertos FastEthernet (Fa0/1 a Fa0/24) y dos puertos GigabitEthernet (Gig0/1 y Gig0/2) están asignados a esta VLAN. |


  * Para ver la configuración de interfaces:
      - ***show running-config | section interface:*** Este comando revela la configuración de las interfaces del dispositivo. Aquí se detallan las interfaces con configuraciones relacionadas con las VLANs, los puertos en modo acceso y los enlaces troncales. Por esta reazon el comando solo se va a usar en la red SOHO donde hay VLANS asignadas
  
  - Enlaces troncales: Los puertos Fa0/1, Fa0/2, y Gi0/1 están configurados como troncales, transportando múltiples VLANs, lo cual es típico en una red donde se requiere que varias VLANs pasen por una única interfaz entre switches o entre un switch y un router.
  - Puertos de acceso: Los puertos Fa0/3, Fa0/4 y Fa0/5 están configurados en modo acceso, cada uno vinculado a una VLAN específica (40, 20 y 55 respectivamente), lo que los conecta a dispositivos que solo necesitan pertenecer a una única VLAN.
  - VLANs: Se tienen varias interfaces VLAN configuradas con direcciones IP específicas, lo que podría indicar que el switch está actuando como un router de capa 3, o que se están utilizando estas VLANs para enrutar tráfico entre diferentes segmentos de red.
    
| Switch red soho  | Descripción                                                                                                                                                                                                                                                                                                             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <img src="imagenesWiki/interfacessoho.jpg" width="1500px"> | ***Interface FastEthernet0/1:*** <br> **switchport trunk native vlan 99:** Configura la VLAN nativa del troncal como la VLAN 99. Esto significa que el tráfico no etiquetado será asignado a esta VLAN <br> **switchport trunk allowed vlan 20,40,55,99:** Permite que las VLANs 20, 40, 55 y 99 puedan pasar por esta interfaz en modo troncal. <br> **switchport mode trunk:** Configura el puerto en modo troncal, lo que significa que puede transportar tráfico de múltiples VLANs. <br><br> ***Interface FastEthernet0/2:*** Tiene la misma configuración que FastEthernet0/1: es una interfaz en modo troncal que permite las VLANs 20, 40, 55 y 99, con la VLAN nativa configurada como 99.<br><br>***Interfaces FastEthernet0/3 y FastEthernet0/4:*** <br>**switchport access vlan 40 (en Fa0/3) y switchport access vlan 20 (en Fa0/4):** Estas interfaces están configuradas en modo acceso, lo que significa que pertenecen a una sola VLAN (40 o 20, respectivamente) y cualquier tráfico en estos puertos será etiquetado para la VLAN correspondiente. <br><br> ***Interface FastEthernet0/5:***<br> **switchport access vlan 55:** Está configurada como un puerto de acceso para la VLAN 55.<br> <br> **Interfaces FastEthernet0/6 a FastEthernet0/24:** Estas interfaces no tienen configuraciones adicionales especificadas, por lo que probablemente estén en su configuración predeterminada, o no están activas.<br><br> **Interfaces GigabitEthernet0/1 y GigabitEthernet0/2:** *GigabitEthernet0/1* tiene una configuración similar a Fa0/1 y Fa0/2, en modo troncal, permitiendo las VLANs 20, 40, 55 y 99, con la VLAN 99 como VLAN nativa y *GigabitEthernet0/2* no tiene configuraciones específicas aparte de estar en modo troncal.<br><br> ***Interfaces de VLAN:*** <br> **interface Vlan20:** Asignada a la dirección *IP 172.23.0.3* con la máscara de red 255.255.255.0.<br> **interface Vlan40:** Asignada a la dirección *IP 172.23.4.3* con la máscara 255.255.255.0.<br> **interface Vlan55:** Asignada a la dirección *IP 172.23.8.3* con la misma máscara 255.255.255.0.<br> **interface Vlan99:** Asignada a la dirección IP *172.23.9.3* con una máscara de 255.255.255.0. |



* **Configuración de dispositivos:** Se aplicaron configuraciones básicas a los switches y router, como la asignación de direcciones IP a las interfaces dependiendo del Subneteo realizado anteriormente y configuración de servicios como DHCP, DNS y NAT. Además, se configuró el WLC y el LAP (punto de acceso inalámbrico) para la red WLAN. La documentación de los routers y switches están en un **archivo .txt adjunto** en el repositorio y en la tarea en teams.


* **Asignación y verificación de IPs:** Se verificó que las direcciones IP fueran asignadas correctamente a todos los dispositivos de la red. Se utilizaron comandos TCP/IP para comprobar la conectividad entre los nodos.
  * **¿Se requiere asignación dinámica y/o estática? ¿Dónde? ¿Traducción de direcciones de forma dinámica y/o estático y/o por puertos? ¿En qué terminales se deben configurar los servicios requeridos?**
   * configuración de **NAT**:
     1. ***Conservación de direcciones IP:*** En una red SOHO como la que se tiene, los dispositivos internos usan direcciones IP privadas (la red  SOHO usa  172.23.0.0), que no son válidas en Internet. NAT actua como un traductor entre direcciones publicas y privadas para que asi, desde la soho aque tiene una direccion piblica se pueda acceder al internet y posteriormente a la red de servidores, que ambos son publicos. En este caso se usó PAT (Port Address Translation), "que es una extensión a la traducción de direcciones de red (NAT), que permite que varios dispositivos en una red de área local (LAN) se puedan asignar a una sola direccion IP publica" [5].
        - ¿Por Qué PAT?
          
            En la topología, donde hay varios dispositivos en la red SOHO (172.23.0.0/16) que necesitan acceder a Internet, PAT es una excelente solución porque permite que todos los dispositivos de la red SOHO utilicen una sola dirección IP pública (o un pequeño rango de IPs públicas) para acceder a Internet. Los dispositivos compartirán la dirección IP pública del router de la zona de interconexión WAN (11.31.12.0) cuando se conecten a Internet. En la topologia el servicio de PAT está configurado en el router de la red WAN (Cisco 2811), que traduce las direcciones IP privadas de la red SOHO a la dirección IP pública utilizada para acceder a Internet."

        - ¿Cómo funciona PAT?
          
          *Dirección IP compartida*: Todos los dispositivos dentro de tu red SOHO (PCs, smartphones, tablets, etc.) envían tráfico a Internet utilizando direcciones IP privadas (como 172.23.x.x). El router que tiene configurado NAT/PAT traduce todas estas direcciones IP           privadas a una única dirección IP pública asignada a la interfaz de salida del router (hacia Internet).

          *Asignación de puertos:* Para diferenciar el tráfico de cada dispositivo dentro de la red, el router cambia los números de puerto en los encabezados de los paquetes TCP/UDP. Esto permite que muchos dispositivos compartan una única dirección IP pública, ya que           cada conexión queda identificada por una combinación de la dirección IP pública y el puerto único asignado a cada dispositivo.
          
     2.  ***Interconexión WAN:*** En la topología, se estan utilizando routers para conectar varios dispositivos a la red del ISP. NAT permite que los routers gestionen múltiples conexiones internas hacia el exterior sin conflictos de dirección IP, facilitando la conectividad hacia servicios externos, como los servidores en la zona azul.
    
     3.  ***Seguridad:*** NAT actúa como una capa de protección, ya que oculta las direcciones IP internas de la red SOHO. Esto significa que desde el exterior (Internet) no se puede acceder directamente a las máquinas en la red interna sin una configuración explícita.


  * Configuración de **DHCP**, asignacion **Dinamica** 
    
    Se usa DHCP en la red SOHO y no en las otras redes debido a las diferentes necesidades de cada zona:
    
    1. ***Red SOHO (172.23.0.0):***
              
       La red SOHO está compuesta por dispositivos como PCs, laptops, tablets, impresoras y otros dispositivos que pertenecen a un entorno de oficina o hogar. Este esta configurado en el Server 0, es un servidor que asigna direcciones IP automáticamente a los dispositivos, esto se hace por las siguientes razones :
         
       - Facilidad y Flexibilidad: Los dispositivos que entran y salen de la red con frecuencia, como tablets, smartphones o laptops, necesitan una forma flexible de obtener una dirección IP sin tener que                configurarla manualmente. DHCP facilita esto, ya que asigna direcciones IP dinámicamente sin intervención del usuario.
       - Configuración Centralizada: El servidor DHCP en la red SOHO centraliza la asignación de IPs, DNS y puerta de enlace predeterminada, lo que hace que la administración sea más simple y eficiente para los          administradores de la red.
       - Escalabilidad: Al tener varios dispositivos que pueden conectarse o desconectarse con regularidad, DHCP asegura que las direcciones IP se asignen y liberen automáticamente, evitando la necesidad de             asignarlas manualmente cada vez.

        ![.](imagenesWiki/dchpsoho.jpg)

  * Asignación **Estatica**
    
    2. ***Red de Servidores (161.130.8.0):***
              
       En la zona de servidores, los dispositivos como el servidor DNS y el servidor web necesitan tener direcciones IP fijas o estáticas:
         
       - Consistencia y Accesibilidad: Los servidores  deben tener direcciones IP fijas para que puedan ser accesibles desde cualquier parte de la red y desde Internet. Las aplicaciones y servicios que dependen       de ellos necesitan saber siempre cuál es su dirección IP.
       - Configuraciones Manuales: Los servidores requieren configuraciones manuales más específicas y detalladas que las que proporciona DHCP. Además, el uso de IPs estáticas asegura que los servidores no           cambien de dirección IP, lo que es crucial para el funcionamiento de servicios de red como DNS o servidores web.


    3. ***Red de Interconexión WAN (11.31.12.0)::***
              
       La red WAN conecta tu red local a Internet mediante routers Cisco 2811 y otros dispositivos de interconexión. En este caso:
         
       - Routers y Gateways: Los routers y otros dispositivos de interconexión también necesitan direcciones IP fijas para garantizar una conexión estable con otras redes externas. Usar direcciones estáticas en esta zona asegura que los routers mantengan la misma IP pública, lo cual es esencial para la comunicación con el ISP y otros routers en la WAN.
       - Tráfico Estable: En una WAN, los dispositivos y routers generalmente no cambian con frecuencia, por lo que no es necesario utilizar DHCP. Al tener direcciones fijas, se garantiza una gestión más controlada y predecible del tráfico de red hacia y desde Internet.




* **Pruebas de conectividad:** Se realizaron pruebas de conectividad entre dispositivos de la misma VLAN y entre diferentes VLANs, utilizando comandos como ping. También se comprobó la conectividad con las puertas de enlace y otros servicios:
  1. Prueba de comunicacion entre dispositivos conectados en la VLAN 40 (PC1 a PC2)
     
      ![.](imagenesWiki/vlan40avlan40.jpg)

  2. Prueba de comunicacion entre dispositivos conectados en diferenetnes VLANs en este cado de la 40 a la 55. desde el PC2 hasta Printer0
     
     ![.](imagenesWiki/vlan40avlan55.jpg)

  3. ***Show interface trunk***, que muestra qué puertos están configurados como trunks y qué VLANs están permitidas en esos puertos.
     
     ![.](imagenesWiki/trunks.jpg)

  4. Conectividad entre el Switch dos y el LAP(access point). Se tiene una tasa de éxito del 100% en el ping hacia 172.23.9.4, lo que significa que la conectividad entre switch2 (IP 172.23.9.3) y el LAP está funcionando perfectamente.
      
      ![.](imagenesWiki/switchlap.jpg)

  5. Concetividad entre diferentes redes, Desde PC1(SOHO) hasta el servidor web (Servers)
     
     ![.](imagenesWiki/pcserver.jpg)

  6. Concetividad entre Red soho a default Gateway de red servidores
      ![.](imagenesWiki/SOHODG.jpg)

 

  * **Se encontraron diversos problemas al momento de probar la comunicacion entre dispositivos y VLANs:**

    1. El primer problema lo encontramos al intentar comunicar diferentes VLANs en la red SOHO, ya que al hacer ping nunca     recibíamos una respuesta. Tras revisar la configuración de las VLANs, descubrimos que las direcciones IP no estaban        bien asignadas, lo que causaba el fallo de conectividad. Para resolverlo, ajustamos la configuración de las IPs en         cada dispositivo dentro de las VLANs afectadas. La solución completa se puede ver en las fotos incluidas en los            puntos i y ii.

    2. Otro de los problemas que enfrentamos fue con la conectividad del WLC (Wireless LAN Controller). Inicialmente, no       se podía conectar. La solución fue ingresar al servidor y asignarle la IP correspondiente al WLC. Luego, en el WLC,        tuvimos que verificar los puertos y asegurarnos de que el puerto que estaba conectado al switch coincidiera con la         configuración del WLC. El problema radicaba en que el WLC había sido configurado desde una computadora que solo            manejaba la VLAN 20. Uno de los puertos debía coincidir con el puerto donde el switch estaba conectado, pero no lo         habíamos configurado correctamente.

    Una vez solucionamos el problema del puerto, la conectividad seguía sin funcionar. Nos dimos cuenta de que el problema     era que, al crear la configuración, se había generado automáticamente una VLAN de administración, que es la que asigna     la IP al LAP (Lightweight Access Point). Al hacer el cambio, no habíamos ajustado correctamente la VLAN de                 administración, lo que causaba el fallo. Tras corregir esto, el WLC comenzó a funcionar correctamente.

       - **Paso a paso de la solcuion**
   
           - ***Acceder al WLC y verificar la configuración:*** Conectarse al WLC mediante su IP desde un navegador o un cliente de administración, y desde ahí, Asegurarse de que el WLC esté configurado correctamente con las VLAN correspondientes. Ademas se debe verificar que el puerto del WLC que está conectado al switch coincida con el puerto configurado en la interfaz del switch. ![.](imagenesWiki/wlc.jpg)
    Como se puede ver en la imagen la **direccion Ip** del WLC es 172.23.9.2 y su **Default gateway** es 172.23.9.1, cabe resaltar que la direccion IP debe ser estatica, si se usa DCHP la direccion cambia e interrumple la comunicación con el LAP
            
           - ***Verificar la VLAN de administración***: Desde el WLC, Asegurarse de que la VLAN de management esté activa y sea la que asigna las IPs al LAP (Lightweight Access Point). En la pestaña de config del LAP en Global dice WLC, la direccion que va en controller desbe ser la dreccion Ip del WLC para que la comunicación sea efectiva.
          ![.](imagenesWiki/LAP.jpg)

          -***Verificar el DCP en los dispositivos inhalambicos conectados al LAP:*** desde la Tablet PC0, la Laptop0 o el Smartphone0 revisar la  direccion IP asignada por el DHCP y si coincide con la VLAN en la que deberia estar
          ![.](imagenesWiki/cel.jpg)

    
* **Verificación del protocolo STP:** Se verificó que el protocolo STP estuviera correctamente configurado para evitar bucles, y se comprobó cuál de los switches fue seleccionado como puente raíz.

    * Para hacer la verificación se utiliza el comando **show spanning-tree** que muestra información como La configuración de STP, las VLANs para las cuales está activo, el estado de los puertos (puerto raíz, puerto designado, etc.), el tiempo de convergencia y otros parámetros relevantes
    
    * la salida del comando show spanning-tree muestra que el switch actual (sw1_Intranet) es el puente raíz para varias VLANs
                                  ![.](imagenesWiki/spanningtree.jpg)
  

      * Información General de VLANs:
      La salida muestra información sobre varias VLANs (VLAN0020, VLAN0040, VLAN0055 y VLAN0099), y cada una tiene configurado el protocolo STP. A continuación se presenta un desglose de cada sección:

         1. ***Root ID:***
         
            - Priority: Indica la prioridad del puente raíz (Root Bridge) de la VLAN. La prioridad más baja gana, por lo que aquí se puede observar que el puente raíz tiene diferentes prioridades para cada VLAN (32788 para VLAN0020, 32808 para VLAN0040, etc.).
            - Address: Muestra la dirección MAC del puente raíz, que en este caso es 0001.4217.5157. Este switch es el puente raíz para todas las VLANs listadas, ya que en cada caso se indica "This bridge is the root".
        
        2. ***Bridge ID:***
            - También incluye la dirección MAC del switch actual y la prioridad del puente. La salida indica que el switch actual es también el puente raíz para las VLANs, como se muestra en las prioridades.
        
        3. ***Parámetros de STP:***
            - Hello Time: 2 segundos, el intervalo entre los mensajes Hello enviados por el puente raíz para mantener la topología.
            - Max Age: 20 segundos, el tiempo que un switch mantiene información de la topología antes de considerarla obsoleta.
            - Forward Delay: 15 segundos, el tiempo que un puerto permanece en estado de escucha antes de poder comenzar a enviar tráfico.

        4. ***Estado de los Puertos:***
            - Cada VLAN muestra una tabla con la información de los puertos:
            - Interface: El puerto del switch (por ejemplo, Fa0/1, Fa0/23, etc.).
            - Role: Indica si el puerto es Designado (Desg) o Raíz (Root). Todos los puertos listados son Designados (Desg), lo que significa que están activos y pueden enviar y recibir tráfico.
            - Sts: Estado del puerto (FWD indica que el puerto está en modo de reenvío).
            - Cost: Costo asociado al puerto. Los costos más bajos son preferidos en la selección de caminos hacia el puente raíz.
            - Prio.Nbr: La prioridad del puerto y su número (por ejemplo, 128.1 para Fa0/1).
            - Type: Indica el tipo de conexión (en este caso, P2p, que significa punto a punto).

            
  * Para verificar si es posible hacer Telnet desde un PC a otros switches y a un router (como R_SOHO) se utiliza el siguiente comando en la línea de comandos (CMD) del PC, **telnet <dirección IP>**, cabe resaltar que "Telnet es el nombre de un protocolo de red que permite acceder a otra máquina para manejarla remotamente como si estuviéramos sentados delante de ella" [6].
    - **Configuración paso a paso**
      - Paso 1: Configurar el Dispositivo de Destino
        
           - Accedee al Dispositivo: El router (R_SOHO) o los switches a los que se va a conectar deben tener configurado un nombre de usuario y una contraseña para Telnet.Esto se hace desde la CLI del dispositivo:
             
                   configure terminal
             
                   line vty 0 4
             
                   login local
             
                   transport input telnet
             
                   username admin password cisco
    
      - Paso 2: Configurar el PC
        
           - Seleccionar el PC desde el que se va a hacer Telnet, en este caso desde el PC1.
           - Acceder a la Consola del PC: Seleccionar la pestaña Desktop y luego hacer clic en Command Prompt
           - Ejecutar el Comando de Telnet: En el símbolo del sistema del PC, se debe escribir el siguiente comando, reemplazando <dirección IP> por la dirección IP del dispositivo al que se va a conectar (por ejemplo, la dirección IP del router R_SOHO ):
             
                   telnet 200.190.7.1
             
                   username:admin
             
                   password: cisco

       - Paso 3: revisar resultado, si aparece el prompt del dispositivo al que se queria acceder entonces fue exitoso. Esto se puede probar en toda la red SOHO
    
        ![.](imagenesWiki/telnetrouter.jpg)



* **Configuración de enrutamiento:** Se configuraron los protocolos de enrutamiento OSPF o EIGRP en las interfaces necesarias, verificando el correcto enrutamiento de paquetes entre las redes LAN y WAN.
  **porque es ese y que interfaces usamos**

* **Pruebas de servicios web:** Se configuró un servidor HTTP y se verificó que todos los usuarios pudieran acceder a la página web personalizada (dvt.net) desde cualquier dispositivo de la red, utilizando el dominio gestionado por el servidor DNS.
  * configuración de **DNS**
  * **pruebas**

## 4. Puntos solicitados en la sección de Resultados y Análisis
1) Responda a cada una de las preguntas guías del diseño estructurado propuesta en clase y documente 
todo el proceso de desarrollo del esquema de direccionamiento IPv4 propuesto por su equipo y diligencie 
las tablas de Subneteo y direccionamiento de la red.
**Subneteo y su tabla**

**Tabla de direccionamiento de red**


3) análisis y proceso de configuración de los servicios de red requeridos para el correcto 
funcionamiento de la red empresarial.

4) Evaluación del flujo bidireccional de datos generado al acceder a la página alojada en el servidor Web por los 
nodos terminales (PCs y dispositivos móviles) de las diferentes VLANs que conforman la topología, utilizando el servicio DNS. Como también, al verificar la conectividad de un PCs a otro y de un 
PC al Gateway. **Justifique su análisis utilizando capturas con el simulador y los filtros de paquetes de Cisco 
Packet Tracer**.

## 5. Retos presentados durante el desarrollo de la práctica
## 6. Conclusiones y recomendaciones

## 7. Referencias 
1: Cisco, "Cisco Catalyst 2960 Series Switches Data Sheet," Cisco, [En línea]. Disponible en: https://www.cisco.com/c/en/us/products/collateral/switches/catalyst-2960-series-switches/product_data_sheet0900aecd806b0bd8.html

2: Cisco, "What is a WLAN Controller," Cisco, [En línea]. Disponible en: https://www.cisco.com/c/en/us/products/wireless/wireless-lan-controller/what-is-wlan-controller.html

3: Cisco, "LAP FAQ,” Cisco, [En línea]. Disponible en: https://www.cisco.com/c/en/us/support/docs/wireless/aironet-1200-series/70278-lap-faq.html

4: Cisco, "Cisco 2811 Integrated Services Router," Cisco Community, [En línea] Disponible en: https://community.cisco.com/t5/networking-knowledge-base/cisco-2811-integrated-services-router/ta-p/3116259.

5: A. Sanchez, "Port Address Translation (PAT): Ejemplos," ADSL FAQS, Jul. 11, 2020. [En línea]. Disponible en: https://adslfaqs.com/port-address-translation-pat-ejemplos/.

6: [1] "Telnet," Wikipedia, The Free Encyclopedia. [En línea]. Disponible: https://es.wikipedia.org/wiki/Telnet. 

