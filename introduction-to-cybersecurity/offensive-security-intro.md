# Offensive Security Intro

## Task 1 - Introduction to Offensive Security
Offensive Security is a proactive approach to cybersecurity that involves "thinking like a hacker." To protect a system, one must understand how it can be broken. This discipline focuses on identifying vulnerabilities, exploiting software bugs, and finding logical loopholes before a malicious actor can.

The ultimate goal is not destruction, but improvement. By legally and ethically uncovering weaknesses, security professionals can provide the necessary data to harden defenses and protect sensitive information.

---

## Task 2 - The Attacker’s Perspective: A Practical Scenario
To understand how offensive security works, suppose there is a digital banking application. Like any modern service, this bank uses a web interface to allow users to manage their accounts, transfer funds, and view balances. 



From a security standpoint, we must assume that the application might have hidden vulnerabilities. An ethical hacker begins by mapping out the application to see what is visible to a regular user and what might be accidentally left exposed by the developers.

---

## Task 3 - Discovery and Hidden Functionality
One of the most common ways to compromise a web application is by discovering "hidden" features or administrative pages that were never intended for public view. Developers sometimes use secret URLs to perform maintenance or testing, assuming that if the link isn't on the homepage, no one will find it.

### Using Directory Brute-Forcing
To find these hidden paths, professionals use a technique called "Directory Brute-Forcing." This involves using a tool—such as `dirb`—to test thousands of common page names against a website's URL.

**The Logic Behind the Attack:**
Humans are predictable. We often name administrative pages things like `/admin`, `/test`, or `/backup`. A brute-force tool takes a "wordlist" (a text file full of these common names) and sends a request to the server for each one. If the server responds with a "Success" code (HTTP 200), the tool knows a hidden page exists.

**Example Command Execution:**
If an analyst were testing a bank's domain, they might run a command in the terminal like this:
`dirb http://bank-example.com`

**Understanding the Output:**
The tool will list any pages it finds. For instance, it might discover a directory named `/images` (which is normal) or something more sensitive like `/bank-transfer-internal` (which is a major security risk).



---

## Task 4 - Exploiting Broken Access Controls
Once a hidden page is discovered, the next step is to determine if it is properly protected. In a secure system, if a user tries to access a "secret" administrative page, the system should ask for a high-level password or block the request entirely.

### The Logic of the Exploit
Suppose the discovery tool found a hidden URL like `http://bank-example.com/internal-deposit`. If the bank has failed to implement **Broken Access Control**, a regular user could simply type that URL into their browser and gain access to an administrative interface.

**Scenario of Unauthorized Action:**
1.  The user discovers the hidden "Deposit" page.
2.  The page contains a form asking for an **Account Number** and an **Amount**.
3.  The user enters their own account number and a large sum of money.
4.  Because the server didn't verify if the user was an authorized bank employee, it processes the request and updates the database.



This demonstrates a failure in the **IAAA** framework (specifically Authorization). The system knew the user's **Identity**, but it failed to check if they were **Authorized** to use that specific function.

---

## Task 5 - Conclusion
Offensive security is the practice of finding these "unlocked doors" before they are used for criminal gain. In our bank example, a simple oversight—failing to hide and password-protect a sensitive URL—could lead to massive financial loss. By thinking like an attacker, organizations can identify these predictable naming patterns and improper access controls, ensuring that administrative functions remain strictly off-limits to unauthorized users.
