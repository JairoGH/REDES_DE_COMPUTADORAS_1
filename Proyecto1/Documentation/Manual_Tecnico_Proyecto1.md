# Manual Tecnico - Proyecto 1: NetCore Academy

**Nombre:** Jairo Adelso Gomez Hernandez  
**Carnet:** 201902672  
**Curso:** Redes de Computadoras 1  
**Repositorio:** `Redes1_1S_2026_201902672/Proyecto1`  
**Entorno:** Cisco Packet Tracer

---

## 1. Resumen Ejecutivo

Este manual documenta el diseno, implementacion y validacion de una red LAN de capa 2 para NetCore Academy, aplicando VLANs, VTP, STP y EtherChannel conforme al enunciado del Proyecto 1.

El diseno se basa en una arquitectura por edificios (A, B, C y D), con enlaces de fibra para interconexion, troncales 802.1Q para transporte de VLANs y enlaces de acceso para dispositivos finales.

---

## 2. Parametros Asignados por Carnet

Para el carnet `201902672` (X = 2, # = 7):

| Parametro | Valor |
|---|---|
| VLAN Administracion | 12 (`ADMIN`) |
| VLAN Docencia | 22 (`DOCENTES`) |
| VLAN Biblioteca | 32 (`BIBLIOTECA`) |
| VLAN Laboratorio | 42 (`LABORATORIO`) |
| VLAN Visitantes | 52 (`VISITANTE`) |
| Red base de trabajo | `192.168.12.0/24` |
| Dominio VTP | `C7_NetCore` |
| Password VTP | `proyecto12026` |
| Version VTP | 2 |
| STP (carnet par) | PVST |
| Root Bridge | `SW-A1` |
| EtherChannel inter-edificios | LACP |
| EtherChannel intra-edificio | PAgP |

---

## 3. Capturas de Topologia

### 3.1 Topologia completa del campus

![Topologia completa](./img/topologia_completa.png)

### 3.2 Topologia por edificio/area

#### **Edificio A**  


![Edificio A](./img/edificio_a.png)

#### **Edificio B**  

![Edificio B](./img/edificio_b.png)

#### **Edificio C**  

![Edificio C](./img/edificio_c.png)

#### **Edificio D**  

![Edificio D](./img/edificio_d.png)

---

## 4. Topologia Etiquetada (Interfaces y Medios por Segmento)

### 4.1 Criterio de etiquetado

- `FFE`: Fibra optica OM3 (100Base-FX), usada en interconexion de edificios y EtherChannel inter-edificios.
- `Gi`: UTP Cat6 GigabitEthernet, usada en distribucion y algunos uplinks internos.
- `Fa`: UTP Cat5e/Cat6 FastEthernet, usada en acceso a usuarios, hubs, repetidor y APs.

### 4.2 Tabla de segmentos principales

| Segmento | Medio | Interfaces esperadas | Proposito |
|---|---|---|---|
| SW-A1 <-> SW-B1 | Fibra OM3 | FFE x2 + Port-Channel | Backbone inter-edificios |
| SW-A1 <-> SW-C4 | Fibra OM3 | FFE x2 + Port-Channel | Backbone inter-edificios |
| SW-C4 <-> SW-D1 | Fibra OM3 | FFE x2 + Port-Channel | Backbone inter-edificios |
| SW-D5 <-> SW-B2 | Fibra OM3 | FFE x2 + Port-Channel | Backbone inter-edificios |
| SW-B1 <-> SW-B2 | Fibra OM3 | FFE x2 + Port-Channel | Redundancia edificio B |
| SW-D1 <-> SW-D5 | Fibra OM3 | FFE trunk | Redundancia edificio D |
| SW-A1 <-> SW-A2/SW-A3 | UTP Cat6 | Gi trunk | Distribucion edificio A |
| SW-B1 <-> SW-B3/SW-B4 | UTP Cat6 | Gi trunk | Distribucion edificio B |
| SW-D5 <-> SW-D2 | UTP Cat6 | Gi trunk | Distribucion edificio D |
| SW-C4 <-> Hub-C1 | UTP Cat5e/Cat6 | Fa trunk | Segmento compartido edificio C |
| Hub-C1 <-> SW-C1/C2/C3 | UTP Cat5e/Cat6 | Fa | Acceso edificio C |
| SW-D2 <-> Repeater-D1 <-> SW-D3 | UTP Cat5e/Cat6 | Fa | Extension de medio compartido |

### 4.3 Evidencia
#### Topologia Etiquetada
![Topologia etiquetada](./img/topologia_completa.png)

---

## 5. Tabla de Dominios de Colision por Area

> Nota: cada puerto de switch representa un dominio de colision inde. Un hub/repeater comparte un mismo dominio de colision entre todos sus puertos.

| Area | Equipos relevantes | Dominios de colision (cantidad) | Cuales son |
|---|---|---:|---|
| Edificio A | SW-A1, SW-A2, SW-A3, AP | 7 | 2 enlaces A1-A2/A3, 2 enlaces EtherChannel A2-A3, 2 enlaces A1 hacia edificios (por cada canal fisico), 1 enlace AP |
| Edificio B | SW-B1, SW-B2, SW-B3, SW-B4, Hub-B1 | 10 | Enlaces troncales y EtherChannel internos; + 1 dominio compartido en Hub-B1 |
| Edificio C | SW-C4, Hub-C1, SW-C1/C2/C3 | 2 logicos principales | 1 enlace SW-C4 a Hub-C1 + 1 dominio compartido de Hub-C1 con SW-C1/C2/C3 |
| Edificio D | SW-D1, SW-D5, SW-D2, SW-D3, SW-D4, SW-E1, Repeater | 11 | Troncales internos y EtherChannel + 1 dominio compartido del tramo con repetidor |
| Backbone campus | A1-B1, A1-C4, C4-D1, D5-B2, D1-D5 | 9 | 4 EtherChannels (8 enlaces fisicos) + 1 trunk FFE |


![Dominios colision - 1](./img/dominios_colision_1.png)
---
![Dominios colision - 2](./img/dominios_colision_2.png)
---

## 6. Tabla de Dominios de Broadcast (Una por VLAN)

Sin enrutamiento inter-VLAN, cada VLAN es un dominio de broadcast separado.

| VLAN ID | Nombre | Dominio de Broadcast | Alcance esperado |
|---:|---|---|---|
| 12 | ADMIN | 1 | Todos los puertos/access VLAN 12 en campus |
| 22 | DOCENTES | 1 | Todos los puertos/access VLAN 22 en campus |
| 32 | BIBLIOTECA | 1 | Todos los puertos/access VLAN 32 en campus |
| 42 | LABORATORIO | 1 | Todos los puertos/access VLAN 42 en campus |
| 52 | VISITANTE | 1 | Puertos VLAN 52, incluyendo SW-E1 (transparent) |

Total dominios de broadcast en el proyecto: **5**.

---

## 7. Tabla de VLANs (ID y Nombre)

| VLAN ID | Nombre | Uso |
|---:|---|---|
| 12 | ADMIN | Personal administrativo |
| 22 | DOCENTES | Aulas y docencia |
| 32 | BIBLIOTECA | Recursos de biblioteca |
| 42 | LABORATORIO | Laboratorio de redes |
| 52 | VISITANTE | Equipos de visitantes |

---

## 8. Lista de Comandos Utilizados por Dispositivo

### 8.1 Base comun (todos los switches)

```bash
enable
configure terminal
hostname SW-XX
no ip domain-lookup
enable secret class
service password-encryption
line console 0
 password cisco
 login
 exit
line vty 0 4
 password cisco
 login
 exit
spanning-tree mode pvst
end
write memory
```

### 8.2 SW-A1 (Server, Root, creacion de VLANs)

```bash
vtp mode server
vtp domain C7_NetCore
vtp version 2
vlan 12
 name ADMIN
vlan 22
 name DOCENTES
vlan 32
 name BIBLIOTECA
vlan 42
 name LABORATORIO
vlan 52
 name VISITANTE
spanning-tree vlan 12,22,32,42,52 priority 4096
```

### 8.3 SW clientes VTP (A2, A3, B1, B2, B3, B4, C1, C2, C3, C4, D1, D2, D3, D4, D5)

```bash
vtp mode client
vtp domain C7_NetCore
vtp version 2
```

### 8.4 SW-E1 (VTP transparente)

```bash
vtp mode transparent
vtp domain C7_NetCore
vtp version 2
vlan 52
 name VISITANTE
```

### 8.5 Troncales 802.1Q (patron)

```bash
interface <PUERTO_TRUNK>
 switchport mode trunk
 switchport trunk allowed vlan 12,22,32,42,52
 no shutdown
```

### 8.6 EtherChannel inter-edificios (LACP)

```bash
interface range <P1>,<P2>
 switchport mode trunk
 switchport trunk allowed vlan 12,22,32,42,52
 channel-group <N> mode active
 no shutdown
exit
interface port-channel <N>
 switchport mode trunk
 switchport trunk allowed vlan 12,22,32,42,52
 no shutdown
```

### 8.7 EtherChannel intra-edificio (PAgP)

```bash
interface range <P1>,<P2>
 switchport mode trunk
 switchport trunk allowed vlan 12,22,32,42,52
 channel-group <N> mode desirable
 no shutdown
exit
interface port-channel <N>
 switchport mode trunk
 switchport trunk allowed vlan 12,22,32,42,52
 no shutdown
```

### 8.8 Puertos de acceso (patron)

```bash
interface <PUERTO_ACCESS>
 switchport mode access
 switchport access vlan <12|22|32|42|52>
 spanning-tree portfast
 no shutdown
```

### 8.9 Comandos de verificacion usados

```bash
show running-config
show vlan brief
show vtp status
show interfaces trunk
show etherchannel summary
show spanning-tree
show spanning-tree vlan 12
show spanning-tree vlan 22
show spanning-tree vlan 32
show spanning-tree vlan 42
show spanning-tree vlan 52
show interfaces status
show mac address-table
```

---

## 9. Tabla de IPs Asignadas por Dispositivo

Mascara en todos los hosts: `255.255.255.0`  
Gateway: vacio (proyecto capa 2 sin routing inter-VLAN)

### 9.1 VLAN 12 - ADMIN

| Dispositivo | IP |
|---|---|
| Laptop-PT Admin 1 | 192.168.12.11 |
| PC-PT Admin 2 | 192.168.12.12 |
| PC-PT Admin 3 | 192.168.12.13 |
| PC-PT Admin 4 | 192.168.12.14 |
| PC-PT Admin 5 | 192.168.12.15 |

### 9.2 VLAN 22 - DOCENTES

| Dispositivo | IP |
|---|---|
| Smartphone-PT Docencia 1 | 192.168.12.21 |
| Laptop-PT Docencia 2 | 192.168.12.22 |
| Laptop-PT Docencia 3 | 192.168.12.23 |
| Laptop-PT Docencia 6 | 192.168.12.24 |
| PC-PT Docencia 7 | 192.168.12.25 |
| PC-PT Docencia 8 | 192.168.12.26 |
| PC-PT Docencia 9 | 192.168.12.27 |
| Server-PT Docencia 10 | 192.168.12.28 |

### 9.3 VLAN 32 - BIBLIOTECA

| Dispositivo | IP |
|---|---|
| PC-PT Biblioteca 1 | 192.168.12.31 |
| Laptop-PT Biblioteca 2 | 192.168.12.32 |
| PC-PT Biblioteca 3 | 192.168.12.33 |
| PC-PT Biblioteca 4 | 192.168.12.34 |
| PC-PT Biblioteca 5 | 192.168.12.35 |
| PC-PT Biblioteca 6 | 192.168.12.36 |
| PC-PT Biblioteca 7 | 192.168.12.37 |

### 9.4 VLAN 42 - LABORATORIO

| Dispositivo | IP |
|---|---|
| PC-PT Laboratorio 2 | 192.168.12.41 |
| PC-PT Laboratorio 4 | 192.168.12.42 |
| PC-PT Laboratorio 3 | 192.168.12.43 |

### 9.5 VLAN 52 - VISITANTES

| Dispositivo | IP |
|---|---|
| PC-PT Visitantes 1 | 192.168.12.51 |
| PC-PT Visitantes 2 | 192.168.12.52 |
| PC-PT Visitantes 3 | 192.168.12.53 |

---

## 10. Pruebas de Ping (2 por VLAN)


| VLAN | Tipo | Origen | Destino | Resultado esperado | Evidencia |
|---|---|---|---|---|---|
| 12 ADMIN | Exitoso | 192.168.12.11 | 192.168.12.12 | Success | [Ver captura](./img/ping_vlan12_ok.png) |
| 12 ADMIN | Fallido | 192.168.12.11 | 192.168.12.31 | Fail | [Ver captura](./img/ping_vlan12_fail.png) |
| 22 DOCENTES | Exitoso | 192.168.12.21 | 192.168.12.22 | Success | [Ver captura](./img/ping_vlan22_ok.png) |
| 22 DOCENTES | Fallido | 192.168.12.21 | 192.168.12.41 | Fail | [Ver captura](./img/ping_vlan22_fail.png) |
| 32 BIBLIOTECA | Exitoso | 192.168.12.31 | 192.168.12.32 | Success | [Ver captura](./img/ping_vlan32_ok.png) |
| 32 BIBLIOTECA | Fallido | 192.168.12.31 | 192.168.12.51 | Fail | [Ver captura](./img/ping_vlan32_fail.png) |
| 42 LABORATORIO | Exitoso | 192.168.12.41 | 192.168.12.42 | Success | [Ver captura](./img/ping_vlan42_ok.png) |
| 42 LABORATORIO | Fallido | 192.168.12.41 | 192.168.12.21 | Fail | [Ver captura](./img/ping_vlan42_fail.png) |
| 52 VISITANTE | Exitoso | 192.168.12.51 | 192.168.12.52 | Success | [Ver captura](./img/ping_vlan52_ok.png) |
| 52 VISITANTE | Fallido | 192.168.12.51 | 192.168.12.11 | Fail | [Ver captura](./img/ping_vlan52_fail.png) |

### 10.1 Bloques para capturas de ping

1. **Ping VLAN 12 exitoso**

![Ping VLAN12 OK](./img/ping_vlan12_ok.png)

2. **Ping VLAN 12 fallido**

![Ping VLAN12 FAIL](./img/ping_vlan12_fail.png)

3. **Ping VLAN 22 exitoso**

![Ping VLAN22 OK](./img/ping_vlan22_ok.png)

4. **Ping VLAN 22 fallido**

![Ping VLAN22 FAIL](./img/ping_vlan22_fail.png)

5. **Ping VLAN 32 exitoso**

![Ping VLAN32 OK](./img/ping_vlan32_ok.png)

6. **Ping VLAN 32 fallido**

![Ping VLAN32 FAIL](./img/ping_vlan32_fail.png)

7. **Ping VLAN 42 exitoso**

![Ping VLAN42 OK](./img/ping_vlan42_ok.png)

8. **Ping VLAN 42 fallido**

![Ping VLAN42 FAIL](./img/ping_vlan42_fail.png)

9. **Ping VLAN 52 exitoso**

![Ping VLAN52 OK](./img/ping_vlan52_ok.png)

10. **Ping VLAN 52 fallido**

![Ping VLAN52 FAIL](./img/ping_vlan52_fail.png)

---

## 11. Evidencias de Comandos Show

### 11.1 Edificio A 

Comandos:

```bash
show spanning-tree
show etherchannel summary
show interfaces trunk
```

![Edificio A - show spanning-tree](./img/A_show_spanning_tree.png)
![Edificio A - show etherchannel summary](./img/A_show_etherchannel_summary.png)
![Edificio A - show interfaces trunk](./img/A_show_interfaces_trunk.png) 

### 11.2 Edificio B

Comandos:

```bash
show spanning-tree
show etherchannel summary
show interfaces trunk
```

![Edificio B - show spanning-tree](./img/B_show_spanning_tree.png)
![Edificio B - show etherchannel summary](./img/B_show_etherchannel_summary.png)
![Edificio B - show interfaces trunk](./img/B_show_interfaces_trunk.png)

### 11.3 Edificio C 

Comandos:

```bash
show spanning-tree
show etherchannel summary
show interfaces trunk
```

![Edificio C - show spanning-tree](./img/C_show_spanning_tree.png)
![Edificio C - show etherchannel summary](./img/C_show_etherchannel_summary.png)
![Edificio C - show interfaces trunk](./img/C_show_interfaces_trunk.png)

### 11.4 Edificio D 

Comandos:

```bash
show spanning-tree
show etherchannel summary
show interfaces trunk
```

![Edificio D - show spanning-tree](./img/D_show_spanning_tree.png)
![Edificio D - show etherchannel summary](./img/D_show_etherchannel_summary.png)
![Edificio D - show interfaces trunk](./img/D_show_interfaces_trunk.png)

---

## 12. Presupuesto Estimado de Equipos

### 12.1 Equipamiento principal

| Item | Cantidad | Precio unitario (Q) | Subtotal (Q) |
|---|---:|---:|---:|
| Switch Cisco 2960-24TT (acceso) | 10 | 1,219.00 | 12,190.00 |
| Switch RUIJIE con modulo FFE (agregacion) | 5 | 5,819.00 | 29,095.00 |
| Modulo PT-SWITCH-NM-1FFE | 10 | 351.00 | 3,510.00 |
| Access Point (AC-A1, AC-D1) | 2 | 850.00 | 1,700.00 |
| Hub Nexxt Solution | 2 | 327.00 | 654.00 |
| Repeater equivalente | 1 | 420.00 | 420.00 |

### 12.2 Cableado y accesorios

| Item | Cantidad | Precio unitario (Q) | Subtotal (Q) |
|---|---:|---:|---:|
| Cable UTP Cat6 (rollo/segmentos) | 8 | 490.00 | 3,920.00 |
| Patch cords Cat6 | 40 | 27.00 | 1,080.00 |
| Tramos de fibra OM3 + conectores | 12 | 155.17 | 1,862.04 |
| Cables de consola USB-RJ45 | 2 | 152.94 | 305.88 |

### 12.3 Total estimado

| Concepto | Monto |
|---|---:|
| Total estimado (Q) | **54,736.52** |

---


