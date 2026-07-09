<h1 align="center">Enterprise Home Infrastructure</h1>

<h3 align="center">
Enterprise-Inspired Network Architecture Project
</h3>

<p align="center">

Designing, securing, operating, monitoring, and documenting an enterprise-inspired home infrastructure using modern networking, cybersecurity, and IT operations best practices.

</p>

<p align="center">

![Status](https://img.shields.io/badge/Status-Active-success)
![Project](https://img.shields.io/badge/Project-Enterprise%20Infrastructure-blue)
![Platform](https://img.shields.io/badge/Platform-Omada%20SDN-0052CC)
![Monitoring](https://img.shields.io/badge/SIEM-Wazuh-purple)
![Documentation](https://img.shields.io/badge/Documentation-18%20Technical%20Guides-orange)
![License](https://img.shields.io/badge/License-MIT-brightgreen)

</p>

---

## Architecture Gallery

| Logical Architecture | Physical Architecture |
|----------------------|-----------------------|
| ![](images/logical_network_architecture.png) | ![](images/physical_infrastructure_architecture.png) |

| Network Segmentation | Traffic Flow |
|----------------------|--------------|
| ![](images/network_segmentation_architecture.png) | ![](https://github.com/khucker3d/enterprise-infrastructure-architecture-public/blob/main/images/enterprise_traffic_flow_and_access_control.png) |

## Overview

Enterprise Home Infrastructure is a long-term engineering project that demonstrates the design, implementation, operation, and continuous improvement of an enterprise-inspired network.

Rather than simply building a functional home network, this project applies enterprise architecture principles to create a secure, segmented, monitored, and well-documented environment similar to those found in professional IT organizations.

This repository focuses not only on technical implementation but also on the operational practices that keep enterprise infrastructure reliable over time.

---

### Why This Project?

The objective was to move beyond individual technologies and understand how networking, security, monitoring, documentation, disaster recovery, and operations work together as a complete infrastructure.

This project was built to gain practical experience in the responsibilities commonly performed by:
- Network Engineers
- Infrastructure Engineers
- Systems Administrators
- Security Engineers
- Blue Team Analysts

---

## Enterprise Features

| Category | Implementation |
|:--|:--|
| Network Architecture | Enterprise-inspired layered network design |
| Segmentation | Multi-VLAN architecture |
| Security | Layer 3 firewall policies & least privilege |
| Wireless | Enterprise Wi-Fi with segmented SSIDs |
| Administration | Dedicated management network |
| Monitoring | Omada SDN & Wazuh SIEM |
| Recovery | Backup & disaster recovery planning |
| Documentation | 18 engineering guides & operational runbooks |
| Operations | Preventive maintenance & change management |

---

## Architecture

```
                    Internet
                        │
                   ISP Gateway
                        │
                Enterprise Router
                        │
                Managed PoE Switch
         ┌──────────────┼──────────────┐
         │              │              │
    Omada SDN      Access Point   Wired Devices
     Controller
                        │
                Enterprise VLAN Fabric
```

## Engineering Principles

Every design decision throughout this project follows the same engineering principles. These principles guided every stage of planning, deployment, validation, and maintenance.

- Security by Design
- Defense in Depth
- Least Privilege
- Network Segmentation
- Operational Simplicity
- Documentation First
- Continuous Improvement
---

## Documentation
***Documentation Scope:** This public repository contains a high-level, sanitized overview of the project. More detailed internal documentation exists separately, including step-by-step walkthroughs, configuration procedures, validation steps, troubleshooting notes, and operational runbooks. Sensitive environment-specific details have been intentionally excluded for security reasons.**

- [1. Executive Overview](https://github.com/khucker3d/enterprise-infrastructure-architecture-public/blob/main/docs/01-executive-overview.md)
- [2. Project Goals](https://github.com/khucker3d/enterprise-infrastructure-architecture-public/blob/main/docs/02-project-goals.md)
- [3. Network Architecture](https://github.com/khucker3d/enterprise-infrastructure-architecture-public/blob/main/docs/03-network-architecture.md)
- [4. Physical Topology](https://github.com/khucker3d/enterprise-infrastructure-architecture-public/blob/main/docs/04-physical-topology.md)
- [5. Logical Topology](https://github.com/khucker3d/enterprise-infrastructure-architecture-public/blob/main/docs/05-logical-topology.md)
- [6. Network Segmentation Architecture](https://github.com/khucker3d/enterprise-infrastructure-architecture-public/blob/main/docs/06-network-segmentation-architecture.md)
- [7. Network Addressing Strategy](https://github.com/khucker3d/enterprise-infrastructure-architecture-public/blob/main/docs/07-network-addressing-strategy.md)
- [8. Wireless Architecture](https://github.com/khucker3d/enterprise-infrastructure-architecture-public/blob/main/docs/08-wireless-architecture.md)
- [9. Firewall Policy](https://github.com/khucker3d/enterprise-infrastructure-architecture-public/blob/main/docs/09-firewall-policy-overview.md)
- [10. Monitoring and Logging](https://github.com/khucker3d/enterprise-infrastructure-architecture-public/blob/main/docs/10-monitoring-and-logging.md)
- [11. Backup and Recovery](https://github.com/khucker3d/enterprise-infrastructure-architecture-public/blob/main/docs/11-backup-and-recovery.md)
- [12. Disaster Recovery Business Continuity](https://github.com/khucker3d/enterprise-infrastructure-architecture-public/blob/main/docs/12-disaster-recovery-business-continuity.md)
- [13. Maintenance and Operations](https://github.com/khucker3d/enterprise-infrastructure-architecture-public/blob/main/docs/13-maintenance-and-operations.md)
- [14. Security Hardening](https://github.com/khucker3d/enterprise-infrastructure-architecture-public/blob/main/docs/14-security-hardening.md)
- [15. Troubleshooting Highlights](https://github.com/khucker3d/enterprise-infrastructure-architecture-public/blob/main/docs/15-troubleshooting-highlights.md)
- [16. Lessons Learned](https://github.com/khucker3d/enterprise-infrastructure-architecture-public/blob/main/docs/16-lessons-learned.md)
- [17. Professional Competencies Demonstrated](https://github.com/khucker3d/enterprise-infrastructure-architecture-public/blob/main/docs/17-professional-competencies-demonstrated.md)
- [18. Project Evolution Revision History](https://github.com/khucker3d/enterprise-infrastructure-architecture-public/blob/main/docs/18-project-evolution-revision-history.md)
- [Bridge Mode-Plan](docs/bridge-mode-decision.md)

*During the initial rollout, the ISP gateway will remain in router mode while the ER605 operates behind it in a temporary Double NAT / NAT x2 design. This provides a safer migration path, keeps a fallback connection available, and allows VLANs, routing, DHCP, DNS, firewall rules, management access, and recovery procedures to be validated before bridge mode is enabled.*

---

## Related Projects: 
- [Pi Network Utility Server](https://github.com/khucker3d/raspberry-pi-network-utility-server/blob/main/README.md)

---

## Skills Demonstrated

| Engineering Discipline | Practical Experience |
|:--|:--|
| Network Engineering | VLAN Design, Routing, Switching, DHCP, DNS |
| Network Security | Firewall Policies, ACL Design, Defense in Depth |
| Wireless Networking | Enterprise Wi-Fi Design & Client Segmentation |
| Infrastructure | Omada SDN, Windows, Linux, VMware |
| Monitoring | Wazuh SIEM, Infrastructure Health Monitoring |
| Operations | Change Management, Preventive Maintenance |
| Disaster Recovery | Backup Strategy, Recovery Validation |
| Technical Documentation | SOPs, Runbooks, Architecture Documentation |

---

<p align="center">

**Designed with enterprise engineering principles. Built for continuous learning.**

</p>
