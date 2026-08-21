# Palo Alto Firewall – Basic Internet Access

## Overview

This lab demonstrates a basic **Palo Alto firewall configuration** to provide Internet access to an internal Windows PC through a Cisco router.

The lab covers **Layer-3 routing, Security Policy, Source NAT, security zones, and connectivity verification**.

---

## Network Topology

```text
Win5
192.168.3.10/24
Default Gateway: 192.168.3.100
        |
        |
Cisco Router
Gi0/0: 192.168.3.100
Gi0/1: 172.16.1.100
        |
        |
Palo Alto Firewall
eth1/1: 172.16.1.1/24
eth1/2: DHCP / WAN
        |
        |
     Internet
```

---

## IP Addressing

| Device       | Interface   | IP Address         | Purpose          |
| ------------ | ----------- | ------------------ | ---------------- |
| Win5         | Ethernet    | `192.168.3.10/24`  | Internal PC      |
| Cisco Router | Gi0/0       | `192.168.3.100/24` | LAN Gateway      |
| Cisco Router | Gi0/1       | `172.16.1.100/24`  | Firewall Transit |
| Palo Alto    | ethernet1/1 | `172.16.1.1/24`    | LAN Interface    |
| Palo Alto    | ethernet1/2 | DHCP               | WAN/Internet     |

---

## Configuration

### 1. Windows PC

Configured Win5 with:

```text
IP Address      : 192.168.3.10
Subnet Mask     : 255.255.255.0
Default Gateway : 192.168.3.100
DNS             : 8.8.8.8
```

### 2. Cisco Router

Configured two Layer-3 interfaces:

```text
Gi0/0 → 192.168.3.100/24
Gi0/1 → 172.16.1.100/24
```

Default route:

```text
0.0.0.0/0 → 172.16.1.1
```

### 3. Palo Alto Interfaces

* `ethernet1/1` → `172.16.1.1/24` → `LAN-ZONE`
* `ethernet1/2` → DHCP → `WAN-Zone`

### 4. Palo Alto Routing

Default route toward the WAN:

```text
0.0.0.0/0 → 10.0.0.1 → ethernet1/2
```

Return route to the internal LAN:

```text
192.168.3.0/24 → 172.16.1.100 → ethernet1/1
```

The return route allows the firewall to send Internet responses back toward Win5 through the Cisco router.

### 5. Security Policy

Created a basic Security Policy:

```text
Source Zone       : LAN-ZONE
Source Address    : 192.168.3.0/24
Destination Zone  : WAN-Zone
Destination       : Any
Application       : Any
Service           : Any
Action             : Allow
```

### 6. Source NAT

Configured Source NAT using:

```text
Source Zone       : LAN-ZONE
Destination Zone  : WAN-Zone
Source Network    : 192.168.3.0/24
Translation       : Dynamic IP and Port
Translated IP     : ethernet1/2 interface address
```

NAT translates the private IP of Win5 to the Palo Alto WAN-side IP before sending traffic to the Internet.

---

## Traffic Flow

```text
Win5
192.168.3.10
    ↓
Cisco Router
192.168.3.100
    ↓
172.16.1.100
    ↓
Palo Alto
172.16.1.1
    ↓
Security Policy
    ↓
Source NAT
    ↓
ethernet1/2
    ↓
Internet
```

---

## Verification

Connectivity was verified from Win5 using:

```cmd
ping 192.168.3.100
ping 172.16.1.1
ping 8.8.8.8
```

Internet connectivity was successfully established.

---

## Key Concepts

* Layer-3 interface configuration
* Cisco static/default routing
* Palo Alto Virtual Router
* Security Zones
* Security Policy
* Source NAT
* Return routing
* Basic firewall connectivity troubleshooting
* Internet connectivity verification

## Screenshots

Screenshots included in this repository demonstrate the topology, PC configuration, router configuration, Palo Alto interfaces, routing table, Security Policy, NAT configuration, and successful Internet connectivity.
