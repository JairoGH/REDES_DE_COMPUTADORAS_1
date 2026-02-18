# Manual Técnico — Práctica 1: Diseño e Implementación de Red LAN Corporativa

**Nombre:** Jairo Adelso Gomez Hernandez
**Carnet:** 201902672  
**Curso:** Redes de Computadoras 1  
**Repositorio:** `Redes1_1S_2026_201902672/Practica1`  
**Entorno de Simulación:** Cisco Packet Tracer  

---

## 1. Resumen

Este documento describe el diseño, configuración e implementación de una red de Área Local (LAN) jerárquica para el nuevo edificio corporativo de "Constructiva S.A.". La infraestructura fue desplegada y validada en un entorno simulado, aplicando principios de cableado estructurado, direccionamiento IP estático y configuración de seguridad básica en dispositivos de capa 2.

El proyecto valida:

- **Diseño de topología jerárquica** separada por niveles físicos (3 pisos) y áreas lógicas (10 departamentos).
- **Asignación estática de IPv4** garantizando comunicación sin colisiones para 60 dispositivos finales.
- **Configuración mediante CLI** de switches Cisco (hostname y protección por contraseña).
- **Validación de conectividad total** mediante pruebas de protocolo ICMP y análisis de resolución de direcciones (ARP).

---

## 2. Objetivos Técnicos

1. Diseñar la topología lógica y física utilizando switches Cisco modelo 2960.
2. Aplicar el estándar **TIA/EIA-568B** empleando cableado *straight-through* para acceso y *crossover* para el *backbone* de la red.
3. Configurar parámetros básicos de seguridad y administración en los switches a través de CLI.
4. Implementar un esquema de direccionamiento IP estático basado en la red `192.178.72.0/24` (derivada del carnet 201902672).
5. Demostrar la convergencia de la red capturando y analizando paquetes en modo simulación.

---

## 3. Arquitectura y Distribución del Edificio

La red está diseñada bajo un modelo de distribución jerárquica, centralizando el tráfico por nivel:

### 3.1 Piso 1 — Nivel Operativo y Atención (30 equipos)

- **Switch Core Local:** `SW_L1` (conecta al Piso 2 y Servidores).
- **Switches de Acceso:** `Recep`, `Conta`, `Legal`, `Reuniones`.
- Contiene equipos de escritorio, laptops y 1 servidor local de Recepción.

### 3.2 Piso 2 — Nivel de Diseño (17 equipos)

- **Switch Core Local:** `SW_L2` (actúa como puente en el backbone hacia el Piso 1 y Piso 3).
- **Switches de Acceso:** `Arqui`, `Urba`, `Planos`.
- Incluye 1 servidor local dedicado a Urbanismo.

### 3.3 Piso 3 — Nivel de Dirección e Ingeniería (10 equipos)

- **Switch Core Local:** `SW_L3`.
- **Switches de Acceso:** `Direccion`, `Ingenieria`.
- Incluye 1 servidor local dedicado a Ingeniería.

### 3.4 Área de Servidores Principales (3 equipos)

- **Switch de Acceso:** `Servidores` (conectado directamente al backbone principal).
- Aloja la infraestructura crítica de TI (3 servidores).

---

## 4. Requerimientos y Estándares

### 4.1 Tecnologías y Componentes

- **Simulador:** Cisco Packet Tracer.
- **Dispositivos de Red (Capa 2):** Switches Cisco 2960 (14 unidades en total).
- **Dispositivos Finales:** Máquinas Virtuales VPC (Computadoras de escritorio y Laptops) y Servidores genéricos.
- **Protocolos verificados:** IPv4, ICMP, ARP.

### 4.2 Estándar de Cableado (TIA/EIA-568B)

- **Cable Directo (Straight-through):** Utilizado para conectar dispositivos de diferentes capas (VPC/Servidor ↔ Switch de Acceso).
- **Cable Cruzado (Crossover):** Utilizado para interconectar dispositivos de la misma capa (Switch Core ↔ Switch Core / Switch Core ↔ Switch de Acceso).

---

## 5. Direccionamiento (IPs) del Entorno

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

---

## 6. Configuración Operativa

Todos los switches fueron configurados mediante la Interfaz de Línea de Comandos (CLI) para establecer su identidad (`hostname`) y restringir el acceso no autorizado mediante el uso del número de carnet (`201902672`) como credencial.

**Script base de configuración aplicado a cada nodo:**

```text
enable
configure terminal
hostname [NOMBRE_DEL_SWITCH]
enable secret 201902672
line console 0
password 201902672
login
exit
end
copy running-config startup-config
```

*(Nota: El comando fue ejecutado 14 veces, iterando el valor `[NOMBRE_DEL_SWITCH]` por: `SW_L1`, `SW_L2`, `SW_L3`, `Recep`, `Conta`, `Legal`, `Reuniones`, `Arqui`, `Urba`, `Planos`, `Direccion`, `Ingenieria` y `Servidores`).*

---

## 7. Evidencia de Configuración de IPs

A continuación, se documenta la inyección de la configuración IP estática en los equipos host correspondientes a las 10 áreas funcionales de la empresa.

1. **Recepción:** ![Configuración Recepcion](./img/recep.png)
2. **Contabilidad:** ![Configuración Contabilidad](./img/conta.png)
3. **Legal:** ![Configuración Legal](./img/legal.png)
4. **Sala de Reuniones:** ![Configuración Reuniones](./img/reuniones.png)
5. **Arquitectura:** ![Configuración Arquitectura](./img/arqui.png)
6. **Urbanismo:** ![Configuración Urbanismo](./img/urba.png)
7. **Revisión de Planos:** ![Configuración Planos](./img/planos.png)
8. **Dirección General:** ![Configuración Direccion](./img/general.png)
9. **Ingeniería Civil:** ![Configuración Ingenieria](./img/civil.png)
10. **Servidores Principales:** ![Configuración Servidores](./img/server.png)

---

## 8. Validación de Conectividad

Se realizaron pruebas de "eco" (Ping) para garantizar que la tabla de direcciones MAC de los switches convergiera correctamente y que todos los nodos tuvieran visibilidad en la capa de red.

1. **Ping 01:** (Recepción a Contabilidad) ![Ping 1](./img/ping1.png)
2. **Ping 02:** (Legal a Reuniones) ![Ping 2](./img/ping2.png)
3. **Ping 03:** (Arquitectura a Urbanismo) ![Ping 3](./img/ping3.png)
4. **Ping 04:** (Planos a Arquitectura) ![Ping 4](./img/ping4.png)
5. **Ping 05:** (Dirección a Ingeniería) ![Ping 5](./img/ping5.png)
6. **Ping 06:** (Piso 3 a Piso 1 - Extremo a Extremo) ![Ping 6](./img/ping6.png)
7. **Ping 07:** (Piso 2 a Piso 1) ![Ping 7](./img/ping7.png)
8. **Ping 08:** (Cualquier PC a Servidor Urbanismo) ![Ping 8](./img/ping8.png)
9. **Ping 09:** (Cualquier PC a Servidor Ingeniería) ![Ping 9](./img/ping9.png)
10. **Ping 10:** (Cualquier PC a Main Server) ![Ping 10](./img/ping10.png)

---

## 9. Análisis de Tráfico de Red

Para validar el entendimiento de la teoría de redes, se capturó el comportamiento de los protocolos fundamentales durante una transmisión de datos.

### 9.1 Resolución de Direcciones (ARP)

El Address Resolution Protocol (ARP) fue observado haciendo un broadcast (`FF:FF:FF:FF:FF:FF`) para descubrir la dirección MAC física del equipo destino asociada a su IP, paso previo indispensable antes de enviar datos.
![Análisis Paquete ARP](./img/package.png)

---

### 9.2 Topologia de Red

- ### **Piso 1**

![Piso 1](./img/piso1.png)

---

- ### **Piso 2**

![Piso 2](./img/piso2.png)

---

- ### **Piso 3**

![Piso 3](./img/piso3.png)

---

## 10. Conclusiones y Observaciones Técnicas

- **Escalabilidad:** El diseño jerárquico implementado facilita la escalabilidad. Si "Constructiva S.A." decide añadir un Piso 4, bastará con añadir un `SW_L4` al backbone sin interrumpir las operaciones de los niveles inferiores.
- **Eficiencia del Direccionamiento:** El uso de una máscara `/24` proporciona 254 hosts utilizables. Al haber utilizado solo 60 IPs, la red actual se encuentra al ~23% de su capacidad, dejando amplio margen de crecimiento en el mismo segmento de red.
- **Rendimiento:** Al mantener la interconexión de equipos por área lógica separada en switches de acceso, se reducen los dominios de colisión, optimizando el rendimiento general de la LAN corporativa.
