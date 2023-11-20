# CCNA Cheatsheet
**Usa este material bajo tu propio riesgo, estos apuntes pueden contener tener errores y solo son de referencia y/o apoyo para estudio**

## Mascaras de red mas ocupadas
- /30: 255.255.255.252
- /24: 255.255.255.0
- /22: 255.255.252.0

## Comandos generales
- set hostname: 
    - <code> hostname [name]</code>
- Copia la config actual a la startup config: 
    - <code>copy running-config startup-config</code>
- Guardar la configuracion del dispositivo:
    - <code>do write</code>
- A configure mode command that sets the IP addresses of DNS servers: <br> 
    - <code>ip name-server [dns-server1] [dns-server2]</code>
- Sets the speed to the specified value or negotiates it automatically: <br>
    - Dentro de la interfaz <code>speed [{10 | 100 | 1000 | auto}]</code>
- In router configuration mode, disables automatic summarization: 
    - <code>no auto-summary</code>
- Rango de interfaces
    - <code>int range [intf_inicio_rango] - [intf_termino_rango], [intf_extras]</code>
- Agregar usuarios:
    - <code>username [name] password [pass]</code>
- Setea password para usar el enable:
    - Sin cirfrar: <code>enable password [pass]</code>
    - Cifrada con MD5:  <code>enable secret [pass]</code>
- Cifrar todas las passwords del dispositivo:
    - <code>service password-encryption</code>
- Configurar nombre de dominio predeterminado:
    - <code>ip domain-name [nombre_dominio]</code>

## Shows
- <code> show running-config </code>
- <code> show interfaces </code>
- <code> show interface status </code>
- <code> show interfaces switchport </code>
- <code> show interfaces trunk </code>
- <code> show vlan </code>
- <code> show vlan brief </code>
- <code> show vtp status </code>
- <code> show mac address-table </code>
- <code> show cdp </code>
- <code> show running-config interface interface [intf] </code>
- <code> show port security [interface] </code>
- <code> show ip protocols </code>
- <code> show ip route static </code> Las rutas estáticas las vemos definidas en el router como 'S'.
- <code> show ip ospf interface </code>
- <code> show ip ospf interface [intf] </code>
- <code> show ip ospf interface brief</code>
- <code> show ipv6 ospf</code>
- <code> show ipv6 ospf interface</code>
- <code> show interfaces port-channel [port_channel_num]</code>
- <code> show ipv6 ospf neighbor</code>
- <code> show spanning-tree vlan [VLAN_ID] ipv6</code>
- <code> show access-list</code>
- <code> show ip nat translations</code>
- <code> show ip nat translations verbose</code>
- <code> show ip nat statistics</code>

- Vecinos: <code>do show ip ospf neighbor</code>
- Inundación de LSAs: <code>show ip ospf </code>database: 
- calculo de spf: <code>show ip ospf rib</code>
- Distancia Administrativa:
    - <code>show ip route </code>:<br>
    En la salida del comando show ip route puedes ver marcadas las rutas que fueron agregadas por OSPF, estas se identifican por la 'O'.
    - <code>show ip route ospf </code><br>
    También puedes ver que la distancia administrativa es 110 [110/2]. El 2 [110/2] hace referencia a la métrica de OSPF para esta ruta.

- IPV6 <hr>
    -   show ipv6 interface
    -   show ipv6  interface brief
    -   ping ipv6 [ip]

# Tabla de enrutamiento (show ip route)
-   O es para OSFP
-   D para EIGRP.
-   C para Directamente Conectadas
-   S para Rutas Estáticas
-   L para Locales.
- [110/128] = [distancia_administrativa/metrica]

# Routing and VLAN Commands
- ### Ruta estatica: <br>
    - <code>ip route [ip_destino] [mascara_ip_destino] [sig_salto/interfaz_salida]</code>
- ### Ruta por defecto: <br>
    - <code>ip route 0.0.0.0 0.0.0.0 [sig_salto/interfaz_salida]</code>
- ### Entrar al modo de configuracion de vlan:
    - <code>vlan [numero_vlan]</code>
    - #### Nombrar una vlan: (dentro de la vlan)
        - <code> name [vlan_name]</code>
- ### (switch) Configurar una vlan a una interfaz:<br>
    - <code>interface [interfaz]</code>
        - <code> switchport mode access</code>
        - <code>switchport access vlan [vlan_num]</code><br>
- ### (switch) Configurar una troncal:
    - <code>interface [interfaz]</code>
        - <code>switchport mode trunk </code>
        - <code>switchport trunk allowed vlan [vlan_n, vlan_n]</code>
  
- ### (router) Configurar una vlan a una subinterfaz: <br>
    - <code>interface [interfaz].[sub-interfaz(preferentemente el numero de la vlan)]</code><br>
        - <code>encapsulation dot1Q número_de_vlan</code>
        - <code>ip address dirección_ip máscara_de_subred</code>

- ### Configurar SSH
    - <code>service password-encryption</code>
    - <code>ip domain-name [nombre_dominio]</code>
    - <code>crypto key generate rsa [num_bits{1024 | 2048}]</code>
    - <code>username [name] secret [password]</code>
    - <code>line vty 0 15</code>
        - <code>transport input ssh</code>
        - <code>login local</code>
    - Conectar por ssh desde un pc:
        - <code>ssh -l [user] [ip_target]</code>

- # Port-Security

- Habilitar port-security:
    - <code>switchport port-security</code>
- Máximo de direcciones MAC permitidas:
    - <code>switchport port-security maximum [num]</code>
- Método de acción en caso de violación:
    - protect: Descarta los paquetes que provienen de direcciones MAC no autorizadas.
    - restrict: Descarta los paquetes y genera un registro de registro.
    - shutdown: Desactiva la interfaz si se detecta una violación.
    - <code>switchport port-security violation {protect | restrict | shutdown}</code>

- Direcciones MAC autorizadas:
    - Esto permite que la interfaz aprenda direcciones MAC automáticamente y las agregue a la configuración de seguridad.
    - <code>switchport port-security mac-address sticky </code>
    - Eliminar direcciones MAC aprendidas:
        - <code>switchport port-security mac-address sticky {address | dynamic}</code>
        - address: Elimina una dirección MAC específica.
        - dynamic: Elimina todas las direcciones MAC aprendidas.
- Bajar la seguridad de puertos:
    - no switchport port-security

# Comandos Port-Security
```
switchport port-security
switchport port-security maximum [num]
switchport port-security violation {protect | restrict | shutdown}
switchport port-security mac-address sticky
```
# OSPFv2 --------------- [(Referencia)](https://ccnadesdecero.com/curso/como-configurar-ospf/)

- Ten en cuenta que el uso de **default-information originate** puede tener un impacto en la topología y el enrutamiento de la red, ya que agrega una entrada de ruta por defecto en las tablas de enrutamiento de los routers OSPF. Es importante considerar cuidadosamente las implicaciones antes de usar este comando, especialmente en redes más grandes y complejas.
## Si quiero agregarle una loopback
```
- interface loopback [numero]
- ip address [ip] [mask]
```

- Router-id [id_router]: si no existe router id, automaticamente selecciona la ip más alta de las loopback, si no están configuradas toma la ip más alta de las interfaces.
- Do show ip ospf neighbor: para comprobar los vecinos a los cuales está conectado.
- Ip de loopback debe pertenecer a la network del ospf.

## Comandos OSPF

```
- router-id [id_router (ej: 1.1.1.1)]
- router ospf [ospf_process_id]
- network [network] [wildcard_mask] area [area(def: area 0)]
- default-information originate
- do show ip ospf neighbor
```

# Configuraciones adicionales de OSPFv2
- ## Interfaces pasivas:
    ```Cisco
    - router ospf [ospf_process_id]
    - passive-interface [intf]

    ---------en caso de querer dejar las intf pasivas por defecto y descontar algunas---------

    - passive-interface default (Dejar por defecto las interfaces pasivas)
    - no passive-interface [intf] (para hacerla no pasiva)
    ```
    - A veces, un router no necesita formar una relación de vecindad con vecinos en una interfaz. A menudo, no existen otros routeres en un enlace en particular, por lo que el router no tiene la necesidad de seguir enviando mensajes repetitivos de Hello OSPF. En estos casos puedes convertir la interfaz en pasiva, lo que significa que

        - OSPF continua anunciando acerca de las subredes que están conectadas a la interfaz.
        - OSPF no no envía paquetes Hellos OSPF en esa interfaz.
        - OSPF no continua procesando cualquier mensajes Hello en esta interfaz.

- ## Configurar rutas por defecto [(Referencia)](https://ccnadesdecero.com/curso/rutas-estaticas/)
    - <code>ip route 0.0.0.0 0.0.0.0 [sig_salto/interfaz_salida]</code>
- ## OSPF Hello and Dead interval
    - Dentro de una interfaz
        - <code>ip ospf hello-interval [num]</code>
        - <code>ip ospf dead-interval [num]</code>

# OSPFv3
-  Habilitar OSPFv3:
    - <code> ipv6 router ospf [PROCESO_OSPF_ID]</code>
- Configurar interfaces para OSPFv3
    - <code>interface [TIPO_INTERFAZ] [NOMBRE_INTERFAZ]</code>
        - <code>ipv6 enable</code>
        - <code>ipv6 address [DIRECCIÓN_IPV6] [MÁSCARA_PREFIX]</code>
        - <code>ipv6 ospf [PROCESO_OSPF_ID] area [NÚMERO_AREA]</code>
- Configurar router id (opcional):
    - <code>router-id [DIRECCIÓN_IPV6]</code>
    - ejemplo:
        - <code>router-id 2001:DB8::1</code>

# VTP (VLAN Trunking Protocol)
VTP se utiliza para propagar la información de VLAN a través de la red, lo que significa que cuando se configura una VLAN en un switch, esa información se propaga automáticamente a otros switches configurados en el mismo dominio VTP. Esto simplifica la administración de VLANs, ya que no es necesario configurar manualmente cada switch con la misma información de VLAN.

- Configurar dominio vtp:
    - <code>vtp domain [nombre]</code>
- Configura el modo de VTP:
    - <code>vtp mode {server | client | transparent}</code>
        - si es server, debera configurar las vlans normalmente para propagarlas.
        - server: El switch actuará como servidor VTP y puede crear, modificar y eliminar VLANs.
        - client: El switch actuará como cliente VTP y solo aceptará actualizaciones de VLAN desde servidores VTP.
        - transparent: El switch actuará en modo transparente, lo que significa que no participará en la propagación de información de VLAN, pero aún puede tener VLANs configuradas localmente.
- Configura la contraseña VTP (opcional):
    - <code>vtp password [password]</code>
- Configura la version:
    - <code>vtp version {1 | 2 | 3}</code>

# EIGRP [referencia](https://ccnadesdecero.es/protocolo-eigrp-definicion-y-caracteristicas/)
El protocolo de routing de gateway interior mejorado (EIGRP – Enhanced Interior Gateway Routing Protocol) es un protocolo de routing vector distancia avanzado desarrollado por Cisco.

- Configuracion

    - <code>router eigrp [eigrp_autonomous_process_num]</code>
    - <code>network [network + wildcard(netmask)]</code>
    - <code>interface [interface] </code>
        - <code>ip eigrp [eigrp_autonomous_process_num]</code>
    - <code>no auto-summary</code>

# EIGRPv6
- Configurar EIGRP para IPv6:
    - <code>ipv6 router eigrp [AS_NUMBER]</code>
- Configurar las interfaces para IPv6 y EIGRP:
    - <code>interface [TIPO_INTERFAZ] [NOMBRE_INTERFAZ]</code>
        - <code>ipv6 enable</code>
        - <code>ipv6 address [DIRECCIÓN_IPV6]/[MÁSCARA_PREFIX]</code>
        - <code>ipv6 eigrp [AS_NUMBER]</code>
- Configurar el Router ID (opcional):
    - <code>eigrp router-id [DIRECCIÓN_IPV6]</code>
- Verificar la configuración de EIGRP para IPv6:
    - <code>show ipv6 eigrp neighbors</code>
    
# CDP

- Habilitar CDP globalmente:
    - <code>cdp run</code>
<code></code>
- Limitar la publicación de CDP en interfaces específicas(Opcional):
    - <code>interface [TIPO_INTERFAZ] [NOMBRE_INTERFAZ]</code>
        - <code>cdp enable</code>
- Verificar la configuración de CDP:
    - <code>show cdp</code>

# Spanning Tree (STP)
El STP, definido por el estándar IEEE 802.1d es un protocolo que funciona en el nivel de la capa 2 del modelo OSI y su principal objetivo es controlar los enlaces redundantes, asegurando el rendimiento de una red.
Como ya se sabe, los switches no filtran los broadcasts y tal situación hace que todos los broadcasts recibidos a una interfaz de un switch sean enviados por otras interfaces, excepto por la interfaz que se ha recibido (flooding), creándose así una tormenta de difusión.
### ¿Cómo funciona el STP?
En términos generales, lo que el STP hace es eliminar lógicamente caminos de comunicación. Para ello el protocolo crea un árbol de switches presentes en la red y elige el switch de referencia, a partir del cual se creará el árbol.

La elección del root bridge es hecha con base en una prioridad y también con base en la dirección MAC. En una red sólo puede haber un root bridge.Considerando el ejemplo, SwitchA es elegido como root bridge (puente raíz), ya que es quien tiene menor prioridad (por defecto, la prioridad es 32768) y también la más bajo dirección física (dirección MAC).
![layers](https://ccnadesdecero.es/wp-content/uploads/2018/08/Elecci%C3%B3n-de-root-bridge-en-STP.jpg)
A continuación, cada switch, que no es root bridge, define lo que es el root port. Esta interfaz es elegida teniendo en cuenta el menor costo (teniendo como base el ancho de banda) para el root bridge. Esta interfaz se coloca en modo de enrutamiento.
Por cada segmento, se establece un designated bridge. Este será el switch con el menor costo hasta el root bridge (en el ejemplo a seguir es el SwitchD). La interfaz de conexión con el root bridge se encuentra en modo “reenvío”. El puerto del SwitchE se coloca en modo de bloqueo, por lo tanto, bloquea los frames y evitar los bucles (loops) en la red.

- Habilitar STP en la vlan
    - <code>interface vlan [vlan_num]</code>
    - <code>spanning-tree vlan [vlan_num] enable</code>
- Configurar el tipo de STP
    - <code>spanning-tree mode {pvst | rapid-pvst | mst}</code>
    - Modos: 
        - PVST (Per-VLAN Spanning Tree): Originalmente, Cisco desarrolló el PVST como una extensión propietaria del STP. Con PVST, se crea un árbol de expansión separado para cada VLAN en el switch. Esto significa que cada VLAN tiene su propia instancia de STP, lo que permite un mejor control y aislamiento de bucles en cada VLAN. PVST es específico de Cisco.

        - PVST+ (Per-VLAN Spanning Tree Plus): PVST+ es una mejora de PVST que es compatible con el estándar IEEE 802.1Q para VLAN. Con PVST+, se sigue utilizando un árbol de expansión separado para cada VLAN, pero es compatible con estándares VLAN como 802.1Q.

        - Rapid PVST (Rapid Per-VLAN Spanning Tree): Esta variante es una versión más rápida de PVST+. En lugar de esperar los 50 segundos típicos para la convergencia de STP original, Rapid PVST puede converger en cuestión de segundos. Cada VLAN todavía tiene su propio árbol de expansión.

        - MSTP (Multiple Spanning Tree Protocol): MSTP es una versión avanzada de STP que permite agrupar múltiples VLAN en el mismo árbol de expansión. En lugar de mantener un árbol separado por VLAN, como en PVST, MSTP agrupa las VLAN lógicamente y, por lo tanto, reduce la sobrecarga y la complejidad del árbol de expansión. Esto se logra mediante la configuración de múltiples instancias de MSTP, cada una de las cuales agrupa un conjunto de VLAN relacionadas.

# STPv6
- Configurar STP IPv6:
    - spanning-tree vlan [VLAN_ID] ipv6

- show spanning-tree vlan [VLAN_ID] ipv6

# BGP
BGP (Border Gateway Protocol) es un protocolo de enrutamiento exterior utilizado en internet y en redes empresariales para intercambiar información de enrutamiento entre sistemas autónomos (AS, Autonomous Systems). A diferencia de los protocolos de enrutamiento interior, como OSPF o EIGRP, que se utilizan para enrutamiento dentro de un mismo sistema autónomo, BGP se centra en el enrutamiento entre sistemas autónomos diferentes.

- Aquí están algunos conceptos clave y características de BGP:
    - Path Vector Protocol.
    - Políticas de Enrutamiento.
    - AS Path: BGP utiliza el atributo AS path para evitar bucles de enrutamiento y para garantizar que el tráfico no vuelva a entrar en el mismo AS.
    - Peering BGP: Los routers BGP se conectan a través de "peers" (pares) BGP. Los peers establecen conexiones TCP para intercambiar actualizaciones de enrutamiento. Los peers pueden ser proveedores de servicios, peers de pares de igual a igual (iBGP) o peers de sistemas autónomos diferentes.
    - BGP Route Reflectors y Confederaciones.
    - Filtros y Control de Tráfico.<br><br>
- Configura BGP: <hr>
    - <code>router bgp [número_de_AS]</code>
        - Donde número_de_AS es el número del sistema autónomo al que pertenece el router.
    - Configura el ID del router:
        - <code>bgp router-id dirección_IP</code>
    - Agrega redes que deseas anunciar:
        - <code>network dirección_red</code>
    - Configura los peers BGP:
        - <code>neighbor dirección_IP remote-as número_de_AS</code>
            - Aquí, dirección_IP es la dirección IP del router vecino y número_de_AS es el número del sistema autónomo vecino.


# Router On Stick:
router-on-a-stick es una configuración de red utilizada en entornos donde se necesita separar múltiples redes virtuales (VLANs) en un único enlace físico. Esta configuración es típicamente implementada en equipos de red de Cisco.
un único enlace físico entre un switch y un router es configurado como un enlace troncal (trunk link) capaz de transportar múltiples VLANs. El router se configura para tener subinterfaces, una para cada VLAN que necesita ser enrutada. Cada subinterfaz se asigna a una VLAN específica y tiene su propia dirección IP.


En la puerta del SW que da al ROUTER: previo haber configurado las vlans y haberlas agregado en mode trunk en la puerta que apunta al router (swport mode trunk -> swport trunk allowed vlan [num]). Comprobar(sh vlan brief)
- <code>switchport mode trunk</code>
- <code>switchport trunk allowed vlan [numeros de vlans ej: 10,14,15]
para agregar otra vlan</code>
- <code>switchport trunk allowed vlan add [vlan num]</code> para adicionar vlans a las ya existentes
- En la puerta del router que da al SW:
    - <code>int g0/1.[vlan_num]</code>  idealmente para llevar un orden, subinterface_num = vlan_num 
    - <code>encapsulation dot1q [vlan n°]</code>
    - <code>ip add [ip y mascara de gateway]</code>
    - <code>description "[vlan n°]"</code>

# PortChannel - EtherChannel [Ref](https://jmcristobal.com/es/2021/02/19/etherchannel-pagp-y-lacp/)
Un "Port Channel", también conocido como "Link Aggregation" o "EtherChannel", es una tecnología de redes que permite combinar múltiples enlaces físicos individuales entre dos dispositivos de red (como switches y servidores) en un solo enlace lógico de alta velocidad. Esto se hace para aumentar la capacidad de ancho de banda, mejorar la redundancia y balancear la carga en una red.

El Port Channel es especialmente útil en entornos donde se necesita más ancho de banda o tolerancia a fallos, pero donde instalar enlaces individuales de alta velocidad puede ser costoso o impráctico.

- ## Configurar PAgP:
- Entra a la interfaz:
    - <code>interface [tipo-interfaz][num-interfaz]</code>
    - <code>channel-protocol pagp</code>
- Configura el portchannel en la puerta:
    - <code>channel-group [portchannel-num] mode { auto | desirable }</code>
- Checkear el etherchannel:
    - <code>show etherchannel summary</code>

- ## Configurar LACP
- Define la prioridad (default 32768):
    - <code>lacp system-priority 4096</code>
- Agregar Portchannel a las interfaces:
    - <code>interface [tipo-interfaz][num-interfaz]</code>
    - <code>channel-protocol lacp</code>
    - Asigna el grupo de LACP:
        - <code>channel-group [num-lacp] mode { active | passive | on }</code>
        - <code>lacp port-priority [priority-num]</code>

    - Cada interfaz que necesitemos incluír a un EtherChannel en específico se debe asignar al mismo id de grupo. La negociación del canal debe estar on (canal sin negociación LACP), passive (escuchar pasivamente y esperar a ser preguntado), o active (preguntar activamente).

        ## Modos para PAgP y LACP
        ```
        Mode on: 
            Este modo se utiliza para forzar la agregación de enlaces sin la negociación de un protocolo de control (LACP o PAgP). Si configuras un Port Channel en modo "on", los enlaces se agregarán al Port Channel sin importar si el otro extremo está configurado en el mismo modo.

        Mode Active (LACP) o Desirable para (PAgP):
            En el caso de LACP, el modo "active" indica que el dispositivo enviará mensajes LACP para iniciar la formación del Port Channel. En el caso de PAgP, el modo "desirable" también indica que el dispositivo iniciará la formación del Port Channel.

        Mode Passive (LACP) o Auto para (PAgP):
            En el caso de LACP, el modo "passive" indica que el dispositivo espera a que el otro extremo inicie la formación del Port Channel. En el caso de PAgP, el modo "auto" también indica que el dispositivo esperará a que el otro extremo inicie la formación del Port Channel.
        ```
## Comandos verificación generales para los etherchannel:

- <code>show etherchhannel summary</code>
- <code>show etherchhannel port</code>
- <code>show etherchhannel detail</code>
- <code>show etherchhannel load-balance</code>
- <code>show pagp | lacp neighbor</code>
### Displays priority and MAC
- <code>show lacp sys-id</code> 


# IPV6
### Enable IPV6

- <code>ipv6 unicast-routing</code>
- <code>ipv6 address </code>

- ip1= 2001:10:10:10::<code>1</code>/64
- ip2= 2001:10:10:10::<code>2</code>/64

# ACL
## ACL Entrada
ACL de Entrada: Los paquetes entrantes se procesan antes de enrutarse a la interfaz de salida. Las ACL de entradas son eficientes, por que ahorran la sobrecarga de enrutar búsquedas si el paquete se descarta.
ACL de Salida: Los paquetes entrantes se enrutan a la interfaz de salida y después de procesan mediante la ACL de salida. Las ACL de salida son ideales cuando se aplica el mismo filtro a los paquetes que provienen de varias interfaces de entradas antes de salidas por la misma interfaz de salida.
## ACL Salida

Las ACL de entrada filtran los paquetes que ingresan a una interfaz específica y lo hacen antes de que se enruten a la interfaz de salida.
Las ACL de salida filtran los paquetes después de que se enrutan, independientemente de la interfaz de entrada.

las listas de acceso por defecto deniegan, permito y luego deniego <code>deny any</code>

## ACL Estándar
Se pueden utilizan para permitir o denegar el tráfico de direcciones IPv4 de origen únicamente. El destino del paquetes o puertos involucrados no se evalúan.<br>
<code>access-list [10] permit [network] [wildcard]</code>

## ACL Extendida
Este tipo de ACL filtran paquetes IPv4 según varios atributo tales como tipo de protocolo, dirección de origen, dirección de destino, puerto TCP o UDP de origen y puerto TCP o UDP de destino.<br>
<code>access-list [103] permit [protocol] [network] [wildcard] any eq [port_number]</code>

Las ACL estándar y extendidas se pueden crear con un número o un nombre para identificar la ACL y su lista de instrucciones.

## ACL Numerada
El uso de ACL numeradas es un método eficaz para determinar el tipo de ACL en redes pequeñas con tráfico definido de formas más homogénea. Sin embargo, un número no proporciona información sobre el propósito de la ACL. Por eso que también se pueden utilizar nombre para poder identificar una ACL de Cisco.Dentro de las ACL, existen dos palabras claves utilizadas, que es necesario saber su interpretación y uso, las cuales son any y host

## Palabras clave
- Any: Sirve para poder sustituir una dirección IPv4 0.0.0.0 con una wildcard 255.255.255.255.
- Host: Se utiliza para sustituir la wildcard de una sola dirección IP en una ACL.

Ejemplo: access-list 1 permit 0.0.0.0 255.255.255.255 es reemplazado por access-list 1 permit any
Ejemplo: access-list 1 permit 192.168.10.10 0.0.0.0 es reemplazada por access-list 1 permit host 192.168.10.10
<hr>

### - ACL standard solo tiene origen, se recomienda lo mas lejos del origen
### - ACL extendida puedo bloquear destino, se recomienda más cerca del origen

# Comandos ACL
## ACL Estándar
    ´´´
    access-list {numero} {permit|deny} {network} {wildcard}
    interface {tipo} {numero}
    ip access-group {numero} {in|out}

    ´´´
## ACL Estándar nombrada
    ´´´
    ip access-list standard {nombre}
    permit {network} {wildcard}
    deny {direccion} {wildcard}
    exit
    interface {tipo} {numero}
    ip access-group {nombre} {in|out}

## ACL Extendida
    ´´´
    ip access-list extended {nombre}
    permit {protocolo} {network-origen} {wildcard-origen} {destino} {wildcard-destino}
    deny {protocolo} {network-origen} {wildcard-origen} {destino} {wildcard-destino}
    exit
    interface {tipo} {numero}
    ip access-group {nombre} {in|out}
    

    ´´´
## ACL Extendida numerada no nombrada
    ´´´
    access-list {numero} {permit|deny} {protocolo} {red-origen} {wildcard-origen} {red-destino} {wildcard-destino} eq {puerto}
    interface {tipo} {numero}
    ip access-group {numero} {in|out}

    ´´´
## Estrategias
- Por defecto ACL luego de los statements, deniega todo el trafico con un deny any any implícito,
depende de lo que quiera hacer tengo dos estrategias:
    - Permitir solo lo que quiero y denegar el resto con deny any any
    - Denegar solo lo que quiero y permitir el resto con permit any any

    - <code>{permit|deny} any any</code>        
   
# NAT

### - Inside Local (IL):
    Definición: Es la dirección IP local (privada) de un dispositivo dentro de tu red local.
    Ejemplo: Si tienes un dispositivo con la dirección IP 192.168.1.10 dentro de tu red, entonces 192.168.1.10 sería la dirección inside local.

### - Inside Global (IG):
    Definición: Es la dirección IP global (pública) que representa a un dispositivo dentro de tu red local cuando se comunica con recursos en Internet.
    Ejemplo: Si tu router utiliza una única dirección IP pública 203.0.113.1 para representar a todos los dispositivos de tu red, entonces 203.0.113.1 sería la dirección inside global.

### - Outside Local (OL):
    Definición: Es la dirección IP local (privada) de un dispositivo fuera de tu red local, es decir, de un servidor o dispositivo en Internet.
    Ejemplo: Si estás accediendo a un servidor en Internet que tiene la dirección IP 216.58.211.110, entonces 216.58.211.110 sería la dirección outside local.

### - Outside Global (OG):
    Definición: Es la dirección IP global (pública) de un dispositivo fuera de tu red local. Esta es la dirección que utiliza Internet para comunicarse con dispositivos en tu red.
    Ejemplo: Si estás accediendo a un servidor en Internet que tiene una dirección IP pública de 172.217.21.174, entonces 172.217.21.174 sería la dirección outside global.


## NAT Dinámico
El NAT dinámico permite la asignación automática de direcciones locales internas a direcciones globales internas. Por lo general, estas direcciones globales internas son direcciones IPv4 públicas. El NAT dinámico utiliza un grupo o un conjunto de direcciones IPv4 públicas para la traducción.

    - Como configurar:
        - Crea un grupo de direcciones IP públicas (pool):
        Esto se hace para definir el conjunto de direcciones IP públicas que el router utilizará para asignar a los dispositivos internos cuando accedan a Internet.
        - Define una lista de acceso (ACL) para especificar qué direcciones IP privadas serán traducidas:
            Esto determinará qué tráfico será sometido a la traducción NAT.
        - Asocia el grupo de direcciones IP públicas (pool) con la lista de acceso
        - Configura la interfaz: ip nat [inside | outside]

## NAT PAT
PAT (también denominada “NAT con sobrecarga”) conserva las direcciones del conjunto de direcciones globales internas al permitir que el router use una dirección global interna para muchas direcciones locales internas. En otras palabras, se puede utilizar una única dirección IPv4 pública para cientos, incluso miles de direcciones IPv4 privadas internas.

    - Defina un conjunto de direcciones IPV4 publicas con un nombre
        - ip nat pool [nombre-pool] [ip-inicio] [ip-termino] netmask [máscara]
    - Defina las direcciones que se pueden traducir
        - access-list [numero-acl] permit [network] [wildcard]
    - Vincule [nombre-pool] a la ACL [numero-acl]
        - ip nat inside source static [dirección IP privada] [dirección IP pública]
        - ip nat inside source list [numero-acl] pool [nombre-pool]

<hr>

- inside: Se refiere a las direcciones IP de la red interna que quieres traducir.
- outside: Se refiere a las direcciones IP de la red externa (generalmente la dirección IP pública que tu ISP te proporciona).

## Comandos NAT
    - NAT Estático
        ´´´
        ip nat inside source static [dirección_interna] [dirección_externa]
        interface [tipo] [numero]
        ip nat [inside | outside]

        ´´´
    - NAT Dinámico
        ip nat pool [nombre-pool] [ip-inicio] [ip-termino] netmask [máscara]
        access-list [número] permit [dirección de origen] [máscara de subred]
        ip nat inside source list [número de ACL] pool [nombre del pool]
        interface [tipo] [número]
        ip nat [inside | outside]

    - NAT PAT
        ip nat pool [nombre-pool] [ip-inicio] [ip-termino] netmask [máscara]
        access-list [numero-acl] permit [network] [wildcard]
        ip nat inside source static [dirección IP privada] [dirección IP pública]
        ip nat inside source list [numero-acl] pool [nombre-pool]

# VPN
## Tunnel GRE

GRE se utiliza para crear un túnel VPN entre dos sitios, como se muestra enla figura. Para implementar un túnel GRE, el administrador de red primero debe descubrir las direcciones IP de las terminales.

 ## Comandos tunnel GRE

    interface Tunnel[int_num]
        tunnel mode gre ip
        ip address [tunnel_net_ip] [tunnel_mask_ip]
        tunnel source [public_ip_source]
        tunnel destination [public_ip_destination]
    show interface tunnel [numero de interfaz]
    ip route [tunnel_destino_ip] [mask] Tunnel[int_num]

### Shows
- show ip interface brief | include Tunnel
- show interface Tunnel [int_num]

## Tunnel IPSec
Un túnel IPsec (Protocolo de Seguridad de la Capa de Internet) en Cisco se utiliza para establecer una conexión segura entre dos puntos finales a través de Internet u otra red no confiable. IPsec proporciona autenticación y cifrado para proteger el tráfico de red entre los puntos finales del túnel. Aquí hay una explicación básica de cómo configurar un túnel IPsec en dispositivos Cisco.
Hay dos fases en la negociación de un túnel IPsec: Fase 1 e Fase 2.
En la Fase 1, se establece una asociación de seguridad inicial y se negocian los parámetros de seguridad. Aquí es donde se utilizan las claves precompartidas (PSK) o certificados RSA para autenticar y asegurar la comunicación entre los puntos finales.

- Claves Precompartidas (PSK): La PSK es simplemente una clave compartida entre los dos puntos finales del túnel IPsec. Ambos puntos finales deben conocer esta clave y usarla para autenticar uno al otro durante la negociación de la Fase 1. La ventaja de la PSK es su simplicidad y facilidad de implementación, pero se debe tener cuidado de mantenerla segura.
- Certificados RSA: En lugar de una clave precompartida, también puedes utilizar certificados RSA para autenticar los extremos. Esto implica el uso de certificados digitales, donde cada extremo del túnel tiene su propio par de claves (pública y privada), y se intercambian certificados para establecer la confianza mutua.

En la Fase 2, se negocian parámetros de seguridad específicos para el tráfico que será encapsulado. Aquí se especifican los algoritmos de cifrado y autenticación que se utilizarán. No se utilizan PSK o certificados RSA en esta fase.

Para la configuración de una configuración de una VPN Site-to-Site utilizando Cisco IOSson necesarios los siguientes 5 pasos:
1. Configuración de la política ISAKMP (IKE Fase 1).
2. Configuración de los IPsec transform sets (IKE Fase 2, terminación del túnel).
3. Configuración de la crypto ACL (tráfico interesante, transferencia segura de datos).
4. Configuración del crypto map (IKE Fase 2).
5. Aplicación del crypto map a la interfaz (IKE Fase 2).

## Comandos Tunel IPSec

# Activar Licencia para el tunel
	license boot level securityk9

- Slide 15 ppt 3.2.1
### Paso 1
Cada paso se repite en el router de destino

    crypto isakmp policy 110
        authentication pre-share
        encryption [algoritmo: aes, 3des, des]
        group [numero de grupo: 1, 2, 5, 14, 19] # mientras mas bajo mas debil, mas alto mas fuerte, pero tiene mayor costo normalmente se ocupa el 2 que es comun y aceptable
        hash [sha, sha256, sha384, md5]
        lifetime 43200

### Paso 2
Cada paso se repite en el router de destino

    crypto isakmp key [clave-compartida(ej: cisco123)] address [public_ip_destination]
    crypto ipsec transform-set MYSET esp-aes 128
        exit
    access-list 110 permit tcp [net_saliente] [wildcard] [net_destino] [wildcard]
    crypto map MYMAP 10 ipsec-isakmp
        match address 110
        set peer [router_destino] default
        set peer [router_destino]
        set pfs group1
        set transform-set nine
        set security-association lifetime seconds 86400
        crypto map [map-name]
        interface [type and number]
            crypto map MYMAP
    show crypto isakmp sa
    show crypto ipsec sa

## CDP discovery
    show cdp
    no cdp run  #Deshabilitar cdp
    cdp run #Habilitar cdp
    interface [tipo] #Habilitar cdp en la interfaz
        cdp enable
    show cdp neighbors

## LLDP
    lldp run #Habilitar lldp
    interface [tipo y num]
        lldp transmit
        lldp receive
        end
    show lldp
    show lldp neighbors

## NTP Clock
    show clock detail
    ntp server [ip_server]
    show ntp associations
    show ntp status

## SNMP



<br><br><br><br><br><br>
<h1 align="center"><strong> Created by <a href="https://github.com/keaguirre">keaguirre</strong></h1>
