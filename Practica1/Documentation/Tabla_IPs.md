# Tabla de IPs — Práctica 1: Diseño e Implementación de Red LAN Corporativa

**Nombre:** Jairo Adelso Gomez Hernandez
**Carnet:** 201902672  
**Curso:** Redes de Computadoras 1  
**Repositorio:** `Redes1_1S_2026_201902672/Practica1`  
**Entorno de Simulación:** Cisco Packet Tracer  

---

### Direccionamiento (IPs) del Entorno

Se determinó el uso de la red base **`192.178.72.0/24`**. Se configuraron **60 direcciones IP** distintas de forma estática, utilizando la máscara de subred `255.255.255.0` en todos los equipos.

A continuación, se detalla la asignación estricta por host:

| ID Host / Nombre | Tipo de Equipo | Departamento (Ubicación) | Dirección IPv4 Asignada |
| :--- | :--- | :--- | :--- |
| **SW_L1** | SWITCH  |  Piso 1 | SWITCH |
| **PC_01** | PC  | Recepción (Piso 1) | `192.178.72.1` |
| **PC_02** | PC  | Recepción (Piso 1) | `192.178.72.2` |
| **PC_03** | PC  | Recepción (Piso 1) | `192.178.72.3` |
| **PC_04** | PC  | Recepción (Piso 1) | `192.178.72.4` |
| **PC_05** | PC  | Recepción (Piso 1) | `192.178.72.5` |
| **PC_06** | PC  | Recepción (Piso 1) | `192.178.72.6` |
| **PC_07** | PC  | Recepción (Piso 1) | `192.178.72.7` |
| **PC_08** | PC  | Recepción (Piso 1) | `192.178.72.8` |
| **PC_09** | PC  | Recepción (Piso 1) | `192.178.72.9` |
| **PC_10** | PC  | Recepción (Piso 1) | `192.178.72.10` |
| **PC_11** | PC  | Recepción (Piso 1) | `192.178.72.11` |
| **Server0** | SERVIDOR | Recepción (Piso 1) | `192.178.72.12` |
| **PC_11** | PC  | Contabilidad (Piso 1) | `192.178.72.13` |
| **PC_12** | PC  | Contabilidad (Piso 1) | `192.178.72.14` |
| **PC_13** | PC  | Contabilidad (Piso 1) | `192.178.72.15` |
| **PC_14** | PC  | Contabilidad (Piso 1) | `192.178.72.16` |
| **PC_15** | PC  | Contabilidad (Piso 1) | `192.178.72.17` |
| **PC_16** | PC  | Contabilidad (Piso 1) | `192.178.72.18` |
| **PC_17** | PC  | Contabilidad (Piso 1) | `192.178.72.19` |
| **PC_37** | PC  | Contabilidad (Piso 1) | `192.178.72.20` |
| **PC_19** | PC  | Legal (Piso 1) | `192.178.72.21` |
| **PC_20** | PC  | Legal (Piso 1) | `192.178.72.22` |
| **PC_21** | PC  | Legal (Piso 1) | `192.178.72.23` |
| **PC_23** | PC  | Legal (Piso 1) | `192.178.72.24` |
| **PC_22** | PC  | Legal (Piso 1) | `192.178.72.25` |
| **Laptop0** | Laptop  | Reuniones (Piso 1) | `192.178.72.26` |
| **Laptop5** | Laptop  | Reuniones (Piso 1) | `192.178.72.27` |
| **Laptop4** | Laptop  | Reuniones (Piso 1) | `192.178.72.28` |
| **Laptop3** | Laptop  | Reuniones (Piso 1) | `192.178.72.29` |
| **Laptop2** | Laptop  | Reuniones (Piso 1) | `192.178.72.30` |
| **SW_L2** | SWITCH  |  Piso 2| SWITCH |
| **PC_25** | PC  | Dept. Arquitectura (Piso 2) | `192.178.72.31` |
| **PC_26** | PC  | Dept. Arquitectura (Piso 2) | `192.178.72.32` |
| **PC_27** | PC  | Dept. Arquitectura (Piso 2) | `192.178.72.33` |
| **PC_28** | PC  | Dept. Arquitectura (Piso 2) | `192.178.72.34` |
| **PC_30** | PC  | Dept. Arquitectura (Piso 2) | `192.178.72.35` |
| **PC_29** | PC  | Dept. Arquitectura (Piso 2) | `192.178.72.36` |
| **PC_32** | PC  | Dept. Urbanizacion (Piso 2) | `192.178.72.37` |
| **PC_33** | PC  | Dept. Urbanizacion (Piso 2) | `192.178.72.38` |
| **PC_35** | PC  | Dept. Urbanizacion (Piso 2) | `192.178.72.39` |
| **PC_34** | PC  | Dept. Urbanizacion (Piso 2) | `192.178.72.40` |
| **PC_31** | PC  | Dept. Urbanizacion (Piso 2) | `192.178.72.41` |
| **Server1** | SERVIDOR | Dept. Urbanizacion (Piso 2) | `192.178.72.42` |
| **Laptop7** | Laptop  | Sala Revision Planos (Piso 2) | `192.178.72.43` |
| **Laptop8** | Laptop  | Sala Revision Planos (Piso 2) | `192.178.72.44` |
| **Laptop11** | Laptop  | Sala Revision Planos (Piso 2) | `192.178.72.45` |
| **Laptop10** | Laptop  | Sala Revision Planos (Piso 2) | `192.178.72.46` |
| **Laptop9** | Laptop  | Sala Revision Planos (Piso 2) | `192.178.72.47` |
| **SW_L3** | SWITCH  |  Piso 3| SWITCH |
| **Laptop6** | Laptop  |  Direccion General (Piso 3) | `192.178.72.48` |
| **Laptop12** | Laptop  | Direccion General (Piso 3) | `192.178.72.49` |
| **Laptop13** | Laptop  | Direccion General (Piso 3) | `192.178.72.50` |
| **Laptop1** | Laptop  |  Direccion General (Piso 3) | `192.178.72.51` |
| **PC_24** | PC  | Dept. Ingenieria Civil (Piso 3) | `192.178.72.52` |
| **PC_36** | PC  | Dept. Ingenieria Civil (Piso 3) | `192.178.72.53` |
| **PC_38** | PC  | Dept. Ingenieria Civil (Piso 3) | `192.178.72.54` |
| **PC_39** | PC  | Dept. Ingenieria Civil (Piso 3) | `192.178.72.55` |
| **PC_41** | PC  | Dept. Ingenieria Civil (Piso 3) | `192.178.72.56` |
| **Server5** | SERVIDOR | Dept. Ingenieria Civil (Piso 3) | `192.178.72.57` |
| **Server3** | SERVIDOR | Dept. Servidores (Piso 3) | `192.178.72.57` |
| **Server2** | SERVIDOR | Dept. Servidores (Piso 3) | `192.178.72.57` |
| **Server4** | SERVIDOR | Dept. Servidores (Piso 3) | `192.178.72.57` |
