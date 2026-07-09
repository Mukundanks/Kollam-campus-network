# Kollam Regional Metro-Campus Network (KRCMN)

 **A real-world enterprise network design connecting 9 educational institutions in Kollam, Kerala, India — built on Indian Railways' RailTel fiber infrastructure.**

![Status](https://img.shields.io/badge/Status-In%20Progress-orange)
![Phase](https://img.shields.io/badge/Phase-VLAN%20Design-blue)
![Institutions](https://img.shields.io/badge/Institutions-9-green)
![Students](https://img.shields.io/badge/Students-6400%2B-green)
![Tools](https://img.shields.io/badge/Tools-GNS3%20%7C%20Packet%20Tracer%20%7C%20VMware-lightgrey)

---

## What Is This Project?

This is a fully designed, simulated and documented enterprise level metro campus network for **9 real educational institutions** including the one I studied. The locations spans in less than 1000 meter in radius of Kollam Junction Railway Station, Kerala, India.

This network leverages **RailTel**, Indian Railway's own high speed fiber backbone as the primary internet uplink, delivering connectivity to morethan **6,400 students and 700 staffs** across colleges, an Engineering school and two schools.

Every institution listed here, it's locatoin and geographic constraints are based on reality while student count, department, and staff counts are 60% accurate. 


---

## Why This Project Is Different

I have designed it out of a curiosity. While studying at Fatima Mata National College, I have thought of getting high speed internet and networks connecting between different institutions. Inorder to increase the complexity, I have added several security, access control and audit aspects to this project. My project consists of 9 sites, 3 distribution zones and full hierarchy. I am attampting to use GNS3 and packet tracer, with GNS3 running within a VM. Additionally, I will integrate Windows server and Linux VMs for DNS, DHCP, AAA, monitoring.


---

## Network At a Glance


```
                        ┌─────────────────────┐
                        │   KOLLAM JUNCTION   │
                        │   RAILWAY STATION   │
                        │   (Core NOC)        │
                        │   RailTel           │
                        └──────────┬──────────┘
                                   │
              ┌────────────────────┼────────────────────┐
              │                    │                    │
    ┌─────────▼──────┐   ┌────────▼───────┐   ┌────────▼───────┐
    │  CHURCH ZONE   │   │  SN  ZONE      │   │  SCHOOL ZONE   │
    │  10 Gbps       │   │  1 Gbps        │   │  1 Gbps        │
    └───────┬────────┘   └───────┬────────┘   └───────┬────────┘
            │                    │                    │
    ┌───────┴────────┐   ┌───────┴────────┐   ┌───────┴────────┐
    │ • Fatima Mata  │   │ • SN Law       │   │ • Kristhraj    │
    │   National     │   │ • SN Womens    │   │   School       │
    │   College      │   │ • SN College   │   │ • SN Central   │
    │ • Bishop Jerome│   │   (Co-ed)      │   │   School       │
    │   Engineering  │   └────────────────┘   └────────────────┘
    │ • Bishop       │
    │   Architecture │
    └────────────────┘
```

---
## Institutions Covered

|   Code   |            Institution            |       Type        |   Backbone   |Students| Staff |
|----------|-----------------------------------|-------------------|--------------|--------|-------|
| KEN-DC   | Railway NOC                       | Core              | RailTel      | —      | —     |
| KEN-FMNC | Fatima Mata National College      | Arts & Science    | 10 Gbps      | 2,000  | 200   |
| KEN-BJEC | Bishop Jerome Engineering School  | Engineering + HPC | 10 Gbps      | 1,000  | 100   |
| KEN-BJA  | Bishop Institute for Architecture | Architecture      | 10 Gbps      | 200    | 20    |
| KEN-SNL  | SN Law College                    | Law               | 1 Gbps       | 500    | 30    |
| KEN-SNW  | SN Womens College                 | Arts & Science    | 1 Gbps       | 500    | 50    |
| KEN-SNC  | SN College (Co-ed)                | Arts & Science    | 1 Gbps       | 2,000  | 180   |
| KEN-KRS  | Kristhraj School                  | School (K-12)     | 1 Gbps       | 700    | 30    |
| KEN-SNT  | SN Central School                 | School (K-12)     | 1 Gbps       | 500    | 30    |


---

## Technology Stack

### Network Simulation
|         Tool            |                         Purpose                               |
|-------------------------|---------------------------------------------------------------|
| **Cisco Packet Tracer** | Layer 2/3 campus switching, OSPF, VLANs, DHCP, basic security |
|   **GNS3 (VMware)**     | Firewall (Cisco ASA), advanced routing, VoIP, HPC design      |
| **GNS3 & Packet Tracer**| Loopback interconnect for unified simulation                  |


### Server Infrastructure (VMware VMs)


|       Platform            | Role |
|---------------------------|-------------------------------------------------------------------------------|
|     **Windows Server**    | Active Directory, DNS, DHCP, RADIUS (NPS)                                     |
| **Linux (Ubuntu Server)** |                                                                               |
| **Docker (on Linux VM)**  |                                                                               |


### Protocols and Technologies
```
Routing          : OSPFv2 Multi-Area (9 areas)
Switching        :  VLANs, Inter-VLAN routing
Security         : 802.1X, RADIUS, ACLs, DAI, DHCP Snooping, Port Security
Wireless         : WiFi 6 (802.11ax), WPA3-Enterprise, Captive Portal
Authentication   : 802.1X EAP-TLS, Active Directory backend
VoIP             : SIP, PSTN Gateway, QoS DSCP EF
HPC              : Dedicated 10GbE VLAN, Jumbo Frames, LACP EtherChannel
Monitoring       : SNMPv3, NetFlow v9, Syslog, LibreNMS
Automation       : Ansible (planned — Phase 17)


## IP Addressing Summary

Master block: **10.10.0.0/16**

|            Site               |    Subnet   | Prefix | Usable Hosts |
|-------------------------------|-------------|--------|--------------|
| Railway Core (KEN-DC)         | 10.10.252.0 |   /24  |     254      |
| Fatima College (KEN-FMNC)     | 10.10.0.0   |   /19  |    8,190     |
| SN College (KEN-SNC)          | 10.10.32.0  |   /19  |    8,190     |
| Engineering (KEN-BJEC)        | 10.10.64.0  |   /19  |    8,190     |
| SN Womens (KEN-SNW)           | 10.10.96.0  |   /19  |    8,190     |
| SN Law (KEN-SNL)              | 10.10.128.0 |   /20  |    4,094     |
| Architecture (KEN-BJA)        | 10.10.144.0 |   /20  |    4,094     |
| Kristhraj School (KEN-KRS)    | 10.10.160.0 |   /21  |    2,046     |
| SN Central School (KEN-SNT)   | 10.10.168.0 |   /21  |    2,046     |


## Project Phases

| Phase |             Description            |    Status   |
|-------|------------------------------------|-------------|
| ✅ 01 | Project Overview and Scope         | Complete    |
| ✅ 02 | Business Requirements              | Complete    |
| ✅ 03 | Site Survey and Geography          | Complete    |
| ✅ 04 | Physical Design                    | Complete    |
| ✅ 05 | Logical Design                     | Complete    |
| ✅ 06 | IP Addressing and VLSM             | Complete    |
| 🔄 07 | VLAN Design                        | In Progress |
| ⏳ 08 | Routing — Multi-Area OSPF          | Pending     |
| ⏳ 09 | Switching — STP, EtherChannel      | Pending     |
| ⏳ 10 | Wireless Design                    | Pending     |
| ⏳ 11 | VoIP                               | Pending     |
| ⏳ 12 | Security — 802.1X, Firewall, ACLs  | Pending     |
| ⏳ 13 | Data Centre Design                 | Pending     |
| ⏳ 14 | AI and HPC Interconnect            | Pending     |
| ⏳ 15 | GNS3 Simulation                    | Pending     |
| ⏳ 16 | Packet Tracer Simulation           | Pending     |
| ⏳ 17 | Automation                         | Pending     |
| ⏳ 18 | Testing and Verification           | Pending     |
| ⏳ 19 | Post Implementation Review         | Pending     |
| ⏳ 20 | Future Expansion Roadmap           | Pending     |

---


## Key Design Highlights
### Three-Tier Hierarchy:
  Core (Railway NOC) -> Distribution (zone) -> Access (per institution)

### Layered Security
  Port security, ACL, 802.1x port authentication, firewalls, DHCP snooping, Dynamic ARP inspection, etc

### HPC and AI Ready
  Dedicated 10GbE interconnect between Fatima Mata, Engineering school and Architecture school enabling remote HPC cluster access across these three institutions. HPC will be installed at the Engineerign School premise.

### Full VoIP


###  Unified Public WiFi
  Public WiFi access under OTP system, directly handled by Railtel ISP. OTP authentication via mobile phone number.


### School-Specific Policy
  No WiFI access for under 18 students or students at schools. Internet access via Content filtering DNS, and time bsaed ACL. Staff only wireless access. 


## Repository Structure

```
KRCMN/
├── 01_Project_Overview/       # Scope, objectives, stakeholders
├── 02_Business_Requirements/  # Institution requirements analysis
├── 03_Site_Survey/            # Geography, distances, constraints
├── 04_Physical_Design/        # Fiber topology, device placement
├── 05_Logical_Design/         # Network architecture, topology diagrams
├── 06_IP_Addressing/          # VLSM tables, subnet allocation
├── 07_VLANs/                  # VLAN design, trunking, inter-VLAN routing
├── 08_Routing/                # Multi-area OSPF configuration
├── 09_Switching/              # STP, EtherChannel, port security
├── 10_Wireless/               # WLC, AP placement, SSID design
├── 11_VoIP/                   # Asterisk PBX, SIP, PSTN gateway
├── 12_Security/               # Firewall, 802.1X, ACLs, hardening
├── 13_Data_Center/            # Server infrastructure, VMware, Docker
├── 14_AI_HPC/                 # GPU cluster, HPC interconnect design
├── 15_GNS3/                   # GNS3 project files and topology
├── 16_PacketTracer/           # Packet Tracer .pkt simulation files
├── 17_Automation/             # Ansible playbooks, scripts
├── 18_Testing/                # Test plans, results, verification
├── 19_Post_Implementation/    # Review, lessons learned
├── 20_Future_Work/            # Expansion roadmap, IPv6, SD-WAN
└── README.md                  # This file
```

---

## About This Project

This project is designed and built by Mukundakrishnan S, as a professional portfolio demonstrating enterprise network engineering capability beyond standard CCNA scope. 

I have picked the institution I studied in the past and the surrounding institutions to bring in more reality, and a real world problem( this project can be implemented by the management to bring in more opportunities to the students) Since I haven't done much of real worl enterprise level designing or implementation, I have created an LLM tool to act as my senior network engineer and correct my mistakes if any. 


**Connect:**
- LinkedIn: [https://www.linkedin.com/in/mukundakrishnansk/]


