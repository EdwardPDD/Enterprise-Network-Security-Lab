# Enterprise Network Security Lab

## Project Overview

I created this project to design and secure an enterprise network from the ground up using Cisco Packet Tracer. Instead of starting with a predefined topology or following a step-by-step lab, I wanted to approach the project more like a real network deployment.

I started by creating a business scenario and gathering requirements before choosing any equipment or building the topology. My goal is to work through the same types of decisions I would need to make in a networking or security role, including capacity planning, redundancy, network segmentation, routing, access control, secure management, and troubleshooting.

This project also serves as a hands-on refresher for networking concepts I have previously studied while giving me an opportunity to apply them together in a larger environment.

The network represents a growing company with a headquarters in Florida, a branch office in Georgia, and remote employees throughout the United States.

---

## Business Scenario

The simulated company currently has approximately:

- 120 employees at its Florida headquarters
- 30 employees at its Georgia branch
- Remote employees located throughout the United States

The headquarters occupies three floors and contains the company's primary server room and network infrastructure. The Georgia branch is smaller and contains a secured network closet but does not host major production servers.

The company operates on a hybrid schedule, with approximately 80–90 HQ employees potentially working on-site at the same time.

I also wanted the design to account for future growth rather than only supporting the company's current size. For planning purposes, I am allowing for approximately **25% employee growth over the next three to five years**, as well as the possibility of additional branch locations in the future.

---

## Requirements Gathering

Before drawing the topology, I worked through the business requirements I would want to understand before designing a real network.

Some of the questions I considered were:

- Are employees primarily on-site, remote, or hybrid?
- Are the offices located near each other or geographically separated?
- Does HQ have a dedicated server room?
- What infrastructure is available at the branch?
- What is the available networking budget?
- Does the company operate its own intranet and internal services?
- What departments and user groups need to be supported?
- What devices will connect to the network?
- Which users should have access to sensitive resources?
- How much downtime can the business tolerate?
- What happens to the branch if its connection to HQ fails?
- How should remote employees access internal resources?
- Is there an existing IP addressing scheme that needs to be preserved?

One thing I learned from this part of the project is that designing the topology should not be the first step. The network design should come from understanding what the organization actually needs.

I grouped the requirements into five areas that I can use throughout the project:

**Users → Devices → Applications → Security → Availability**

---

## Business Requirements

Based on the requirements gathering process, my network needs to support:

- Approximately 120 HQ employees with room for growth
- Approximately 30 branch employees
- Hybrid and remote workers
- Multiple business departments
- Wired and wireless corporate connectivity
- Guest wireless Internet access
- IP phones
- Network printers
- Wireless access points
- Security cameras and IoT devices
- Internal servers and intranet resources
- Secure communication between HQ and the branch
- Secure remote access to authorized resources
- Separation between different users and services
- Restricted access to network-management interfaces
- Redundancy for critical HQ infrastructure
- Continued branch Internet connectivity if connectivity to HQ is lost
- Possible future expansion to additional offices

---

# Initial Network Design

## HQ Architecture

For headquarters, I decided to use a **collapsed core/distribution architecture**.

Because this is a medium-sized environment rather than a large campus or data center, I did not think a full three-tier access/distribution/core architecture was necessary.

At the same time, I did not want the entire headquarters to depend on one central switch.

I therefore decided to use:

- **2 core/distribution switches**
- Multiple access switches distributed throughout the building
- Redundant uplinks from the access layer toward both core/distribution switches

This gives the network a simpler architecture while still providing redundancy for critical switching infrastructure.

---

## Internet Edge and Perimeter

Availability was one of the first things I considered when designing the Internet edge.

Using two Internet connections would not provide much protection if both connections ultimately depended on the same router or firewall.

For that reason, my initial HQ perimeter design includes:

- 2 ISP connections
- 2 edge routers
- 2 firewalls operating as a redundant pair
- Redundant connectivity toward the internal network

My reasoning was to avoid creating an obvious single point of failure at the Internet perimeter.

As I build the network, I will continue evaluating whether each level of redundancy is justified by the business requirements and project scope.

---

## Access Layer Capacity Planning

Rather than choosing an arbitrary number of access switches, I estimated the number of physical connections the headquarters may eventually require.

The company currently has approximately **120 employees**.

Allowing for 25% growth:

**120 × 1.25 = 150 employees**

Because an employee computer can connect through an IP phone where appropriate, I am estimating approximately one physical access-switch port per employee workstation/phone location.

Additional devices include approximately:

- 8 wireless access points
- 6 network printers
- 8 security cameras/IoT devices

This gives me an initial estimate of approximately:

**150 + 8 + 6 + 8 = 172 access ports**

I chose **48-port access switches** for the design.

Four switches would provide 192 ports, which technically meets the estimate but leaves little additional capacity.

Five switches provide:

**5 × 48 = 240 available ports**

That provides more flexibility for additional equipment, changes in floor layouts, and future expansion.

I therefore decided on:

- **5 production access switches**
- **1 additional access switch kept as a cold spare**

The sixth switch is not intended to operate as another production switch. I wanted an on-site replacement available in case one of the production access switches experiences a hardware failure.

This also means switch configurations should be backed up so the spare can be configured and deployed quickly if needed.

---

## Access Switch Distribution

HQ occupies three floors:

| Floor | Approx. Employees | Departments | Access Switches |
| Floor 1    | 50         | Operations / IT    | 2 |
| Floor 2    | 45         | Sales / Management | 2 |
| Floor 3    | 25         | Finance / HR       | 1 |

I chose a **2-2-1 distribution** based on the number of employees and devices expected on each floor.

I also decided not to dedicate every physical switch to a single department.

Instead, physical switch placement will primarily follow the building layout and endpoint locations, while **VLANs will provide logical separation between departments and services**.

This gives me more flexibility if departments grow or move without requiring the physical network design to change every time.

---

## Building Backbone

The two core/distribution switches will be located in the secured server room on the first floor.

Access switches will be located in telecommunications closets throughout the building.

For connections between floors, I chose **fiber uplinks** rather than relying on copper Ethernet.

My reasoning includes:

- Higher backbone bandwidth
- Support for longer cable distances
- Resistance to electromagnetic interference
- Better suitability for carrying aggregated traffic from many endpoints

Copper Ethernet will primarily be used for endpoint connectivity within each floor.

Each production access switch will eventually have connectivity toward both core/distribution switches so that the failure of a single uplink or core/distribution switch does not unnecessarily isolate the access layer.

---

# Security Goals

Security will be built into the network as the project develops rather than added only after connectivity is working.

Some of the requirements I plan to address include:

- Separating corporate and guest traffic
- Segmenting departments and services using VLANs
- Protecting Finance/HR resources
- Creating dedicated network-management access
- Preventing normal users from accessing infrastructure management interfaces
- Securing communication between HQ and the branch
- Providing secure access for remote employees
- Controlling traffic between network segments
- Using secure protocols for network-device administration
- Securing unused switch ports
- Testing both permitted and denied traffic

I will document the specific technologies used for these controls as I implement and verify them.

---

# Project Phases

I plan to build the network in stages:

1. Requirements gathering and capacity planning
2. Logical and physical topology design
3. IPv4 addressing and subnet planning
4. VLAN and segmentation design
5. Layer 2 switching and redundancy
6. Inter-VLAN and dynamic routing
7. Internet and WAN connectivity
8. DHCP and supporting network services
9. NAT/PAT
10. Access-control implementation
11. Secure network-device management
12. Branch connectivity
13. Security hardening
14. Connectivity and security testing
15. Failure and redundancy testing
16. Troubleshooting and validation
17. Final documentation

---

# Current Project Status

**Current Phase: Network Planning and Architecture Design**

So far, I have:

- Gathered the initial business requirements
- Identified current and projected network capacity
- Selected a collapsed core/distribution architecture
- Planned dual ISP connectivity for HQ
- Planned redundant edge and firewall infrastructure
- Selected two core/distribution switches
- Estimated HQ access-layer capacity
- Selected five production access switches
- Added one cold-spare access switch
- Distributed the access layer across the three HQ floors
- Selected fiber for the inter-floor backbone

## Next Step

My next step is to create the **logical network architecture diagram** and use it to begin planning the VLAN structure and IPv4 addressing scheme.

As the project develops, I will continue documenting the reasoning behind my design decisions, the configurations I implement, problems I encounter, and how I verify that the final network works as intended.
