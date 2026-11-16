# cisco-packet-tracer-base-network-
a company containing (sales, marketing and client ) networks configured my part to design the network 
MSS Ltd Network Implementation - README
📌 Project Overview
This repository contains the Cisco Packet Tracer (.pkt) file and documentation for the MSS Ltd network implementation. The goal of this project is to design and build a secure, redundant, and scalable Local Area Network (LAN) for a three-floor company.

Coursework Requirement: Part 1 - Network Implementation (70%)
Company: MSS Ltd
Departments:

Floor 1: Marketing (20 PCs) - VLAN 10

Floor 2: Sales (25 PCs) - VLAN 20

Floor 3: General Support (15 PCs) - VLAN 30

Server Room: 8 Servers (Email, Intranet, 3x DB, 2x Backup) - VLAN 40

🗺️ Network Topology & IP Addressing
The network is built using a Core-to-Access topology with a centralized Firewall and Edge Router.

VLANs and Subnets
VLAN ID	Department / Role	Network Subnet	Default Gateway (SW-Core)
10	Marketing	192.168.10.0 /24	192.168.10.1
20	Sales	192.168.20.0 /24	192.168.20.1
30	General Support	192.168.30.0 /24	192.168.30.1
40	Servers	192.168.40.0 /24	192.168.40.1
Point-to-Point Links
Link	Subnet	IP Addresses
SW-Core to FW1	10.0.0.0 /30	Core: .2, FW1: .1
FW1 to R1	10.0.1.0 /30	FW1: .1, R1: .2
R1 to ISP	203.0.113.0 /30	R1: .2, ISP: .1
ISP to Web Server	8.8.8.0 /24	ISP: .1, Web: 8.8.8.8
Server Assignments (VLAN 40)
192.168.40.10 - Email Server

192.168.40.11 - Intranet Server

192.168.40.12 - Marketing DB

192.168.40.13 - Sales DB

192.168.40.14 - Support DB

192.168.40.15 - Backup DB Server

192.168.40.16 - Backup Server

192.168.40.17 - Spare / Future Use

🔌 Physical Port Map
SW-Core (3650-24PS Multilayer Switch)
Gi1/0/1 & Gi1/0/4 -> EtherChannel Po1 -> Marketing Switch (Switch2)

Gi1/0/2 & Gi1/0/5 -> EtherChannel Po2 -> Sales Switch (Switch4)

Gi1/0/3 & Gi1/0/6 -> EtherChannel Po3 -> Support Switch (Switch3)

Gi1/0/10 - Gi1/0/17 -> Server0 to Server7

Gi1/0/24 -> Routed Port (10.0.0.2/30) -> FW1 (Gi0/0)

Floor Access Switches (2960)
Fa0/1 & Fa0/3 -> EtherChannel Trunk back to SW-Core

Fa0/2 - Fa0/24 -> Access Ports for End-User PCs (Spanning-Tree PortFast enabled)

⚙️ Configuration Highlights
Inter-VLAN Routing: Handled by the 3650 Multilayer Switch (SW-Core) using SVIs (Switch Virtual Interfaces) and ip routing.

DHCP: SW-Core is acting as the DHCP server for all three PC VLANs. Addresses are excluded from .1 to .10 to reserve them for network devices.

Redundancy: EtherChannels (Port-Channels) are configured between the Core and Access switches using channel-group X mode on to provide link redundancy and increased bandwidth.

NAT (PAT): Router R1 is configured to translate all internal 192.168.0.0/16 traffic to the public IP address of its outside interface (g0/1) using an access-list and ip nat inside source list 1 interface g0/1 overload.

Security:

Local user accounts (enable secret) and SSH configured on switches.

service password-encryption enabled.

Port Security (sticky MAC) on access ports to prevent unauthorized devices.

DHCP Snooping to prevent rogue DHCP servers.

BPDU Guard on access ports to prevent STP manipulation.

✅ Testing Checklist
(Use this to verify your work before submission)

□ PC IP Addressing: Ensure all PCs are set to DHCP and can successfully pull an IP address matching their VLAN subnet.
□ Inter-VLAN Routing: A PC in Marketing (VLAN 10) can ping a PC in Sales (VLAN 20).
□ Server Access: PCs can ping the Servers (e.g., 192.168.40.10).
□ Internet Access: PCs can ping the Web Server (8.8.8.8).
□ NAT Translation: Run show ip nat translations on R1 while a PC pings 8.8.8.8 to see the active translation.
□ Redundancy Test: Go to SW-Core and shutdown Gi1/0/1 (the primary link to Marketing). Ensure PC1 can still ping its gateway (the EtherChannel should failover instantly).
□ Save Work: Run write memory on all devices and Ctrl+S in Packet Tracer.
💡 Design Justification (For the Report)
Why VLANs? VLANs segment broadcast domains, which improves network performance and provides security boundaries between departments (Marketing, Sales, Support).
Why EtherChannel? Instead of relying on Spanning Tree Protocol (STP) to block redundant links, EtherChannel bundles multiple physical links into one logical link, providing both redundancy (if one cable fails) and double the bandwidth.
Why a Multilayer Switch? Using a Layer 3 switch (3650) for inter-VLAN routing is much faster than using a traditional router-on-a-stick method, which can become a bottleneck for 60 PCs.
