# Networking Labs

Hands-on networking labs built while studying for CompTIA Network+ (N10-009) and Cisco CCNA, using Cisco Packet Tracer and my home lab (GNS3, VirtualBox, and a managed switch). Each lab documents the real process: the configuration, the problems that came up, how I verified the result, and what I learned.

Companion repo to [Windows-AD-labs](https://github.com/PRPHLX/Windows-AD-labs).

## Labs

Each lab follows the same structure: objective, environment/topology, steps, verification, what I learned, and a commands reference.

| Lab | Topics | Tool | Status |
|---|---|---|---|
| [Lab 01 — Basic Switch Configuration](lab-01-basic-switch-config/) | Console access, device passwords, management SVI, default gateway, saving the configuration | Packet Tracer | ✅ Complete |
| [Lab 02 — GRE Tunnel](lab-02-gre-tunnel/) | GRE encapsulation, static routing, underlay vs overlay, tunnel troubleshooting | Packet Tracer | ✅ Complete |

More labs will be added as I work through Network+ and CCNA topics: VLANs and trunking on the home lab switch, subnetting, OSPF, and DMVPN/mGRE in GNS3.

## Why networking

I'm working toward an IT support role. Many tickets that look like application problems ("the shared drive is gone", "I can't reach the server") turn out to be DNS, addressing, or connectivity problems. Knowing how traffic normally moves is what makes a broken path easy to recognize, and it is the foundation for everything that comes after support, including security work.
