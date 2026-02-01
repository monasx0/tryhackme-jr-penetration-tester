# Burp Suite Repeater

## 1. Repeater Overview

**Repeater** is a manual request manipulation and replay tool in Burp Suite.

**Purpose:**
- Edit, resend, and analyze **HTTP/HTTPS** requests
- Perform deep **manual testing** of endpoints

**Requests can be:**
- Sent directly from **Proxy**
- Created **manually** (similar to `curl`)

---

## 2. Why Repeater Matters in Real Pentesting

Repeater provides **fine-grained control** over every part of an HTTP request.

**Ideal for:**
- SQL Injection (SQLi) payload testing
- Authentication bypass validation
- Business logic flaws
- Header manipulation
- HTTP method tampering

---

## 3. Repeater Interface Components

### Request List
- Located in the **top-left panel**
- Stores multiple Repeater requests
- Enables quick switching between test cases

### Request Controls
- **Send** request
- **Cancel** hanging requests
- Navigate request history (**back / forward**)

### Request & Response View
- Main workspace
- **Left:** Editable Request
- **Right:** Server Response
- Sending a request updates the response instantly

### Target
- Destination IP or domain
- Auto-filled when sent from Proxy
- Can be manually changed to retarget requests

### Inspector
- Structured, high-level editor
- Safer and faster than raw editing
- Keeps request syntax intact

---

## 4. Basic Workflow

1. Capture request in **Proxy**
2. Right-click → **Send to Repeater**
   - Shortcut: `Ctrl + R`
3. Modify parameters, headers, or body
4. Click **Send**
5. Observe response behavior

---

## 5. Request Modification & History

- Any request component can be edited
- Each change creates a new **history state**
- History navigation allows:
  - Comparing responses
  - Rolling back changes

**Use case:** Controlled payload evolution.

---

## 6. Message Analysis Toolbar (Response Views)

### Pretty
- Default view
- Slight formatting for readability
- Best for most testing scenarios

### Raw
- Exact server response
- No formatting
- Useful for protocol-level analysis

### Hex
- Byte-level representation
- Used for:
  - Binary data
  - File responses
  - Encoding issues

### Render
- Renders response like a browser
- Rarely used in Repeater
- Helpful for quick visual validation

### Show Non-Printable Characters
- Displays characters like `\r \n`
- Useful for:
  - Debugging malformed headers
  - Analyzing protocol behavior

---

## 7. Inspector

### Purpose
- Structured view of request/response components
- Enables safe editing without breaking syntax
- Shows how high-level changes affect raw requests

### Editable Request Sections

#### Request Attributes
- URL / path
- HTTP method
- Protocol version (HTTP/1 ↔ HTTP/2)

#### Query Parameters
- URL parameters (`?key=value`)

#### Body Parameters
- POST data

#### Cookies
- Session and tracking cookies

#### Request Headers
- Add, modify, or remove headers

### Read-Only Section
- Response headers
- Server-controlled
- Visible after sending request

### Pentesting Value
- Faster payload experimentation
- Cleaner request crafting
- Reduced syntax errors
- Ideal for header-based attacks

---

## 10. Key Takeaways

- Repeater is the **core manual exploitation tool** in Burp Suite
- Essential for:
  - Real-world pentesting
  - Security audits
  - Bug bounty hunting
