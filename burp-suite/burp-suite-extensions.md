# Burp Suite Extensions
## Introduction
- Burp Suite supports **extensions** to add custom functionality
- Expand Burp Suite’s core capabilities by installing third-party extensions written in Java, Python, or Ruby. 
- Extensions are installed via the **BApp Store** or locally
---

## Extensions Interface
- Used to **manage installed extensions**

**Main Components**
- **Extensions List**
  - Shows installed extensions for current project
  - Enable / disable individual extensions

- **Management Controls**
  - **Add**: Install extension from local file
  - **Remove**: Uninstall selected extension
  - **Up / Down**: Change execution order  
    - Important when extensions modify requests

- **Extension Details**
  - **Details**: Name, version, description
  - **Output**: Runtime messages from extension
  - **Errors**: Debugging and failure messages

---

## BApp Store
- Official store for **trusted Burp extensions**
- Common languages:
  - **Java** (native support)
  - **Python** (requires Jython)

**Example: Request Timer**
- Logs **request–response timing**
- Useful for **time-based attacks** (e.g., username enumeration)

**Install Steps**
- Extensions → **BApp Store**
- Search extension name
- Select → **Install**
- New tab or menu options may appear after install

---

## Jython
Required to run **Python-based extensions**

**Setup**
- Download **Jython Standalone JAR**
- Burp → Extensions → **Settings**
- Set Jython JAR path under **Python Environment**

- Works the same on all OS (Java-based)

---

## Burp Suite API
- APIs allow **custom extension development**
- Located under **Extensions → APIs**

**Supported Languages**
- **Java** (native)
- **Python** (via Jython)
- **Ruby** (via JRuby)

---

