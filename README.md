# Enterprise Core/Distribution Network Simulation & SAN Infrastructure

Built a scaled-down, enterprise-grade lab in Cisco Packet Tracer to simulate a redundant Core/Distribution architecture. The goal of this project was to model a realistic production environment—focusing on Layer 3 high availability, strict VLAN segmentation, centralized Active Directory DHCP relaying, and dedicated SAN storage networking.

The topology supports around 200–250 simulated end devices, balancing real-world network design with Packet Tracer's performance limits.

---

## Architecture & Design Highlights

### Perimeter & Edge
* **Edge Firewall (`Core-FW`):** Sits at the perimeter with a default static route (`0.0.0.0 0.0.0.0`) pointing to the ISP.
* **Transit Routing:** Connects the edge firewall to the internal L3 core switch pair.

### Core Layer & High Availability (Dual Cisco 3560s)
* **HSRP Active/Passive Failover:** Dual Layer 3 core switches handle routing across all SVIs. The primary core switch is set with higher HSRP priority (`110`) to take active gateway traffic, while the secondary switch acts as the standby target.
* **Inter-Switch Trunking:** An EtherChannel trunk links the two core switches, carrying tagged 802.1Q VLAN traffic, maintaining state, and handling fast failover if a core link goes down.

### Network Segmentation (VLANs)
* **VLAN 10:** Corporate / Office
* **VLAN 50:** Printers
* **VLAN 55:** OT / Industrial Systems
* **VLAN 100:** Server Infrastructure (Active Directory, DNS, Central Services)
* **VLAN 110:** Voice (VoIP / Option 150)

### Distribution & Access
* **IDF Switches (Distribution):** Connected straight to the core L3 switches over 802.1Q trunks.
* **Access Switches:** Standard access-layer switches and stacks feeding end-user desktops, IP phones, printers, and OT devices.

---

## Storage & Compute Architecture (SAN)

* **Dedicated SAN Switching:** Block-level storage traffic (iSCSI) is kept completely off the general LAN and offloaded onto a separate `SAN SWITCH`.
* **Core Uplink:** The SAN switch uplinks directly into the primary core switch for host access.
* **Traffic Isolation:** Keeping storage traffic separated prevents heavy disk I/O from hitting client SVIs or saturating core links used by general user and voice traffic.

---

## Services & Configurations

### Centralized Active Directory DHCP Relay (`ip helper-address`)
Instead of running lightweight DHCP pools directly on the switch ASICs, the setup uses centralized enterprise DHCP:
1. Active Directory domain controllers run centralized scopes inside **VLAN 100**.
2. Every client SVI on both core switches uses `ip helper-address <AD_Server_IP>` to
