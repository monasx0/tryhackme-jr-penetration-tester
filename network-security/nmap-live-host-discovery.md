# Nmap Live Host Discovery
Nmap (Network Mapper) is an industry-standard open-source tool for network reconnaissance. It identifies live hosts and discovers running services using various scanning techniques across different TCP/IP layers. Before port scanning, host discovery is crucial to avoid wasting time on offline systems.

---

## Target Enumeration

Specify targets as lists, ranges, or subnets before scanning. Use `-sL` to preview targets without scanning.

```bash
# List of targets
nmap MACHINE_IP scanme.nmap.org example.com

# IP range
nmap 10.11.12.15-20

# Subnet
nmap MACHINE_IP/30

# From file
nmap -iL list_of_hosts.txt

# Preview hosts without scanning
nmap -sL TARGETS

# Preview without reverse-DNS lookup
nmap -sL -n TARGETS
```

---

## ARP Scan (Link Layer)

ARP scan works only on the same subnet. It sends ARP requests to discover live hosts by retrieving MAC addresses. Requires privileged access.

```bash
# Nmap ARP scan
sudo nmap -PR -sn TARGETS

# Example: Scan /24 subnet
sudo nmap -PR -sn 10.10.210.6/24

# Using arp-scan tool
sudo arp-scan -l
sudo arp-scan 10.10.210.6/24
sudo arp-scan -I eth0 -l
```

---

## ICMP Echo Scan (Network Layer)

Sends ICMP Echo requests (Type 8) to discover live hosts. Many firewalls block ICMP, making this method unreliable. Windows hosts block ICMP by default.

```bash
# ICMP Echo request
sudo nmap -PE -sn MACHINE_IP/24

# Example
sudo nmap -PE -sn 10.10.68.220/24
```

---

## ICMP Timestamp Scan

Uses ICMP Timestamp requests (Type 13) instead of Echo requests. Useful when ICMP Echo is blocked.

```bash
# ICMP Timestamp scan
sudo nmap -PP -sn MACHINE_IP/24

# Example
sudo nmap -PP -sn 10.10.68.220/24
```

---

## ICMP Address Mask Scan

Sends ICMP Address Mask requests (Type 17) to detect online hosts. Often blocked by firewalls, making it unreliable.

```bash
# ICMP Address Mask scan
sudo nmap -PM -sn MACHINE_IP/24

# Example
sudo nmap -PM -sn 10.10.68.220/24
```

---

## TCP SYN Ping (Transport Layer)

Sends TCP SYN packets to port 80 (default) and waits for SYN/ACK response. Efficient because privileged users don't complete the handshake.

```bash
# TCP SYN Ping on default port 80
sudo nmap -PS -sn MACHINE_IP/24

# Specify port(s)
sudo nmap -PS21 -sn TARGETS
sudo nmap -PS21-25 -sn TARGETS
sudo nmap -PS80,443,8080 -sn TARGETS

# Example
sudo nmap -PS -sn 10.10.68.220/24
```

---

## TCP ACK Ping (Transport Layer)

Sends TCP ACK packets to port 80 (default) and expects RST response. Useful for firewall evasion since ACK packets are often less filtered than SYN packets. Requires root access.

```bash
# TCP ACK Ping on default port 80
sudo nmap -PA -sn MACHINE_IP/24

# Specify port(s)
sudo nmap -PA21 -sn TARGETS
sudo nmap -PA21-25 -sn TARGETS
sudo nmap -PA80,443,8080 -sn TARGETS

# Example
sudo nmap -PA -sn 10.10.68.220/24
```

---

## UDP Ping (Transport Layer)

Sends UDP packets to closed UDP ports and listens for ICMP port unreachable responses. Indicates host is online if ICMP error is received.

```bash
# UDP Ping on default ports
sudo nmap -PU -sn MACHINE_IP/24

# Specify port(s)
sudo nmap -PU53 -sn TARGETS
sudo nmap -PU53,161 -sn TARGETS

# Example
sudo nmap -PU -sn 10.10.68.220/24
```

---

## Default Nmap Host Discovery Behavior

Nmap automatically selects discovery methods based on user privilege and target location:

- **Privileged user, local subnet**: ARP requests
- **Privileged user, external network**: ICMP Echo + TCP ACK (80) + TCP SYN (443) + ICMP Timestamp
- **Unprivileged user, external network**: TCP 3-way handshake on ports 80 and 443

```bash
# Basic ping scan (default behavior)
nmap -sn TARGETS

# Full host discovery with port scan
nmap MACHINE_IP
```

---

## Masscan

Aggressive mass scanner similar to Nmap but generates packets at extremely high rates for faster scanning.

```bash
# Scan single port
masscan MACHINE_IP/24 -p443

# Scan multiple ports
masscan MACHINE_IP/24 -p80,443

# Scan port range
masscan MACHINE_IP/24 -p22-25

# Scan top 100 ports
masscan MACHINE_IP/24 --top-ports 100
```

---

## Reverse-DNS Lookup

Nmap performs reverse-DNS lookups on discovered hosts by default. Hostnames can reveal valuable information.

```bash
# Skip DNS lookups
nmap -n TARGETS

# Force DNS lookup on all hosts (including offline)
nmap -R TARGETS

# Use specific DNS server
nmap --dns-servers DNS_SERVER TARGETS
```

---

## Quick Reference

| Method | Flag | Default Port | Best For |
|--------|------|--------------|----------|
| ARP | `-PR` | - | Same subnet |
| ICMP Echo | `-PE` | - | Local network |
| ICMP Timestamp | `-PP` | - | When Echo blocked |
| ICMP Mask | `-PM` | - | Less common |
| TCP SYN | `-PS` | 80 | Privileged users |
| TCP ACK | `-PA` | 80 | Firewall evasion |
| UDP | `-PU` | 40125 | Closed ports |

---

**Key Takeaway**: Always use `-sn` flag to perform host discovery without port scanning.
