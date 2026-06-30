<h1 align="center">Double NAT Transition Strategy</h1>

<h3 align="center">
Enterprise Home Network Bridge Mode Decision
</h3>

<p align="center">

Documenting the decision to temporarily delay bridge mode and operate the home enterprise network using a controlled Double NAT rollout strategy.

</p>

<p align="center">

![Status](https://img.shields.io/badge/Status-Active-success)
![Project](https://img.shields.io/badge/Project-Enterprise%20Home%20Infrastructure-blue)
![Network%20Mode](https://img.shields.io/badge/Network%20Mode-Double%20NAT-orange)
![Router](https://img.shields.io/badge/Router-TP--Link%20ER605-0052CC)
![Platform](https://img.shields.io/badge/Platform-Omada%20SDN-purple)
![Documentation](https://img.shields.io/badge/Documentation-Bridge%20Mode%20Decision-lightgrey)
![License](https://img.shields.io/badge/License-MIT-brightgreen)

</p>

---

# Overview

This document explains the current decision to delay bridge mode on the ISP gateway while building and validating an enterprise-inspired home network.

The current design intentionally uses **Double NAT**, also referred to as **NAT x2**, as a temporary transition strategy. This allows the new Omada-based network to be deployed, tested, documented, and improved without immediately disrupting the existing home internet connection.

This is not the final long-term architecture. It is a controlled migration phase designed to reduce risk while supporting hands-on network engineering practice.

---

# Why This Decision?

The purpose of delaying bridge mode is to support a slower and safer rollout of the enterprise home network.

Instead of immediately placing the ISP gateway into bridge mode, the ISP gateway remains in router mode while the TP-Link ER605 operates behind it as the primary router for the internal Omada network.

This creates a temporary Double NAT design:

```text
Internet
   │
ISP Gateway
Router Mode / NAT #1
   │
TP-Link ER605
Router Mode / NAT #2
   │
Omada Switch / Access Point / Controller / VLANs / Devices
```

This approach provides a stable fallback path while the new network is being configured and validated.

---

# Important Note

Bridge mode is being intentionally delayed for the initial rollout.

The goal is not to avoid bridge mode permanently. The goal is to build the new network in controlled stages before making the ER605 the true edge router.

Keeping the ISP gateway in router mode during the early phase provides:

* A stable fallback path
* Easier ISP troubleshooting
* Lower risk of disrupting the entire home internet connection
* More time to validate Omada routing, VLANs, DHCP, DNS, and firewall behavior
* A safer learning environment for enterprise-style network design
* Practical experience troubleshooting layered network environments

This Double NAT design is temporary, intentional, and documented.

---

# Current Network Design

## Current Mode

| Component       | Current Role             |
| :-------------- | :----------------------- |
| ISP Gateway     | Router mode              |
| ISP Gateway NAT | Enabled                  |
| ER605 Router    | Router mode              |
| ER605 NAT       | Enabled                  |
| Network Mode    | Double NAT / NAT x2      |
| Omada Network   | Primary internal network |
| Bridge Mode     | Deferred                 |

---

## Current Network Diagram

```text
                           Internet
                              │
                              │
                    ┌─────────────────────┐
                    │     ISP Gateway     │
                    │  Router Mode / NAT  │
                    │        NAT #1       │
                    └──────────┬──────────┘
                               │
                               │
                    ┌──────────▼──────────┐
                    │   TP-Link ER605     │
                    │  Router Mode / NAT  │
                    │        NAT #2       │
                    └──────────┬──────────┘
                               │
                               │
                    ┌──────────▼──────────┐
                    │   Omada PoE Switch  │
                    └──────────┬──────────┘
                               │
          ┌────────────────────┼────────────────────┐
          │                    │                    │
 ┌────────▼────────┐  ┌────────▼────────┐  ┌────────▼────────┐
 │ OC200 Controller│  │ Omada Access AP │  │  Wired Devices  │
 │ Management Plane│  │ Segmented Wi-Fi │  │ Lab / Personal  │
 └─────────────────┘  └────────┬────────┘  └─────────────────┘
                               │
                ┌──────────────┼──────────────┐
                │              │              │
        Management VLAN   Personal VLAN   Cybersecurity VLAN
```

---

# Future Network Design
Bridge Mode Plan

During the initial rollout, the ISP gateway remains in router mode while the enterprise router operates behind it. This approach provides a safer migration path, keeps a known-good fallback connection available, and allows the Omada network to be configured and validated before making the enterprise router the true edge device.

This is a temporary design choice, not the final architecture.

The long-term plan is to move the ISP gateway into bridge mode after VLANs, routing, DHCP, DNS, firewall rules, management access, and recovery procedures have been fully tested.

For the full decision record, current topology, future bridged topology, pros and cons, and rollout plan, see: 

---

## Future Mode

| Component       | Future Role                  |
| :-------------- | :--------------------------- |
| ISP Gateway     | Bridge mode                  |
| ISP Gateway NAT | Disabled                     |
| ER605 Router    | Primary edge router          |
| ER605 NAT       | Enabled                      |
| Network Mode    | Single NAT                   |
| Omada Network   | Full production home network |
| Bridge Mode     | Enabled after validation     |

---

## Future Bridged Network Diagram

```text
                           Internet
                              │
                              │
                    ┌─────────────────────┐
                    │     ISP Gateway     │
                    │     Bridge Mode     │
                    │ Internet Handoff    │
                    └──────────┬──────────┘
                               │
                               │
                    ┌──────────▼──────────┐
                    │   TP-Link ER605     │
                    │ Edge Router/Firewall│
                    │      Single NAT     │
                    └──────────┬──────────┘
                               │
                               │
                    ┌──────────▼──────────┐
                    │   Omada PoE Switch  │
                    └──────────┬──────────┘
                               │
          ┌────────────────────┼────────────────────┐
          │                    │                    │
 ┌────────▼────────┐  ┌────────▼────────┐  ┌────────▼────────┐
 │ OC200 Controller│  │ Omada Access AP │  │  Wired Devices  │
 │ Management Plane│  │ Segmented Wi-Fi │  │ Lab / Personal  │
 └─────────────────┘  └────────┬────────┘  └─────────────────┘
                               │
                ┌──────────────┼──────────────┐
                │              │              │
        Management VLAN   Personal VLAN   Cybersecurity VLAN
```

---

# Current Double NAT Pros and Cons

## Pros

| Benefit                     | Explanation                                                                                      |
| :-------------------------- | :----------------------------------------------------------------------------------------------- |
| Safer rollout               | The new network can be built without immediately replacing the ISP gateway routing function.     |
| Easier rollback             | The ISP gateway remains available as a known-good recovery path.                                 |
| Lower outage risk           | Misconfigurations on the ER605 are less likely to take down the entire home network permanently. |
| Better learning             | Double NAT provides practical experience with real-world layered network troubleshooting.        |
| ISP support remains simpler | The ISP gateway remains in its expected operating mode during early migration.                   |
| Additional boundary         | Double NAT creates another network layer while firewall rules and VLANs are still being tested.  |

---

## Cons

| Limitation                | Explanation                                                                           |
| :------------------------ | :------------------------------------------------------------------------------------ |
| More complexity           | Traffic passes through two NAT layers instead of one.                                 |
| Harder troubleshooting    | Network issues may need to be checked at both the ISP gateway and ER605.              |
| Port forwarding is harder | Inbound services may require forwarding through both routers.                         |
| VPN hosting may be harder | Self-hosted VPN services can be more difficult behind Double NAT.                     |
| Less clean architecture   | The ER605 is not yet the true edge router.                                            |
| Device discovery issues   | Devices split between ISP Wi-Fi and Omada Wi-Fi may not discover each other properly. |

---

# Future Bridge Mode Pros and Cons

## Pros

| Benefit                    | Explanation                                                         |
| :------------------------- | :------------------------------------------------------------------ |
| Cleaner architecture       | The ER605 becomes the true edge router and firewall.                |
| Better enterprise practice | The design more closely reflects professional network architecture. |
| Easier port forwarding     | Inbound services are managed from one router instead of two.        |
| Better VPN design          | Remote access is easier to configure and troubleshoot.              |
| Better visibility          | Routing and firewall decisions are centralized on the ER605.        |
| Better documentation       | The network edge is easier to define and explain.                   |

---

## Cons

| Limitation                | Explanation                                                                                |
| :------------------------ | :----------------------------------------------------------------------------------------- |
| More responsibility       | The ER605 becomes responsible for the entire home network internet path.                   |
| Less ISP safety net       | The ISP gateway no longer provides normal router fallback features.                        |
| Higher cutover risk       | A bad ER605 configuration could disrupt the whole network.                                 |
| Requires validation first | VLANs, DHCP, DNS, firewall rules, and admin access should be confirmed before bridge mode. |

---

# Setup Plan Overview

## Phase 1: Build Behind ISP Gateway

The ISP gateway remains in router mode.

```text
ISP Gateway
   │
TP-Link ER605
   │
Omada Switch / AP / Controller
```

Primary goals:

* Confirm ER605 internet access
* Adopt Omada devices
* Confirm Omada Controller access
* Configure basic LAN settings
* Validate wired and wireless connectivity
* Keep ISP gateway available as fallback

---

## Phase 2: Configure Core Omada Network

Build the internal network architecture.

Recommended VLAN structure:

| VLAN Type          | Purpose                                          |
| :----------------- | :----------------------------------------------- |
| Management VLAN    | Network administration and controller access     |
| Personal VLAN      | Daily-use personal devices                       |
| Cybersecurity VLAN | Lab systems, security tools, and testing devices |
| IoT VLAN           | Smart devices and lower-trust endpoints          |
| Guest VLAN         | Internet-only access for temporary devices       |

Primary goals:

* Create VLANs
* Create DHCP scopes
* Map SSIDs to VLANs
* Assign switch ports
* Validate internet access from each VLAN
* Confirm management access works as intended

---

## Phase 3: Move Devices Slowly

Devices should be migrated to the Omada network in stages.

Recommended migration order:

```text
1. Test laptop
2. Admin device
3. Personal laptop
4. Cybersecurity lab laptop
5. Phone or tablet
6. IoT devices
7. Printer or shared device
8. NAS or backup storage
```

The goal is to avoid moving everything at once.

Each device should be validated before moving the next group.

---

## Phase 4: Validate Segmentation

Each VLAN should be tested to confirm it behaves as expected.

Example validation goals:

| Network Segment    | Expected Behavior                                              |
| :----------------- | :------------------------------------------------------------- |
| Management VLAN    | Can access Omada Controller and network devices                |
| Personal VLAN      | Can access internet and approved shared resources              |
| Cybersecurity VLAN | Can access internet but remains isolated from personal systems |
| IoT VLAN           | Can access internet but cannot reach sensitive systems         |
| Guest VLAN         | Internet-only access                                           |

Firewall validation examples:

* Guest VLAN cannot access Management VLAN
* IoT VLAN cannot access Management VLAN
* IoT VLAN cannot access Cybersecurity VLAN
* Cybersecurity VLAN cannot access Personal VLAN unless explicitly allowed
* Personal VLAN can access approved shared resources if required

---

## Phase 5: Reduce ISP Gateway Network Usage

Once the Omada network is stable, daily-use devices should move away from the ISP gateway network.

Recommended target state:

```text
ISP Gateway: Internet handoff and fallback
Omada Network: Primary home network
```

If Omada Wi-Fi coverage is stable, ISP gateway Wi-Fi can be disabled or left unused.

---

## Phase 6: Prepare for Bridge Mode

Before enabling bridge mode, validate the following:

* ER605 WAN connection is stable
* Omada Controller is reachable
* Admin device can access the Management VLAN
* All VLANs receive DHCP correctly
* DNS works across required segments
* Internet access works from each VLAN
* Firewall rules are documented
* Recovery steps are documented
* Configuration backups are saved
* ISP gateway login access is available
* ER605 admin access is available

---

## Phase 7: Enable Bridge Mode Later

After validation, the ISP gateway can be placed into bridge mode.

Final target state:

```text
ISP Gateway: Bridge mode
ER605: Primary edge router/firewall
Omada Network: Primary production network
```

---

# Important Callouts

## Double NAT Is Temporary

The current Double NAT design is not the final network architecture.

It is a temporary transition strategy used to support a safer rollout.

---

## Avoid Public Hosting During Double NAT

While Double NAT is active, avoid exposing internal services directly to the internet unless there is a specific documented need.

Avoid or delay:

* Public NAS access
* Public web servers
* Public remote desktop access
* Port-forwarded lab services
* Unnecessary inbound firewall rules

---

## Prefer Safer Remote Access Options Later

For future remote access, evaluate options that reduce direct exposure.

Possible future options:

* Tailscale
* WireGuard
* Omada VPN
* Cloudflare Tunnel for specific web services

These should be reviewed separately before implementation.

---

## Keep a Recovery Path

During rollout, maintain a known recovery path.

Recommended recovery items:

* ISP gateway access
* ER605 local admin access
* OC200 controller access
* Saved Omada configuration backup
* Known-good Ethernet cable
* Known-good laptop
* Documented IP ranges
* Documented admin URLs

---

## Document Addressing Clearly

Each network should use a clearly separated private IP range.

Example sanitized addressing plan:

| Network             | Example Range   |
| :------------------ | :-------------- |
| ISP Gateway Network | 10.0.0.0/24     |
| Management VLAN     | 192.168.10.0/24 |
| Personal VLAN       | 192.168.20.0/24 |
| Cybersecurity VLAN  | 192.168.30.0/24 |
| IoT VLAN            | 192.168.40.0/24 |
| Guest VLAN          | 192.168.50.0/24 |

Actual IP ranges should be updated based on the final configuration.

---

## Avoid Random Device Placement

Critical devices should be placed intentionally.

Recommended placement:

| Device Type               | Recommended Placement                  |
| :------------------------ | :------------------------------------- |
| Omada Controller          | Management VLAN                        |
| Admin laptop              | Management access allowed              |
| Personal devices          | Personal VLAN                          |
| Cybersecurity lab devices | Cybersecurity VLAN                     |
| IoT devices               | IoT VLAN                               |
| Guest devices             | Guest VLAN                             |
| NAS or backup storage     | Dedicated or controlled-access segment |

---

## Save Configuration Backups

Before major changes, save configuration backups from:

* Omada Controller
* ER605 Router
* Managed Switch
* Access Point configuration

Backups should be stored somewhere safe and offline when possible.

---

# Decision Summary

## Current Decision

The ISP gateway will remain in router mode for now.

The TP-Link ER605 will operate behind the ISP gateway.

This creates a temporary Double NAT design.

```text
Current Mode: NAT x2 / Double NAT
Bridge Mode: Delayed
Reason: Safer slow rollout and better learning experience
```

---

## Future Decision

Bridge mode may be enabled later after the Omada network is fully configured, tested, and validated.

```text
Future Mode: Single NAT
ISP Gateway: Bridge mode
ER605: Primary edge router/firewall
Reason: Cleaner enterprise-style network architecture
```

---

# Skills Demonstrated

| Engineering Discipline    | Practical Experience                                      |
| :------------------------ | :-------------------------------------------------------- |
| Network Engineering       | NAT, routing, VLAN planning, DHCP, DNS                    |
| Network Security          | Segmentation, firewall planning, least privilege          |
| Infrastructure Operations | Change planning, staged rollout, rollback planning        |
| Troubleshooting           | Double NAT analysis, layered network validation           |
| Documentation             | Architecture notes, migration planning, decision records  |
| Risk Management           | Controlled cutover, fallback planning, operational safety |

---

# Technologies

## Networking

* ISP Gateway
* TP-Link ER605
* TP-Link Omada SDN
* Managed PoE Switching
* Wi-Fi 6 Access Point
* VLAN Segmentation

## Security

* Network Segmentation
* Layer 3 Firewall Rules
* Defense in Depth
* Least Privilege
* Controlled Management Access

## Operations

* Configuration Backups
* Change Management
* Recovery Planning
* Validation Testing
* Technical Documentation

---

# Project Status

| Area                           | Status      |
| :----------------------------- | :---------- |
| Double NAT transition decision | Documented  |
| Current network diagram        | Documented  |
| Future bridge mode design      | Documented  |
| VLAN planning                  | In progress |
| Firewall validation            | Planned     |
| Bridge mode cutover            | Deferred    |
| Final production architecture  | Planned     |

---

# Portfolio Notice

This document has been sanitized for public release.

Sensitive information has been removed or generalized, including:

* Public IP addresses
* Internal IP addresses
* Hostnames
* SSIDs
* MAC addresses
* Device serial numbers
* Administrative credentials
* Physical location details
* ISP account details

The purpose of this document is to demonstrate network engineering decision-making while protecting operational security.

---

# Final Recommendation

The current Double NAT setup is a reasonable and intentional transition design.

It supports:

* Stability
* Safer migration
* Hands-on troubleshooting
* Enterprise network learning
* Documentation-first implementation
* Reduced risk during setup

The long-term target remains a bridged ISP gateway with the ER605 operating as the true edge router.

For now, the correct approach is:

```text
Build slowly.
Validate each layer.
Document issues.
Keep fallback access.
Bridge later.
```

---

<p align="center">

**Designed as a controlled transition from home networking to enterprise-style infrastructure.**

</p>



