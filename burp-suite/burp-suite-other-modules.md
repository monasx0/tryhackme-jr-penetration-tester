# Burp SUite Other Modules

This write up cover lesser-used but **highly practical Burp Suite modules** that help improve efficiency during real-world web application penetration tests. While often overlooked, these tools save time, reduce mistakes, and improve analysis quality.

---

## Modules Covered
- **Decoder**
- **Comparer**
- **Sequencer**
- **Organizer**

---

## Decoder

### Overview
**Decoder** is used to **encode, decode, and hash data** directly inside Burp Suite. It is commonly used when dealing with encoded parameters, cookies, tokens, or payload preparation.

It also includes **Smart Decode**, which recursively attempts to decode unknown encodings.

**Access:** `Top Menu → Decoder`  
Data can be sent from anywhere in Burp via **Right-click → Send to Decoder**.

---

### Core Capabilities
- Encode and decode intercepted data
- Generate hashes (MD5, SHA-1, SHA-256, etc.)
- Switch between **Text View** and **Hex View**
- Stack multiple transformations

---

### Common Encoding / Decoding Types

**URL Encoding**
- Converts characters to `%HH` format  
- Used for safe transmission in URLs and parameters  
- Example: `/` → `%2F`

**Base64**
- Converts data into ASCII-safe format
- Common in tokens, cookies, and API traffic

**ASCII Hex**
- Converts characters to hexadecimal values  
- Example: `ASCII` → `4153434949`

**Gzip**
- Compresses data  
- Often used in HTTP responses

> Encoding methods can be **stacked** (e.g., Text → ASCII Hex → Octal).

---

## Decoder – Hashing

### Overview
Hashing is a **one-way process** used to:
- Verify file integrity
- Securely store passwords
- Compare values without revealing plaintext

Even small changes in input result in **completely different hashes**.

> **Note:** MD5 is **deprecated** and should not be relied upon for security.

---

### Hashing in Decoder
- Supports many hashing algorithms (MD5, SHA-1, SHA-256, etc.)
- Hash output is binary → typically converted to **hexadecimal**
- Useful for:
  - Hash comparison
  - Password analysis
  - Understanding authentication logic

---

## Comparer

### Overview
**Comparer** allows side-by-side comparison of:
- HTTP requests
- HTTP responses
- Any arbitrary data

Comparison can be done by:
- **Words**
- **Bytes**

---

### Practical Use Cases
- Identify subtle response differences
- Detect authorization bypasses
- Compare valid vs invalid requests
- Spot hidden logic changes

---

## Sequencer

### Overview
**Sequencer** analyzes the **randomness** (**entropy**) of tokens used by applications.

Common token types:
- Session cookies
- CSRF tokens
- Password reset tokens

Poor randomness can allow **token prediction**, leading to account takeover.

---

### Capture Methods

**Live Capture**
- Sends the same request repeatedly
- Collects thousands of tokens automatically
- Best for real-time analysis

**Manual Load**
- Analyzes pre-collected tokens
- Less noisy but requires existing data

---

## Sequencer – Live Capture Workflow
1. Capture a token-generating request in Proxy
2. `Right-click → Send to Sequencer`
3. Select token location:
   - Cookie
   - Form field
   - Custom
4. Start live capture
5. Pause → Analyze

---

## Organizer

### Overview
**Organizer** is used to **store, annotate, and manage important HTTP requests** during a pentest.

It helps maintain structure and improves reporting efficiency.

---

### Why It Matters
- Keeps track of interesting findings
- Stores requests for later investigation
- Helps during report writing
- Prevents losing important evidence
---

### Viewing Requests
- Click any item to view request and response
- Supports searching within requests
- Ideal for documentation and reporting

---

## Final Notes
These Burp Suite modules are **force multipliers** during real engagements. While not exploit tools by themselves, they significantly improve **analysis speed, accuracy, and workflow quality** — traits highly valued in professional penetration testing roles.
