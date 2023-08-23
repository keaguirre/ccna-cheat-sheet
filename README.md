# CCNA Cheetsheet
**Usa este material bajo tu propio riesgo, estos apuntes pueden contener tener errores y solo son de referencia y/o apoyo para estudio y/o ejecucion de ejercicios**

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
- <code> show ip ospf interface </code>
- <code> show ip ospf interface [intf] </code>
- <code> show ip ospf interface brief</code>
- Vecinos: <code>do show ip ospf neighbor</code>
- Inundación de LSAs: <code>show ip ospf </code>database: 
- calculo de spf: <code>show ip ospf rib</code>
- Distancia Administrativa:
    - <code>show ip route </code>:<br>
    En la salida del comando show ip route puedes ver marcadas las rutas que fueron agregadas por OSPF, estas se identifican por la 'O'.
    - <code>show ip route ospf </code><br>
    También puedes ver que la distancia administrativa es 110 [110/2]. El 2 [110/2] hace referencia a la métrica de OSPF para esta ruta.

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
    ```
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

<br><br><br><br><br><br>
<h1 align="center"><strong> Created by <a href="https://github.com/keaguirre">keaguirre</strong></h1>