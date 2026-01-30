# Intro to SSRF
## Server-Side Request Forgery (SSRF)

### Definition
**Server-Side Request Forgery (SSRF)** is a **web application vulnerability** that allows an attacker to induce a server to send HTTP requests to unintended internal or external resources.

### Types
- **Regular SSRF**: Response is returned to the attacker.
- **Blind SSRF**: No response is returned, but the request is executed.

### Impact
- Access to **internal services**
- Exposure of **sensitive data**
- Lateral movement within **internal networks**
- Leakage of **authentication tokens or credentials**

### Common SSRF Entry Points
- Full URLs in parameters
- Hidden form fields
- Hostname or path-based parameters

### SSRF Defense Bypasses
- **Deny Lists**: Bypassed using alternative IP formats or DNS tricks
- **Allow Lists**: Bypassed using attacker-controlled subdomains
- **Open Redirects**: Used to redirect internal requests to attacker-controlled URLs
