# Project Overview

## Secure Corporate Network Infrastructure & SOC Defense Monitoring

**Project type:** Enterprise network engineering and cybersecurity simulation  
**Platform:** Cisco Packet Tracer  
**Status:** Implementation and testing in progress

## 1. Executive summary

This project models a corporate headquarters and remote branch network. It demonstrates segmented departmental networks, Layer 3 switching, dynamic routing, redundant default gateways, centralised network services, secure device administration and basic centralised event collection.

The environment is built and tested in Cisco Packet Tracer. It is an educational simulation, not a production deployment.

## 2. Objectives

- Design a hierarchical headquarters and branch network.
- Segment departments using VLANs and IP subnets.
- Configure inter-VLAN routing on multilayer switches.
- Implement OSPF for dynamic route exchange.
- Configure HSRP for headquarters default-gateway redundancy.
- Provide DHCP, DNS and an internal web service.
- Apply ACLs and switch port security.
- Secure device administration with SSH.
- Forward network-device events to a central Syslog server.
- Simulate security events and record observed outcomes.

## 3. Device inventory

| Device | Model / type | Role |
|---|---|---|
| HQ-R1 | Cisco 2911 | Headquarters edge router |
| ISP-R1 | Cisco router | Simulated ISP/transit router |
| BRANCH-R1 | Cisco 2911 | Branch edge router |
| HQ-CORE-1 | Cisco 3560-24PS | Primary headquarters Layer 3 core |
| HQ-CORE-2 | Cisco 3560-24PS | Secondary headquarters Layer 3 core |
| HQ-ACCESS-1 | Cisco 2960 | Headquarters access switch |
| HQ-ACCESS-2 | Cisco 2960 | Headquarters access switch |
| BR-CORE-1 | Cisco 3560-24PS | Branch Layer 3 core |
| BR-ACCESS-1 | Cisco 2960 | Branch access switch |

## 4. VLAN and addressing plan

### Headquarters

| VLAN | Name / purpose | Subnet | Default gateway |
|---:|---|---|---|
| 10 | HR | 192.168.10.0/24 | 192.168.10.1 |
| 20 | Finance | 192.168.20.0/24 | 192.168.20.1 |
| 30 | IT | 192.168.30.0/24 | 192.168.30.1 |
| 40 | Guest | 192.168.40.0/24 | 192.168.40.1 |
| 50 | SOC / servers | 192.168.50.0/24 | 192.168.50.1 |
| 99 | Management | 192.168.99.0/24 | 192.168.99.1 |

### Branch

| VLAN | Name / purpose | Subnet | Default gateway |
|---:|---|---|---|
| 10 | HR | 192.168.110.0/24 | 192.168.110.1 |
| 20 | Finance | 192.168.120.0/24 | 192.168.120.1 |
| 30 | IT | 192.168.130.0/24 | 192.168.130.1 |
| 99 | Management | 192.168.199.0/24 | 192.168.199.1 |

### Headquarters server addresses

| Server | Address |
|---|---|
| DHCP/DNS | 192.168.50.10/24 |
| Internal web server | 192.168.50.11/24 |
| SOC Syslog server | 192.168.50.12/24 |

The listed gateways are the configured gateway addresses in the addressing plan. HSRP failover behaviour remains subject to a separate test.

## 5. Technologies and controls

- **VLANs:** Departmental and service segmentation.
- **Inter-VLAN routing:** Layer 3 switching at the headquarters and branch.
- **OSPF:** Dynamic routing between network segments and sites.
- **HSRP:** Redundant virtual gateways at headquarters.
- **DHCP and DNS:** Centralised address allocation and name resolution.
- **ACLs:** Guest network restrictions with DNS permitted to the central DNS server.
- **Port security:** Sticky MAC learning, MAC limits and restrict-mode violation handling.
- **SSH:** Version 2 remote administration using local authentication.
- **Syslog:** Centralised collection of messages from nine network devices.

## 6. Security incident simulations

### 6.1 Unauthorised endpoint / port-security violation

A test access port (HQ-ACCESS-1 Fa0/5) was configured for one secure MAC address. After the first test endpoint was learned, a second endpoint generated a port-security violation.

Observed message:

`PORT_SECURITY-2-PSECURE_VIOLATION`

Reported offending MAC: `0010.11D2.A42A`  
Interface: `FastEthernet0/5`

The event was observed on the switch and received by the central Syslog server. The port was configured in restrict mode.

### 6.2 Failed SSH authentication

An SSH login attempt using an unauthorised username and incorrect password was rejected by HQ-R1. Related messages were observed on HQ-R1 and the SOC Syslog server.

### 6.3 Guest network isolation

A Guest endpoint was able to resolve `www.corporate.local` through DNS at `192.168.50.10`. Its ICMP test to the internal web server at `192.168.50.11` was blocked, and the internal website was inaccessible from the Guest PC.

These tests demonstrate the observed Packet Tracer ACL behaviour. They do not constitute a full penetration test.

## 7. Verification register

| Test / capability | Evidence status |
|---|---|
| OSPF neighbour formation and route propagation | Verified |
| HQ-to-branch connectivity | Verified |
| DHCP addressing on end devices | Verified |
| DNS name resolution | Verified |
| Internal web service reachable from authorised network | Verified |
| HSRP active/standby roles | Observed |
| HSRP failover under core-switch failure | Pending |
| SSH administrative access | Verified |
| Failed SSH authentication rejection and Syslog messages | Verified |
| Port-security violation and central Syslog receipt | Verified |
| Syslog delivery from all nine network devices | Verified |
| Guest DNS access | Verified |
| Guest access to internal web server | Blocked as intended in test |

## 8. Known scope and limitations

- Cisco Packet Tracer provides a simulated environment; results may differ from physical Cisco equipment.
- The SOC component is centralised Syslog collection, not a full SIEM platform with correlation rules, dashboards, alert triage or case management.
- HSRP failover and remaining final validation tasks must be completed before the project is described as fully tested.
- The branch Finance endpoint's reported address should be reconciled against the approved branch subnet during final validation.
- The test port Fa0/5 was repurposed for the port-security demonstration; its final intended VLAN and endpoint assignment should be reviewed before project handover.

## 9. Planned deliverables

- Cisco Packet Tracer topology file (`.pkt`).
- Network topology diagram.
- Device configuration extracts.
- VLAN and IP addressing schedules.
- Security policy and incident test records.
- Screenshots of routing, redundancy, Syslog and security tests.
- Final validation and handover report.

## 10. Disclaimer

This repository documents an educational Cisco Packet Tracer lab. IP addresses, credentials and device configurations are lab examples. Do not reuse lab passwords or expose operational credentials in a public repository.
