# Lab 01 — Basic Switch Configuration

Configuration of a Cisco switch from scratch using console access, followed by IP connectivity validation across a small LAN.

**Tool:** Cisco Packet Tracer
**Source:** Based on a Cisco Networking Academy (NetAcad) Packet Tracer activity. The write-up, explanations and next steps are my own.

---

## Objective

Configure a Cisco 2960 switch with basic access security and a management IP address, then verify end-to-end connectivity between hosts on the same network.

---

## Environment / Topology

| Device | Interface | IP Address | Subnet Mask | Gateway |
|---|---|---|---|---|
| Sw-Floor-1 | VLAN 1 (SVI) | 192.168.1.20 | 255.255.255.0 | 192.168.1.1 |
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

The passwords are the activity's lab defaults, not values to reuse anywhere real.

### 3. Verification and saving

```
show running-config
show ip interface brief
copy running-config startup-config
reload
```

### 4. Host configuration

Disconnected the console cable and connected both hosts to the switch using straight-through Ethernet cables. Assigned static IP addressing to each host.

---

## Verification

Verified connectivity using ICMP echo requests:

| Test | Result |
|---|---|
| PC → Laptop | ✅ Success |
| PC → Switch VLAN 1 | ✅ Success |
| Laptop → Switch VLAN 1 | ✅ Success |

---

## What I learned

**Console vs network access:** The console cable provides out-of-band management access. Without an IP configured on VLAN 1, the switch is only reachable physically. This is why the management IP matters for remote administration.

**Switch VLAN 1 is not a routing interface:** Assigning an IP to VLAN 1 does not make the switch route traffic. It is purely for management access to the device itself.

**Password layers:** The console password protects physical access, the enable secret protects privileged mode, and the VTY passwords protect remote sessions. Each covers a different attack surface.

**`enable secret` vs `password`:** `enable secret` is stored as a one-way hash, not encrypted. The line passwords (`password cisco`) are stored in plain text in the running configuration unless `service password-encryption` is enabled, and even then that is only weak, reversible obfuscation.

**Default gateway on a switch:** A Layer 2 switch needs a default gateway only for its own management traffic, not for forwarding host traffic.

---

## Commands reference

| Command | Purpose |
|---|---|
| `enable secret` | Hashed password for privileged EXEC mode |
| `line console 0` | Configure physical console access |
| `line vty 0 15` | Configure remote terminal sessions |
| `interface vlan 1` | Access the switch management interface (SVI) |
| `ip default-gateway` | Set gateway for switch management traffic |
| `show ip interface brief` | Verify interface status and IP assignment |
| `copy running-config startup-config` | Save configuration to NVRAM |

---

## Next steps

The original activity leaves remote access on Telnet with plain-text passwords. Planned extension, to be documented here once done:

- Replace Telnet with SSH (`ip domain-name`, `crypto key generate rsa`, a local user, `transport input ssh`, `login local`).
- Enable `service password-encryption` and add a login banner.
- Move management off VLAN 1 to a dedicated management VLAN.
