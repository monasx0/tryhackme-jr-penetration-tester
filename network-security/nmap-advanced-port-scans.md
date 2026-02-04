# Nmap Advanced Port Scans
Advanced scanning techniques exploit TCP behavior and firewall configurations. These methods are useful for evading IDS/firewall detection and mapping firewall rules. Understanding each technique helps choose the right approach for different scenarios.

---

## Null Scan (-sN)

Sends TCP packets with no flags set (all six bits are zero). Open ports don't respond; closed ports respond with RST. Results show `open|filtered` when uncertain due to firewall blocking.

```bash
# Null scan
sudo nmap -sN MACHINE_IP

# With specific ports
sudo nmap -sN -p22,80,443 MACHINE_IP

# Combined with other options
sudo nmap -sN -p- -T4 MACHINE_IP
```

---

## FIN Scan (-sF)

Sends TCP packets with FIN flag set. Open ports don't respond; closed ports respond with RST. Like Null scan, cannot distinguish between open and filtered ports due to firewall rules.

```bash
# FIN scan
sudo nmap -sF MACHINE_IP

# Scan specific ports
sudo nmap -sF -p22,80,443 MACHINE_IP

# Fast FIN scan
sudo nmap -sF -F MACHINE_IP
```

---

## Xmas Scan (-sX)

Named after Christmas tree lights, sets FIN, PSH, and URG flags simultaneously. Open ports don't respond; closed ports respond with RST. Results in `open|filtered` state for uncertain ports due to firewall filtering.

```bash
# Xmas scan
sudo nmap -sX MACHINE_IP

# Xmas scan with port range
sudo nmap -sX -p1-1023 MACHINE_IP

# Xmas scan on common ports
sudo nmap -sX -F MACHINE_IP
```

---

## TCP Maimon Scan (-sM)

Sets FIN and ACK flags simultaneously. Described by Uriel Maimon in 1996. BSD-derived systems drop packets at open ports, exposing them. Modern systems respond with RST regardless of port state, making this scan ineffective on most targets.

```bash
# Maimon scan
sudo nmap -sM MACHINE_IP

# Scan specific ports
sudo nmap -sM -p22,80,443 MACHINE_IP

# Note: Rarely useful on modern systems
```

---

## TCP ACK Scan (-sA)

Sends TCP packets with ACK flag set. Targets respond with RST regardless of port state. Without firewall, all ports appear `unfiltered`. With firewall, unblocked ports show `unfiltered`; blocked ports show `filtered`. Useful for **mapping firewall rules, not finding open services**.

```bash
# ACK scan (maps firewall rules)
sudo nmap -sA MACHINE_IP

# ACK scan specific ports
sudo nmap -sA -p22,80,443 MACHINE_IP

# ACK scan full port range
sudo nmap -sA -p- MACHINE_IP
```

---

## Window Scan (-sW)

Similar to ACK scan but examines TCP Window field in RST responses. On specific systems, reveals port state. Works better than ACK scan against some firewalls. Shows `closed` for unfiltered ports and `filtered` for blocked ports.

```bash
# Window scan
sudo nmap -sW MACHINE_IP

# Window scan with port range
sudo nmap -sW -p1-1023 MACHINE_IP

# Compare with ACK scan
sudo nmap -sA MACHINE_IP
sudo nmap -sW MACHINE_IP
```

---

## Custom Scan (--scanflags)

Create custom TCP flag combinations beyond built-in scan types. Useful for testing specific scenarios and understanding target behavior. Syntax: `--scanflags FLAGNAME1FLAGNAME2...`

```bash
# Custom scan with SYN, RST, and FIN
sudo nmap --scanflags RSTSYNFIN MACHINE_IP

# Custom scan with URG and PSH
sudo nmap --scanflags URGPSH -p22,80,443 MACHINE_IP

# ACK and SYN combination
sudo nmap --scanflags ACKSYNFIN MACHINE_IP
```

---

## IP Spoofing (-S)

Spoof source IP address to hide true identity. Only works if attacker can monitor network responses. Requires specifying network interface and disabling ping scan.

```bash
# Spoof IP address
sudo nmap -e NET_INTERFACE -Pn -S SPOOFED_IP MACHINE_IP

# Example
sudo nmap -e eth0 -Pn -S 10.10.0.5 MACHINE_IP

# Spoof MAC address (same subnet only)
sudo nmap --spoof-mac SPOOFED_MAC MACHINE_IP

# Random MAC
sudo nmap --spoof-mac 0 MACHINE_IP
```

---

## Decoy Scan (-D)

Make scan appear to come from multiple IP addresses, hiding true source among decoys. Decoys receive response traffic, masking attacker's IP.

```bash
# Decoy scan with specific IPs
sudo nmap -D 10.10.0.1,10.10.0.2,ME MACHINE_IP

# Decoy scan with random IPs
sudo nmap -D 10.10.0.1,10.10.0.2,RND,RND,ME MACHINE_IP

# Decoy scan with many random sources
sudo nmap -D RND,RND,RND,ME MACHINE_IP

# Combine with scan type
sudo nmap -sS -D RND,RND,ME MACHINE_IP
```

---

## Fragmented Packets (-f)

Fragment IP packets into smaller chunks to evade IDS/firewall detection. Default fragments are 8 bytes; `-ff` uses 16-byte fragments. Must be multiple of 8.

```bash
# Fragment packets into 8-byte chunks
sudo nmap -sS -f MACHINE_IP

# Double fragmentation (16-byte chunks)
sudo nmap -sS -ff MACHINE_IP

# Custom MTU (must be multiple of 8)
sudo nmap -sS --mtu 24 MACHINE_IP

# Fragment with port scan
sudo nmap -sS -f -p80,443 MACHINE_IP

# Append data to packets
sudo nmap -sS --data-length 256 MACHINE_IP
```

---

## Idle/Zombie Scan (-sI)

Spoofs packets to appear from idle host (zombie) by monitoring target's IP ID field. Extremely stealthy as scanner's IP is hidden. Requires finding idle system on network that scanner can communicate with.

**Three-step process:**
1. Trigger idle host, record IP ID
2. Send spoofed SYN to target (appears from zombie IP)
3. Trigger zombie again, compare new IP ID

**Results:**
- IP ID increased by 1 = port closed/filtered
- IP ID increased by 2 = port open

```bash
# Idle scan using zombie IP
sudo nmap -sI ZOMBIE_IP MACHINE_IP

# Example
sudo nmap -sI 10.10.0.50 MACHINE_IP

# Idle scan with verbose output
sudo nmap -sI ZOMBIE_IP -v MACHINE_IP
```

---

## Output and Verbosity

Get detailed information about scan reasoning and process. Useful for debugging and understanding scan behavior.

```bash
# Show reason for conclusions
sudo nmap -sS --reason MACHINE_IP

# Verbose output (basic details)
sudo nmap -sS -v MACHINE_IP

# Very verbose output
sudo nmap -sS -vv MACHINE_IP

# Debug output
sudo nmap -sS -d MACHINE_IP

# Extra debug details
sudo nmap -sS -dd MACHINE_IP

# Combine verbosity with other options
sudo nmap -sS --reason -v MACHINE_IP
```

---

## Scan Type Comparison

| Scan | Flag | Firewall Blocking | Common Use |
|------|------|-------------------|------------|
| Null | None | open\|filtered | Evade IDS |
| FIN | FIN | open\|filtered | Evade IDS |
| Xmas | FIN,PSH,URG | open\|filtered | Evade IDS |
| Maimon | FIN,ACK | Unreliable | Historical |
| ACK | ACK | Shows rules | Map firewall |
| Window | ACK (window check) | Shows rules | Map firewall |
| Spoofing | Custom | Requires monitor | Hide identity |
| Idle | IP ID check | Extremely stealthy | Maximum stealth |

---

## Practical Scenarios

**Stealth scanning:**
```bash
sudo nmap -sS -f -T1 --max-rate 5 MACHINE_IP
```

**Firewall mapping:**
```bash
sudo nmap -sA -p- MACHINE_IP
sudo nmap -sW -p- MACHINE_IP
```

**Evasion with decoys:**
```bash
sudo nmap -sS -D RND,RND,ME MACHINE_IP
```

**Maximum stealth (if zombie available):**
```bash
sudo nmap -sI ZOMBIE_IP MACHINE_IP
```

---

**Key Takeaway**: Use Null/FIN/Xmas for IDS evasion, ACK/Window for firewall mapping, and Idle scan for maximum stealth when resources are available.
