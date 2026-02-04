# Nmap Commands
Comprehensive table of all Nmap commands discussed in TryHackMe rooms with descriptions and use cases.

---

## Host Discovery Commands

| Command | Syntax | Description |
|---------|--------|-------------|
| Basic Host Discovery | `nmap -sn TARGET` | Performs ping scan to discover live hosts without port scanning. Default method uses ARP/ICMP. |
| ARP Scan | `sudo nmap -PR -sn TARGET` | Discovers live hosts on same subnet using ARP requests. Most reliable for local network scanning. |
| ICMP Echo Scan | `sudo nmap -PE -sn TARGET` | Uses ICMP Echo requests to find live hosts. May be blocked by firewalls. Useful for internal networks. |
| ICMP Timestamp Scan | `sudo nmap -PP -sn TARGET` | Uses ICMP Timestamp requests instead of Echo. Works when ICMP Echo is blocked by firewall. |
| ICMP Address Mask Scan | `sudo nmap -PM -sn TARGET` | Uses ICMP Address Mask requests. Rarely effective as most systems block this type. |
| TCP SYN Ping | `sudo nmap -PS -sn TARGET` | Sends TCP SYN to port 80 (default). Expects SYN/ACK or RST response. Effective across networks. |
| TCP SYN Ping Custom Port | `sudo nmap -PS21,80,443 -sn TARGET` | TCP SYN ping on specific ports. Can specify single port, range, or list of ports. |
| TCP ACK Ping | `sudo nmap -PA -sn TARGET` | Sends TCP ACK to port 80 (default). Expects RST response. Less commonly used than SYN. |
| TCP ACK Ping Custom Port | `sudo nmap -PA21-25 -sn TARGET` | TCP ACK ping on port range. Useful when SYN is blocked. |
| UDP Ping | `sudo nmap -PU -sn TARGET` | Sends UDP packets to closed ports. Listens for ICMP port unreachable. Slower but effective method. |
| List Targets | `nmap -sL TARGET` | Lists all targets that would be scanned without actually scanning. Performs reverse-DNS lookups. |
| List Targets No DNS | `nmap -sL -n TARGET` | Lists targets without performing DNS resolution. Reduces network noise. |
| arp-scan | `sudo arp-scan -l` | Alternative tool for ARP scanning. Discovers hosts on local network using ARP requests. |
| arp-scan Specific Interface | `sudo arp-scan -I eth0 -l` | ARP scan on specific network interface. Useful for multi-homed systems. |

---

## Basic Port Scanning Commands

| Command | Syntax | Description |
|---------|--------|-------------|
| TCP Connect Scan | `nmap -sT TARGET` | Completes full TCP 3-way handshake for each port. Only option for unprivileged users. More visible but reliable. |
| TCP SYN Scan | `sudo nmap -sS TARGET` | Default privileged scan. Stealthy as it doesn't complete handshake. Faster and less logged than Connect scan. |
| UDP Scan | `sudo nmap -sU TARGET` | Scans UDP ports instead of TCP. Useful for discovering UDP services. Much slower than TCP scans. |
| Combined TCP/UDP | `sudo nmap -sS -sU TARGET` | Scans both TCP and UDP ports simultaneously. Most comprehensive for service discovery. |
| Scan Default 1000 Ports | `nmap TARGET` | Scans 1000 most common ports. Nmap default behavior. Balances speed and coverage. |
| Fast Scan | `nmap -F TARGET` | Scans only 100 most common ports. Quick reconnaissance of most likely open services. |
| All Ports | `nmap -p- TARGET` | Scans all 65535 ports. Time-consuming but comprehensive. Catches non-standard service ports. |
| Top N Ports | `nmap --top-ports 10 TARGET` | Scans the 10 most common ports. Adjust number as needed. Quick targeted scanning. |
| Specific Port | `nmap -p80 TARGET` | Scans only port 80. Efficient when targeting specific service. Can specify multiple ports. |
| Port Range | `nmap -p80-443 TARGET` | Scans ports 80-443 inclusive. Useful for scanning ranges like web servers or well-known services. |
| Port List | `nmap -p22,80,443 TARGET` | Scans specific ports: 22, 80, 443. Any combination can be specified with commas. |
| Sequential Ports | `nmap -r TARGET` | Scans ports in consecutive order instead of random. Useful for testing consistent behavior. |

---

## Advanced Scanning Commands

| Command | Syntax | Description |
|---------|--------|-------------|
| Null Scan | `sudo nmap -sN TARGET` | Sends TCP packet with no flags set. Detects open ports by lack of response. Evades simple firewalls. |
| FIN Scan | `sudo nmap -sF TARGET` | Sends TCP packet with only FIN flag. Open ports don't respond; closed ports send RST. Firewall evasion. |
| Xmas Scan | `sudo nmap -sX TARGET` | Sets FIN, PSH, URG flags simultaneously. Open ports silent; closed ports respond with RST. IDS evasion. |
| Maimon Scan | `sudo nmap -sM TARGET` | Sets FIN and ACK flags. Exploits BSD behavior. Unreliable on modern systems. Historical significance. |
| ACK Scan | `sudo nmap -sA TARGET` | Sends TCP packet with ACK flag. Maps firewall rules, not port states. Shows which ports are unblocked. |
| Window Scan | `sudo nmap -sW TARGET` | Similar to ACK scan but examines TCP Window field. Better for some firewall configurations. |
| Custom Flags | `sudo nmap --scanflags RSTSYNFIN TARGET` | Creates custom scan with specified flags. Allows experimentation with flag combinations. |
| Fragmented Packets | `sudo nmap -sS -f TARGET` | Fragments packets into 8-byte chunks. Evades IDS/firewall detection of malicious patterns. |
| Double Fragmentation | `sudo nmap -sS -ff TARGET` | Fragments into 16-byte chunks. More fragmentation = harder to detect but slower scanning. |
| Custom MTU | `sudo nmap -sS --mtu 24 TARGET` | Sets custom packet size (must be multiple of 8). Fine-tunes fragmentation for specific filters. |
| Add Junk Data | `sudo nmap -sS --data-length 256 TARGET` | Appends bytes of data to packets. Makes scan traffic appear more legitimate. |
| IP Spoofing | `sudo nmap -e eth0 -Pn -S 10.10.0.5 TARGET` | Spoofs source IP address. Requires monitoring network responses. Only works in specific setups. |
| Spoof MAC | `sudo nmap --spoof-mac SPOOFED_MAC TARGET` | Spoofs source MAC address. Only works on same Ethernet network. Enhances anonymity. |
| Random MAC | `sudo nmap --spoof-mac 0 TARGET` | Spoofs random MAC address. Useful for testing MAC filtering without knowing valid MACs. |
| Decoy Scan | `sudo nmap -D 10.10.0.1,10.10.0.2,ME TARGET` | Makes scan appear from multiple IPs. Attacker's IP hidden among decoys. Decoys receive responses. |
| Decoy Random | `sudo nmap -D RND,RND,ME TARGET` | Uses random IPs as decoys. Different random IPs on each scan. More difficult to filter. |
| Idle Scan | `sudo nmap -sI ZOMBIE_IP TARGET` | Ultimate stealth scan using third-party host. Attacker's IP never exposed. Extremely complex technique. |
| No Ping | `nmap -Pn TARGET` | Skips host discovery, assumes target is online. Useful when ping is blocked. Faster if you know target is up. |

---

## Service & OS Detection Commands

| Command | Syntax | Description |
|---------|--------|-------------|
| Service Detection | `sudo nmap -sV TARGET` | Probes ports to identify running services and versions. Requires TCP connection. Detailed service info. |
| Light Version Detection | `sudo nmap -sV --version-light TARGET` | Version detection intensity 2. Fast but less complete service identification. Good for quick scans. |
| Complete Version Detection | `sudo nmap -sV --version-all TARGET` | Version detection intensity 9. Most thorough and accurate service version detection. Slower scanning. |
| Custom Intensity | `sudo nmap -sV --version-intensity 5 TARGET` | Sets version detection intensity (0-9). Balances between speed and accuracy. 5 is moderate. |
| OS Detection | `sudo nmap -O TARGET` | Detects operating system using fingerprinting. Requires both open and closed ports. Can be inaccurate. |
| Aggressive OS | `sudo nmap -O --osscan-guess TARGET` | Forces OS guessing when fingerprint unclear. More results but less accuracy. Uses all fingerprinting techniques. |
| OS Detection Limit | `sudo nmap -O --osscan-limit TARGET` | Skips OS detection if conditions aren't met (needs open/closed ports). Speeds up scanning. |
| Traceroute | `sudo nmap --traceroute TARGET` | Maps network hops to target. Shows router path. Starts with high TTL, decreases. Useful for network topology. |

---

## Nmap Scripting Engine (NSE) Commands

| Command | Syntax | Description |
|---------|--------|-------------|
| Default Scripts | `sudo nmap -sC TARGET` | Runs default category NSE scripts. Safe, non-intrusive. Provides service enumeration and information gathering. |
| Default Scripts Explicit | `sudo nmap --script=default TARGET` | Equivalent to -sC flag. Explicitly specifies default category scripts. Both achieve same result. |
| Specific Script | `sudo nmap --script "http-date" TARGET` | Runs single NSE script by name. Useful for targeted vulnerability checks or specific information gathering. |
| Script Pattern | `sudo nmap --script "http*" TARGET` | Runs scripts matching pattern. Wildcard matching on script names. Efficient for related scripts. |
| Script Category | `sudo nmap --script "vuln" TARGET` | Runs all scripts in specified category. Available: auth, brute, discovery, vuln, safe, default, and more. |
| Multiple Scripts | `sudo nmap --script "http-date,http-robots.txt" TARGET` | Runs multiple specific scripts. Comma-separated script names. Targeted reconnaissance. |
| Auth Scripts | `sudo nmap --script "auth" TARGET` | Runs all authentication-related NSE scripts. Useful for finding default credentials or auth mechanisms. |
| Brute Force Scripts | `sudo nmap --script "brute" TARGET` | Runs password brute-force scripts. Intrusive, requires authorization. Can lock accounts. |
| Discovery Scripts | `sudo nmap --script "discovery" TARGET` | Runs service discovery scripts. Retrieves accessible information like databases and DNS names. |
| Vulnerability Scripts | `sudo nmap --script "vuln" TARGET` | Runs vulnerability detection scripts. Checks for known CVEs and exploitable conditions. |
| Safe Scripts | `sudo nmap --script "safe" TARGET` | Runs non-destructive scripts. Won't crash services. Safe for production systems. |
| Intrusive Scripts | `sudo nmap --script "intrusive" TARGET` | Runs aggressive scripts including brute-force and exploitation. Requires authorization. Can cause disruption. |
| SSH Enumeration | `sudo nmap --script "ssh-hostkey" TARGET` | Retrieves SSH host keys. Identifies key algorithms supported. Useful for SSH fingerprinting. |
| SSH Algorithms | `sudo nmap --script "ssh2-enum-algos" TARGET` | Enumerates SSH encryption/compression algorithms. Shows server capabilities. Identifies weak algorithms. |
| HTTP Enumeration | `sudo nmap --script "http-title,http-headers" TARGET` | Retrieves HTTP title and headers. Identifies web server type and configuration. Basic web reconnaissance. |
| RPC Enumeration | `sudo nmap --script "rpcinfo" TARGET` | Enumerates RPC services. Maps RPC program numbers to ports. Useful for service discovery. |
| SMTP Enumeration | `sudo nmap --script "smtp-commands" TARGET` | Retrieves SMTP commands supported by server. Shows capabilities like TLS, VRFY, ETRN. |
| SMB Enumeration | `sudo nmap --script "smb-enum*" TARGET` | Enumerates SMB/Windows shares and users. Multiple related scripts for comprehensive SMB mapping. |
| Robots.txt Check | `sudo nmap --script "http-robots.txt" TARGET` | Checks for robots.txt file on web servers. Reveals disallowed web paths and directories. |

---

## Output & Reporting Commands

| Command | Syntax | Description |
|---------|--------|-------------|
| Normal Format | `sudo nmap -sV TARGET -oN output.nmap` | Saves scan results in normal (human-readable) format. Similar to terminal output. Good for manual review. |
| Grepable Format | `sudo nmap -sV TARGET -oG output.gnmap` | Saves scan results in grepable format. Optimized for grep command parsing. Each line complete. |
| XML Format | `sudo nmap -sV TARGET -oX output.xml` | Saves scan results in XML format. Machine-readable structure. Ideal for automation and integration. |
| All Formats | `sudo nmap -sV TARGET -oA output` | Saves in all three formats simultaneously. Creates .nmap, .gnmap, and .xml files with same base name. |
| Script Kiddie Format | `sudo nmap -sV TARGET -oS output.kiddie` | Saves in "leet speak" format with random capitalization. Not recommended for professional use. Novelty only. |
| Verbose Output | `sudo nmap -sV TARGET -v` | Provides standard verbose output. Shows detailed scan progress and findings. Better detail than default. |
| Very Verbose | `sudo nmap -sV TARGET -vv` | Very verbose output. Shows each scan phase and discovered ports as they're found. Maximum detail. |
| Debug Output | `sudo nmap -sV TARGET -d` | Provides debugging information. Shows packet exchanges and scan logic. Advanced troubleshooting. |
| Extra Debug | `sudo nmap -sV TARGET -dd` | Extra debugging information. Even more detailed than -d. Creates very lengthy output. |
| Reason Flag | `sudo nmap -sS TARGET --reason` | Shows reasoning for each conclusion. Why port is open, why host is up. Helps verify scan accuracy. |
| Grep Results | `grep "80/open" output.gnmap` | Filters grepable output for specific patterns. Extracts relevant information efficiently. Requires grepable format. |
| Grep Multiple | `grep "ssh" *.gnmap` | Searches all grepable files for pattern. Useful for analyzing results from multiple scans. Batch analysis. |

---

## Timing & Performance Commands

| Command | Syntax | Description |
|---------|--------|-------------|
| Paranoid Timing | `sudo nmap -T0 TARGET` | Slowest timing template. 5-minute wait between probes. Extremely stealthy. Only for maximum evasion. |
| Sneaky Timing | `sudo nmap -T1 TARGET` | Very slow timing. Used during real engagements requiring stealth. Avoids IDS alerts. Time-consuming. |
| Polite Timing | `sudo nmap -T2 TARGET` | Slower scan respecting network load. Considerate of target resources. Rarely used in practice. |
| Normal Timing | `sudo nmap -T3 TARGET` | Default timing when not specified. Balanced speed and network behavior. Standard choice for most scans. |
| Aggressive Timing | `sudo nmap -T4 TARGET` | Fast scanning for CTFs and practice targets. Assumes good network conditions. Commonly used in learning. |
| Insane Timing | `sudo nmap -T5 TARGET` | Fastest possible scanning. Very aggressive. Can lose accuracy due to packet loss. Use cautiously. |
| Min Packet Rate | `sudo nmap --min-rate 100 TARGET` | Minimum packets per second to send. Maintains at least this rate. Speeds up slow scans. |
| Max Packet Rate | `sudo nmap --max-rate 10 TARGET` | Maximum packets per second to send. Never exceeds this rate. Reduces network impact and detection. |
| Min Parallelism | `sudo nmap --min-parallelism 512 TARGET` | Minimum parallel probes to maintain. Ensures at least this many concurrent probes running. |
| Max Parallelism | `sudo nmap --max-parallelism 100 TARGET` | Maximum parallel probes allowed. Limits concurrent probes running. Reduces network load. |

---

## Comprehensive Scan Commands

| Command | Syntax | Description |
|---------|--------|-------------|
| Full Aggressive | `sudo nmap -sS -sV -O -sC TARGET` | Combines SYN scan, service detection, OS detection, and default scripts. Comprehensive reconnaissance. |
| With Traceroute | `sudo nmap -sS -sV -O -sC --traceroute TARGET` | Full scan plus network topology mapping. Shows path to target. Most informative general scan. |
| All Ports Full | `sudo nmap -sS -p- -sV -O -sC TARGET` | Full scan on all 65535 ports. Time-consuming but complete. Most thorough reconnaissance possible. |
| Quick Web Scan | `sudo nmap -p80,443 -sV --script=http* TARGET` | Focused web server reconnaissance. Detects HTTP/HTTPS services and web vulnerabilities. |
| Quick DNS Scan | `sudo nmap -p53 -sU -sV TARGET` | Quick DNS service detection. Combines TCP and UDP on port 53. Identifies DNS servers. |
| SMB Enumeration | `sudo nmap -p139,445 --script=smb* TARGET` | Focused Windows/SMB reconnaissance. Enumerates shares, users, and vulnerabilities. Windows-specific. |
| Database Scan | `sudo nmap -p3306,5432,1433 -sV TARGET` | Scans common database ports (MySQL, PostgreSQL, MSSQL). Detects database servers. Service specific. |
| Stealth Full Scan | `sudo nmap -sS -f -T1 --max-rate 5 TARGET` | Maximum stealth with fragmentation and slow timing. For real engagements. Takes very long time. |
| CTF Fast Scan | `sudo nmap -sS -p- -T4 TARGET` | Aggressive full port scan for competitions. Fast complete scanning. Assumes secure environment. |

---

## Specification & Control Commands

| Command | Syntax | Description |
|---------|--------|-------------|
| Target File | `nmap -iL targets.txt TARGET` | Reads targets from file. One target per line. Useful for scanning multiple hosts. Batch scanning. |
| Exclude Hosts | `nmap --exclude 192.168.1.5 RANGE` | Excludes specific IP from scanning. Useful for avoiding important systems. Can specify multiple IPs. |
| No Reverse DNS | `nmap -n TARGET` | Skips reverse-DNS lookup. Reduces network traffic and time. No hostname resolution performed. |
| Force Reverse DNS | `nmap -R TARGET` | Forces reverse-DNS lookup on all hosts. Even on offline hosts (if found). Reveals hostnames. |
| Custom DNS Server | `nmap --dns-servers 8.8.8.8 TARGET` | Uses specified DNS server for lookups. Useful if default DNS is unavailable or filtered. |
| CIDR Notation | `nmap 192.168.1.0/24 TARGET` | Scans entire subnet using CIDR notation. Scans all IPs in range. Efficient for subnet scanning. |
| Wildcard Range | `nmap 192.168.1.1-100 TARGET` | Scans IP range using wildcard notation. Scans 192.168.1.1 through 192.168.1.100. |
| Simple List | `nmap 192.168.1.1 192.168.1.5 10.0.0.1 TARGET` | Scans multiple individual targets. Space-separated targets. Useful for specific known IPs. |

---

**Note**: All commands requiring root privileges should be prefixed with `sudo`. Some features (like SYN scan, OS detection, version detection) require elevated privileges to function properly.

**Pro Tip**: Combine flags as needed. Most commands can be modified and customized. Test with `-sL -n` first to verify targets without scanning.
