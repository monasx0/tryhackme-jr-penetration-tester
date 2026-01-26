# Defensive Security Intro

## Task 1 - Introduction to Defensive Security
**Defensive security**, often called **Blue teaming**, is the practice of shielding an organization from cyber threats. When defenses are weak, companies face massive financial losses and data leaks.

**Real-World Consequences:**
* **British Airways (2018):** Payment data for **400,000 customers** was stolen. The fine was **£20 million**.
* **Marriott International (2020):** **334 million guest records** were exposed. This led to a **$52 million lawsuit**.
* **SK Telecom (2024):** Due to poor server settings, data for **27 million users** leaked. The fine was **$97 million**.

---

## Task 2 - Responsibilities and the Team
Defensive teams don't just wait for attacks, they are proactive. Their main goals are:
* **Monitoring:** Keeping an eye on systems to catch weird behavior, like a login from a new country.
* **Incident Response:** Acting fast to remove a threat once it is found.
* **Vulnerability Management:** Finding and fixing bugs before hackers can use them.

**The Professional Roles:**
1. **SOC Analyst:** The first responder who watches the security screens.
2. **Incident Responder:** The person who fights active threats.
3. **Security Engineer:** The one who builds and maintains tools like the **SIEM**.
4. **Digital Forensics:** The detective who finds out how an attack happened by looking at evidence.

---

## Task 3 - Defensive Security in Practice
To be safe, organizations use **Defence in Depth**. This means having many layers of security, like a castle with a moat, walls, and guards.



**Standard Defensive Tools:**
* **Firewalls:** They act as security guards to block bad internet traffic.
* **IDS (Intrusion Detection System):** These are like surveillance cameras for the network.
* **SIEM (Security Information and Event Management):** A central dashboard that acts like a digital radar, showing everything happening in the organization.
* **Security Policies:** Rules that require **strong passwords** and block risky websites.

---

## Task 4 - Practical Scenario - Defending FakeBank
In this exercise, we joined the team at **FakeBank** to stop a **Web Discovery Attack**. An attacker was trying to guess hidden page names to find sensitive data.

**The Response Steps:**
1. **Analyze Logs:** We looked for a high number of `404 Not Found` errors in the **SIEM**.
2. **Identify Attacker:** We found the malicious source at IP address `10.10.201.241`.
3. **Block the Attack:** We used the firewall command `iptables -A INPUT -s 10.10.201.241 -j DROP` to stop them immediately.
4. **Hardening:** We set up a rate-limit using `limit_req_zone $binary_remote_addr zone=mylimit:10m rate=5r/s;` to slow down automated guessing tools.

---

## Task 5 - Conclusion
Defensive security is a 24/7 job. By using **layers of defense** and the right **monitoring tools**, organizations can stop hackers before they cause real damage.
