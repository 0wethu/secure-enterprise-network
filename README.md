# Secure Enterprise Network 🔐

### Cisco Packet Tracer | Network Engineering | Network Security

**Author:** Kwanele Khumalo  
**Project Type:** Personal Networking Project  
**Status:** Completed

A practical enterprise networking project built in Cisco Packet Tracer, demonstrating network segmentation, inter-VLAN routing, access control, secure remote administration, and connectivity testing.

---

## 📌 Table of Contents

- [Project Overview](#-project-overview)
- [Network Topology](#-network-topology)
- [VLAN and IP Addressing](#-vlan-and-ip-addressing)
- [Security Implementation](#-security-implementation)
- [Testing and Verification](#-testing-and-verification)
- [Technologies Used](#-technologies-used)
- [Project Structure](#-project-structure)
- [Limitations](#-limitations)
- [Author](#-author)

---

## 🎯 Project Overview

The objective of this project was to design, configure, and test a segmented enterprise network with controlled communication between departments.

The network separates administrative users, engineering users, application servers, guest devices, and management devices into dedicated VLANs. Security policies restrict unauthorised communication while allowing approved access to the internal application server.

I made the final network design and configuration decisions, carried out the verification tests, and documented the implementation.

### Project Objectives

- Design a structured enterprise network topology.
- Configure VLANs to separate network departments.
- Implement router-on-a-stick inter-VLAN routing.
- Apply Access Control Lists (ACLs) to control network traffic.
- Configure SSH for secure remote device administration.
- Restrict management access to an authorised management device.
- Test permitted and denied traffic and document the results.

---

## 🖥️ Network Topology
<img width="1920" height="1080" alt="Screenshot (99)" src="https://github.com/user-attachments/assets/af23def0-efdd-41ed-9505-352b5babe4ac" />


The network uses a Cisco router and managed switch to connect five logical network segments.

**Topology overview:**

- **R1:** Cisco 2911 router
- **S1:** Cisco 2960 switch
- **End devices:** Administration PC, Engineering PC, application server, Guest PC, and Management PC
- **Routing method:** Router-on-a-stick using IEEE 802.1Q trunking


---

## 🌐 VLAN and IP Addressing

| VLAN | Department | Network | Default Gateway |
|---:|---|---|---|
| 10 | ADMIN | `192.168.10.0/24` | `192.168.10.1` |
| 20 | ENGINEERING | `192.168.20.0/24` | `192.168.20.1` |
| 30 | SERVERS | `192.168.30.0/24` | `192.168.30.1` |
| 40 | GUEST | `192.168.40.0/24` | `192.168.40.1` |
| 99 | MANAGEMENT | `192.168.99.0/24` | `192.168.99.1` |

### Endpoint Addressing

| Device | IP Address | VLAN |
|---|---|---:|
| PC-ADMIN | `192.168.10.10` | 10 |
| PC-ENG | `192.168.20.10` | 20 |
| SRV-APP | `192.168.30.10` | 30 |
| PC-GUEST | `192.168.40.10` | 40 |
| PC-MGMT | `192.168.99.10` | 99 |

All listed endpoints use the subnet mask `255.255.255.0`.

### VLAN Configuration Evidence

<img width="1920" height="1080" alt="P3-VLANs png" src="https://github.com/user-attachments/assets/89c314ec-6b4e-4dad-ad6e-40295dd400a9" />


---

## 🔒 Security Implementation

### 1. VLAN Segmentation

Separate VLANs and IP subnets isolate the five departments, reducing unnecessary inter-department communication.

### 2. Inter-VLAN Routing

Router subinterfaces on R1 provide default gateways for the VLANs. The link between R1 and S1 uses an 802.1Q trunk that carries the required VLANs.

### 3. Access Control Lists (ACLs)

Extended ACLs enforce the project's traffic policies:

- Administration can access the application server over HTTPS.
- Engineering can access the application server over HTTPS.
- Administration and Engineering are prevented from communicating directly with each other.
- Guest traffic is denied access to the internal departmental, server, and management networks.
- Other unauthorised access to protected subnets is denied by the configured policies.

The Guest ACL permits other IP traffic after its internal-network deny rules. However, external connectivity is not demonstrated because the topology does not include an ISP or internet-facing router.

### 4. Secure Remote Administration

- SSH version 2 is enabled on R1 and S1.
- Telnet access is disabled on the configured VTY lines.
- A management access ACL permits SSH administration from PC-MGMT (`192.168.99.10`).
- Unauthorised management access attempts are blocked.

### 5. Switch Port Security Hygiene

Unused switch ports are administratively shut down to reduce the risk of unauthorised devices being connected to inactive ports.

---

## 🧪 Testing and Verification

The following tests were performed in Cisco Packet Tracer.

| Test | Expected Result | Outcome |
|---|---|---|
| Admin to application server over HTTPS | Allowed | Passed |
| Engineering to application server over HTTPS | Allowed | Passed |
| Guest to internal application server | Blocked | Passed |
| Admin to Engineering | Blocked | Passed |
| Engineering to Admin | Blocked | Passed |
| PC-MGMT SSH access to R1 | Allowed | Passed |
| PC-MGMT SSH access to S1 | Allowed | Passed |
| Unauthorised PC-ADMIN SSH access to S1 | Blocked | Passed |
| Routing table verification | Connected VLAN routes present | Passed |

### HTTPS Access

Administration and Engineering were able to access the internal application server over HTTPS.

<img width="1920" height="1080" alt="P7-Admin-HTTPS png" src="https://github.com/user-attachments/assets/c8859c4d-057e-4188-a536-93ee2c6a8f35" />


<img width="1920" height="1080" alt="P7-Engineering-HTTPS png" src="https://github.com/user-attachments/assets/ad49f8f4-3fb5-4bcb-8900-162844b15d50" />


### Access Restriction Tests

Tests confirmed that Guest traffic could not reach the protected application server and that direct communication between Administration and Engineering was blocked.

<img width="1920" height="1080" alt="P7-Guest-Blocked png" src="https://github.com/user-attachments/assets/d3fc0c72-1dd6-4126-8093-c5561e175c73" />


<img width="1920" height="1080" alt="P7-Engineering-Blocked-Admin png" src="https://github.com/user-attachments/assets/5167d5e5-c9f7-4e70-b53a-04bfe152ac91" />


<img width="1920" height="1080" alt="P7-Admin-Blocked-Engineering png" src="https://github.com/user-attachments/assets/833d0ac1-48e9-495f-92cb-65440e7761e8" />


### SSH Verification

Authorised SSH sessions from PC-MGMT to R1 and S1 were successful. An unauthorised SSH attempt from PC-ADMIN to S1 was unsuccessful.

<img width="1920" height="1080" alt="P3-SSH png" src="https://github.com/user-attachments/assets/3bb2c7cf-173e-4bc8-8dcb-63e8e32c2f9d" />




### Routing Verification

The router's routing table was inspected to verify that the VLAN networks were directly connected through their respective subinterfaces.

<img width="1920" height="1080" alt="P7-Routing-Table png" src="https://github.com/user-attachments/assets/1663edde-df45-4896-bb91-21ea74b77ee8" />


---

## 🛠️ Technologies Used

- **Cisco Packet Tracer** — Network design and simulation
- **Cisco IOS CLI** — Router and switch configuration
- **VLANs** — Logical network segmentation
- **IEEE 802.1Q** — VLAN trunking
- **IPv4** — Network addressing
- **Router-on-a-stick** — Inter-VLAN routing
- **Extended and standard ACLs** — Traffic filtering and management access control
- **SSH version 2** — Secure remote administration
- **HTTPS** — Secure application access testing

---

## 📁 Project Structure

```text
secure-enterprise-network/
├── README.md
└── Secure Enterprise Network/
    ├── secure-enterprise-network.pkt
    ├── configs/
    │   ├── R1-running-config.txt
    │   └── S1-running-config.txt
    └── screenshots/
        ├── 01-P1-Network-Topology.png
        ├── 03-P3-VLANs.png
        ├── 04-P3-Trunk.png
        ├── 08-P4-Subinterfaces.png
        ├── 14-P6-ADMIN-ACL.png
        ├── 15-P6-ENG-ACL.png
        ├── 16-P6-GUEST-ACL.png
        ├── 20-P7-Admin-HTTPS.png
        └── 26-P7-Routing-Table.png
```

*The filenames shown are examples. Keep your actual filenames and include only files that exist in your repository.*

---

## ⚠️ Limitations

- The network is simulated and has not been validated on physical networking equipment.
- No ISP or external router is included, so internet connectivity is not tested.
- The ACLs are stateless traffic filters and do not provide stateful firewall functionality.
- The topology uses a single router and switch and does not demonstrate hardware redundancy.
- HTTPS access was tested within Packet Tracer; this project does not demonstrate a production certificate-management deployment.

---

## 👤 Author

**Kwanele Khumalo**

Personal networking project demonstrating practical skills in network configuration, segmentation, access control, and security verification.

**GitHub Repository:** [Secure Enterprise Network](https://github.com/0wethu/secure-enterprise-network)

---

*This project is intended for learning, portfolio demonstration, and further development of networking skills.*
