# Secure Enterprise Network — Cisco Packet Tracer

**Project Author:** Kwanele Khumalo  
**Project Type:** Personal Networking Project  
**Tools Used:** Cisco Packet Tracer  
**Status:** Completed

## About This Project

This is a networking project designed, configured, and tested by me using Cisco Packet Tracer. It demonstrates my practical understanding of enterprise network design, VLAN segmentation, inter-VLAN routing, Access Control Lists (ACLs), SSH access control, and network security testing.

I made the final design and configuration decisions, performed connectivity and security tests, and documented the results through screenshots and device configurations.


## Objectives

- Design a logical enterprise network topology.
- Segment network devices using VLANs.
- Configure inter-VLAN routing using router-on-a-stick.
- Restrict inter-department traffic using extended ACLs.
- Allow Admin and Engineering users to access the application server over HTTPS.
- Isolate the Guest network from internal networks.
- Restrict router and switch SSH management to the designated management PC.
- Verify the configuration through connectivity and security tests.

## Network Topology

The network consists of a Cisco 2911 router, a Cisco 2960 switch, departmental PCs, and an application server.

The router connects to the switch using an IEEE 802.1Q trunk. Router subinterfaces provide the default gateways for the VLANs.

**Devices**
- R1 — Cisco 2911 router
- S1 — Cisco 2960 switch
- PC-ADMIN — Admin workstation
- PC-ENG — Engineering workstation
- SRV-APP — Application server
- PC-GUEST — Guest workstation
- PC-MGMT — Network management workstation

## VLAN and IP Addressing Plan

| VLAN | Name | Network | Default Gateway |
|---|---|---|---|
| 10 | ADMIN | 192.168.10.0/24 | 192.168.10.1 |
| 20 | ENGINEERING | 192.168.20.0/24 | 192.168.20.1 |
| 30 | SERVERS | 192.168.30.0/24 | 192.168.30.1 |
| 40 | GUEST | 192.168.40.0/24 | 192.168.40.1 |
| 99 | MANAGEMENT | 192.168.99.0/24 | 192.168.99.1 |

### Endpoint Addresses

| Device | IP Address | Default Gateway |
|---|---|---|
| PC-ADMIN | 192.168.10.10 | 192.168.10.1 |
| PC-ENG | 192.168.20.10 | 192.168.20.1 |
| SRV-APP | 192.168.30.10 | 192.168.30.1 |
| PC-GUEST | 192.168.40.10 | 192.168.40.1 |
| PC-MGMT | 192.168.99.10 | 192.168.99.1 |
| S1 management interface | 192.168.99.2 | 192.168.99.1 |

Subnet mask for all listed networks: `255.255.255.0`.

## Security Configuration

### VLAN Segmentation

Each department and network function has a dedicated VLAN. This provides logical separation and supports the application of different access policies.

### Inter-VLAN Routing

R1 uses router-on-a-stick with 802.1Q subinterfaces to route traffic between VLANs. ACLs are applied inbound on the Admin, Engineering, and Guest subinterfaces.

### Access Control Lists

The following policies were configured:

- **Admin:** HTTPS access to the application server is permitted; other traffic to the server subnet and unauthorised inter-VLAN destinations is denied by the configured rules.
- **Engineering:** HTTPS access to the application server is permitted; other traffic to the server subnet and unauthorised inter-VLAN destinations is denied by the configured rules.
- **Guest:** Access to the Admin, Engineering, Servers, and Management subnets is denied. Other traffic is permitted by the final ACL rule, allowing for a future external-network connection.
- **Management:** A standard ACL permits SSH management access only from `192.168.99.10`.

### Secure Device Management

SSH version 2 and local user authentication are configured on R1 and S1. Telnet access is disabled on the VTY lines, and the management ACL restricts remote access to the designated management workstation.

### Switch Port Security Hygiene

Unused switch ports are administratively shut down. Access ports are assigned to their respective VLANs, and the router uplink is configured as an 802.1Q trunk carrying the required VLANs.

## Testing and Verification

The following tests were performed in Packet Tracer:

| Test | Result |
|---|---|
| Admin accesses application server over HTTPS | Passed |
| Engineering accesses application server over HTTPS | Passed |
| Guest attempts to access application server | Blocked as intended |
| Admin attempts to reach Engineering | Blocked as intended |
| Engineering attempts to reach Admin | Blocked as intended |
| PC-MGMT establishes SSH sessions to R1 and S1 | Successful |
| PC-ADMIN attempts SSH access to S1 | Failed as intended |
| Router routing table shows all five VLAN networks as directly connected | Verified |
| ACL counters and interface ACL assignments | Verified |

## Project Limitations

- The topology does not include an ISP or external router, so internet connectivity was not tested.
- The ACLs provide stateless packet filtering; they are not a stateful firewall.
- The current configuration uses a single router and switch, without redundancy.
- The application server is addressed at `192.168.30.10`; HTTPS testing was successful in the simulation.

## Project Structure

```text
secure-enterprise-network/
├── secure-enterprise-network.pkt
├── README.md
├── configs/
│   ├── R1-running-config.txt
│   └── S1-running-config.txt
└── screenshots/
    ├── P3-VLANs.png
    ├── P3-Trunk.png
    ├── P4-Subinterfaces.png
    ├── P6-ADMIN-ACL.png
    ├── P6-ENG-ACL.png
    ├── P6-GUEST-ACL.png
    ├── P6-SSH-R1.png
    ├── P6-SSH-S1.png
    ├── P6-ACL-Verification.png
    ├── P7-Engineering-HTTPS.png
    ├── P7-Admin-Blocked-Engineering.png
    ├── P7-Engineering-Blocked-Admin.png
    ├── P7-Unauthorized-SSH.png
    └── P7-Routing-Table.png
```

*The filenames above are a suggested structure; include only files that exist in your project.*

## Skills Demonstrated

- Cisco IOS CLI configuration
- VLAN creation and port assignment
- 802.1Q trunking
- Router-on-a-stick inter-VLAN routing
- Extended and standard ACL configuration
- SSH configuration and access restrictions
- Network troubleshooting and verification
- Technical documentation and evidence collection

## Conclusion

This project demonstrates a segmented enterprise network with controlled inter-VLAN communication, secure SSH management, and verified access policies. It provides a foundation for future improvements, including DHCP, a dedicated external network, redundancy, and additional monitoring.

**Platform:** Cisco Packet Tracer  
**Project Type:** Networking and Network Security Lab
