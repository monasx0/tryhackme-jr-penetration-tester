# Subdomain Enumeration
Subdomain enumeration is the process of finding subdomains from the domain. Subdomain enumeration is necessary because it expand our attack surface and provide more potential points of vulnerability.
We will try to explore three different methods of subdomain enumeeration.
1. OSINT (Open-Source-Intelligence)
2. Brute Force
3. Virtual Hosts

## OSINT
### SSL/TLS Certificates
When an SSL/TLS certificate is created for a domain. CA (Certificate Authorities) publishes certificates they issue called Certificate Transparency (CT) logs.
 The purpose of these logs is to stop malicious and accidentally made certificates from being used. These logs are publicly accessible and we can use it to our advantage. 
 Below is the websites that offers a database of certificates that shows current and historical results.
 [crt.sh](https://crt.sh/)
### Search Engines
Search engines contains trillions of links to more than one billion websites which can be excellent for subdomain enumeration. we can use Google search engine with filters to look for subdomains. For Example, `site:*.google.com -site:www.google.com` first parameter show the website that contains `google.com` in its domain and the second parameter excludes the results from `www.google.com` resulting in to showing subdomains.
### OSINT Sublist3r
To speed up the process of OSINT subdomain discovery, we can automate the above methods using tools like Sublist3r. Sublist3r is an open‑source OSINT tool designed to enumerate subdomains of a target domain by querying multiple public sources such as search engines, DNS records, and certificate transparency logs. It helps security researchers and penetration testers quickly identify subdomains without actively interacting with the target system.
## DNS Brute Force
DNS brute force is a method of trying millions of possible subdomain names from a predefined list of commonly used subdomains. This method requires many requests, so we can automate the process using a tool to make it quicker. A very popular brute‑force tool is `dnsrecon`.
## Virtual Hosts
Some subdomains do not appear in public DNS records, such as development sites or admin panels. These may exist on a private DNS server or be stored locally in the hosts file (`/etc/hosts` on Linux or `C:\Windows\System32\drivers\etc\hosts` on Windows), which maps domain names to IP addresses.
A single web server can host multiple websites. The server determines which website the client is requesting by checking the **Host** header. By modifying this header and analyzing the server’s response, hidden virtual hosts can be discovered.
Virtual host enumeration is similar to DNS brute forcing. It uses a wordlist of common subdomain names and automates testing by changing the Host header.
#### ffuf
**Basic command:**
```
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.example.com" -u http:10.10.10.10
```
Here, `-w` specifies the wordlist, `-H` modifies the Host header, `FUZZ` is replaced with subdomain names, and `-u` defines the target URL.
**Filtered command:**
```
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.example.com" -u http:10.10.10.10 -fs {size}
```
The `-fs` option filters out responses with the most common size.
