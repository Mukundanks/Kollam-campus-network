# Business Requirement

## Overview
This document covers the requirement of multiple educational institutions in Kollam area. This fictional educational network backbone is designed to connect 9 educational institutions with high-speed internet connectivity. 

The objective is to provide a secular, scalable and stable high-speed internet, which can potentially increase the teaching and research throughput of each institutions while enabling central IT services, resource sharing and future expansions. 

Unlike a CCNA project, this project focuses on enterprise network architecture, capacity planning, IP addressing strategy, routing design, security segmentation, and operational documentation.


## Business Problem
Currently, each institutions has their own independent internet access, and the setuo is often replicated. Some institution runs on older infrastructure and their upgradation is nearly due. 
Due to these issues, there is no possible way of interconnection or resource sharing. Faculties or students are forced to physically arrive on premises or pay huge amount to access add on services. Though these institutions are located very close to each other, this issue has limited their capacity to work as a team and being in innovations. The Railways has addressed this issue and proposed a collaboration to setup a centralised internet backbone, with a core center at Railway's premises. 

## Business Objectives
The proposed network shall connect all the institutions with high speed internet backbone, with the ability to interconnect institutions based on demands. This can provide centralised IT services, and offer a range of features like added layer of security on top of the institutional ones. The backbone can support future expansions without major overhaul and new institutions can be added to the network. While there is an option to interconnect these institutions, there will be a logical separation between them, which will prevent unnecessary dataflow between them, and preventing cross network attacks. Since educational institutions run at a limited budget, this centralised service can potentially help them to reduce operational complexities and setup secure communication between institutions at much lower cost. Additionally, the major objective is to help the institutions to adapt modern educational technologies and research workloads. 

## Per-Institution Requirements




## Functional Requirements
The network must support:
- Internet access to all campuses
- Centralised DNS and DHCP services ( where applicable)
- Secure remote administration
- CCTV integration
- Wireless connectivity
- Wired connectivity
- VoIP
- Authentication services
- Guest WiFi with restricted access
- Future HPC cluster integration

## Non-Functional Requirements
The network should provide:
1. Scalability - support new institutions without renumbering the existing network
2. Availability - critical services should be available in the event of individual campus network failure
3. Security - separate user groups for VLANs, ACL and firewall policies
4. Performance - low latency conenctivity based on the requirement. eg- HPC interconenct
5. Maintainability - consistent IP addressing, documentation and device naming
6. Reliability - redundant routing plans where applicable
7. Manageability - centralised network management using enterprise management tools

## Stakeholders

  |       Stakeholder       |   Responsibility        |                   
  |------------------------ |-------------------------|
  |  Railways               | Backbone provider       |
  |  Institution Management | Campus admin            |
  |  Network Ops team       | Daily ops               |
  |  System Admins          | Server infrastructure   |
  |  Faculty                | Teaching and research   |
  |  Students               | Internet consumers      |


  ## Project Scope

  ### Included:
  - Enterprise network design
  - VLSM
  - Campus LAN network
  - VLAN segmentation
  - Wireless AP
  - Routing
  - DNS & DHCP
  - VoIP
  - Network monitoring
  - Security
  - Documentation
  - Disaster recovery
  - Future expansion slots
 ### Excluded:
  - Cost estimation
  - Real procurement
  - Physical cable installation
  - Civil engineering works
  - Vendor-specific licensing
  - Government approvals

## Success Criteria

This project will be considered success if:
- Every institution has sufficient address space for current and future growth
- Campus network communicate securely through the backbone
- Centralised services operate at the core
- Network documentation allows another engineer to improve the capacity or troubleshoot it
- The architecture supports future expansion with limited changes to the current infrastructutre

## Design Principles
Usually the design princples are to *simplify the design*. But in here, I have tried to ** make it complex  **. This is to implement more technologies to this proejct and make it future proof. Additionally, I have tried to standardise the network across the institutions, so troubleshooting comes easy. Also, documentation is mandatory and publish it in a timely manner. 

## Assumptions
 - We assume every institution listed here will agree with the network infrastructure offered
 - The IP address range is available with the Railways for the usage
 - Fiber connectivity is made available through the backbone, and road authority approves it.
 - Campus locations have suitable locations or they are happy to build one for network equipments
 - Each institutions has their own IT team to maintain internal matters while central operation will be maintained by the Railways
 - Future slots are based on 5 year expansion.
 - Project is designed mainly with Cisco's standards.

  

---
*Last updated: [13/07/2026]*
*Status: [In Progress ]*
