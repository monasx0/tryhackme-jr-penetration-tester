# Offensive Security Intro

## Task 1 - Introduction to Offensive Security
**Offensive Security** is a proactive approach to cybersecurity. It involves **thinking like a hacker** to keep systems safe. To protect a system, you must first understand exactly how it can be broken. This discipline focuses on finding **vulnerabilities**, exploiting software bugs, and finding logical loopholes before a criminal does.

The ultimate goal is not to destroy things, but to improve them. By finding weaknesses legally and ethically, security experts can provide the data needed to **harden defenses** and protect sensitive information.

---

## Task 2 - The Attacker’s Perspective: A Practical Scenario
To understand how offensive security works, imagine a digital banking application. This bank uses a web interface so users can manage accounts, transfer money, and view balances.

From a security view, we must assume the application has hidden vulnerabilities. An **ethical hacker** begins by mapping out the application. They look at what a regular user can see and search for things that developers might have accidentally left exposed.



---

## Task 3 - Discovery and Hidden Functionality
One common way to break a web app is by finding **hidden features** or **admin pages** that were never meant for the public. Developers sometimes use secret URLs for maintenance or testing. They assume that if there is no link on the homepage, no one will find it.

### Using Directory Brute-Forcing
To find these hidden paths, professionals use a technique called **Directory Brute-Forcing**. This uses a tool like `dirb` to test thousands of common page names against a website's URL.

**The Logic Behind the Attack:**
Humans are predictable. We often name admin pages things like `/admin`, `/test`, or `/backup`. A brute-force tool takes a **wordlist** (a text file full of these common names) and sends a request to the server for each one. If the server responds with a **Success code (HTTP 200)**, the tool knows a hidden page exists.

**Example Command Execution:**
If an analyst were testing a bank's domain, they might run a command like: `dirb http://bank-example.com`

**Understanding the Output:**
The tool lists every page it finds. It might find a normal folder like `/images`, or it might find a high-risk directory like `/bank-transfer-internal`.



---

## Task 4 - Exploiting Broken Access Controls
After finding a hidden page, the next step is to see if it is protected. A secure system should ask for a high-level password or block the request if a regular user tries to open a "secret" admin page.

### The Logic of the Exploit
Suppose the tool finds a hidden URL like `http://bank-example.com/internal-deposit`. If the bank has **Broken Access Control**, a regular user could simply type that URL into their browser and get inside the admin interface.

**Scenario of Unauthorized Action:**
1. The user discovers the hidden **Deposit** page.
2. The page has a form asking for an **Account Number** and an **Amount**.
3. The user enters their own account number and a large sum of money.
4. Because the server did not verify if the user was an authorized employee, it processes the request and updates the database.

This shows a failure in the **IAAA framework**, specifically **Authorization**. The system knew the user’s **Identity**, but it failed to check if they were **Authorized** to use that specific function.



---

## Task 5 - Conclusion
**Offensive security** is the practice of finding these **unlocked doors** before they are used for crime. In the bank example, a simple mistake—like failing to hide or password-protect a sensitive URL—could lead to massive financial loss. By thinking like an attacker, organizations can find these predictable naming patterns and fix improper access controls to keep unauthorized users out.
