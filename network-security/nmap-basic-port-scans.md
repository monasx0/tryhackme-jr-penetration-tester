# Nmap Basic Port Scans

Port scanning identifies which network services are running on a host. A TCP or UDP port is used to identify a specific service running on that host. Each port can be in various states depending on whether a service is listening and if firewalls are blocking access.

---

## Port States

Nmap classifies ports into six states to provide detailed reconnaissance information. Understanding these states helps identify firewall configurations and service availability.

```bash
# States returned by Nmap:
# Open      - Service listening on the port
# Closed    - No service, but port is accessible (not firewalled)
# Filtered  - Port is not accessible, blocked by firewall
# Unfiltered- Port accessible but cannot determine if open/closed (ACK scan)
# Open|Filtered  - Cannot determine if open or filtered
# Closed|Filtered- Cannot determine if closed or filtered
```

---

## TCP Flags Overview

TCP header flags control connection establishment and data flow. Nmap manipulates these flags to determine port states without establishing full connections.

```bash
# TCP Flags (highlighted in red in RFC 793):
# URG - Urgent flag (process data immediately)
# ACK - Acknowledgement flag (receipt confirmation)
# PSH - Push flag (pass data to application promptly)
# RST - Reset flag (tear down connection)
# SYN - Synchronize flag (initiate 3-way handshake)
# FIN - Final flag (no more data to send)
```

---

## TCP Connect Scan (-sT)

TCP Connect scan completes the full 3-way handshake for each port. It's the only option for unprivileged users and is slower but more reliable when you lack root access.

```bash
# Basic TCP Connect scan
nmap -sT MACHINE_IP

# Scan specific ports
nmap -sT -p22,80,443 MACHINE_IP

# Fast mode (100 most common ports)
nmap -sT -F MACHINE_IP

# Scan ports in order instead of random
nmap -sT -r MACHINE_IP
```

---

## TCP SYN Scan (-sS)

TCP SYN scan is the default for privileged users. It doesn't complete the handshake, instead sending RST after receiving SYN/ACK. This is faster, stealthier, and less likely to be logged.

```bash
# Default privileged scan
sudo nmap -sS MACHINE_IP

# Scan specific ports
sudo nmap -sS -p22,80,443 MACHINE_IP

# SYN scan is default when running as root
sudo nmap MACHINE_IP
```

---

## UDP Scan (-sU)

UDP is connectionless, so there's no handshake. Closed UDP ports respond with ICMP port unreachable. Open UDP ports remain silent, making identification uncertain.

```bash
# UDP scan only
sudo nmap -sU MACHINE_IP

# Combined TCP and UDP scan
sudo nmap -sS -sU MACHINE_IP

# Scan specific UDP ports
sudo nmap -sU -p53,67,68 MACHINE_IP

# Scan top UDP ports
sudo nmap -sU --top-ports 20 MACHINE_IP
```

---

## Port Specification

Specify target ports instead of scanning the default 1000. Multiple formats are supported for flexibility.

```bash
# Specific ports (comma-separated)
nmap -p22,80,443 MACHINE_IP

# Port range
nmap -p1-1023 MACHINE_IP
nmap -p20-25 MACHINE_IP

# All 65535 ports
nmap -p- MACHINE_IP

# Fast mode (100 most common)
nmap -F MACHINE_IP

# Top N common ports
nmap --top-ports 10 MACHINE_IP
nmap --top-ports 100 MACHINE_IP

# Combine with scan type
sudo nmap -sS -p1-1023 MACHINE_IP
```

---

## Timing and Performance Tuning

Control scan speed using timing templates or packet rate/parallelism options. Balance between speed and accuracy based on your environment.

```bash
# Timing templates (-T0 to -T5)
nmap -T0 MACHINE_IP          # paranoid (1 port per 5 min)
nmap -T1 MACHINE_IP          # sneaky (stealth, real engagements)
nmap -T2 MACHINE_IP          # polite
nmap -T3 MACHINE_IP          # normal (default)
nmap -T4 MACHINE_IP          # aggressive (CTFs, practice)
nmap -T5 MACHINE_IP          # insane (fastest, may lose accuracy)

# Control packet rate (packets per second)
nmap --min-rate 100 MACHINE_IP
nmap --max-rate 10 MACHINE_IP

# Control probe parallelization
nmap --min-parallelism 512 MACHINE_IP
nmap --max-parallelism 100 MACHINE_IP

# Combine with other options
sudo nmap -sS -p- -T4 MACHINE_IP
sudo nmap -sS -p1-1023 -T1 MACHINE_IP
```

---

## Practical Examples

Real-world scanning scenarios combining multiple options for common use cases.

```bash
# Fast scan on practice target (CTF)
sudo nmap -sS -p- -T4 MACHINE_IP

# Stealthy scan on real engagement
sudo nmap -sS -p- -T1 --max-rate 5 MACHINE_IP

# Comprehensive TCP scan
sudo nmap -sS -p- -T3 MACHINE_IP

# TCP + UDP full scan
sudo nmap -sS -sU -p- -T3 MACHINE_IP

# Quick common ports
nmap -F MACHINE_IP

# Top 1000 ports on multiple hosts
nmap -sS 10.0.0.1-10 10.0.0.20-30
```

---

## Scan Type Comparison

| Scan Type | Flag | User | Speed | Stealth | Connection |
|-----------|------|------|-------|---------|------------|
| TCP Connect | -sT | Any | Slow | Low | Completes handshake |
| TCP SYN | -sS | Root | Fast | High | Partial handshake |
| UDP | -sU | Root | Varies | High | No handshake |

---

**Key Takeaway**: Use `-sS` for speed and stealth (requires root), `-sT` for unprivileged users, and combine with `-p-` to scan all ports.
