# Static SNAT – Palo Alto Firewall

## Overview

This lab demonstrates **Static Source NAT (SNAT)** configuration on a Palo Alto Firewall.

The objective is to translate the source IP of a specific internal host (`192.168.3.10`) to a fixed IP address (`172.16.1.10`) when it accesses the external/WAN network.

## Lab Topology

```text
Win5
192.168.3.10
     |
     | 192.168.3.0/24
     |
Router
Gi0/0 = 192.168.3.100
Gi0/1 = 172.16.1.100
     |
     | 172.16.1.0/24
     |
Palo Alto
eth1/1 = 172.16.1.1
     |
     | WAN
     ↓
```

## NAT Policy Configuration

### General

| Parameter     | Value            |
| ------------- | ---------------- |
| NAT Rule Name | `Dummy_Test_NAT` |
| NAT Type      | `IPv4`           |

### Original Packet

| Parameter           | Configuration     |
| ------------------- | ----------------- |
| Source Zone         | `LAN-ZONE`        |
| Destination Zone    | `WAN-ZONE`        |
| Source Address      | `192.168.3.10/32` |
| Destination Address | `Any`             |
| Service             | `Any`             |

This means the rule matches traffic originating from `192.168.3.10` and going toward the WAN zone.

### Translated Packet

| Parameter                 | Configuration    |
| ------------------------- | ---------------- |
| Source Translation        | `Static IP`      |
| Translated Source Address | `172.16.1.10/32` |
| Destination Translation   | `None`           |
| Bi-directional            | Disabled         |

## How Static SNAT Works

When `192.168.3.10` sends traffic that matches the NAT policy, the Palo Alto firewall changes the **source IP address**.

```text
Original Packet

Source      = 192.168.3.10
Destination = <WAN destination>

              ↓ Static SNAT

Translated Packet

Source      = 172.16.1.10
Destination = <WAN destination>
```

The destination address is not modified.

## Traffic Flow

```text
192.168.3.10
     |
     | Original Source IP
     ↓
Router
     |
     ↓
Palo Alto Firewall
     |
     | Static SNAT
     | 192.168.3.10 → 172.16.1.10
     ↓
WAN
```

## Why Static SNAT?

Static SNAT is useful when a particular internal host needs a **predictable and fixed source IP** when communicating with another network.

For example, a remote network may allow traffic only from:

```text
172.16.1.10
```

The internal server can remain:

```text
192.168.3.10
```

while the remote network sees:

```text
172.16.1.10
```

## Key Points

* **SNAT** = Source Network Address Translation.
* Static SNAT provides a **fixed translated source IP**.
* `192.168.3.10` is the original source address.
* `172.16.1.10` is the translated source address.
* The destination address is unchanged.
* The NAT rule applies only when the traffic matches its configured conditions.
* This is a **one-to-one source address translation** for the specified host.

## Verification

After committing the configuration, verify the NAT operation using:

* **Monitor → Traffic**
* NAT policy hit count
* Session details
* Packet capture, if required

The traffic session should show the original source IP and the translated source IP.

## Lab Objective

To understand and configure **Static SNAT on a Palo Alto Firewall**, where traffic from a specific internal host is translated from `192.168.3.10` to the fixed address `172.16.1.10` when communicating toward the WAN.
