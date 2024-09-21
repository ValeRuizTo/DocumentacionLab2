# DocumentacionLab2

## Miembros: Tomas Barrios Guavara, Valentina Ruiz Torres y Darek Aljuri Martinez

## 1. Introducción
En este laboratorio, se busca afianzar los conceptos fundamentales de redes, tales como LAN, WLAN, VLAN, IPv4 y subneteo, a través de la construcción y configuración de una red SOHO en un entorno emulado con Cisco Packet Tracer. Se implementarán y configurarán servicios esenciales como DHCP, DNS y NAT, asegurando una eficiente asignación de direcciones IP. Asimismo, se establecerán protocolos de enrutamiento EIGRP u OSPF en las interfaces requeridas, permitiendo el correcto enrutamiento de paquetes IP a través de la red WAN. Además, se configurará un servidor HTTP para alojar contenidos web accesibles desde cualquier dispositivo dentro de la red. Durante el desarrollo del laboratorio, se simulará el flujo de datos y se analizará su comportamiento, garantizando que todos los servicios funcionen correctamente.
## 2. Topologia de red
![](imagenesWiki/topologia.jpg)
La topología mostrada es una red SOHO (Small Office Home Office) con interconexión a Internet y servidores remotos, los omponentes principales de esta red son 

### Zona SOHO (en verde):
Incluye dispositivos de una oficina o hogar como PCs, laptops, tablets, impresoras, y un servidor local.
Hay varios PCs conectados por cable Ethernet, así como dispositivos móviles conectados a la red inalámbrica.
Está compuesta por switches (2960-24TT) y un Wireless LAN Controller (WLC) con un Access Point (LAP) para la red inalámbrica.

* Switches 2960-24TT : Segun la pagina oficial de Cisco los Switches Cisco Catalyst 2960-24TT forman parte de la serie Cisco Catalyst 2960. Esta serie es una familia de switches de capa 2 diseñados para implementaciones en redes de pequeñas y medianas empresas, ademas cuentan con "24 y 48 puertos de conectividad Gigabit Ethernet (GbE) de escritorio 10/100/1000 " [^1].
  
* Wireless LAN controller : "Un controlador WLAN gestiona los puntos de acceso de red inalámbrica que permiten a los dispositivos inalámbricos conectarse a la red." [^2]. Otra de sus funciones escentralizar la administración de la red Wi-Fi, lo que permite a los administradores gestionar configuraciones, políticas de seguridad y monitoreo de rendimiento de manera más eficiente.
  
*  Access Point (LAP): Un punto de acceso, es un dispositivo de red que permite la conexión de dispositivos inalámbricos a una red cableada. Actúa como un puente entre los dispositivos inalámbricos (como teléfonos, laptops y tabletas) y la red local (LAN). En este caso se usó un Lightweight Access Point (LAP) que es un punto de acceso (AP) que está diseñado para ser conectado a un controlador de red inalámbrica (WLC)." [^3] Es decir que a diferencia de los access point tradicionales los LAPs dependen de un controlador para gestionar su configuración, seguridad y operación.

### Interconexión WAN (en amarillo):

La topología muestra una conexión a Internet mediante routers (Cisco 2811) que representan diferentes puntos de conexión: ISP_BOG, ISP_NET, e ISP_TX.
Estos routers permiten la comunicación entre la red SOHO y servidores externos.

* El Cisco 2811 es un router de la serie 2800 de Cisco, diseñado principalmente para pequeñas y medianas empresas. Ofrece una combinación de funcionalidades de red y seguridad, y es parte de la arquitectura de red integrada de Cisco.
Zona de servidores (en azul):

Aquí se encuentran un servidor DNS y un servidor web, conectados a un switch (2960-24TT) en una DMZ, que ofrece servicios externos.
Router principal (R_SOHO):

Este dispositivo gestiona la conexión entre la red SOHO y la red WAN, asegurando el acceso a Internet y a los servidores remotos.

## 9. Referencias 
[^1]: Cisco, "Cisco Catalyst 2960 Series Switches Data Sheet," Cisco, [En línea]. Disponible en: https://www.cisco.com/c/en/us/products/collateral/switches/catalyst-2960-series-switches/product_data_sheet0900aecd806b0bd8.html

[^2]: Cisco, "What is a WLAN Controller," Cisco, [En línea].Disponible en: https://www.cisco.com/c/en/us/products/wireless/wireless-lan-controller/what-is-wlan-controller.html

[^3]: Cisco, "LAP FAQ,"  Cisco, [En línea]. Disponible en: https://www.cisco.com/c/en/us/support/docs/wireless/aironet-1200-series/70278-lap-faq.html

4 : Cisco, "Cisco 2811 Integrated Services Router," Cisco Community, [En línea] Disponible en: https://community.cisco.com/t5/networking-knowledge-base/cisco-2811-integrated-services-router/ta-p/3116259.


