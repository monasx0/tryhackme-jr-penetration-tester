# Active Reconnaissance

## Introduction

**Active reconnaissance** requires direct contact with target systems. Unlike passive reconnaissance, active recon makes traceable connections logged by the target system. It must only be performed with explicit legal authorization.

---

## Web Browser

The web browser is readily available and useful for reconnaissance.

### Key Ports
* **HTTP** — Port 80
* **HTTPS** — Port 443

### Developer Tools
Press **Ctrl+Shift+I** (Windows/Linux) or **Option+Command+I** (Mac) to open Developer Tools.

**Use for:** Inspect JavaScript, view cookies, analyze network requests, discover page structure.

### Useful Extensions
* **Wappalyzer** — Identifies technologies used on websites
* **FoxyProxy** — Switch proxy servers quickly
* **User-Agent Switcher** — Change browser/OS identity

---

## Ping

**Purpose:** Check if a target system is online and reachable.

### Command
```bash
ping -c 10 MACHINE_IP          # Linux/macOS
ping -n 10 MACHINE_IP          # Windows
```

### What It Does
* Sends ICMP Echo packets
* Receives ICMP Echo Reply from online systems
* Shows response time and packet loss

### Example Output
```
5 packets transmitted, 5 received, 0% packet loss
rtt min/avg/max = 0.396/0.475/0.636 ms
```

---

## Traceroute

**Purpose:** Map the path packets take from your system to the target.

### Command
```bash
traceroute MACHINE_IP          # Linux/macOS
tracert MACHINE_IP             # Windows
```

### What It Does
* Shows all intermediate routers (hops)
* Reveals number of hops to target
* Displays response time for each hop
* Uses TTL (Time To Live) to force routers to respond

### Example Output
```
 1  router1.com (192.168.1.1)  2.5 ms
 2  isp-router.com (10.0.0.1)  15.3 ms
 3  target.com (172.67.69.208) 20.1 ms
```

---

## Telnet

**Purpose:** Connect to services and grab banners; identify service versions.

### Command
```bash
telnet MACHINE_IP PORT
```

### Example: Web Server
```bash
telnet MACHINE_IP 80
GET / HTTP/1.1
host: telnet
[Press Enter twice]
```

### What You Get
* Server type and version
* HTTP headers
* Content information

---

## Netcat (nc)

**Purpose:** Versatile tool for connecting to services and creating listeners.

### As a Client (Banner Grabbing)
```bash
nc MACHINE_IP PORT
GET / HTTP/1.1
host: netcat
[Press Shift+Enter]
```

### As a Server (Listen on Port)
```bash
nc -vnlp 1234
```

### Options
| Option | Meaning |
|--------|---------|
| `-l` | Listen mode |
| `-p` | Specify port |
| `-n` | No DNS lookup |
| `-v` | Verbose output |
| `-k` | Keep listening |

---

## Key Takeaways

**Active reconnaissance is traceable** — Every ping and connection is logged. Blend reconnaissance activities with legitimate user behavior to avoid detection.

**Tools are powerful** — Simple, built-in tools like ping, traceroute, telnet, and nc provide critical reconnaissance information without sophisticated tooling.


