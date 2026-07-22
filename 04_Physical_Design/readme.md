## Overview 
The physical design is drafted carefully after analysing site survey and the business requirements. Considering the depth of this project, various network designs were analysed, and selected. The reference taken was from Cisco's CVD, because it explains a scalable network design perfectly. The document explains the follwing key points like physical topology, device placement, cabling, backbone, hierarchy, and resilience of the network detailed below. 


## Three-Tier Hierarchy
The network layout is hierarchical by design. This three-tier architecture helps to breakup the network into modular layers or segments. This will help in assigning specific functions or roles to specific layers, and hence simplifing the network design. Also, when the network is modular, the elemts can be replicated and used across the network. This helps in reducing operational complexity and improving scalability. When the llayout is not tiered, for example a flat layout, failures will affect the network, and this reduces resiliency. Thus, the network is split into easy to understand, fault-isolatable layers. A three tiered hierarchy contains these layers:
  * Access layer - Acts as the endpoint of the network. Clients access the network via this layer, and provides high bandwidth connectivity. Here, PC, wireless devices, phones, CCTV cameras etc will be connected here.
  * Distribution layer - Simply, many access layers are interconnected via this layer. It also provides connectivity to services, and loweer the cost of netowrk operations. This layer increases network availability by limiting failures to small domains or segments. 
  * Core layer - Acts as the brain of the network. connectivity across LANs, and outside internet. The main responsibilities include inter-campus routing, high speed backbone, policy enforcement DC connectivity. 
Each layer provides different functionality to the network. Here, the three-tier architecture will be used for modularity and reusability.

### Why Three-tier?
There are different architectures like three-tier, spine-leaf and collapsed core or two-tier design. The data flow can be broadly classified into north/south and east/west. East/west is mostly within the nodes of the server, while other is leaving the server and to the clients. Here the data flow is noth/south style, so spine/leaf architecture is ruled out. Two-tier design is mostly for small areas, but considering the centralised nature of the project, and the railway core, three-tier system is selected. This lets the metro-network add additional institutions without redesigning the network. 

## Redundancy Model
  The metropolitian network must be resilient to failures. So the network is planned to support redundancy at every tier of the network. The redundancy is achieved at each tier of the hierarchy. 
  1. Access layer:
     Setting up the redundancy model for this layer took some critical thinking. It is because mostly the end devices are not enterprise machines, they are PC or handheld devies. Setting up a redundancy model in those is not ideal. The next place where it is possible is the end network devices. The acccess layer has many redundancy tecnhiques like dual-homing, Etherchannal, STP, and L3 switching. Each of them was carefully assessed, and the follwoing was selected based on the requirement.
     * Dual-homing- All the access layer network devices will be equipped with 2 input/output channels to avoid single failures.
     * Layer-2 - The schools (KRS and SNT) will be equipped with layer 2 STPs. This is to reduce capital and operational expenses. The schools have relatively low network usage.
     * Layer 3 - All the remaining higher education institutions will be made redundant redy by layer 3 switches. This overcomes the issue of STP loops with layer-2 in a large network, and reduces the time for the backup machine to go live. Addition to this, the institutions will have high-speed bandwidth in the near-future, and this will cover the need.
  2. Distribution layer:
     The possibe redundancy methods are stand-alone switch pairs and introducing port aggregation between them. They will be done in Layer-3 switches because the failure rates between access layer and distribution layer are generally higher.
  3. Core layer:
     The core layer redundancy is often the most expensive, as it requires all the high-performance network gears to be doubled and protocols introduced. Additional to the network devices, power is a major factor to be considered, so is cooling. 

## Backbone design
 Each institution here will act as seperate LANs and they will be conencted each other and to the central core via a high speed, low latency fiber backbone. It will aggregate data from various subnetworks. This setup will allow better redundance for the network, while reducing network outages and adding security. Additionally, centralising the network will help in adding better QoS and optimised data flow. 
 Here, the backbone network used will be a distributed core. This will support the three-tiered approcah, while supporting future upgrades. 
 The backbone will follow a hub and spoke design, where all institutions will be connected to the central core, and inter-institution will takes place through the core. The fiber will be laid from core to FMNC, then split to the adjacent sites. Similarly, for SNT and SN group, another fiber will be laid to the sites. 

//// add available types of fiber and the one using here. then short explanation of the mode. only do backbone. 

## Fiber Link Table 

## Device Placement Per Site

## Rack/Room Design — Railway NOC


## Cabling Standards


## Constraints Carried Forward from Site Survey



*Last updated: [22/07/2026]*
*Status: [In Progress ]*
