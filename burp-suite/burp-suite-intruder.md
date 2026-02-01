# Burp Suite Intruder

## Intruder

**Intruder** is an **automated request manipulation tool** used for controlled web application testing.

**Primary Uses**
- Fuzzing
- Brute‑forcing
- Parameter discovery

**How It Works**
- Uses a captured request (usually from **Proxy**)
- Modifies selected parts
- Replays requests with payload variations


---

## Real‑World Pentest Use Cases

- Login brute‑force testing
- Credential stuffing
- Endpoint & parameter fuzzing
- Header manipulation
- Business logic edge cases (rate‑limited)

**Best Practice:**  
Use Intruder for **controlled automation**, not mass brute‑forcing.

---

## Workflow

1. Capture request in **Proxy**
2. Send to Intruder  
   - Right‑click → **Send to Intruder**  
   - Shortcut: **Ctrl + I**
3. Configure **Positions**
4. Configure **Payloads**
5. Select **Attack Type**
6. Start attack and analyze responses

---

## Tabs Overview

### Positions
- Defines injection points
- Payload markers: **§**

**Controls**
- Add §
- Clear §
- Auto § (detect common inputs)

---

### Payloads

**Payload Sets**
- **Sniper / Battering Ram:** single set
- **Pitchfork / Cluster Bomb:** one set per position
- Order: top‑to‑bottom, left‑to‑right

**Payload Sources**
- Simple list
- Wordlist
- Numbers / dates

> Avoid large wordlists (performance issues)

**Payload Processing**
- Prefix / suffix
- Regex filtering
- Case modification

**Payload Encoding**
- Control URL encoding
- Important for APIs and encoded inputs

---

## Attack Types

### Sniper
- One payload, one position at a time  
- **Requests:** `payloads × positions`

**Best For**
- Single‑parameter fuzzing
- Endpoint discovery
- Password testing

---

### Battering Ram
- Same payload in all positions
- One request per payload

**Best For**
- Identical field testing
- Simple logic checks

---

### Pitchfork
- One payload set per position
- Runs in parallel
- Stops at shortest list

**Best For**
- Known username/password pairs

---

### Cluster Bomb
- All payload combinations  
- **Requests:** `list1 × list2 × ... × listN`

**Best For**
- Unknown credential mapping


---

## Attack Type Selection

| Scenario                     | Attack Type |
|------------------------------|-------------|
| Single parameter fuzzing     | Sniper |
| Same payload everywhere      | Battering Ram |
| Known credential pairs       | Pitchfork |
| Unknown credential mapping   | Cluster Bomb |

---

## Community Edition Limitations

- Heavy rate limiting
- Long execution times

**Not Suitable For**
- Large wordlists
- High‑volume brute‑forcing

**Common Alternatives**
- `ffuf`
- `hydra`
- Custom scripts

---

## Best Practices

- Always define scope
- Keep payloads small and focused
- Compare status codes, length, and content
- Validate results manually with **Repeater**
- Minimize unnecessary traffic




