# Topología del Laboratorio

![Topología real del laboratorio en GNS3](topologia-gns3.png)

```mermaid
flowchart LR
    PC["Windows 10<br/>10.21.40.10/25"]
    SW["Switch GNS3<br/>VLAN 10"]
    FG1["FortiGate 1<br/>VLAN10: 10.21.40.1/25<br/>WAN: 203.0.113.2/30"]
    ISP["Router ISP<br/>Fa0/0: 203.0.113.1/30<br/>Fa0/1: 198.51.100.1/30<br/>Fa1/0: 192.168.18.159/24"]
    FG2["FortiGate 2<br/>WAN: 198.51.100.2/30<br/>SERVER-LAN: 10.21.40.129/28"]
    SRV["Ubuntu Server<br/>10.21.40.130/28<br/>Apache2 + HTTPS"]
    INTERNET["Internet<br/>Gateway: 192.168.18.1"]

    PC --> SW
    SW --> FG1
    FG1 --> ISP
    ISP --> FG2
    FG1 <-->|"VPN IPsec Site-to-Site"| FG2
    FG2 --> SRV
    ISP --> INTERNET
```

## Redes involucradas

```text
Usuarios:      10.21.40.0/25
Servidores:    10.21.40.128/28
WAN FG1:       203.0.113.0/30
WAN FG2:       198.51.100.0/30
Red externa:   192.168.18.0/24
```

## Flujo principal

```text
Windows 10
10.21.40.10
   |
   v
FortiGate 1
10.21.40.1 / 203.0.113.2
   |
   | VPN IPsec Site-to-Site
   v
FortiGate 2
198.51.100.2 / 10.21.40.129
   |
   v
Ubuntu Server
10.21.40.130
```
