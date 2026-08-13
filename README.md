# Enterprise Network & Security Lab

## Overview

This project is a simulated enterprise network infrastructure and security environment designed and implemented using Cisco Packet Tracer.

The lab demonstrates practical skills in network design, VLAN segmentation, inter-VLAN routing, DHCP, wireless networking, access control, network device hardening, NAT/PAT, and enterprise network troubleshooting.

The environment was designed to simulate a small organization's network with separate departmental, server, management, and guest networks.

---

## Project Objectives

* Design a segmented enterprise network architecture.
* Implement VLAN-based network segmentation.
* Configure Layer 3 inter-VLAN routing.
* Implement centralized DHCP services.
* Configure secure management access using SSH.
* Apply ACLs to control traffic between network segments.
* Implement Guest Wi-Fi isolation.
* Configure NAT/PAT for Internet access.
* Configure and verify enterprise network infrastructure.
* Troubleshoot connectivity and DHCP failures.
* Validate the final configuration through systematic testing.

---

## Technologies Used

* Cisco Packet Tracer
* Cisco IOS
* Layer 2 Switching
* Layer 3 Switching
* VLANs
* 802.1Q Trunking
* Inter-VLAN Routing
* DHCP
* IPv4
* Access Control Lists (ACLs)
* SSH
* NAT/PAT
* Wireless Networking
* Network Hardening
* Network Troubleshooting

---

## Network Architecture

```text
                            ISP-SERVER
                           198.51.100.10
                                |
                              ISP-R1
                                |
                              R1-EDGE
                                |
                             SW1-CORE
                            Layer 3 Core
                                |
              +-----------------+-----------------+
              |                 |                 |
          SW2-ACCESS        SW3-ACCESS        SW4-ACCESS
              |                 |                 |
            Users           Endpoints          AP-GUEST
                                                  ))))
                                               LAPTOP-GUEST
```

---

## VLAN Design

| VLAN | Name         | Network         | Gateway      | Purpose                   |
| ---- | ------------ | --------------- | ------------ | ------------------------- |
| 10   | MANAGEMENT   | 192.168.10.0/24 | 192.168.10.1 | Network management        |
| 20   | IT           | 192.168.20.0/24 | 192.168.20.1 | IT department             |
| 30   | FINANCE      | 192.168.30.0/24 | 192.168.30.1 | Finance department        |
| 40   | HR           | 192.168.40.0/24 | 192.168.40.1 | Human Resources           |
| 50   | USERS        | 192.168.50.0/24 | 192.168.50.1 | General users             |
| 60   | SERVERS      | 192.168.60.0/24 | 192.168.60.1 | Server infrastructure     |
| 70   | GUEST        | 192.168.70.0/24 | 192.168.70.1 | Guest wireless access     |
| 99   | NETWORK-MGMT | 192.168.99.0/24 | 192.168.99.1 | Network device management |

---

## Infrastructure Components

### SW1-CORE

The core multilayer switch provides:

* VLAN configuration
* Layer 3 switching
* Inter-VLAN routing
* SVI gateways
* DHCP services
* ACL enforcement
* Routing toward R1-EDGE

### Access Switches

The access layer provides:

* End-device connectivity
* VLAN assignment
* 802.1Q trunking
* SSH management
* Network segmentation
* Access-port configuration

The access switches are:

* SW2-ACCESS
* SW3-ACCESS
* SW4-ACCESS

### R1-EDGE

The edge router provides:

* Internal-to-WAN connectivity
* Routing
* NAT/PAT
* Simulated Internet connectivity

### ISP-R1

ISP-R1 provides the simulated external network used to test WAN connectivity and NAT/PAT.

### ISP-SERVER

The simulated external server uses:

```text
IP Address: 198.51.100.10
Subnet Mask: 255.255.255.0
Gateway: 198.51.100.1
```

---

## DHCP

DHCP services are configured on SW1-CORE.

DHCP pools were configured for:

* VLAN 10
* VLAN 20
* VLAN 30
* VLAN 40
* VLAN 50
* VLAN 70

Guest DHCP configuration:

```text
 ip dhcp pool VLAN70-GUEST
 network 192.168.70.0 255.255.255.0
 default-router 192.168.70.1
 dns-server 8.8.8.8
```

Infrastructure and gateway addresses are excluded from the DHCP pools.

---

## Inter-VLAN Routing

SW1-CORE provides Layer 3 routing between VLANs using Switched Virtual Interfaces (SVIs).

Example:

```text
 interface Vlan70
 ip address 192.168.70.1 255.255.255.0
```

Inter-VLAN communication is controlled by security policies and ACLs.

---

## Guest Network Security

VLAN 70 is dedicated to guest wireless access.

The Guest network is isolated from internal networks using an extended ACL.

Guest clients are prevented from directly accessing:

* Management
* IT
* Finance
* HR
* Users
* Servers
* Network management

Permitted external connectivity remains available.

```text
  Guest VLAN 70
      |
      +----> Internal Networks  ❌
      |
      +----> Internet           ✅
```

DHCP traffic is explicitly permitted so Guest clients can obtain network configuration.

---

## Wireless Network

A Cisco Packet Tracer Access Point provides wireless connectivity for the Guest network.

The Guest laptop uses a WPC300N wireless module.

Wireless clients are connected to:

```text
      VLAN 70 - GUEST
```

The Guest laptop successfully obtains an IPv4 address through DHCP.

Example:

```text
       IP Address: 192.168.70.x
       Subnet Mask: 255.255.255.0
       Default Gateway: 192.168.70.1
```

---

## NAT/PAT

R1-EDGE performs NAT/PAT for internal networks.

Private internal addresses are translated to the WAN interface address when accessing external networks.

This allows multiple internal clients to share the available WAN address.

```text
 Internal Network
       |
    R1-EDGE
       |
    WAN/ISP
       |
 External Server
```

---

## Network Security

Security controls implemented in the lab include:

* VLAN segmentation
* Guest network isolation
* Extended ACLs
* SSH management
* Restricted management access
* Switch hardening
* Network segmentation
* NAT/PAT
* Dedicated management VLAN
* Separate server network

---

## SSH Management

Access switches were configured for secure remote management using SSH.

Management access is restricted using an access control list.

SSH provides encrypted administrative access instead of insecure Telnet.

---

## Routing

SW1-CORE provides a routed connection toward R1-EDGE.

The core-to-edge transit network uses:

```text
 10.255.255.0/30
```

The simulated external network uses:

```text
 198.51.100.0/24
```

---

## WAN Addressing

| Device     | Interface          | IP Address       |
| ---------- | ------------------ | ---------------- |
| SW1-CORE   | G0/1               | 10.255.255.1/30  |
| R1-EDGE    | Internal interface | 10.255.255.2/30  |
| R1-EDGE    | WAN interface      | 203.0.113.1/30   |
| ISP-R1     | WAN interface      | 203.0.113.2/30   |
| ISP-R1     | LAN interface      | 198.51.100.1/24  |
| ISP-SERVER | NIC                | 198.51.100.10/24 |

---

## Verification & Testing

The network was tested systematically using Cisco IOS verification commands and end-to-end connectivity tests.

### VLAN Verification

```text
 show vlan brief
```

VLANs were verified on the core and access switches.

### Trunk Verification

```text
 show interfaces trunk
```

VLAN 70 was verified as:

* Allowed on the trunk
* Active
* Forwarding
* Not pruned

### SVI Verification

```text
 show ip interface brief
```

The VLAN interfaces were verified as operational.

### Routing Verification

```text
 show ip route
```

Connected VLAN networks and the routed path toward R1-EDGE were verified.

### DHCP Verification

```text
 show ip dhcp pool
 show ip dhcp binding
```

DHCP pools and client leases were verified.

### ACL Verification

```text
 show access-lists
```

Guest traffic restrictions were verified.

### NAT Verification

```text
show ip nat translations
show ip nat statistics
```

NAT/PAT operation was verified.

---

## Connectivity Testing

The following tests were successfully performed:

```text
 Client → Default Gateway
 R1-EDGE → ISP-R1
 R1-EDGE → ISP-SERVER
 Internal Client → ISP-SERVER
 Guest Laptop → Guest Gateway
 Guest Laptop → ISP-SERVER
```

Guest clients were prevented from accessing restricted internal VLANs.

---

## Troubleshooting Performed

### DHCP Failure on Guest VLAN

The Guest DHCP pool initially lacked the required network statement.

The issue was corrected with:

```text
 network 192.168.70.0 255.255.255.0
```

### Guest DHCP Blocked by ACL

The Guest ACL initially prevented DHCP traffic.

The ACL was redesigned to explicitly permit DHCP traffic before applying Guest network isolation rules.

DHCP functionality was then successfully restored while maintaining Guest network isolation.

### Wireless Client Connectivity

The wireless laptop initially lacked the required wireless adapter.

A WPC300N wireless module was installed and the laptop was successfully associated with the Guest Access Point.

### WAN Connectivity

The WAN path was configured and tested between the enterprise edge router, simulated ISP router, and external server.

NAT/PAT was verified using the simulated external network.

---

## Skills Demonstrated

* Enterprise network design
* Cisco switching
* Layer 3 switching
* VLAN segmentation
* Inter-VLAN routing
* DHCP
* IPv4 addressing
* Wireless networking
* ACL implementation
* Network security
* SSH
* NAT/PAT
* Static routing
* Network troubleshooting
* Security policy implementation
* Infrastructure documentation

---

## Project Status

**Status: Completed and Tested**

The final Packet Tracer topology successfully demonstrates:

* Segmented enterprise VLAN architecture
* Departmental network separation
* Centralized DHCP
* Inter-VLAN routing
* Secure Guest Wi-Fi
* ACL-based network isolation
* SSH management
* NAT/PAT
* Simulated Internet connectivity
* End-to-end connectivity testing
* Network troubleshooting

---

## Author

**Leshoboro Letsie**

Bachelor of Engineering in Computer Systems and Networks (Honours)

Computer Systems and Networks Engineer

---

## Lab Environment

**Platform:** Cisco Packet Tracer

This project is a simulated enterprise environment created for professional portfolio and technical demonstration purposes.
