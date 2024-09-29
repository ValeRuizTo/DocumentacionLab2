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
          
            En la topología, donde hay varios dispositivos en la red SOHO (172.23.0.0/16) que necesitan acceder a Internet, PAT es una excelente solución porque permite que todos los dispositivos de la red SOHO utilicen una sola dirección IP pública (o un pequeño rango de IPs públicas) para acceder a Internet. Los dispositivos compartirán la dirección IP pública del router de la zona de interconexión WAN (11.31.12.0) cuando se conecten a Internet.

        - ¿Cómo funciona PAT?
          
          *Dirección IP compartida*: Todos los dispositivos dentro de tu red SOHO (PCs, smartphones, tablets, etc.) envían tráfico a Internet utilizando direcciones IP privadas (como 172.23.x.x). El router que tiene configurado NAT/PAT traduce todas estas direcciones IP           privadas a una única dirección IP pública asignada a la interfaz de salida del router (hacia Internet).

          *Asignación de puertos:* Para diferenciar el tráfico de cada dispositivo dentro de la red, el router cambia los números de puerto en los encabezados de los paquetes TCP/UDP. Esto permite que muchos dispositivos compartan una única dirección IP pública, ya que           cada conexión queda identificada por una combinación de la dirección IP pública y el puerto único asignado a cada dispositivo.
          
     2.  ***Interconexión WAN:*** En la topología, se estan utilizando routers para conectar varios dispositivos a la red del ISP. NAT permite que los routers gestionen múltiples conexiones internas hacia el exterior sin conflictos de dirección IP, facilitando la conectividad hacia servicios externos, como los servidores en la zona azul.
    
     3.  ***Seguridad:*** NAT actúa como una capa de protección, ya que oculta las direcciones IP internas de la red SOHO. Esto significa que desde el exterior (Internet) no se puede acceder directamente a las máquinas en la red interna sin una configuración explícita.


  * configuración de **DHCP** o manuales


* **Pruebas de conectividad:** Se realizaron pruebas de conectividad entre dispositivos de la misma VLAN y entre diferentes VLANs, utilizando comandos como ping y tracert. También se comprobó la conectividad con las puertas de enlace y otros servicios.:)
 **fotos**
  * **Se encontraron problemas, como se solucionaron? evidencia de la solución y la prueba del funcionamiento**

* **Verificación del protocolo STP:** Se verificó que el protocolo STP estuviera correctamente configurado para evitar bucles, y se comprobó cuál de los switches fue seleccionado como puente raíz.
  **por qué?**
  * Para hacer la verificación se utiliza el comando **show spanning-tree** que muestra información como La configuración de STP, las VLANs para las cuales está activo, el estado de los puertos (puerto raíz, puerto designado, etc.), el tiempo de convergencia y otros parámetros relevantes **foto**
  * Para verificar si es posible hacer Telnet desde un PC a otros switches y a un router (como R_SOHO) se utiliza el siguiente comando en la línea de comandos (CMD) del PC, **telnet <dirección IP>** 

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
