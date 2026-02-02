# Passive Reconnaissance
## Introduction

This write up introduces the passive reconnaissance phase of network security. Passive recon focuses on collecting information about a target using publicly available sources, without directly interacting with the target systems.

Tools covered in this room include **whois**, **nslookup**, and **dig**, which are used to query public WHOIS and DNS records. Since these records are public, their usage does not alert the target.

Online services such as **DNSDumpster** and **Shodan.io** are also introduced to gather intelligence without direct connection.


---

## Passive vs Active Reconnaissance

Reconnaissance is the first phase of the **Unified Kill Chain**, where information is gathered about a target before exploitation.

### Passive Reconnaissance

**Passive reconnaissance** relies on publicly available information and does not involve direct interaction with the target. It is stealthy and low risk.

Examples include:
* Querying DNS records from public DNS servers
* Reviewing job postings
* Reading public news or documentation

### Active Reconnaissance

**Active reconnaissance** involves direct engagement with the target and is more invasive. It increases the chance of detection and legal consequences.

Examples include:
* Connecting to web, FTP, or mail servers
* Social engineering phone calls
* Physical access attempts

**Important:** Active reconnaissance should only be performed with explicit legal authorization.

---

## WHOIS

**WHOIS** is a request–response protocol defined in **RFC 3912** and operates on **TCP port 43**. It is used to retrieve domain registration information maintained by registrars.

### WHOIS Query Information

WHOIS queries can reveal:
* Registrar details
* Registrant contact information (if not private)
* Domain creation, update, and expiration dates
* Authoritative name servers

### Command Usage

```
whois DOMAIN_NAME
```

This information can help identify potential attack surfaces such as admin email addresses, DNS infrastructure, or ownership patterns. Many domains use privacy services, which may hide sensitive details.

---

## nslookup and dig

After identifying name servers via WHOIS, DNS records can be queried using **nslookup** or **dig**.

### nslookup

**nslookup** is commonly used for basic DNS lookups such as IPv4, IPv6, and mail servers.

Common record types include:
* **A** (IPv4)
* **AAAA** (IPv6)
* **MX** (Mail servers)
* **TXT** (Text records)
* **CNAME** (Aliases)
* **SOA** (Authority information)

#### Example Usage

```
nslookup -type=A tryhackme.com 1.1.1.1
nslookup -type=MX tryhackme.com
```

### dig

**dig** provides more detailed DNS output such as TTL values and flags and is preferred for advanced analysis.

#### Example Usage

```
dig tryhackme.com MX
dig @1.1.1.1 tryhackme.com MX
```

These queries help identify IP addresses and mail infrastructure, which can later be assessed if within scope.

---

## DNSDumpster

**DNSDumpster** is an online DNS intelligence platform used to discover subdomains and DNS infrastructure that standard DNS queries cannot enumerate.

### DNSDumpster Capabilities

It can reveal:
* Hidden or forgotten subdomains
* DNS, MX, and TXT records
* IP addresses, hosting providers, and geolocation
* Visual DNS relationship graphs

This is useful for identifying outdated or weakly maintained subdomains that may expose vulnerabilities.

---

## Shodan.io

**Shodan** is a search engine for internet-connected devices, not web pages. It continuously scans the internet and indexes exposed services.

### Shodan Search Information

Shodan searches can reveal:
* IP addresses
* Hosting providers
* Geographic locations
* Service types and versions

It is useful for asset discovery during passive reconnaissance and can be used by both attackers and defenders. Basic information can be accessed without a premium account.

---

## Pentester Reflections

Completing this passive reconnaissance room reinforces a fundamental truth in cybersecurity: **information is power**, and much of that information is freely available to anyone willing to look. The beauty of passive reconnaissance lies in its stealth and effectiveness—an attacker can gather substantial intelligence about a target without triggering a single alarm.



**Interesting Observation:** Subdomains discovered through DNSDumpster or services identified through Shodan often represent the "edge cases" of network administration—systems that were set up years ago, forgotten, and left running without updates or proper monitoring. These outdated systems frequently become the easiest entry points during a penetration test, highlighting the importance of comprehensive asset inventory and lifecycle management.

The passive reconnaissance phase is truly the "quiet reconnaissance" where patience, systematic enumeration, and knowledge of the right tools can reveal an organization's entire attack surface before any active testing even begins.
