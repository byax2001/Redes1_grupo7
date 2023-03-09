## **Tabla 1.0**
| **VLAN** | **#VLAN** | **Dirección de Red** | **Gateway** |
| :-- | :-- | :-- |:-- |
| RRHH | 10 | 192.168.71.0/24 | 192.168.71.1 |
| Informatica | 20 | 192.168.72.0/24 | 192.168.72.1 |
| Contabilidad | 30 | 192.168.73.0/24 | 192.168.73.1 |
| Ventas | 40 | 192.168.74.0/24 | 192.168.74.1 |

Nota: /24 es una notación de máscara subred. Tome en cuenta que esto es quivalente a 255.255.255.0.


## **DANNY (Area de Trabajo)**
~~~cli

~~~


## **BRANDON (Backbone)**
~~~cli

~~~


## **JAVSKOW (Zona de Servidores)**
~~~bash
# Configuración de puertos ACCESS
conf t
int f1/2
switchport mode access
end

# Configuración de puertos TRUNK
conf t
int f1/0
switchport mode trunk
switchport trunk allowed vlan 1,10,20,30,1002-1005
end
sh int tr

# Configuración de VTPS
conf t
vtp domain GRUPO7
vtp password grupo7
vtp version 2
vtp mode client
end
sh vtp status 
~~~
IP OpenVPN: 10.8.0.2

Clases:
- 18/02/2023 (VTP)