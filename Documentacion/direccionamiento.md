# Plan de Direccionamiento

## VPN Site-to-Site con FortiGate en GNS3

Este documento describe el esquema de direccionamiento utilizado en el laboratorio de VPN IPsec Site-to-Site.

---

## Red general

El direccionamiento privado del laboratorio está basado en:

```text
10.21.40.0/24
```

Esta red se subdividió en dos segmentos:

| Segmento | Red | Máscara | Uso |
|---|---|---|---|
| Usuarios | `10.21.40.0/25` | `255.255.255.128` | Windows / VLAN 10 |
| Servidores | `10.21.40.128/28` | `255.255.255.240` | Ubuntu Server |

---

## Red de usuarios

```text
Red: 10.21.40.0/25
Máscara: 255.255.255.128
Rango: 10.21.40.1 - 10.21.40.126
Broadcast: 10.21.40.127
Gateway: 10.21.40.1
```

El gateway corresponde a FortiGate 1, interfaz `VLAN10-USUARIOS`.

---

## VLAN 10

```text
Nombre: VLAN10-USUARIOS
VLAN ID: 10
Interfaz física: port2
IP: 10.21.40.1/25
```

---

## DHCP

FortiGate 1 funciona como servidor DHCP para la VLAN 10.

```text
Rango: 10.21.40.10 - 10.21.40.100
Gateway: 10.21.40.1
DNS: 8.8.8.8
     1.1.1.1
```

Cliente Windows observado durante las pruebas:

```text
IP: 10.21.40.10
Máscara: 255.255.255.128
Gateway: 10.21.40.1
```

---

## Red de servidores

```text
Red: 10.21.40.128/28
Máscara: 255.255.255.240
Rango utilizable: 10.21.40.129 - 10.21.40.142
Broadcast: 10.21.40.143
Gateway: 10.21.40.129
```

---

## FortiGate 2 - SERVER-LAN

```text
Interfaz: port2
Alias: SERVER-LAN
IP: 10.21.40.129/28
```

---

## Ubuntu Server

```text
Interfaz: ens3
IP: 10.21.40.130/28
Máscara: 255.255.255.240
Gateway: 10.21.40.129
DNS: 8.8.8.8
     1.1.1.1
```

---

## WAN FortiGate 1

Red:

```text
203.0.113.0/30
```

| Dispositivo | Dirección |
|---|---|
| Router ISP Fa0/0 | `203.0.113.1/30` |
| FortiGate 1 port1 | `203.0.113.2/30` |

Broadcast: `203.0.113.3`

Gateway de FortiGate 1: `203.0.113.1`

---

## WAN FortiGate 2

Red:

```text
198.51.100.0/30
```

| Dispositivo | Dirección |
|---|---|
| Router ISP Fa0/1 | `198.51.100.1/30` |
| FortiGate 2 port1 | `198.51.100.2/30` |

Broadcast: `198.51.100.3`

Gateway de FortiGate 2: `198.51.100.1`

---

## Red externa del ISP

```text
Red: 192.168.18.0/24
Router ISP Fa1/0: 192.168.18.159/24
Gateway: 192.168.18.1
NAT: outside
```

---

## Tabla general

| Equipo | Interfaz | IP | Máscara | Gateway |
|---|---|---|---|---|
| Windows 10 | Ethernet | `10.21.40.10` | `/25` | `10.21.40.1` |
| FortiGate 1 | VLAN10-USUARIOS | `10.21.40.1` | `/25` | — |
| FortiGate 1 | port1 | `203.0.113.2` | `/30` | `203.0.113.1` |
| Router ISP | Fa0/0 | `203.0.113.1` | `/30` | — |
| Router ISP | Fa0/1 | `198.51.100.1` | `/30` | — |
| FortiGate 2 | port1 | `198.51.100.2` | `/30` | `198.51.100.1` |
| FortiGate 2 | port2 | `10.21.40.129` | `/28` | — |
| Ubuntu Server | ens3 | `10.21.40.130` | `/28` | `10.21.40.129` |
| Router ISP | Fa1/0 | `192.168.18.159` | `/24` | `192.168.18.1` |

---

## Selectores de la VPN

### FortiGate 1

```text
Peer remoto: 198.51.100.2
Red local: 10.21.40.0/25
Red remota: 10.21.40.128/28
```

### FortiGate 2

```text
Peer remoto: 203.0.113.2
Red local: 10.21.40.128/28
Red remota: 10.21.40.0/25
```

---

## Resumen

```text
Usuarios
10.21.40.0/25
      |
      | VPN IPsec
      |
Servidores
10.21.40.128/28
```

```text
FG1
203.0.113.2/30
        |
        | Router ISP
        |
198.51.100.2/30
FG2
```
