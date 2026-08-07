# Basic Switch Configuration Lab

Configuration of a Cisco switch from scratch using console access, followed by IP connectivity validation across a small LAN.

**Tool:** Cisco Packet Tracer

---

## Objective

Configure a Cisco 2960 switch with basic security settings and management IP, then verify end-to-end connectivity between hosts on the same network.

---

## Topology

| Device | Interface | IP Address | Subnet Mask | Gateway |
|---|---|---|---|---|
| Sw-Floor-1 | VLAN 1 | 192.168.1.20 | 255.255.255.0 | 192.168.1.1 |
| Laptop | FastEthernet0 | 192.168.1.30 | 255.255.255.0 | 192.168.1.1 |
| PC | FastEthernet0 | 192.168.1.40 | 255.255.255.0 | 192.168.1.1 |

---

## Steps

### 1. Initial console access

Connected a laptop to the switch console port using a rollover cable and accessed the CLI through the terminal emulator.

### 2. Switch configuration

```
enable
configure terminal
hostname Sw-Floor-1

line console 0
 password cisco
 login
 exit

enable secret class

line vty 0 15
 password cisco
 login
 exit

interface vlan 1
 ip address 192.168.1.20 255.255.255.0
 no shutdown
 exit

ip default-gateway 192.168.1.1
exit
```

### 3. Verification and saving

```
show running-config
show ip interface brief
copy running-config startup-config
reload
```

### 4. Host configuration

Disconnected the console cable and connected both hosts to the switch using straight-through Ethernet cables. Assigned static IP addressing to each host.

### 5. Connectivity testing

Verified connectivity using ICMP echo requests:

- PC → Laptop ✅
- PC → Switch VLAN 1 ✅
- Laptop → Switch VLAN 1 ✅

---

## What I learned

**Console vs network access:** The console cable provides out-of-band management access. Without an IP configured on VLAN 1, the switch is only reachable physically — this is why the management IP matters for remote administration.

**Switch VLAN 1 is not a routing interface:** Assigning an IP to VLAN 1 doesn't make the switch route traffic. It's purely for management access to the device itself.

**Password layers:** Console password protects physical access, enable secret protects privileged mode, and VTY passwords protect remote sessions. Each covers a different attack surface.

**Default gateway on a switch:** A Layer 2 switch needs a default gateway only for its own management traffic — not for forwarding host traffic.

---

## Commands reference

| Command | Purpose |
|---|---|
| `enable secret` | Encrypted privileged mode password |
| `line console 0` | Configure physical console access |
| `line vty 0 15` | Configure remote terminal sessions |
| `interface vlan 1` | Access the switch management interface |
| `ip default-gateway` | Set gateway for switch management traffic |
| `show ip interface brief` | Verify interface status and IP assignment |
| `copy running-config startup-config` | Save configuration to NVRAM |
