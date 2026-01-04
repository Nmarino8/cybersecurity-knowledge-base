# Cross-Site Request Forgery (CSRF)

---

## Overview

Cross-Site Request Forgery (CSRF) is a **web security vulnerability** that tricks a user’s browser into performing **unintended actions** on a web application where the user is authenticated.  

Unlike XSS, CSRF **does not directly steal data**, but exploits the trust a website has in the user’s browser.

---

## How It Works

1. The user logs into a trusted website (e.g., bank.com).  
2. The user visits a malicious site while still authenticated.  
3. The malicious site sends a request to the trusted site using the user’s credentials.  
4. The action executes without the user’s consent (e.g., transferring money, changing email).  

**Key Point:** The attack relies on **existing authentication cookies** or tokens automatically sent by the browser.

---

## Types of CSRF Attacks

| Type | Description |
|------|-------------|
| GET-based CSRF | Uses simple GET requests to trigger actions (less common today). |
| POST-based CSRF | Uses forms or AJAX POST requests to perform sensitive actions. |
| Login CSRF | Forces a user to log in as the attacker, compromising session state. |
| OAuth / API CSRF | Exploits third-party APIs or OAuth tokens to perform unintended actions. |

---

## Real-World Examples

| Incident | Year | Impact |
|----------|------|--------|
| GitHub CSRF Vulnerability | 2012 | Allowed session hijacking for logged-in users |
| WordPress Plugin Exploit | 2015 | Allowed attackers to change admin settings |
| Online Banking CSRF attacks | Various | Unauthorized fund transfers |

---

## Prevention & Mitigation

1. **CSRF Tokens** – Add unique, unpredictable tokens to sensitive forms and verify them server-side.  
2. **SameSite Cookies** – Configure cookies to prevent cross-origin requests.  
3. **Double Submit Cookie** – Verify a token in both cookie and request parameters.  
4. **Check Referer/Origin Headers** – Ensure requests come from trusted domains.  
5. **Avoid GET for state-changing operations** – Always use POST with proper verification.  

---

## Tools for Testing CSRF

- [Burp Suite](tools/burpsuite.md) – Test and exploit CSRF in web apps  
- [OWASP ZAP](https://www.zaproxy.org/) – Detect CSRF vulnerabilities  
- [CSRF-Tester](https://github.com/) – Automated CSRF testing scripts  

---

## References

- [OWASP CSRF](https://owasp.org/www-community/attacks/csrf)  
- [PortSwigger: CSRF Guide](https://portswigger.net/web-security/csrf)  
- [MDN Web Docs – CSRF](https://developer.mozilla.org/en-US/docs/Glossary/CSRF)  

---
