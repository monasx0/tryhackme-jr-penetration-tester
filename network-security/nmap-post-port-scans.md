# Nmap Post Port Scans

Post-scanning involves identifying services running on open ports, detecting the operating system, using NSE scripts for deeper reconnaissance, and saving results in useful formats. Service and OS detection requires establishing TCP connections, so stealth scans are not possible with these options.

---

## Service Detection (-sV)

Probe open ports to identify running services and software versions. Connects to each port to grab banners and version information. Cannot be combined with stealth SYN scan.

```bash
# Basic service detection
sudo nmap -sV MACHINE_IP

# Light version detection (intensity 2)
sudo nmap -sV --version-light MACHINE_IP

# Complete version detection (intensity 9)
sudo nmap -sV --version-all MACHINE_IP

# Custom intensity level (0-9)
sudo nmap -sV --version-intensity 5 MACHINE_IP

# Combined with port scan
sudo nmap -sV -p22,80,443 MACHINE_IP

# Service detection + OS detection
sudo nmap -sV -O MACHINE_IP
```

---

## OS Detection (-O)

Detect target's operating system based on behavior and response patterns. Requires at least one open and one closed port for reliable detection. Accuracy affected by virtualization and network configuration.

```bash
# Basic OS detection
sudo nmap -O MACHINE_IP

# OS detection with SYN scan
sudo nmap -sS -O MACHINE_IP

# Aggressive OS detection (all techniques)
sudo nmap -O --osscan-guess MACHINE_IP

# Maximum OS detection effort
sudo nmap -O --osscan-limit MACHINE_IP

# Combined with service detection
sudo nmap -sV -O MACHINE_IP
```

---

## Traceroute (--traceroute)

Discover routers and network hops between attacker and target. Nmap's traceroute starts with high TTL and decreases it (opposite of standard traceroute).

```bash
# Traceroute to target
sudo nmap --traceroute MACHINE_IP

# Traceroute with SYN scan
sudo nmap -sS --traceroute MACHINE_IP

# Traceroute with service detection
sudo nmap -sV --traceroute MACHINE_IP

# Full reconnaissance with traceroute
sudo nmap -sS -sV -O --traceroute MACHINE_IP
```

---

## Nmap Scripting Engine (NSE)

NSE extends Nmap with Lua scripts for vulnerability detection, service enumeration, exploitation, and custom reconnaissance. ~600 scripts available by default at `/usr/share/nmap/scripts`.

### Running Default Scripts (-sC)

```bash
# Run default scripts category
sudo nmap -sC MACHINE_IP

# Equivalent to --script=default
sudo nmap --script=default MACHINE_IP

# Default scripts with SYN scan
sudo nmap -sS -sC MACHINE_IP

# Default scripts with service detection
sudo nmap -sV -sC MACHINE_IP
```

### Running Specific Scripts

```bash
# Single script by name
sudo nmap --script "http-date" MACHINE_IP

# Pattern matching (wildcard)
sudo nmap --script "ftp*" MACHINE_IP
sudo nmap --script "http*" MACHINE_IP

# Multiple specific scripts
sudo nmap --script "http-date,http-robots.txt" MACHINE_IP

# Script by category
sudo nmap --script "auth" MACHINE_IP
sudo nmap --script "vuln" MACHINE_IP
sudo nmap --script "safe" MACHINE_IP
```

### NSE Script Categories

| Category | Purpose |
|----------|---------|
| auth | Authentication scripts |
| broadcast | Discover hosts via broadcast |
| brute | Brute-force password attacks |
| default | Default safe scripts |
| discovery | Retrieve accessible information |
| dos | Denial of Service detection |
| exploit | Exploitation attempts |
| external | Third-party service checks |
| fuzzer | Fuzzing attacks |
| intrusive | Intrusive attacks/exploitation |
| malware | Backdoor detection |
| safe | Non-destructive scripts |
| version | Service version detection |
| vuln | Vulnerability checks |

### Common NSE Scripts

```bash
# SSH enumeration
sudo nmap --script "ssh-hostkey" MACHINE_IP

# HTTP enumeration
sudo nmap --script "http-title,http-headers" MACHINE_IP

# RPC enumeration
sudo nmap --script "rpcinfo" MACHINE_IP

# SMTP enumeration
sudo nmap --script "smtp-commands" MACHINE_IP

# SMB enumeration
sudo nmap --script "smb-enum*" MACHINE_IP

# Vulnerability scanning
sudo nmap --script "vuln" MACHINE_IP

# Brute force (use with caution)
sudo nmap --script "brute" MACHINE_IP
```

---

## Complete Reconnaissance Scan

Combine multiple techniques for comprehensive target assessment.

```bash
# Full aggressive scan
sudo nmap -sS -sV -O -sC MACHINE_IP

# With traceroute and service details
sudo nmap -sS -sV -O -sC --traceroute MACHINE_IP

# All port scan with comprehensive detection
sudo nmap -sS -p- -sV -O -sC MACHINE_IP

# Top 1000 ports with everything
sudo nmap -sS -sV -O -sC --top-ports 1000 MACHINE_IP
```

---

## Output Formats

Save scan results in various formats for analysis, reporting, and integration with other tools.

### Normal Format (-oN)

Human-readable format, same as terminal output.

```bash
# Save in normal format
sudo nmap -sV -O -oN MACHINE_IP_scan.nmap MACHINE_IP

# Example filename convention
sudo nmap -sV -p- -oN target_full_scan.nmap MACHINE_IP
```

### Grepable Format (-oG)

Optimized for grep searches. Each line contains complete host information. Better for parsing multiple results.

```bash
# Save in grepable format
sudo nmap -sV -O -oG MACHINE_IP_scan.gnmap MACHINE_IP

# Filter results with grep
grep "80/open" MACHINE_IP_scan.gnmap
grep "ssh" MACHINE_IP_scan.gnmap

# Extract specific ports across multiple targets
grep "443/open" *.gnmap
```

### XML Format (-oX)

Structured format for automated processing and integration with other tools and security frameworks.

```bash
# Save in XML format
sudo nmap -sV -O -oX MACHINE_IP_scan.xml MACHINE_IP

# Parse XML with other tools
```

### Combined Format (-oA)

Save in all three formats simultaneously (normal, grepable, XML).

```bash
# Save in all formats at once
sudo nmap -sV -O -oA MACHINE_IP_scan MACHINE_IP

# Creates: MACHINE_IP_scan.nmap, MACHINE_IP_scan.gnmap, MACHINE_IP_scan.xml
```

### Script Kiddie Format (-oS)

"Leet" format with random capitalization and character substitutions. Not recommended for professional use.

```bash
# Save in script kiddie format (not recommended)
sudo nmap -sV -O -oS MACHINE_IP_scan.kiddie MACHINE_IP
```

---

## Output Examples

### Normal Format Output

```
# Nmap 7.60 scan initiated Fri Sep 10 05:14:19 2021
Nmap scan report for MACHINE_IP
Host is up (0.00086s latency).

PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 6.7p1 Debian 5+deb8u8 (protocol 2.0)
80/tcp  open  http    nginx 1.6.2

OS details: Linux 3.13
# Nmap done at Fri Sep 10 05:14:28 2021 -- 1 IP address scanned
```

### Grepable Format Output

```
Host: MACHINE_IP	Status: Up
Host: MACHINE_IP	Ports: 22/open/tcp//ssh//OpenSSH 6.7p1/, 80/open/tcp//http//nginx 1.6/	OS: Linux 3.13
```

---

## Practical Scanning Workflows

### Full Service Enumeration

```bash
# Discover all services with versions
sudo nmap -sS -p- -sV MACHINE_IP -oA full_scan
```

### OS and Service Detection

```bash
# Identify OS and services
sudo nmap -sS -sV -O MACHINE_IP -oN os_service_scan.nmap
```

### NSE Vulnerability Scanning

```bash
# Check for known vulnerabilities
sudo nmap -sV --script=vuln MACHINE_IP -oA vuln_scan

# SSH-specific enumeration
sudo nmap -p22 --script=ssh* MACHINE_IP
```

### Web Service Enumeration

```bash
# HTTP service detection and enumeration
sudo nmap -p80,443 -sV --script=http* MACHINE_IP -oA web_scan
```

### Save Everything

```bash
# Comprehensive scan with all formats
sudo nmap -sS -p- -sV -O -sC --traceroute MACHINE_IP -oA comprehensive_scan

# Output files: comprehensive_scan.nmap, comprehensive_scan.gnmap, comprehensive_scan.xml
```

---

## Output File Naming Convention

Organize scan results with descriptive naming:

```bash
# Format: TARGET_DATE_SCANTYPE.FORMAT

# Examples:
MACHINE_IP_20240115_basic.nmap
MACHINE_IP_20240115_full.gnmap
target.com_20240115_aggressive.xml
192.168.1.0-24_20240115_udp.nmap
```

---

**Key Takeaway**: Use `-sV` for service detection, `-O` for OS detection, `-sC` for NSE default scripts, and `-oA` to save in all formats simultaneously for maximum information gathering.
