# Network-Security
This CCNA‑level enterprise lab implements a secure multi‑router campus network using 4x L2 switches and a Router‑on‑a‑Stick design for inter‑VLAN routing between VLAN 10 (HR), VLAN 20 (DEV), and VLAN 30 (SERVER). VLANs are centrally managed via VTP, with DHCP Snooping and Dynamic ARP Inspection (DAI) enabled on all three VLANs

# CCNA-Level

This project implements a **multi‑router enterprise campus network** using Cisco Packet Tracer.  
It includes:

- 4x L2 switches + **Router‑on‑a‑Stick** (inter‑VLAN routing)  
- **VTP** for VLANs `10 (HR)`, `20 (DEV)`, `30 (SERVER)`  
- **DHCP Snooping** and **Dynamic ARP Inspection (DAI)** on VLANs 10, 20, 30  
- **Two office routers** (`R1`, `R2`) running **OSPF**  
- **ISP Router** running **BGP**  
- **Route redistribution** between OSPF and BGP  
- **SSH v2** on office routers (`R1`, `R2`) for secure management  

---

## 🎯 Lab Objectives

- Configure VLANs `10 (HR)`, `20 (DEV)`, `30 (SERVER)` using **VTP** across 4x L2 switches.  
- Implement **Router‑on‑a‑Stick** inter‑VLAN routing on **R1**.  
- Enable **DHCP Snooping** and **DAI (ARP inspection)** on VLANs 10, 20, 30.  
- Configure **OSPF** between **R1** and **R2** (Area 0).  
- Configure **BGP** (eBGP) between **R2** and **ISP Router** (AS 65001).  
- Perform **route redistribution** between **OSPF** and **BGP**.  
- Secure router management using **SSH v2** on **R1** and **R2** (disable Telnet).

---

## 📐 Network Topology

![topology](/Img/Topology.png)

---
## Router 
### R1
 - Router-on-a-stick for VLANs 10, 20, 30 (subinterfaces .10, .20, .30)
 - Trunk link to SW-1 carrying VLANs 10, 20, 30,
 - DHCP snooping and Dynamic ARP Inspection (DAI) configured
 - SSH v2 enabled for management

## Switch
### SW-1 (VTP server)
 - Trunk to R1, access/trunks to downstream switches
 - VLANs: 10 (HR), 20 (DEV), 30 (SERVERS)
### SW-2 (HR,DEV)
 - Access ports assigned to VLAN 10,20(HR,DEV PCs)
### SW-3 (DEV)
 - Access ports assigned to VLAN 10,20 (DEV,HR PCs)

### SW-4 (SERVER)
 - Access port in VLAN 30 hosting SERVER
---

## 🌐 VLAN and IP Addressing

| VLAN | Name   | Subnet           | Gateway (R1)        | Purpose        |
|------|--------|------------------|---------------------|----------------|
| 10   | HR     | 192.168.1.0/24  | 192.168.1.1        | HR department  |
| 20   | DEV    | 192.168.2.0/24  | 192.168.2.1        | Development    |
| 30   | SERVER | 192.168.3.0/24  | 192.168.3.1        | Internal servers |

- ISP facing (R1–ISP): `203.0.113.0/30` (AS 100 / 101)

---

## 🔧 Device Roles

| Device   | Role                                                                 |
|----------|----------------------------------------------------------------------|
| R1       | Office Router‑on‑a‑Stick (RoAS), OSPF Area 0, BGP As 100, NAT, security (DHCP Snooping/DAI), SSH v2. |
| ISP      | ISP/Internet router (BGP AS 65001), eBGP peering with R2.            |
| SW‑1     | VTP Server (VLAN 10/20/30), trunk to R1, SW‑2, SW‑3, SW‑4.          |
| SW‑2     | VTP Client; HR VLAN 10 access (HR PCs).                              |
| SW‑3     | VTP Client; DEV VLAN 20 access (DEV PCs).                            |
| SW‑4     | VTP  Transparent ; SERVER VLAN 30 access (Server).                          |

---
## Verification commands
### R1/R2(office/ISP)
 - show ip route
 - show ip bgp summary
 - show ip ospf neighbor
 - show running-config | section interface

### Switches
 - show vlan brief
 - show vtp status
 - show ip dhcp snooping
 - show ip arp inspection
 - show ip arp inspection status
 - show port security address
 - show port security interface f0/1
---

## 🧪 How to Test the Lab
- **R1 password**:`secret:` 1234 `password` 12345
- **VTP & VLANs**: `show vlan brief` on SW‑2/3/4.  
- **Inter‑VLAN routing**: From HR, DEV, SERVER PCs, ping:
  - Gateway, then each other gateway (10→20→30).  
- **OSPF**: `show ip ospf neighbor` and `show ip route ospf` on R1/R2.  
- **BGP**: `show ip bgp summary` and `show ip route bgp` on R2 and ISP.  
- **Redistribution**:  
  - ISP should see `192.168.10.0/24`, `192.168.20.0/24`, `192.168.30.0/24` via BGP.  
  - R1/PCs should have a default route via OSPF.  
- **DHCP Snooping / DAI**:  
  - Normal DHCP = OK.  
  - Rogue DHCP or ARP spoofing = blocked.  
- **SSH**:  
  - From a PC, run `ssh -l admin 192.168.1.1` → login works.
  - ssh user password 1234
  - Telnet attempts → should fail.

---

## 📦 Files Included

- `net.pkt` – Cisco Packet Tracer project.  
- `README.md` – This document.  
- `Config folder` – Copy‑paste snippets for VTP, RoAS, OSPF, BGP, redistribution, DAI, DHCP Snooping, SSH v2.

---

## 📚 CCNA/CCNP Relevance

Covers:
- VLANs, trunking, VTP.  
- Router‑on‑a‑Stick inter‑VLAN routing.  
- **OSPF**, **BGP**, **redistribution**.  
- **Layer 2 security** (DHCP Snooping, DAI).  
- **Secure management** (SSH v2 on R1/R2).

---

## 🙋 How to Use This Guide

- Open `net.pkt` in Cisco Packet Tracer.  
- Apply the commands above.  
- Test routing, security, BGP redistribution, and SSH.  
- Take screenshots for your portfolio or resume.

---

Created: 2026‑05‑21  
Author: CCNA Aspirant (your name)
