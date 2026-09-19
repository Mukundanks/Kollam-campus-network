///Chapter 6 – Logical Network Design

## 6.1 Overview

Logical design defines the business requirements, site survey findings, physical architecture and IP capacity planning based on the previous findings. These findings are then populated into the logical operating model for the KMRN network. 

While the physical design defines where the physical components belongs and what physical devicesor components we need for the project, logical section defines how they communicate between each other. These include how traffic is segmented, routers are connected and where the boundary exists for the network. Additionally, it also explains how the network remains scalable and resilient thorughout. 

### Logical Hirearchy
                KMRN Core
                   |
                   |
                   |
              Distribution
                   |
                   |
              Access Blocks
                   |
                   |
                   |
                Endpoints

The core provides high-speed connectivity between Distribution blocks and metropolitian backbone. The Distribution layer aggregates and enfore policies for individual campuses, while the Access layer provides connectivity to end devices. The traditional 3-tier architectural model is followed. The logical seperation enables the institution-level networks and shared metropolitian infrastructure. 

Here we use a deliberate logical seperation. The traditional three-tier model is particularly appropriste for larger campus environments with multiple functional distribution blocks. The collapsed core model is not being used here because it is more appropriate when the additional seperation doesnot justify the cost or complexity.

#### Design Assumptions & Standards
- IPv4 private addressing (10.10.0.0/16) used throughout
- All inter-campus routing uses Layer 3 links
- Open standards where practical
- Institutions maintain administrative independence while consuming 
  shared core services
- Standardized device configuration templates
- Five-year logical growth horizon with reserved address space

## 6.2 Enterprise Logical Architecture


#### 6.2.1 Purpose

The KMRN logical architecture defines how the metropolitian network is divided into these functional logical domains. This also explains how these domains interact with each other. Here, the architecture seperates the network into distinct areas based on function, trust level, traffic characteristics and operational responsibility. This is to avoid treating the project network as a flat network. 

The architecture consists of:

                         ┌─────────────────────┐
                         │    INTERNET / ISP   │
                         └──────────┬──────────┘
                                    │
                              ┌─────▼─────┐
                              │ KMRN CORE │
                              │   / DC1   │
                              └─────┬─────┘
                                    │
             ┌──────────────────────┼──────────────────────┐
             │                      │                      │
       ┌─────▼─────┐          ┌─────▼─────┐         ┌─────▼─────┐
       │  CAMPUS   │          │  SERVER   │         │ MANAGEMENT │
       │  NETWORKS │          │  NETWORKS │         │  NETWORKS  │
       └─────┬─────┘          └───────────┘         └────────────┘
             │
     ┌───────┼────────┬────────┐
     │       │        │        │
   FMNC     SNC     BJEC      ...
     │
 ┌───┴──────────────────────────────┐
 │ Student │ Staff │ Voice │ CCTV  │
 │ Guest   │ IoT   │ Labs  │ etc.  │
 └──────────────────────────────────┘


#### 6.2.2 Network Hierarchy

This is about the logical domains. Here the guest and WAN are treated as a seperate network function or domain rather than a normal peer to peer network. 

1. Enterprise core or DC1

It is the central routing and connectivity layer of the build. The primary responsibilities of the layer are:
     1. interconnecting the participating institutions
     2. providing layer 3 function
     3. connecting campus network to the shared services
     4. providing connectivity to the Internet
     5. providing logical boundary between the campus networks and shared infrastructure

Most importantly, the core functionality of the DC1 is to provide high speed dependable and predictable transport, not to enforce every campus specific policies. 

2. Campus Networks

Within the KRMN, every campus network is treatd as a seperate, independent logical division. The idea is to contain the traffic and data to the respective networks, and improve security. 
It can be illustrated this way:

                    KMRN CORE
                       │
              ┌────────┼────────┐
              │        │        │
             FMNC     SNC      BJEC
              │        │        │
           Campus   Campus   Campus
           Network  Network  Network


Then each within each campus, with a 10.0.0.0/16 network, it will be divided into: 

               FMNC
               10.10.0.0/19
                    │
                    ├── Student
                    ├── Faculty/Staff
                    ├── Administration
                    ├── Voice
                    ├── CCTV
                    ├── Servers
                    ├── Network Management
                    ├── Guest
                    └── Future




3. Server Networks
Server infrastructure is treated different from the campus endpoint networks. Within the server network, there are two main logical categories, which are divided based on their functions:

3.1 Centralised Server Networks:
These are located at the DC1 and provides core services to the participating institutions. They are not limitd to:
     1. DNS
     2. DHCP
     3. NTP
     4. AAA
     5. Monitoring
     6. Logging
     7. Backup

3.2 Institutional Server Networks

Each instituiton will have their own local server for services which must be hosted locally. These includes servers for academics, teaching and HR. 

4. Management Network

Network management must be logically seperated from the user traffic. This will ensure easy access to the device management in the event of an outage. The devices which needs management traffic are:
     * Routers
     * Core Switches
     * Distribution Switches
     * Access Switches
     * Wireless Infrastructure
     * Network controlled power equipments
     * Monitoring systems
     * Servers

It can be shown as: 
     Management Network
        │
        ├── Network Devices
        ├── Wireless Controllers/APs
        ├── Monitoring
        └── Servers

The core idea is that a student or staff device will be not be able to reach this network. This network will be secured with the security architectures defined.

5. WAN or the Backbone

Eventhough the KRMN acts like a WAN, where many local independent networks are connected via a cental backbone, it is better calld in a differetn way. The backbone is the layer 3 transport network connecting all the participating campuses into the central network infrastructure. As mentioned above, the backbone hsould transport traffic without becoming a unit for enforcing individual or campus endpoint policies. 

6. Guest Network

The guest model has a seperate logical domain because it is fundamentally treated as a seperate trust model. The guest endpoints will be treated untrusted by design. The traffic will travel through a seperate network, as defined below:

          Guest Device
               │
               ▼
          Guest VLAN
               |
               ▼
          Guest Gateway
               │
               ▼
          Internet

#### 6.2.3 DMZ
KMRN does not currently require a dedicated DMZ. It is because the proposed and identified service requirements do not include independently hosted public accessible servers. The guest WiFi is treated as part of the Guest access architecture rather than a DMZ. It is also worth notign that the Guest network has no access to any internal networks whatsoever. 

The design lets the option of introducing a possible DMZ in the future, if required. 

#### Proposed Logical Zones

|      Zone         |                               Purpose                                            |       Trust               |
|-------------------|----------------------------------------------------------------------------------|---------------------------|
|Core Infrastructure|                               Backbone                                           |        High               |         
|Campus User        |                               Students/Staff                                     |        Controlled         |
|Server             |                               DC services                                        |   High/Controlled         |
|Management         |                               Network devices                                    |    Highly trusted         |
|Voice              |                               IP Telephony                                       |      Controlled           |
|Wireless           |                               APs                                                |      Controlled           |
|CCTV               |                               Cameras                                            |      Controlled           |
|Guest              |                               Internet only                                      |       Untrusted           |
|DMZ                |                               N/A                                                |          N/A              |


          Desicion on Dedicated DMZ

          Status:        Not implemented in the current architecture
          Reason:        The current requirements does not justify the need to implementing a dedicated DMZ, with regards to the services it offers.
                         The untrusted member in this architecture, the Guest network is not accessign any internal resources, and it is placed in a seperate access architecture. 
          Future 
          consideration:A dedicated DMZ can be introduced later if the scope of the project changes, which including hosting of internet facing 
                        applications, or services which are need to be isolated
          Impact:       This avoids unncecssary architectural complexities and helps to bring the projct within the budget constraints. 

## 6.3 IP Addressing Strategy

This section covers the logic, rules and strategy behind the IP allocation.

### 6.3 IP Addressing Strategy
This subsection explains why the address space was designed this way, and the principles behind the planning and future use. 

1. Private Enterprise Address Space

KMRN uses 10.10.0.0/16 as its private IPV4 addressing space. This is not only because of the vast availability of the IP address, but also the single hierarchial address space for the whole network. This allows the institutions, infrastructure, services and future expansions to remain within a predictable addressing framework. in a /16 address, there exits 65, 534 usable host addresses from a total of 65536 addresses. 

This will allow the network to have sufficient address capacity for the current institutions and for future expansion. Keeping this one single address space helps in more centralised infrastructure while segmenting will help with security. Additionally, this will help to simplify the network in terms of route summarisation, and avoids the fragmented addressing. 

2. VLSM

The design is using VLSM to save addressess, but most imprortantly the following things. The sites in this network requires different addressign requirements. The best example is the schools require relatively small number of hosts compared to campuses. Also, infrastructure networks and management networks requires very few compared to the main campuses or schools. SO, rater than applying a unifrom subnet for all the cliemts, VLSM allows address blocks to be sized accordign to the actual and projected requirement.

3. Hierarchial addressing
KMRN uses hierarchial addressing. The enterprise address space is divided into institution level parent allocations. These parent allocations is subsequently subdivided into the designated VLANs. 

This helps to keep the relation between the units strong. It can be illustrated as:

Enterprise ---> Institution ---> VLAN ---> Endpoint

This will help in later for route summarisation, troubleshooting, security policies, DHCP, future expansion etc.

4. Institution first allocation

############# do this part later

#### 6.3.2 Parent Allocation Summary

The KMRN enterprise address space has been divided into hierarchical parent allocations. This is based on the projected requirements of participating institutions and shared infrastructure. The complete data of the allocations are maintained within the KMRN IP allocation register. This register serves as the authoritative IP addre4ssing source of truth. 

#### 6.3.3 Institution Allocation

The allocation is based on the current endpoint requirements with projected growth of the devices at a time. For example, a student might have more than one device like mobile phone and PC connected to the network at any time. Additionally, it is also based on the requirements of the infrastructure, the future growth and also considering the operational simplicity and efficiency. 

Each insitution receive an appropriately sized parent allocation based on the current requirements and future projections. The unused addresses will be reserved for the future requirements of the institutions. 

#### 6.3.4 VLAN Allocation Strategy
The VLAN networks are carved out from the parent subnet allocated to each institutions. 

It can be visually represented as:
          FMNC /19
          │
          ├── Student VLAN
          ├── Staff VLAN
          ├── Voice VLAN
          ├── Server VLAN
          ├── Management VLAN
          ├── CCTV VLAN
          └── Future VLAN capacity

SInce the VLANS are within the institution boundary, the carving will be on the following concept: 

          10.10.0.0/19
              ↓
        Institution boundary
              ↓
          10.10.x.x/yy
              ↓
          Functional VLAN



This will bring the final view as:


                KMRN Core
                 |
              FMNC /19
                 |
       ----------+----------
       |         |         |
    Student    Staff     Servers
      /xx       /xx        /xx


#### 6.3.5 Gateway Addressign Convention

The first usable address of each VLAN (.1) is used as the default gateway throughout the network. This is to ease the complexity and to increase the troubleshooting capabilities. Additionaslly, the next available host to a given range (.2 - xx) will be used for infrastructure or static assignments and the rest available hosts in the subnet will be allocated based on DHCP to endpoints. 

It is worth noting that the KMRN network is not equipped with FHRP technologies. This is a design decision. The physical design intentionally uses single core and distribution devices at specific locaitons. This is to save infrastructure costs. This design accepts single point of failure at the DC level. Introducing redundancy protocol at the core level deals with extra Layer 3 devices, and not justified at the current project scope. 

The absence of FHRP doesnot mean the network is immune to resilience. It is considered at other layers and implemented at the backbone connectivity, link redundancy etc. Here, gateway redundancy is simply not implemented. 

#### 6.3.6 Address Reservation Policy

The address is allocated on the following strategy. The exact numbers does not follow, but the policy follows this standard. 

               Subnet
               │
               ├── .0       Network address
               ├── .1       Default gateway
               ├── .2-.49   Network infrastructure
               ├── .50-.99  Servers / controlled static devices
               ├── .100-.229 DHCP pool
               ├── .230-.254 Future/static reserve
               └── Broadcast



## 6.4 VLAN Architecture
The VLAN architecture provides logical segmentationn within each institution in the KMRN. This is done while maintaining the hierearchical model established. Each institution gets parent IP allocation from the core pool, from where the functional VLANs are carved out. 

Instead of treating all the endpoints in the campus as a single layer 2 domain, VLAN design seperates them. They are seperated based on the users, infrastructure, services and specialised workloads, accordign to their function, security requirement and traffic characteristics. 

#### 6.4.1 VLAN Numbering Philosophy

KRMN aims to use a constant VLAN numbering scheme throughout the network, based on function. This helps administrators to identify the purpose of a VLAN from the number, and maintain same logical convention throughout the campuses. 

               |--------|------------------|-------------------------------------------------------------|
               |VLAN ID |    Function      |                      Design Purpose                         |
               |--------|------------------|-------------------------------------------------------------|
               |  10    |     Staff        |           Faculty and staff endpoints                       |
               |  20    |    Student       |           Student endpoints                                 |
               |  30    |     Admin        |           Administrative systems / endpoints                |
               | 40-49  |    LAB-xx        |           Individual lab network, multiple range possible   |
               |  50    |  HPC-Cluster     |           HPC/compute cluster workloads                     |
               |  51    |   GPU-Nodes      |           GPU/AI Compute nodes                              |
               |  60    |    VOIP          |           IP Telephony                                      |
               |  70    |    CCTV          |           Surveillance cameras and related endpoints        |
               |  99    |  Management      |           Network infrastructure and admin management       |
               | 100    |    GUEST         |           GUEST access                                      |
               | 200    | HPC Interconnect |           Dedicated HPC interconnect traffic                |
               | 999    | Native-blackhole |           Unused native VLAN                                |
               |-----------------------------------------------------------------------------------------|

The numbering philosophy deliberately leaves some unused numerical space between the major functions. This will help with future funtional VLANs without requiring the existing numbering scheme to be reorganised.

Here,the VLAN IDs are standardised by the design. But, IP subnets remains institution specific. For example, VLAN IDs for FMNC and SNC remains the same, but the IPs are allocated from the parent allocation for each institution.  This maintains both the functional consistency and addressing hierarchy. 

#### 6.4.2 Standard VLAN Template

The following table shows the standard logical segmentation and trust values of VLANs throughout the network. Here, not every VLAN necessarily needs to be instantited at every institution. The deployment depends completely on the services and operational requirement of the individual locations. For example, an institution without HPC cluster does not need to deploy the VLAN 50 and 51 or 200. The idea is to give KMRN a standard architecture, without forcing unnecessary complexities. 

               |--------|--------------------------------|---------------------------------------------------------------------|
               |VLAN ID |           Purpose              |                   Trust and access principles                       |
               |--------|--------------------------------|---------------------------------------------------------------------|
               |  10    |    Faculty and  staff          |           Controlled access to institutional services               |
               |  20    |    Student endpoints           |           Restricted access to internal endpoints                   |
               |  30    |    Admin users/sytems          |           Higher trust institutional access                         |
               | 40-49  |    Teachingg & research labs   |           Segmented by laboratory, when required                    |
               |  50    |    HPC-Environment             |           Restricted to authorised works and users                  |
               |  51    |    GPU-Nodes                   |           Restricted to authorised users and works                  |
               |  60    |    IP Phones                   |           QoS enabled voice traffic                                 |
               |  70    |    CCTV Cameras                |           Restricted to monitoring infrastructure                   |
               |  99    |    Network Device management   |           Highly restricted administratice access                   |
               | 100    |    GUEST Clients               |           Internet only / untrusted access                          |
               | 200    |    HPC Interconnect            |           Dedicated workload traffic, isolated from normal traffic  |
               | 999    |    Native-blackhole            |           Unused native VLAN                                        |
               |---------------------------------------------------------------------------------------------------------------|

#### 6.4.3 Native VLAN

Across KMRN network, VLAN 999 is used as the native VLAN. This is not used for any end users or servers or any services. This is done to prevent legitimate endpoint traffic from using the native VLAN traffic. This reduces the risk of VLAN hopping and double-tagging attacks. Thus, it seperates native VLAN which is required by the trunking mechanism with the VLANs carrying production traffic. Additionally, no production endpoints or service is assigned to this VLAN. 

## 6.5 Routing Architecture

The KMRN routing architecture uses dymanic layer-3 routing to provide autimatic route learning, path selection and convergence across the whole network. Static routing is not implemented, except where the routing requirement is simple or specific use cases. 

#### 6.5.1 Routing overview - Static vs Dymanic

Static routing is unsuited for the network project due to the scope of the network, devices and possible topology changes. This is also due to the fact that static routing does not support scalability out of the box or automatic adaptation of the network changes.

Dymanic routing is used because:
     * Supports automatic route discovery
     * Toloplogy aware path selection
     * route convergence following the link failures
     * reduced route maintaince
     * scalable for future expansion 

#### 6.5.2 IGP Selection
KMRN's Interior Gateway Protocol (IGP) uses OSPF. The possible protocols were EIGRP, IS-IS and OSPF. EIGRP was omitted because of its non-vendor neutral architecture, and IS-IS due to the complexity, and this is purely an enterprise level network. OSPF wins due to its open standard, summarisation, hierarchical and scalable architecture. The presence of areas limits the issues with link-state information and LSA flooding. 

#### 6.5.3 OSPF Area Design

KMRN uses a two level OSPF hierarchy. The backbone lies in area 0 and the institutions lies in area 1 to 9. This means one dedicated area for each institution. These areas connect with the area 0 through the respective Area Backbone Routers. This design provides a natural boundary for routing information, fault isolation, route summarisation, and future institutional administration. Area 0 will act as the central, inter area transit domain for the institutional areas. 

#### 6.5.4 Route Summarisation Strategy

It is aligned with the hierarchical VLSM addressing strategy established in section 6.3. Each institution has a contiguous parent address allocation containing its internal VLAN subnets. This allpws the institutions internal networks to be represented to the KMRN backbone by an appropriate summary prefix. This is preferable than addressing every individual VLAN prefix throughout the network. 
This helps with smaller routign tables, reduced inter-area routign information and LSA propagation, simpler backbone routing and improved fault isolation. 

#### 6.5.5 Default Route Distribution

THe default route is used by the IP to forward any packets with a destionation which is not found in any routing table. This is also called the gateway of last resort. Following the standard method, 0.0.0.0/0 is set at the KMRN edge and distributed through OSPF to the internal routing domains. 

#### 6.5.6 ECMP 
Equal Cost Multi path will be evaluated where the physical topology provides multiple paths with equal OSPF cost. If two or more valid paths have same metric, OSPF can install multiple next hop paths and distribute traffic across them. This helps in utilisation of both paths and resilience rather than leaving an available equal cost link idle. 
ECMP is revelant to KMRN since the presence of redundant links between the core and isntitutions. But it is implemented based on the final physical topology, link costs and routing design. 


## 6.6 High Availability

KMRN's high availability is based on link redundancy, routing convergence and alternative network paths where provided in the physical topology. There are places were redundancy is introduced and not. The design has accepted to go with single point of failures in some areas of the network, due to cost constraints. 

#### 6.6.1 First Hop Redundancy Protocol (FHRP)

FHRP is not implementedin the KMRN design. Protocols like FHRP, VRRP, and GLBP require more layer - 3 gateway devices to make it redundant. The current architecture intentionally uses single core and distribution devices at selected locations to control capital expenses. Introducing FHRP is left for a future update. 

The default gateway remains the physical layer - 3 device mentioned in section 6.3, and represents a SPOF. By design, it is an accepted way, not an unadressed design deficiency. 

#### 6.6.2 Gateway Redundancy

It is not provided at the sites where single layer - 3 devices are used. So, the gateway/device availability is limited by a single-device architecture. The network path availablility is improved through redundant links and routing designed. The server/service architeture decides the service availability. 
This helps in consistency among the logical and physical designs. 

#### 6.6.3 Link Redundancy
Link redundancy is used where multiple physical links connect the same logical netowrk devices. Protocols like LACP (IEEE 802.1AX) is used to form a Link Aggregation Group

The LACP helps to achieve increased aggregate bandwidth and protect against individual link failures. It also logically abstracts multiple physical links and reduces dependency on a single physical cable. The best part is the failed member link can be removed while the remaining links continue to forward the traffic. The important thing to keep in mind is it does not protect against connected switch, but just the links.

#### 6.6.4 Routing Redundancy

OSPF provides routing level redundancy. This is achieved where multiple layer - 3 paths are available. If a link fails, the OSPF can detect the topology change and recalculate the shortest path available. The protocol also removes unreachable paths and installs an alternative valid route. Additinally, since we have incorporated ECMP along with OSPF, it can also provide multiple forwarding paths. So, alternative layer - 3 paths are used rather than FHRP for routing redundancy in KMRN. There are demerits associated with this, but cost constraints justify this. 

The failure scenarios can be a core switch failure, which is accepted as SPOF. The next can be distribution switch failure, where the institution where the switch failed is only affected. Link failure is addressed by LACP or OSPF, while Fiber route is another SPOF. OSPF path failure is addressed by its reconvergence, while LACP failure is substituted by remaining members, subject to capacity. 

## 6.7 Network Services
DHCP — central vs distributed
DNS / NTP
AAA — RADIUS, TACACS+ (both already in your original brief's security 
design — reference, don't redecide)
Syslog / SNMP v3
IPAM

The KMRN architecture puts forward a common infrastructure for address allocation, name resolution, time synchronisation, authentication, monitoring, logging and IP address management. This is doen to reduce operational complexity for participating institutions, and to improve the troubleshooting capabilities. 

#### 6.7.1 DHCP

KMRN will use a centralisd DHCP service model, with the DHCP services hosted within designated server infrastructure reachable by institutional VLANs through the DHCP relay network. This provides centralised DHCP administration and avoids maintaining independent DHCP servers in the institutions. This also helps with constant address allocation policies. 
The scope of the DHCP remains institutional or VLAN specific, which was discussed earlier. Those critical infrastructure such as management and servers use static addressign rather than dymanic ones. 

#### 6.7.2 DNS/NTP 

DNS provides address resolution for KMRN's internal services or servers. The internal DNS records will support institutional services, network management and centrally hosted applications. 
NTP provides a consistent time source across the network. This is crucial for OSPF, monitoring, logging and incidents. 

#### 6.7.3 AAA

Centralised Authentication, Authorisation and Accounting is used throughout KMRN. Two protocols, RADIUS and TACAS+ is used, where the former is primarily used for user/network authentication while the latter is for administrative authentication and authorisation of network device access. 

The detailed AAA security is defined in section 6.9

#### 6.7.4 Syslog and SNMPv3

Syslog provides centralised collection os network-devices and events for monitorign, troubleshooting and security investigations. 
SNMPv3 provides authenticated anbd encrypted network management for supported infrastructure. SNMPv3 is preferred over earlier SNMP because it provides security features for management traffic. 

Syslog and SNMPv3 together provides centralised event visibility, device health monitoring, fault detection, troubleshooting, security-event correlation and historical monitoring data. 

#### 6.7.5 IPAM

IP Address Management (IPAM) is used in KMRN as an authoritative system for address allocation and utilisation tracking. The IPAM records maintains enterprise address space, institution parent allocations, VLAN subnets, network/broadcast address. gateway assignments. DHCP ranges, static and reserved addresses. utilisation and available capacity and future growth allocations. 

The KMRN IP Allocation Register remians the adressing source of truth established in section 6.3. IPAM provides operational mechanism for maintaining that information throughout the network lifecycle.  

## 6.8 Wireless Logical Design

The wireless logical design for KMRN follows the same logical segmentation model as the wired network for KMRN. The wireless clients are mapped towards appropriate institutional VLAN, and it is based on user role and access purpose. There is seperate SSIDs for students, staff and guests. 

#### 6.8.1 SSID

Each institution uses the follwoing standard SSID naming convention: 
          |---------------|---------------------------------------------------|-------------------------------------------------|
          |    SSID       |                    Purpose                        |                    Access                       |
          |---------------|---------------------------------------------------|-------------------------------------------------|
          |STAFF-[code]   |       Faculty and staff wireless access           |         Internal authorised users               |
          |STUDENT-[code] |       Student wireless access                     |         Student network services                |
          |PUBLIC-KRMN    |       Guest/Public wireless access                |         Internet only access                    |
          |---------------|---------------------------------------------------|-------------------------------------------------|

The [code] section represents the designated site code for each institution. This allows the SSIDs to be identified with its originating institution while maintaining a consistent naming standard. 
The public SSID is intentinally seperated from institutional user networks and follows the Guest netowrk security model defined in the VLAN architecture
As of now, an IoT SSID is not establised with the baseline design. This can be achieved by introducing a dedicated VLAN and SSID, if requirement is established. 

#### 6.8.2 VLAN Mapping
Each SSID maps directly to the corresponding logical VLAN within each institution. 

SSID Staff will be connected to VLAN-10 for staff, Student to VLAN 20 and Public to VLAN 100 throughout the institutions. 
This helps to maintain a seperation between wireless and wired clients, while performing the same functional role. Though using standard VLAN identifiers throughout the KMRN network, the wireless VLANs are very institutional specific IP subnets.

#### 6.8.3 Authentication

The Staff and Student use WPA3-Enterprise with IEEE802.1X authentication. Authentication is integrated using RADIUS via AAA architecture. The design provides individual user authentication, centralised management, encrypted wireless access, separation of user roles, and reduced dependence on shared wireless passwords. 

The Public/Guest access uses designated guest access mechanism and does not provide authenticated users with access to the internal institutional networks. It remains the part of Guest architecture define earlier, and OTP mechanism is used for authentication. 

#### 6.8.4 Roaming

Wireless roaming is possible with consistent SSID confguration across APs within each institution. This allows clients to move between locations within the institutions without unnecessary sesion interruption. 

Currently, Roaming is designed intra-campus only. By design, it does not require a single layer 2 wireless network throughout the project.


## 6.9 Security Architecture
Trust Zones, ACL Strategy, East-West / North-South Traffic, 
Micro-segmentation (conceptual)
Network Access Control — 802.1X
Device Management Security — SSH, HTTPS, SNMPv3, AAA (all already 
specified in  original brief's device hardening list — reference 
it here rather than re-deriving)


The security is imposed based on segmentation, least privilage access, controlled inter-zone comunication, authenticated network access and secure device management. Security controls are applied at the VLAN, routing, access control, and management layers. 

#### 6.9.1 Trust Zones
KMRN logical domains are treated as distinct security zones. 

     * Management zone is highly trusted, and the access is restricted to infrastructure administration
     * Server networks are controlled, and high trust. It hosts shared and institutional services
     * Campus networks are controlled, has staff, student and administartive end points
     * Guest network is untrusted and the access is internet only
     * Enterprise core is a transit infrastructure by design, and not a endpoint trust zone
     * WAN/MAN is transport domain and carries routed traffic between domains

The inter-zone communication is explicitly controlled rather than implicitly trusted. This is particularly for Guest, student, management and server networks. 

#### 6.9.2 ACL Strategy
The logical security policy between VLANs, institutions and shared services are enforced by implementing ACLs. The strategy assumes least privilage. They are:
     1. permit only required source to destination communication
     2. Deny unauthorised inter-VLAN and inter-zone traffic
     3. Restrict student access to staff and management networks
     4. Prevent guest from accessing internal KMRN networks
     5. Permit approved access to shared services
     6. Restrict infrastructure management traffic to authorised management sources
     7. Apply explicit controls to sensitive services such as CCTV, VoIP and HPC infrastructure. 

The ACL is placed in a way that the unwanted traffic is filtered at a location very close to the source. 

#### 6.9.3 East-West and North-South Traffic

East west traffic represents the traffic within the KMRN while north-south is the traffic entering or leaving the network. 
The traffic between student and staff, between campuses, campus and server networks, management and network infrstructure are some of the examples of east-west traffic. Campus to internet and guest to internet is the best example for north-south traffic. 
The traffic is controlled in east-west via VLAN segmentation, ACLs and routing boundaries while in north-south, it is done via internet security policies and firewalls. 

#### 6.9.4 Network Access Control - 802.1X

802.1X provides authenticated network access for wired and wireless clients. Staff and student wireless access uses WPA3 Enterprise with 802.1X, and authentication integrated with the Radius/AAA infrastructure. Access policy can be associated with the authenticated user or device role and unauthorised devices are prevented from receiving normal network access. 

#### Device Management Security

Network-device management follows the device hardening requirements defined in the original project brief. This section references those requirements. 
     * SSH to secure CLI administration
     * HTTPs for secure web based management
     * SNMPv3 for authenticated/encrypted monitoring
     * AAA for centralised authentication and authorisation
     * Management access restricted to the management network
     * Central logging and monitoring va Syslog & SNMPv3 services

## 6.10 QoS Strategy

KMRN QoS prioritises traffic according to application sensitivity and business importance, building on the DSCP marking requirements defined in the original brief. QoS is particularly relevant to VoIP, video, critical services and shared metropolitan links.

#### 6.10.1 DSCP Classification 

| Traffic Class   | DSCP                                | KMRN Application                                           |
| --------------- | ----------------------------------- | ---------------------------------------------------------- |
|  Voice          |  EF (46)                            | VoIP signalling/media requiring low latency and jitter     |
|  Video          | AF-class / defined video marking    | Video conferencing and other latency-sensitive video       |
|  Critical       | CS-class / defined critical marking | Critical infrastructure and selected priority applications |
|  Best Effort    | Default DSCP 0                      | Normal user, web, file-transfer and general traffic        |

EF for VoIP is retained from the original project brief and receives strict priority treatment where congestion occurs.

#### 6.10.2 QoS Policy

QoS is applied consistently across the KMRN traffic path:

          *Classify and mark traffic at the appropriate network edge.
          *Trust DSCP markings only from controlled/trusted sources.
          *Preserve required DSCP markings across routed KMRN infrastructure.
          *Use priority queuing for voice where congestion requires it.
          *Allocate bandwidth to critical and video traffic according to defined requirements.
          *Allow Best-Effort traffic to use remaining available capacity.
          *Avoid unnecessary QoS complexity where links are not congested.

QoS therefore provides preferential treatment during contention, rather than guaranteeing bandwidth under all conditions.

#### 6.10.3 QoS Scope

QoS is most significant on WAN/MAN and oversubscribed uplinks, where multiple institutions compete for shared bandwidth. Campus access links should primarily be designed with sufficient capacity; QoS provides protection for latency-sensitive applications when contention nevertheless occurs.

The detailed queueing model, bandwidth percentages and device-specific QoS configuration are implementation decisions and are outside the logical design baseline.

## 6.11 Traffic Flow Analysis

Traffic flows in KMRN are evaluated by source, destination, routing domain and security policy. The following flows represent the principal traffic patterns that the logical design must support.

| Traffic Flow             | Logical Path                                                                                              | Security / QoS Consideration                                                                                       |
| ------------------------ | --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| Student → Internet       | Student VLAN → Campus L3 gateway → KMRN Core → Internet edge → ISP                                        | Internet access permitted; internal networks remain inaccessible; Best Effort                                      |
| Student → Library Server | Student VLAN → Campus L3 gateway → KMRN routing → Library Server VLAN                                     | Permit only required library services; deny unrelated server access                                                |
| VoIP Call                | Voice VLAN → Campus gateway → KMRN routed network → Destination Voice VLAN                                | DSCP EF preserved; prioritised for low latency, jitter and loss                                                    |
| CCTV                     | CCTV VLAN → Campus gateway → NVR/monitoring server                                                        | Restrict traffic to authorised CCTV/NVR destinations; Internet access normally denied                              |
| Inter-campus             | Institution VLAN → Institution gateway/ABR → OSPF Area 0 → Destination institution ABR → Destination VLAN | Routed Layer-3 communication; ACLs control institution-to-institution access                                       |
| AI Cluster               | AI/GPU VLAN → Campus L3 gateway → HPC/AI network → GPU/HPC resources                                      | High-throughput traffic; restricted to authorised research workloads; HPC interconnect remains logically separated |

#### Traffic-Flow Principles

     *Layer-3 routing is maintained between institutions; KMRN does not stretch campus VLANs across the metropolitan backbone.
     *Security policy is applied according to the source and destination security zones rather than simply allowing routed connectivity.
     *Voice traffic receives QoS priority, while normal user traffic remains Best Effort.
     *High-volume AI/HPC traffic is contained within its designated network segments to minimise unnecessary impact on general campus traffic.
     *CCTV and management traffic are restricted to their required services and destinations.
     *Internet-bound traffic follows the centralised north-south path through the designated Internet edge.

## 6.12 Naming Standards

KMRN uses a consistent naming convention aligned with the site codes defined in the IP Allocation Register. Naming is structured so that the site, device role and device sequence can be identified without consulting the topology.

#### 6.12.1 Device Naming

Format:

     KRMN-[SITE]-[ROLE]-[NN]

Examples:

     KRMN-KEN-FMNC-ASW-01
     KRMN-KEN-FMNC-DSW-01
     KRMN-KEN-BJEC-ASW-01
     KRMN-KEN-BJEC-DSW-01

Where:

     KRMN = project/network identifier
     KEN-FMNC / KEN-BJEC = registered site code
     ASW = Access Switch
     DSW = Distribution Switch
     NN = sequential device number

The site code must match the authoritative IP Allocation Register; institution names should not be substituted for registered codes.

#### 6.12.2 Interface Naming

Physical and logical interfaces use the platform's native interface notation while documentation identifies the connected endpoint and purpose.

Examples:

     Gi1/0/1 — access/endpoint connection
     Gi1/0/48 — uplink
     Te1/1/1 — high-speed backbone/uplink
     Port-channel1 — LACP bundle
     Vlan10 — routed VLAN interface

Interface descriptions should identify the remote device, interface and link purpose where applicable.

VLAN naming is not lisitng here as it was defined in the **section 6.4.2**

#### 6.12.4 Loopback Naming

Loopback interfaces use:

Loopback0

The loopback address provides a stable logical endpoint for routing identification and network-management functions. Where required, additional loopbacks should follow sequential numbering and a documented purpose.

#### 6.12.5 OSPF Router IDs

OSPF Router IDs use a unique, stable 32-bit identifier for every participating router.

Recommended KMRN convention:

     <site/device-specific identifier>

The Router ID should be associated with the device's loopback identity where possible, rather than depending on a physical interface address. It must remain unique across the entire KMRN OSPF domain.

This provides consistent OSPF neighbour identification and avoids Router ID changes caused by physical-interface failure or address changes.

## 6.13 Logical Design Constraints

The KMRN logical design is subject to the following constraints:

#### 6.13.1 IPv4 Only

The baseline design uses IPv4 exclusively. IPv6 is outside the current project scope and is not included in the addressing, routing or security model.

#### 6.13.2 Private Addressing

KMRN uses the private 10.10.0.0/16 address space. VLSM provides institution-level allocation and efficient use of available address space. Internet connectivity therefore requires appropriate NAT at the external network boundary.

#### 6.13.3 Budget Constraints

The design operates within an education-sector, cost-conscious environment. High availability and redundancy are therefore applied selectively rather than eliminating every potential single point of failure.

Single-device core/distribution elements may remain accepted SPOFs where the cost of duplication is not justified. Logical scalability and standardisation are prioritised so that future upgrades can be introduced without redesigning the entire addressing architecture.

#### 6.13.4 Campus Autonomy

Each institution retains operational autonomy over its campus network while participating in the shared KMRN backbone.

This requires:

     * Institution-specific address ownership within the allocated parent subnet.
     * Independent campus VLAN and access policies.
     * Controlled inter-campus communication through the KMRN routed backbone.
     * Local administration of campus resources and endpoints.
     * Shared KMRN standards for addressing, routing, security and services.

KMRN therefore provides inter-campus connectivity and common architectural standards without requiring each institution to operate as a single shared Layer-2 network.


## 6.14 Design Decisions

The following decisions establish the baseline logical architecture for KMRN and provide the rationale behind the selected design.

|      Decision Area            |                 Selected Design                     |                                           Rationale                                                                           |
|-------------------------------|-----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Addressing                    | Private IPv4 `10.10.0.0/16` with VLSM               | Provides a structured address space for current requirements and future growth while minimising address                       |
|                               |                                                     |    wastage.                                                                                                                   |
| Address Allocation            | Institution parent subnet → VLAN subnets            | Establishes clear institutional ownership and allows VLAN networks to be summarised at the institution                        | 
|                               |                                                     |   boundary.                                                                                                                   |
| VLAN Architecture             | Function-based VLANs                                | Separates Staff, Student, Admin, Voice, CCTV, Management, Guest and HPC functions while maintaining a common KMRN             |
|                               |                                                     |  standard.                                                                                                                    |
| Inter-campus Connectivity     | Layer-3 routed backbone                             | Prevents unnecessary Layer-2 extension between institutions and provides clear routing and security                           |
|                               |                                                     |   boundaries.                                                                                                                 |
| IGP                           | OSPF                                                | Provides an open-standard, hierarchical routing protocol with support for summarisation, ECMP and multi-area                  |
|                               |                                                     |   operation.                                                                                                                  |
| OSPF Architecture             | Area 0 + one area per institution                   | Aligns routing boundaries with institutional and addressing boundaries and limits LSA propagation within individual           |
|                               |                                                     |   sites.                                                                                                                      |
| Route Summarisation           | Institution parent prefixes                         | Reduces routing-table complexity and contains internal VLAN addressing within each institution's summarised                   |
|                               |                                                     |   prefix.                                                                                                                     |
| Default Routing               | Centralised default-route origination               | Provides a consistent path for external/Internet destinations without requiring individual Internet routes at each            |
|                               |                                                     |   institution.                                                                                                                |
| ECMP                          | Used where equal-cost paths exist                   | Provides load sharing and path resilience where the physical topology supports multiple equal-cost                            |
|                               |                                                     |   routes.                                                                                                                     |
| Gateway Redundancy            | No FHRP in baseline                                 | The selected cost-constrained topology uses single L3 gateway devices at relevant sites; introducing HSRP/VRRP would require  | 
|                               |                                                     |  redundant gateway devices that are outside the baseline design.                                                              |
| Link Redundancy               | LACP where multiple physical links are available    | Provides aggregate bandwidth and protection against individual member-link failure without requiring redundant gateway        |
|                               |                                                     |    devices.                                                                                                                   |
| Network Access Control        | 802.1X with RADIUS                                  | Provides identity-based authentication for controlled wired/wireless access and supports centralised access                   |
|                               |                                                     |   policy.                                                                                                                     |
| QoS                           | DSCP-based classification                           | Prioritises latency-sensitive traffic such as VoIP while allowing normal traffic to operate as Best                           |
|                               |                                                     |    Effort.                                                                                                                    |
| Guest Access                  | Dedicated Guest VLAN with Internet-only policy      | Separates untrusted visitor traffic from institutional networks and supports the KMRN OTP   model.                            |
| HPC/AI Segmentation           | Dedicated HPC, GPU and HPC-interconnect networks    | Prevents high-volume research traffic  competing with normal campus traffic and provides appropriate security boundaries.     |
| Device Management             | Management VLAN + SSH/HTTPS/SNMPv3/AAA              | Restricts infrastructure administration to controlled management paths.                                                       |
| Campus Autonomy               | Institution-specific VLANs, addressing and policies | Allows each institution to operate independently while conforming to common KMRN standards and using the shared backbone.     |


## 6.15 Validation Matrix
Requirement → Design Feature — maps back to your Business Requirements 
doc (02_Business_Requirements). Good closing-the-loop section.

## 6.16 Logical Topology Diagram
Finish with completed logical topology diagram.




*Last updated: [18/09/2026]*
*Status: [ In Progress ]*