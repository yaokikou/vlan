# 🔧 TOGINET S.A. — Infrastructure Réseau Multi-sites

Projet réalisé sous Cisco Packet Tracer dans le cadre d’un laboratoire d’administration réseau.

---

# 📌 Description du projet

Ce projet consiste à concevoir et configurer une architecture réseau complète comprenant :

- VLANs
- Router-on-a-Stick
- Routage dynamique RIP v2
- WAN multi-sites
- DHCP
- DNS
- HTTP

---

# 🏢 Architecture

## Sites

- Siège
- Agence
- Entrepôt

## VLANs utilisés

| VLAN | Description |
|---|---|
| 10 | Direction |
| 20 | Informatique |
| 30 | Commercial |
| 40 | Support |
| 50 | Entrepôt |
| 99 | Management |

---

# 🔀 Configuration des VLANs

## Création des VLANs

```bash
conf t

vlan 10
name DIRECTION

vlan 20
name INFORMATIQUE

vlan 30
name COMMERCIAL

vlan 40
name SUPPORT

vlan 50
name ENTREPOT

vlan 99
name MANAGEMENT
```

---

# 🖥 Attribution des ports aux VLANs

## VLAN 10

```bash
interface fa0/2
switchport mode access
switchport access vlan 10
```

## VLAN 20

```bash
interface fa0/3
switchport mode access
switchport access vlan 20
```

## VLAN 30

```bash
interface fa0/2
switchport mode access
switchport access vlan 30
```

## VLAN 40

```bash
interface fa0/3
switchport mode access
switchport access vlan 40
```

## VLAN 50

```bash
interface fa0/4
switchport mode access
switchport access vlan 50
```

---

# 🔁 Configuration des trunks

```bash
interface fa0/1
switchport trunk encapsulation dot1q
switchport mode trunk
```

---

# 🌐 Router-on-a-Stick (R1)

## VLAN 10

```bash
interface g0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
```

## VLAN 20

```bash
interface g0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
```

## VLAN 99

```bash
interface g0/0.99
encapsulation dot1Q 99
ip address 192.168.99.1 255.255.255.0
```

---

# 🌐 Router-on-a-Stick (R2)

## VLAN 30

```bash
interface g0/0.30
encapsulation dot1Q 30
ip address 192.168.30.1 255.255.255.0
```

## VLAN 40

```bash
interface g0/0.40
encapsulation dot1Q 40
ip address 192.168.40.1 255.255.255.0
```

---

# 🌐 Router-on-a-Stick (R3)

## VLAN 50

```bash
interface g0/0.50
encapsulation dot1Q 50
ip address 192.168.50.1 255.255.255.0
```

---

# 📡 Configuration WAN

## R1 ↔ R2

### R1

```bash
interface s0/1/0
ip address 10.0.12.1 255.255.255.252
clock rate 64000
no shutdown
```

### R2

```bash
interface s0/1/0
ip address 10.0.12.2 255.255.255.252
no shutdown
```

---

## R1 ↔ R3

### R1

```bash
interface s0/1/1
ip address 10.0.13.1 255.255.255.252
clock rate 64000
no shutdown
```

### R3

```bash
interface s0/1/1
ip address 10.0.13.2 255.255.255.252
no shutdown
```

---

## R2 ↔ R3

### R2

```bash
interface s0/1/1
ip address 10.0.14.1 255.255.255.252
clock rate 64000
no shutdown
```

### R3

```bash
interface s0/1/0
ip address 10.0.14.2 255.255.255.252
no shutdown
```

---

# 📶 Configuration RIP v2

## R1

```bash
router rip
version 2
no auto-summary
network 10.0.0.0
network 192.168.10.0
network 192.168.20.0
network 192.168.99.0
```

## R2

```bash
router rip
version 2
no auto-summary
network 10.0.0.0
network 192.168.30.0
network 192.168.40.0
```

## R3

```bash
router rip
version 2
no auto-summary
network 10.0.0.0
network 192.168.50.0
```

---

# 🌐 Configuration Serveur WEB

## Adresse IP

```text
IP Address : 192.168.10.5
Subnet Mask : 255.255.255.0
Gateway : 192.168.10.1
DNS : 192.168.10.5
```

## Activation HTTP

Services → HTTP → ON

## index.html

```html
<html>
<head>
<title>TOGINET</title>
</head>
<body>
<h1>Bienvenue sur TOGINET S.A.</h1>
<p>Serveur Web opérationnel</p>
</body>
</html>
```

---

# 🌐 Configuration DNS

Services → DNS → ON

## Entrée DNS

```text
www.toginet.local → 192.168.10.5
```

---

# 📥 Configuration DHCP

## Serveur DHCP

```text
IP : 192.168.10.7
Mask : 255.255.255.0
Gateway : 192.168.10.1
DNS : 192.168.10.5
```

## Pool VLAN10

```text
Pool Name : VLAN10
Gateway : 192.168.10.1
DNS : 192.168.10.5
Start IP : 192.168.10.100
Mask : 255.255.255.0
```

## Pool VLAN20

```text
Pool Name : VLAN20
Gateway : 192.168.20.1
DNS : 192.168.10.5
Start IP : 192.168.20.100
Mask : 255.255.255.0
```

## Pool VLAN30

```text
Pool Name : VLAN30
Gateway : 192.168.30.1
DNS : 192.168.10.5
Start IP : 192.168.30.100
Mask : 255.255.255.0
```

## Pool VLAN40

```text
Pool Name : VLAN40
Gateway : 192.168.40.1
DNS : 192.168.10.5
Start IP : 192.168.40.100
Mask : 255.255.255.0
```

## Pool VLAN50

```text
Pool Name : VLAN50
Gateway : 192.168.50.1
DNS : 192.168.10.5
Start IP : 192.168.50.100
Mask : 255.255.255.0
```

---

# 🧪 Commandes de vérification

## Vérifier les VLANs

```bash
show vlan brief
```

## Vérifier les trunks

```bash
show interfaces trunk
```

## Vérifier les routes

```bash
show ip route
```

## Vérifier RIP

```bash
show ip protocols
```

## Vérifier interfaces

```bash
show ip interface brief
```

---

# 💾 Sauvegarde des configurations

```bash
copy running-config startup-config
```

ou

```bash
wr
```

---

# 🧪 Tests réalisés

```bash
ping 192.168.x.x
ping www.toginet.local
```

## Test navigateur

```text
http://www.toginet.local
```

---

# ✅ Résultat final

- Communication inter-VLAN opérationnelle
- Communication inter-sites fonctionnelle
- RIP v2 actif
- DHCP fonctionnel
- DNS opérationnel
- Serveur WEB accessible

---

# 👨‍💻 Auteur

Projet réalisé par lazarus.

---

# 🏷 Tags

Cisco · Packet Tracer · VLAN · RIP · WAN · DHCP · DNS · HTTP · Networking
