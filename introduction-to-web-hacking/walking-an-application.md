# Walking An Application

## Task 1 - Introduction to Browser Tools
You can find security issues in web applications by using the tools already built into your browser. Automated tools often miss things that a human can find when reviewing an application manually.

* **View Source:** Used to see the raw code of the website.
* **Inspector:** Used to view and change page elements in real-time.
* **Debugger:** Used to control and pause the website's JavaScript.
* **Network Tab:** This tool acts like a tracker. It shows every request the webpage makes to the server, helping you understand exactly what the page is doing when it "talks" to the backend.

---

## Task 2 - Looking At The Website
A security tester starts by exploring the website to find interactive parts, such as login forms or contact pages. You should map out the site by writing down the URLs and a short description of what each page does. This helps you understand the **attack surface**—the parts of the target that are open to attack.



---

## Task 3 - Looking At The Page Source
The page source is the code sent by the server. By right-clicking and selecting **View Page Source**, you can find valuable information hidden from the average user:

* **Comments:** Notes left by developers, often marked with ``. These may contain reminders or links to hidden test pages.
* **Hidden Links:** Links present in the code that are not visible on the actual webpage.
* **Directory Listing:** Sometimes, looking at file paths reveals a **Directory Listing** error. This allows you to see every file on the server, including sensitive configuration or backup files.
* **Frameworks:** Comments may reveal the name and version of the software used to build the site. If the version is old, it might have **known vulnerabilities** that hackers can exploit.

---

## Task 4 - Developer Tools – Inspector
The **Inspector** shows you what the website looks like at this exact moment. Because JavaScript can change the page after it loads, the Inspector is more accurate than the Page Source for seeing the live state of the site.

* **Bypassing Paywalls:** You can use the Inspector to find elements that block content, such as "Premium Only" boxes. By changing the CSS style of that element from `display: block` to `display: none`, the box will disappear, revealing the hidden content.



---

## Task 5 - Developer Tools – Debugger
The **Debugger** (called **Sources** in Chrome) allows you to inspect the JavaScript files that make the website work.

* **Pretty Print:** Many JavaScript files are **minified** (squashed into one line). You can click the `{ }` button to format the code and make it easier to read.
* **Breakpoints:** These allow you to tell the browser to stop running code at a specific line. This is helpful if a script is hiding information or removing an element from the screen too quickly.

---

## Task 6 - Developer Tools – Network
The **Network tab** is vital for tracking the data flowing between your browser and the server.

* **AJAX Requests:** When you submit a form, the Network tab shows exactly where that data is being sent in the background. By examining these background requests, you can find hidden **API endpoints** or pages that might contain sensitive data or "flags."
