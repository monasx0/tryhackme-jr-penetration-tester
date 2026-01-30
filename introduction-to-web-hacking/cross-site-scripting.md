# Cross-site Scripting

### What is XSS?
**Cross-Site Scripting (XSS)** is a **web application vulnerability** where malicious JavaScript is injected into a website and executed in the **victim’s browser**.

XSS occurs when user-controlled input is included in a webpage without proper validation or output encoding.

---

## XSS Payloads

### What is an XSS Payload?
An **XSS payload** is the JavaScript code an attacker wants to execute in the victim’s browser.

A payload consists of:
- **Intention**: What the attacker wants to achieve (e.g., proof of concept, data theft)
- **Modification**: Adjustments made to bypass filters or fit the injection context

---

## Reflected XSS

### What is Reflected XSS?
**Reflected XSS** occurs when user input from an HTTP request is reflected directly in the response without validation.

### Common Entry Points
- URL query parameters
- URL paths
- Occasionally HTTP headers

---

## Stored XSS

### What is Stored XSS?
**Stored XSS** occurs when malicious JavaScript is permanently stored on the server (e.g., in a database) and executed when other users view the affected page.
### Common Entry Points
- Blog comments
- User profile fields
- Website listings or forms

---

## DOM-Based XSS

### What is DOM-Based XSS?
**DOM-Based XSS** occurs entirely on the client side when JavaScript processes user-controlled data and writes it to the **DOM** insecurely.

The payload is never sent to the server.

### Impact
- Session theft
- Content manipulation
- User redirection
---

## Blind XSS

### What is Blind XSS?
**Blind XSS** is similar to Stored XSS, but the attacker cannot directly see the payload execute.

Execution typically occurs in:
- Admin panels
- Staff dashboards
- Internal systems


### Detection
Blind XSS requires payloads that **call back** to an attacker-controlled server to confirm execution.

---

## XSS Filter Evasion

Applications may implement filters to block dangerous characters or keywords.

Common evasion techniques include:
- Breaking filtered keywords
- Escaping HTML attributes
- Using alternative event handlers
- Leveraging polyglot payloads that bypass multiple filters


