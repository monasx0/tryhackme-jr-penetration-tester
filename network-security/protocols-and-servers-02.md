# Protocols and Servers 02

## Introduction to Security Concepts

### The CIA Triad

Security is built on three core principles called the **CIA Triad**:

| Principle | What It Protects |
|-----------|------------------|
| **Confidentiality** | Keeps information private and accessible only to intended parties |
| **Integrity** | Ensures data remains accurate and unmodified during transmission |
| **Availability** | Guarantees services are accessible when needed |

Different organizations prioritize these differently:
- **Intelligence agencies** focus on **confidentiality**
- **Banks** prioritize **integrity** of transactions
- **Ad platforms** need high **availability**

### The DAD Attack Model

Attackers aim to break the **CIA triad** through **DAD attacks**:

| Attack Type | What It Does | CIA Component Broken |
|-------------|--------------|---------------------|
| **Disclosure** | Reveals confidential information | Confidentiality |
| **Alteration** | Modifies data during transmission | Integrity |
| **Destruction** | Makes services unavailable | Availability |

### Common Network Attacks

**Packet capture** - Violates **confidentiality** by intercepting network traffic

**Password attacks** - Lead to **disclosure** of credentials

**Man-in-the-Middle (MITM)** - Breaks **integrity** by altering communications

This room focuses on protecting **confidentiality** and **integrity** through **protocol upgrades** and **encryption**. We'll also learn to use **Hydra** for testing password security.

---

## Sniffing Attack

### What is a Sniffing Attack?

A **sniffing attack** captures network traffic to steal information. When **protocols** send data in **cleartext** (unencrypted), attackers can:
- Read private messages
- Steal **login credentials**
- View sensitive data

### Tools for Packet Capture

| Tool | Type | Platform |
|------|------|----------|
| **Tcpdump** | Command-line | Linux, macOS, Windows |
| **Wireshark** | Graphical interface | Linux, macOS, Windows |
| **Tshark** | Command-line | Linux, macOS, Windows |

### Requirements for Sniffing

To capture network traffic, attackers need:
- Access to the network (physical or remote)
- **Root/Administrator privileges**
- Network card in promiscuous mode
- Or access via **MITM attack** or network tap

### Example: Capturing POP3 Credentials with Tcpdump

**Command used:**
```bash
sudo tcpdump port 110 -A
```

**Command breakdown:**
- `sudo` - Requires root privileges
- `port 110` - Filter only **POP3** traffic (port 110)
- `-A` - Display packet contents in **ASCII** format

**Captured output shows:**
```
USER frank
PASS D2xc9CgD
```

The attacker can clearly see:
- **Username**: `frank`
- **Password**: `D2xc9CgD`

### Using Wireshark

**Wireshark** provides a graphical interface for packet capture:

1. Start capture on network interface
2. Filter traffic by typing `pop` in the filter field
3. View captured **username** and **password** in plain text
4. Export or analyze captured data

### Vulnerable Protocols

All **cleartext protocols** are vulnerable to sniffing:
- **Telnet**
- **HTTP**
- **FTP**
- **SMTP**
- **POP3**
- **IMAP**

### Protection Against Sniffing

The solution is **encryption**. Modern secure versions:

| Original Protocol | Secure Version | What Changed |
|-------------------|----------------|--------------|
| **HTTP** | **HTTPS** | Added **TLS encryption** |
| **FTP** | **FTPS/SFTP** | Added **SSL/TLS** or **SSH** |
| **Telnet** | **SSH** | Complete secure replacement |
| **SMTP** | **SMTPS** | Added **TLS encryption** |
| **POP3** | **POP3S** | Added **TLS encryption** |
| **IMAP** | **IMAPS** | Added **TLS encryption** |


---

## Man-in-the-Middle (MITM) Attack

### What is MITM?

A **Man-in-the-Middle (MITM) attack** happens when an attacker secretly intercepts and potentially alters communication between two parties.

**How it works:**
1. Victim A wants to communicate with B
2. Attacker E positions themselves between A and B
3. A thinks they're talking to B (but it's actually E)
4. B thinks they're talking to A (but it's actually E)
5. E can read, modify, or block all messages


### Why MITM Works

**MITM attacks** succeed because:
- **Protocols** don't verify sender/receiver identity
- No **integrity checking** on messages
- Communication happens in **cleartext**
- Users can't detect the attack is happening

### Vulnerable Protocols

Any **cleartext protocol** is vulnerable:
- **HTTP** - Web browsing
- **FTP** - File transfers
- **SMTP** - Sending email
- **POP3** - Receiving email
- **Telnet** - Remote access

### MITM Attack Tools

Common tools used for **MITM attacks**:
- **Ettercap** - Network sniffer and interceptor
- **Bettercap** - Modern MITM framework
- **mitmproxy** - Interactive HTTPS proxy

### Protection Against MITM

**Solutions require cryptography:**

1. **Authentication** - Verify who you're communicating with
2. **Encryption** - Protect message contents
3. **Digital signatures** - Verify message integrity
4. **PKI (Public Key Infrastructure)** - Trust certificates
5. **TLS (Transport Layer Security)** - Encrypts and authenticates

**Key takeaway**: **TLS/SSL** with proper **certificate verification** protects against **MITM attacks**.

---

## Task 4: Transport Layer Security (TLS)

### What is TLS?

**TLS (Transport Layer Security)** is the modern standard for encrypting internet communications. It evolved from **SSL (Secure Sockets Layer)**.

**History:**
- **1994** - Netscape introduces **SSL**
- **1996** - **SSL 3.0** released
- **1999** - **TLS 1.0** replaces **SSL**
- **Today** - **TLS** is the standard (SSL is outdated)

### How TLS Works

**TLS** adds an **encryption layer** to existing **protocols**. It sits in the **presentation layer** of the network model, encrypting data before transmission.

**Without TLS**: Data sent as readable text

**With TLS**: Data sent as encrypted ciphertext

### Protocol Upgrades with TLS

| Protocol | Default Port | Secure Version | Secure Port |
|----------|--------------|----------------|-------------|
| **HTTP** | 80 | **HTTPS** | 443 |
| **FTP** | 21 | **FTPS** | 990 |
| **SMTP** | 25 | **SMTPS** | 465 |
| **POP3** | 110 | **POP3S** | 995 |
| **IMAP** | 143 | **IMAPS** | 993 |


### TLS Handshake Process

The **TLS handshake** establishes a secure connection:

**Step 1: ClientHello**
- Client tells server its capabilities
- Lists supported **encryption algorithms**

**Step 2: ServerHello**
- Server selects connection parameters
- Sends its **digital certificate** (proves identity)
- Sends information to generate **encryption key**

**Step 3: ClientKeyExchange**
- Client sends information for **key generation**
- Switches to **encrypted communication**
- Sends **ChangeCipherSpec** message

**Step 4: Server confirms**
- Server switches to **encrypted communication**
- Sends **ChangeCipherSpec** message
- **Secure connection established!**

### How TLS Protects You

After the **TLS handshake**:
- All communication is **encrypted**
- Only client and server can read messages
- Attackers see only scrambled data
- **MITM attacks** become ineffective

### Certificate Verification

**TLS** relies on **trusted certificates**:

**Certificate contains:**
- Who the certificate is issued to (company name)
- Who issued the certificate (**Certificate Authority**)
- Validity period (start and end dates)
- **Public key** for encryption

**Your browser automatically checks:**
- Is the **certificate** valid?
- Is it issued by a **trusted authority**?
- Is the **domain name** correct?
- Has it expired?

**If checks fail**: Browser shows a warning

**If checks pass**: Secure connection established (padlock icon)

---

## Secure Shell (SSH)

### What is SSH?

**SSH (Secure Shell)** is a **protocol** for secure remote system administration. It replaced **Telnet** as the secure way to access computers remotely.

### Why SSH is Secure

**SSH** provides three key protections:

1. **Identity verification** - Confirms you're connecting to the right server
2. **Encryption** - All messages are scrambled during transmission
3. **Integrity checking** - Detects any tampering with messages

### SSH Connection Methods

**SSH** supports two authentication methods:

| Method | What You Need |
|--------|---------------|
| **Username & Password** | Just your login credentials |
| **Public/Private Keys** | Key pair (more secure) |

### Using SSH

**Basic SSH command:**
```bash
ssh username@MACHINE_IP
```

**What happens:**
1. Client connects to **SSH server** (port 22)
2. Server requests authentication
3. You enter **password**
4. Access granted to remote **terminal**

### SSH Connection Example
```bash
user@TryHackMe$ ssh mark@MACHINE_IP
mark@MACHINE_IP's password: XBtc49AB

Last login: Mon Sep 20 13:53:17 2021
mark@debian8:~$
```

### Using SCP (Secure Copy)

**SCP** transfers files securely over **SSH**.

**Download from remote server:**
```bash
scp mark@MACHINE_IP:/home/mark/archive.tar.gz ~
```

**Command breakdown:**
- `mark@MACHINE_IP` - Remote server and username
- `:/home/mark/archive.tar.gz` - Remote file path
- `~` - Copy to your home directory

**Upload to remote server:**
```bash
scp backup.tar.bz2 mark@MACHINE_IP:/home/mark/
```

---

## Password Attack

### Why Passwords Matter

**Authentication** means proving your identity. Most **protocols** require authentication:
- **POP3** - Access email
- **SSH** - Remote login
- **FTP** - File transfer
- **IMAP** - Email management

### Authentication Methods

You can prove identity using:

| Method | Example |
|--------|---------|
| **Something you know** | Password, PIN code |
| **Something you have** | SIM card, USB token |
| **Something you are** | Fingerprint, iris scan |

### Common Weak Passwords

Top 10 passwords from 150 million leaked accounts (Adobe breach 2013):

1. `123456`
2. `123456789`
3. `password`
4. `adobe123`
5. `12345678`
6. `qwerty`
7. `1234567`
8. `111111`
9. `photoshop`
10. `123123`

**Unfortunately, these are still commonly used today!**

### Types of Password Attacks

| Attack Type | How It Works |
|-------------|--------------|
| **Password Guessing** | Using personal information (pet names, birth years) |
| **Dictionary Attack** | Trying all words from a wordlist |
| **Brute Force Attack** | Trying every possible character combination |

### Dictionary Attacks

**Dictionary attacks** use lists of common passwords:

**Popular wordlists:**
- **RockYou** - Contains leaked passwords from data breaches
- **Located at**: `/usr/share/wordlists/rockyou.txt` on AttackBox
- Choose wordlist based on target (e.g., French wordlist for French users)

### Using Hydra for Password Attacks

**Hydra** is a powerful tool for automated password attacks.

**Basic syntax:**
```bash
hydra -l username -P wordlist.txt server service
```

**Options explained:**

| Option | What It Does |
|--------|--------------|
| `-l username` | Specify the **username** to attack |
| `-P wordlist.txt` | Specify the **password list** file |
| `server` | Target server IP or hostname |
| `service` | Service to attack (ftp, ssh, pop3, etc.) |

### Hydra Examples

**Attack FTP server:**
```bash
hydra -l mark -P /usr/share/wordlists/rockyou.txt MACHINE_IP ftp
```

**Alternative FTP syntax:**
```bash
hydra -l mark -P /usr/share/wordlists/rockyou.txt ftp://MACHINE_IP
```

**Attack SSH server:**
```bash
hydra -l frank -P /usr/share/wordlists/rockyou.txt MACHINE_IP ssh
```

### Optional Hydra Arguments

| Option | What It Does |
|--------|--------------|
| `-s PORT` | Specify non-default port number |
| `-V` or `-vV` | Verbose mode (shows attempts) |
| `-t n` | Number of parallel connections (e.g., `-t 16`) |
| `-d` | Debug mode (detailed output) |

**Example with options:**
```bash
hydra -l admin -P passwords.txt MACHINE_IP ssh -t 16 -V
```

**Stop attack**: Press `CTRL-C` when password is found

### Defending Against Password Attacks

| Defense Method | How It Works |
|----------------|--------------|
| **Password Policy** | Enforce minimum complexity requirements |
| **Account Lockout** | Lock account after failed attempts |
| **Throttling** | Delay response to slow down attacks |
| **CAPTCHA** | Require solving human-verification puzzles |
| **Certificate Authentication** | Use certificates instead of passwords (SSH) |
| **Two-Factor Authentication (2FA)** | Require code from phone/email |

**Best practice**: Use multiple defense methods together for maximum security!

---
