
# VLANs & Inter-VLAN Routing (Router-on-a-Stick)

## 📌 Project Overview

This lab demonstrates how to configure **VLANs** on switches and use a **Router-on-a-Stick** to enable communication between different VLANs.
The setup also includes **DHCP pools** for automatic IP assignment to hosts in each VLAN.

✔ Segmentation with VLANs (IT & Sales)
✔ Inter-VLAN communication using sub-interfaces on a router
✔ DHCP configuration for dynamic IP allocation

---

## 🖥️ Topology

```
   [PC0]---Fa0/1    Fa0/1---[PC2]
           VLAN 10   VLAN 10

   [PC1]---Fa0/2    Fa0/2---[PC3]
           VLAN 12   VLAN 12

          Switch0         Switch1
             | Trunk      | Trunk
             | Gi0/2      | Gi0/1
                \         /
                 \       /
                 Router (G0/0/1)
```

---

## ⚙️ Configuration Summary

### 🔹 Switch 0

```plaintext
interface FastEthernet0/1
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/2
 switchport access vlan 12
 switchport mode access
!
interface GigabitEthernet0/2
 switchport trunk allowed vlan 10,12
 switchport mode trunk
```

### 🔹 Switch 1

```plaintext
interface FastEthernet0/1
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/2
 switchport access vlan 12
 switchport mode access
!
interface GigabitEthernet0/1
 switchport trunk allowed vlan 10,12
 switchport mode trunk
!
interface GigabitEthernet0/2
 switchport trunk allowed vlan 10,12
 switchport mode trunk
```

### 🔹 Router (Router-on-a-Stick)

```plaintext
ip dhcp pool IT
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
!
ip dhcp pool Sales
 network 192.168.12.0 255.255.255.0
 default-router 192.168.12.1
!
interface GigabitEthernet0/0/1.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
!
interface GigabitEthernet0/0/1.12
 encapsulation dot1Q 12
 ip address 192.168.12.1 255.255.255.0
```

---

## ✅ Verification

* **VLAN assignment** confirmed with `show vlan brief`.
* **Trunking** verified with `show interfaces trunk`.
* **DHCP** working → PCs in VLAN 10 & 12 receive IPs automatically.
* **Ping Test** → PC in VLAN 10 successfully pings PC in VLAN 12.

---

## 📂 Video Attached

* **Video Demo** (https://youtu.be/meUMUuldWOM)

---

⚡ *By Edafe Urhukpeoghene Osagie*

---
