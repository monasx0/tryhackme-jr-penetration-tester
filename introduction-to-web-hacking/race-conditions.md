# Race Conditions
# Introduction to Race Conditions
A **Race Condition** occurs when the **timing or sequence of events** influences the behavior of a program. This happens when a **shared variable** is accessed and modified by **multiple threads simultaneously**.

Without proper locking or synchronization, an attacker can exploit this to perform actions multiple times, such as:

- Reusing a **single gift card**
- Applying the **same discount repeatedly**
- Exceeding an allowed **bank balance**

---

## TOCTOU (Time-of-Check to Time-of-Use)

This is the **critical window** where a race condition exploit occurs.

- **Check**  
  The system verifies a condition (e.g., sufficient balance or valid coupon).

- **Use**  
  The system performs the action (e.g., deducting money or applying the coupon).

If **two or more requests** are sent at nearly the same time, they may **all pass the Check phase** before any reaches the Use phase—resulting in unintended behavior.

---

## Exploitation with Burp Suite

Exploiting race conditions requires **precise timing**, ensuring multiple requests reach the server within **milliseconds** of each other.

---

### Step-by-Step Methodology

1. **Capture Request**  
   Intercept the target `POST` request (e.g., money transfer or discount application).

2. **Send to Repeater**  
   Right-click the request → **Send to Repeater**.

3. **Create Tab Group**
   - Click the `+` icon next to the request tabs  
   - Select **Create tab group**
   - Add the target request and name the group

4. **Duplicate Requests**  
   Duplicate the request **multiple times** (e.g., 20 tabs) within the group.

5. **Configure Send Settings**
   - Click the arrow next to **Send**
   - Select **Send group in parallel**

---

## Synchronization Techniques

- **HTTP/1 – Last-byte synchronization**  
  Burp withholds the last byte of each request and releases them simultaneously to ensure parallel processing.

- **HTTP/2 – Single-packet attack**  
  Burp attempts to send all requests in a **single TCP packet**, minimizing network jitter and maximizing simultaneity.

---

## Detection and Mitigation

### Detection

- **Logic Review**  
  Identify functionality with limits, such as:
  - “Use once”
  - “One vote per user”
  - “Balance-based restrictions”

- **Testing**  
  Use **Burp Repeater** to send high-frequency **parallel requests** and observe if limits are bypassed.

---

### Mitigation

- **Synchronization Mechanisms**  
  Use **locks (mutexes)** so only one thread accesses a resource at a time.

- **Atomic Operations**  
  Ensure critical actions execute as a **single, indivisible operation**.

- **Database Transactions**  
  Group operations so they either **all succeed or all fail**, maintaining data consistency.

---
