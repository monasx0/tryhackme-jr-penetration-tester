# Nmap - Complete Concepts & Definitions

Beginner-friendly definitions of all key Nmap concepts and networking principles discussed in the TryHackMe rooms.

---

## Networking Fundamentals

### IP Address
A unique numerical address assigned to each device on a network. Used to identify and communicate with devices across networks. Format: `192.168.1.1` (IPv4) or `2001:db8::1` (IPv6).

### MAC Address
A unique hardware identifier for network interface cards. Used for communication within the same local network (subnet). Format: `00:1A:2B:3C:4D:5E`. Required at the link layer before TCP/IP communication.

### Port
A logical communication endpoint on a device. Used to identify specific services running on a host. Ranges from 0 to 65535. Well-known ports: SSH (22), HTTP (80), HTTPS (443).

### Protocol
A set of rules governing how data is transmitted and received over a network. Common protocols: TCP (reliable, connection-oriented), UDP (fast, connectionless), ICMP (diagnostics).

### Subnet / Subnetwork
A logical division of an IP network. Contains a group of IP addresses that can communicate directly with each other without routing. Example: `192.168.1.0/24` contains 256 addresses.

### Firewall
A security system that monitors and controls incoming and outgoing network traffic. Permits or blocks packets based on predefined rules. Can be hardware-based or software-based.

### IDS (Intrusion Detection System)
A security system that monitors network traffic for suspicious behavior. Detects and alerts on malicious patterns or signatures. Can be network-based (NIDS) or host-based (HIDS).

### TTL (Time To Live)
A field in the IP header that decreases by 1 each time a packet passes through a router. Prevents packets from circulating indefinitely. Reaches zero, packet is discarded.

---

## Nmap Fundamentals

### Nmap
Network Mapper - an open-source tool for network discovery, port scanning, and service detection. Identifies active hosts, open ports, running services, and operating systems on networks.

### Host Discovery
The process of identifying which devices are currently online/active on a network. Nmap uses various techniques (ARP, ICMP, TCP) before attempting port scans to avoid wasting time on offline hosts.

### Port Scanning
The process of probing network ports on a target to determine if they are open, closed, or filtered. Identifies which services are listening and potentially accessible.

### Service Detection
The process of identifying what software/service is running on an open port and determining its version. Requires establishing connection to grab banners or service information.

### OS Fingerprinting / OS Detection
The process of identifying the operating system running on a target host based on its behavior, response patterns, and TCP/IP implementation characteristics.

---

## Port States

### Open
A service is actively listening and accepting connections on that port. The port responds to connection attempts. Indicates potential vulnerability exposure if the service is vulnerable.

### Closed
No service is listening on that port, but the port is accessible and not blocked by a firewall. The target responds to probe packets with RST (reset). Indicates the host is online.

### Filtered
Nmap cannot determine if the port is open or closed because a firewall, IDS, or other security device is blocking access. Port does not respond to probes. Cannot verify if service is running.

### Unfiltered
Nmap cannot determine if the port is open or closed, but the port is accessible (responds to probes). Typically encountered with ACK scan. Requires additional scans to determine true state.

### Open|Filtered
Nmap cannot determine whether the port is open or filtered. No response received, but absence of response could mean open or blocked. Requires further investigation.

### Closed|Filtered
Nmap cannot determine whether the port is closed or filtered. Response received but cannot definitively determine state. Rare condition from certain scan types.

---

## TCP/IP Layers

### Link Layer (Layer 1-2)
The lowest layer in TCP/IP model. Handles physical transmission of data between devices on the same network. Protocols: ARP, Ethernet. Uses MAC addresses. Nmap uses ARP here for host discovery.

### Network Layer (Layer 3)
Handles routing and logical addressing across networks. Protocols: IP, ICMP. Uses IP addresses to route packets between different networks. Nmap uses ICMP here for host discovery.

### Transport Layer (Layer 4)
Handles end-to-end communication and data reliability. Protocols: TCP (reliable), UDP (fast). Uses ports to identify services. Nmap manipulates TCP/UDP flags here for port scanning.

---

## TCP Flags

### SYN (Synchronize)
Flag that initiates a TCP connection. Client sends SYN to request connection. First step of TCP 3-way handshake. Used in SYN scans for port scanning.

### ACK (Acknowledge)
Flag that confirms receipt of data and acknowledges previous packets. Used in ongoing TCP communications. Nmap uses ACK scans to map firewall rules.

### FIN (Finish)
Flag indicating that the sender has finished sending data. Closes TCP connection gracefully. Nmap sends FIN in FIN scans to detect open ports.

### RST (Reset)
Flag that abruptly terminates a TCP connection. Sent when unexpected packet received or port is closed. Indicates to Nmap that the port is closed.

### PSH (Push)
Flag requesting TCP to push data to the application immediately without buffering. Tells recipient to process data right away.

### URG (Urgent)
Flag indicating that data in the packet is urgent and should be processed immediately. The urgent pointer field indicates where urgent data ends.

---

## TCP 3-Way Handshake

### Overview
The process of establishing a TCP connection. Involves three packets exchanged between client and server before data transmission begins.

### Step 1: SYN
Client sends TCP packet with SYN flag set to server's port. Includes client's initial sequence number.

### Step 2: SYN/ACK
Server responds with SYN/ACK flags set. Acknowledges client's sequence number and sends its own sequence number.

### Step 3: ACK
Client sends final ACK packet. Acknowledges server's sequence number. Connection is now established.

### Connection Termination
Either side can send FIN flag to gracefully close the connection. Requires acknowledgement from the other side.

---

## Nmap Scan Types

### SYN Scan (Stealth Scan)
Sends SYN packets and waits for SYN/ACK response. Does not complete handshake, instead sends RST immediately. Stealthier than Connect scan, less likely to be logged. Requires root privileges.

### Connect Scan
Completes full TCP 3-way handshake for each port. Establishes actual connection then closes it. More visible but reliable. Only option for unprivileged users.

### FIN Scan
Sends TCP packet with only FIN flag set. Open ports don't respond; closed ports respond with RST. Results in open|filtered state. Used to evade simple firewalls.

### Null Scan
Sends TCP packet with no flags set (all flags = 0). Open ports don't respond; closed ports respond with RST. Results in open|filtered state. Used to evade firewall detection.

### Xmas Scan
Sends TCP packet with FIN, PSH, and URG flags set simultaneously (looks like Christmas tree lights). Open ports don't respond; closed ports respond with RST. Results in open|filtered state.

### Maimon Scan
Sends TCP packet with FIN and ACK flags set. Exploits BSD behavior where open ports drop the packet. Unreliable on modern systems. More for understanding scan mechanisms.

### ACK Scan
Sends TCP packet with only ACK flag set. Cannot determine port state (all respond with RST). Used to map firewall rules, not detect open ports. Shows which ports firewall blocks.

### Window Scan
Similar to ACK scan but examines TCP Window field in responses. Can reveal port state on some systems. Better than ACK for firewall rule mapping on certain targets.

### UDP Scan
Sends UDP packets to ports. Open ports don't respond; closed ports respond with ICMP port unreachable. Useful for UDP service discovery. Much slower than TCP scans.

---

## Host Discovery Techniques

### ARP Scan
Sends ARP (Address Resolution Protocol) requests to discover hosts on the same subnet. Only works on local network. Very reliable for local discovery. Used by Nmap automatically for same-subnet scans.

### ICMP Echo Scan (Ping)
Sends ICMP Echo (ping) requests. Expects ICMP Echo Reply from online hosts. Many firewalls block ICMP, making this unreliable. Still useful for internal networks.

### ICMP Timestamp Scan
Sends ICMP Timestamp requests instead of Echo. Works when ICMP Echo is blocked. Requests timestamp information from target.

### ICMP Address Mask Scan
Sends ICMP Address Mask requests. Rarely used as most systems block this. Less reliable than other ICMP methods.

### TCP SYN Ping
Sends TCP SYN packet (default port 80). Expects SYN/ACK or RST response. Useful when ICMP is blocked. Works reliably across networks.

### TCP ACK Ping
Sends TCP ACK packet (default port 80). Expects RST response indicating host is alive. Useful when SYN is blocked. Less commonly used than SYN.

### UDP Ping
Sends UDP packets to closed ports. Expects ICMP port unreachable response indicating host is alive. Useful when TCP is blocked. Slower than TCP methods.

---

## Advanced Scanning Techniques

### Fragmentation
Dividing IP packets into smaller fragments to evade firewall/IDS detection. Default fragment size is 8 bytes. Can set custom size with --mtu. Used for evasion purposes.

### IP Spoofing
Forging source IP address in packets to hide true identity. Only works if attacker can receive responses (same network typically). Requires monitoring responses to determine results.

### Decoy Scan
Including additional decoy IP addresses in scan so attacker's IP is masked among multiple sources. Decoys receive responses, further obfuscating actual attacker. Makes scan attribution difficult.

### MAC Spoofing
Forging source MAC address in packets. Only works on same Ethernet/WiFi network. Used in conjunction with IP spoofing for network-level anonymity.

### Idle Scan (Zombie Scan)
Using a third-party idle host as intermediary to spoof scan origin. Attacker's IP remains hidden throughout scan. Extremely stealthy but complex. Requires finding suitable "zombie" host.

---

## Firewall & IDS Evasion

### Timing Templates
Nmap timing levels that control scan speed. Slower scans are stealthier but take longer. Faster scans are noisier but complete quicker. Ranges from Paranoid (T0) to Insane (T5).

### Packet Rate Control
Limiting packets per second to avoid detection. `--min-rate` and `--max-rate` allow granular control. Lower rates = stealthier. Higher rates = faster but louder.

### Probe Parallelization
Controlling how many probes run simultaneously. `--min-parallelism` and `--max-parallelism` adjust this. More parallelism = faster. Less parallelism = stealthier.

### Reverse-DNS Lookup
Nmap's automatic DNS query to resolve IP addresses to hostnames. Hostnames can reveal information. Can be skipped with `-n` flag to reduce network noise.

---

## Output & Reporting

### Normal Format
Human-readable Nmap output. Same format as terminal display. Good for reviewing results manually. Suitable for reports.

### Grepable Format
Optimized for `grep` command filtering. Each line contains complete host information. Better for parsing and automated processing. Compact but less readable.

### XML Format
Structured machine-readable format. Ideal for parsing by scripts and security tools. Can be imported into dashboards and frameworks. Most flexible for integration.

### Verbosity
Controlling level of detail in output. `-v` provides standard verbosity. `-vv` provides very verbose output. `-d` and `-dd` provide debugging information.

### Reason Flag
Shows explicit reason why Nmap concluded a port is open or host is online. Example: "syn-ack", "reset", "arp-response". Useful for understanding scan logic.

---

## Service & System Information

### Service Detection
Identifying what application/service is running on an open port. Requires connecting to port and analyzing response banners. Provides version information for vulnerability research.

### Version Intensity
Controls how deeply Nmap probes services for version information. Range 0-9 where 0 is lightest and 9 is most thorough. Higher intensity = more accurate but slower.

### OS Fingerprinting
Analyzing target's TCP/IP behavior to determine operating system. Examines response patterns, TTL values, window sizes, and other characteristics. More reliable with both open and closed ports.

### Network Distance
Number of network hops (routers) between scanner and target. Helps identify if target is local or remote. Determined from TTL responses.

---

## Nmap Scripting Engine (NSE)

### NSE (Nmap Scripting Engine)
Nmap's built-in scripting system using Lua language. Extends Nmap functionality beyond basic scanning. ~600 default scripts included. Allows vulnerability detection, exploitation, and custom reconnaissance.

### Script Categories
Groupings of NSE scripts by function: auth, broadcast, brute, default, discovery, dos, exploit, external, fuzzer, intrusive, malware, safe, version, vuln. Each category serves specific reconnaissance purpose.

### Default Scripts
Baseline NSE scripts in the "default" category. Safe, non-intrusive scripts that run by default with `-sC`. Provide service enumeration, banner grabbing, and basic information gathering.

### Safe Scripts
NSE scripts that won't crash targets or cause harm. Can be safely run on production systems. Recommended for authorized testing without risk of disruption.

### Intrusive Scripts
NSE scripts that perform aggressive actions like brute-force, exploitation, or DoS. Can crash targets or disrupt services. Require explicit authorization before execution.

---

**Key Principle**: Understanding these concepts allows you to choose appropriate scanning techniques for any reconnaissance scenario, balance between speed and stealth, and interpret Nmap results accurately.
