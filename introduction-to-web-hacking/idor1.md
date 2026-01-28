# IDOR

## Task 1 - What is an IDOR?
**IDOR** stands for **Insecure Direct Object Reference**. It is a type of access control vulnerability. 

This happens when a web server uses input provided by a user to retrieve objects (like files, data, or documents) but places too much trust in that input. The server fails to check on its own side if the requested object actually belongs to the user asking for it.



---

## Task 2 - Basic IDOR Example
Imagine you log into a website and click on your profile. The URL looks like this:
`http://example-service.com/profile?user_id=1305`

If you change the `user_id` in the address bar to `1000`:
`http://example-service.com/profile?user_id=1000`

If the website suddenly shows you someone else's private information, you have discovered an **IDOR vulnerability**. A secure website should always verify that the logged-in session matches the ID being requested.

---

## Task 3 - IDOR in Encoded IDs
Sometimes IDs are not plain numbers. Developers often **encode** them (commonly using **Base64**) to make them safe for the server to read. You can usually spot Base64 because it uses letters, numbers, and `=` signs.

**How to exploit it:**
1. Capture the encoded string (e.g., `MTMwNQ==`).
2. Use a decoder tool to see the raw value (this decodes to `1305`).
3. Change the value to a different number (e.g., `1000`).
4. Re-encode the new number and resubmit the request.

---

## Task 4 - IDOR in Hashed IDs
**Hashed IDs** are more complex but often follow a predictable pattern. For example, the ID `123` might always be turned into an MD5 hash like `202cb962ac59075b964b07152d234b70`.

**How to exploit it:**
* If you see a long string of random letters and numbers, try running it through a service like `CrackStation`. This checks if the hash matches a common integer. If it does, you can hash a different number and try to access another user's data.

---

## Task 5 - IDOR in Unpredictable IDs
If the IDs are random strings that cannot be decoded or cracked, the best method is to use **two accounts**:
1. Log into **Account A** and find its ID.
2. Log into **Account B**.
3. While logged into **Account B**, try to access a URL or data using **Account A's ID**.
4. If it works, the system is not checking if the ID belongs to the current logged-in user.

---

## Task 6 - Where IDORs are Located
You won't always find IDORs in the address bar. They can be hidden in:
* **AJAX Requests:** Background requests the browser makes while you stay on the same page.
* **JavaScript Files:** Code that references specific data endpoints.
* **Parameter Mining:** Sometimes a page like `/user/details` doesn't show an ID, but if you manually add `?user_id=123`, the server might process it anyway.

---

## Task 7 - Practical Examples
To find an IDOR in a real-world scenario, you can use the browser **Network Tab** to see how the website pulls your data.

### Example A: Finding the Hidden API
1. Log into your account and open **Developer Tools** (Network Tab).
2. Refresh the page and look for a request like `/api/v1/customer?id=15`.
3. This shows that the page uses an API to get your info.

### Example B: Exploiting the Endpoint
1. Take that API link: `http://site.com/api/v1/customer?id=15`.
2. Change the ID to `1`: `http://site.com/api/v1/customer?id=1`.
3. If the server returns the username and email of the administrator (User 1), the API is vulnerable to IDOR.
