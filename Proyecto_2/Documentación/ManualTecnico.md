<<<<<<< Updated upstream
# **MANUAL TÉCNICO - GRUPO 7**
## **Integrantes**
| **CARNET** | **NOMBRE** | 
|:-: |:-- |
| 201807428 | Wilson Kevin Javier Chávez Cabrera |
| 201709528 | Danny Eduardo Cuxum Sánchez  |
| 201800534 | Brandon Oswaldo Yax Campos  |


## **Tabla Topologia 1**
| **Direccion de Red ** | **#Mascara de Red** | **Primera IP Asignable ** | **Ultima IP Asignable** | **Direccion de BROADCAST** | ** Total Host** | **Cantidad de Host **|
| :-- | :-- | :-- |:-- | :-- | :-- |:-- |
| 192.168.75.0 | 255.255.255.248 ó /29 | 192.168.75.1 | 192.168.75.6 |192.168.75.7|8|7 |

## **Asignacion de IP**

| DISPOSITIVO | INTERFAZ | DIRECCIÓN IP | MASCARA DE RED | GATEWAY       |
| ----------- | -------- | ------------| --------------| -------------|
| R1          | F3/0     | 192.168.47.1| 255.255.255.240 ó /28 | N/A |
| PC1         | E0       | 192.168.47.2| 255.255.255.240 ó /28 | 192.168.47.1 |
| PC2         | E1       | 192.168.47.3| 255.255.255.240 ó /28 | 192.168.47.1 |
| PC3         | E2       | 192.168.47.4| 255.255.255.240 ó /28 | 192.168.47.1 |
| PC4         | E3       | 192.168.47.5| 255.255.255.240 ó /28 | 192.168.47.1 |
| PC5         | E4       | 192.168.47.6| 255.255.255.240 ó /28 | 192.168.47.1 |
| PC6         | E5       | 192.168.47.7| 255.255.255.240 ó /28 | 192.168.47.1 |


=======
>>>>>>> Stashed changes
EXTRAS:
~~~bash
sh ip ro

sh standby
sh standby brie

sh glbp
sh glbp brief

show etherchannel port-channel
show etherchannel summary

sh vlan-sw
~~~

---

## TOPO 1

R1:
~~~bash
conf t
int f0/0
ip address 10.7.0.9 255.255.255.248
no shut
exit
int f1/0
ip address 10.7.0.1 255.255.255.252
no shut
exit

router rip
version 2
network 10.7.0.0
network 10.7.0.8
exit

int f0/0
glbp 1 ip 10.7.0.14
glbp 1 preempt
glbp 1 priority 150
glbp 1 load-balancing round-robin
end

copy running-config startup-config
~~~

R2:
~~~bash
conf t
int f0/0
ip address 10.7.0.10 255.255.255.248
no shut
exit
int f1/0
ip address 10.7.0.5 255.255.255.252
no shut
exit

router rip
version 2
network 10.7.0.4
network 10.7.0.8
exit

int f0/0
glbp 1 ip 10.7.0.14
glbp 1 load-balancing round-robin
end

copy running-config startup-config
~~~

R3:
~~~bash
conf t
int f0/0
ip address 10.7.0.11 255.255.255.248
no shut
exit
int f1/0
ip address 10.7.0.17 255.255.255.252
no shut
exit

router ospf 1
network 10.7.0.16 0.0.0.3 area 1
exit

router rip
version 2
network 10.7.0.8
exit

router rip
version 2
redistribute ospf 1 metric 1
exit

router ospf 1
redistribute rip subnets
exit

int f0/0
standby 1 ip 10.7.0.13
standby 1 priority 150
standby 1 preempt
end

copy running-config startup-config
~~~

R4:
~~~bash
conf t
int f0/0
ip address 10.7.0.12 255.255.255.248
no shut
exit
int f1/0
ip address 10.7.0.21 255.255.255.252
no shut
exit

router ospf 1
network 10.7.0.20 0.0.0.3 area 1
exit

router rip
version 2
network 10.7.0.8
exit

router rip
version 2
redistribute ospf 1 metric 1
exit

router ospf 1
redistribute rip subnets
exit

int f0/0
standby 1 ip 10.7.0.13
end

copy running-config startup-config
~~~

---

## TOPO 2

R1:
~~~bash
conf t
int f0/0
ip address 10.7.0.2 255.255.255.248
no shut
exit
int f1/0
ip address 10.7.0.6 255.255.255.252
no shut
exit
int f3/0
ip address 192.168.47.1 255.255.255.240
no shut
exit

router rip
version 2
network 10.7.0.0
network 10.7.0.4
network 192.168.47.0
end

copy running-config startup-config
~~~

ESW1:
~~~bash
conf t
vtp domain redes1gp7grupo
vtp password redes1gp7grupo
vtp version 2
vtp mode server

int range f1/0 - 4
switchport mode trunk
switchport trunk allowed vlan 1,10,20,30,40,1002-1005
exit

vlan 10
name RHUMANOS
exit
vlan 20
name CONTABILIDAD
exit
vlan 30
name VENTAS
exit
vlan 40
name INFORMATICA
exit

int range f1/1 - 2
channel-group 1 mode on
exit
int range f1/3 - 4
channel-group 2 mode on
exit

spanning-tree vlan 1 root primary
spanning-tree vlan 10 root primary
spanning-tree vlan 20 root primary
spanning-tree vlan 30 root primary
spanning-tree vlan 40 root primary
end

copy running-config startup-config
~~~

ESW2:
~~~bash
conf t
vtp domain redes1gp7grupo
vtp password redes1gp7grupo
vtp version 2
vtp mode client

int range f1/0 - 8
switchport mode trunk
switchport trunk allowed vlan 1,10,20,30,40,1002-1005
exit

int range f1/0 - 1
channel-group 1 mode on
exit
int range f1/2 - 4
channel-group 3 mode on
exit
int range f1/5 - 6
channel-group 4 mode on
end

copy running-config startup-config
~~~

ESW3:
~~~bash
conf t
vtp domain redes1gp7grupo
vtp password redes1gp7grupo
vtp version 2
vtp mode client

int range f1/0 - 8
switchport mode trunk
switchport trunk allowed vlan 1,10,20,30,40,1002-1005
exit

int range f1/0 - 1
channel-group 2 mode on
exit
int range f1/2 - 4
channel-group 3 mode on
exit
int range f1/5 - 6
channel-group 5 mode on
end

copy running-config startup-config
~~~

ESW4:
~~~bash
conf t
vtp domain redes1gp7grupo
vtp password redes1gp7grupo
vtp version 2
vtp mode client

int range f1/0 - 5
switchport mode trunk
switchport trunk allowed vlan 1,10,20,30,40,1002-1005
exit

int range f1/0 - 1
channel-group 4 mode on
exit
int range f1/2 - 3
channel-group 5 mode on
end

copy running-config startup-config
~~~

PC1:
~~~bash
ip 192.168.47.2/28 192.168.47.1
save
~~~

PC2:
~~~bash
ip 192.168.47.3/28 192.168.47.1
save
~~~

PC3:
~~~bash
ip 192.168.47.4/28 192.168.47.1
save
~~~

PC4:
~~~bash
ip 192.168.47.5/28 192.168.47.1
save
~~~

PC5:
~~~bash
ip 192.168.47.6/28 192.168.47.1
save
~~~

PC6:
~~~bash
ip 192.168.47.7/28 192.168.47.1
save
~~~

---

## TOPO 3