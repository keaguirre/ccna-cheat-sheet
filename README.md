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
- <code> show interfaces port-channel [port_channel_num]</code>

- Vecinos: <code>do show ip ospf neighbor</code>
- Inundación de LSAs: <code>show ip ospf </code>database: 
- calculo de spf: <code>show ip ospf rib</code>
- Distancia Administrativa:
    - <code>show ip route </code>:<br>
    En la salida del comando show ip route puedes ver marcadas las rutas que fueron agregadas por OSPF, estas se identifican por la 'O'.
    - <code>show ip route ospf </code><br>
    También puedes ver que la distancia administrativa es 110 [110/2]. El 2 [110/2] hace referencia a la métrica de OSPF para esta ruta.

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

# OSPFv2 --------------- [(Referencia)](https://ccnadesdecero.com/curso/como-configurar-ospf/)
```
- router-id [id_router (ej: 1.1.1.1)]
- router ospf [ospf_process_id]
- network [network] [wildcard_mask] area [area(def: area 0)]
- default-information originate
- do show ip ospf neighbor
```
- Ten en cuenta que el uso de **default-information originate** puede tener un impacto en la topología y el enrutamiento de la red, ya que agrega una entrada de ruta por defecto en las tablas de enrutamiento de los routers OSPF. Es importante considerar cuidadosamente las implicaciones antes de usar este comando, especialmente en redes más grandes y complejas.
## Si quiero agregarle una loopback
```
- interface loopback [numero]
- ip address [ip] [mask]
```

- Router-id [id_router]: si no existe router id, automaticamente selecciona la ip más alta de las loopback, si no están configuradas toma la ip más alta de las interfaces.
- Do show ip ospf neighbor: para comprobar los vecinos a los cuales está conectado.
- Ip de loopback debe pertenecer a la network del ospf.

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

# EIGRP [referencia](https://ccnadesdecero.es/configuracion-eigrp-con-ipv4-ejemplo/)
EIGRP es un protocolo de enrutamiento híbrido porque combina elementos de protocolos de enrutamiento de vector de distancia (como RIP) y protocolos de enrutamiento de estado de enlace (como OSPF). Esto significa que EIGRP puede adaptarse a diferentes topologías de red y proporcionar un enrutamiento rápido y eficiente.
Es un protocolo propietario de cisco por lo que solo se puede implementar en hardware cisco a diferencia de OSPF.

- Configuracion

    - <code>router eigrp [autonomous_process_num]</code>
    - <code>network [network]</code>
    - <code>no auto-summary</code>
# Falta por completar EIGRP
    
# BGP
BGP (Border Gateway Protocol) es un protocolo de enrutamiento exterior utilizado en internet y en redes empresariales para intercambiar información de enrutamiento entre sistemas autónomos (AS, Autonomous Systems). A diferencia de los protocolos de enrutamiento interior, como OSPF o EIGRP, que se utilizan para enrutamiento dentro de un mismo sistema autónomo, BGP se centra en el enrutamiento entre sistemas autónomos diferentes.

- Aquí están algunos conceptos clave y características de BGP:
    - Path Vector Protocol.
    - Políticas de Enrutamiento.
    - AS Path: BGP utiliza el atributo AS path para evitar bucles de enrutamiento y para garantizar que el tráfico no vuelva a entrar en el mismo AS.
    - Peering BGP: Los routers BGP se conectan a través de "peers" (pares) BGP. Los peers establecen conexiones TCP para intercambiar actualizaciones de enrutamiento. Los peers pueden ser proveedores de servicios, peers de pares de igual a igual (iBGP) o peers de sistemas autónomos diferentes.
    - BGP Route Reflectors y Confederaciones.
    - Filtros y Control de Tráfico.

### Config ------>
- Configura BGP:
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
En la puerta del SW que da al ROUTER: previo haber configurado las vlans y haberlas agregado en mode access a las puertas(swport access vlan [num]). Comprobar(sh vlan brief)
- #switchport mode trunk
- #switchport trunk allowed vlan [numeros de vlans ej: 10,14,15]
para agregar otra vlan
- #switchport trunk allowed vlan add [vlan num]
En la puerta del router que da al SW:
- #int g0/1.10     -----> idealmente para llevar un orden, poner .[n° vlan]
- #encapsulation dot1q [vlan n°]
- #ip add [ip y mascara de gateway]
- #description "[vlan n°]"

# PortChannel
Un "Port Channel", también conocido como "Link Aggregation" o "EtherChannel", es una tecnología de redes que permite combinar múltiples enlaces físicos individuales entre dos dispositivos de red (como switches y servidores) en un solo enlace lógico de alta velocidad. Esto se hace para aumentar la capacidad de ancho de banda, mejorar la redundancia y balancear la carga en una red.

El Port Channel es especialmente útil en entornos donde se necesita más ancho de banda o tolerancia a fallos, pero donde instalar enlaces individuales de alta velocidad puede ser costoso o impráctico.

- Crea el port-Channel:
    - <code>interface port-channel [port-channel_num]</code>
- Configura el modo de operación LACP:
    - <code>channel-group [port-channel_num] mode active</code>
        - Establece el modo LACP como "active". Este modo permitirá que el switch envíe y reciba mensajes LACP para formar el Port Channel.
        
        ```
        Mode on: 
            Este modo se utiliza para forzar la agregación de enlaces sin la negociación de un protocolo de control (LACP o PAgP). Si configuras un Port Channel en modo "on", los enlaces se agregarán al Port Channel sin importar si el otro extremo está configurado en el mismo modo.

        Mode Active (LACP) o Desirable (PAgP):
            En el caso de LACP, el modo "active" indica que el dispositivo enviará mensajes LACP para iniciar la formación del Port Channel. En el caso de PAgP, el modo "desirable" también indica que el dispositivo iniciará la formación del Port Channel.

        Mode Passive (LACP) o Auto (PAgP):
            En el caso de LACP, el modo "passive" indica que el dispositivo espera a que el otro extremo inicie la formación del Port Channel. En el caso de PAgP, el modo "auto" también indica que el dispositivo esperará a que el otro extremo inicie la formación del Port Channel.
        ```
- Agrega los enlaces físicos al Port Channel:
    - <code>interface range gigabitethernet [int_inicio] - [int_fin] channel-group [port-channel_num] mode active</code>








# To do
- Terminar EIGRP
- CDP

<br><br><br><br><br><br>
<h1 align="center"><strong> Created by <a href="https://github.com/keaguirre">keaguirre</strong></h1>
