# Directory Traversal (Path Traversal)

---

## Overview

**Directory Traversal**, also known as **Path Traversal**, is a web security vulnerability that allows an attacker to **access files and directories outside the intended directory structure** of a web application.

This occurs when user-supplied input is improperly validated and is used to construct file paths on the server. By manipulating path parameters, attackers can read sensitive system files, application source code, or configuration data.

---

## How Directory Traversal Works

Many web applications allow users to request files, such as:
- Downloading documents
- Viewing images
- Accessing logs or reports

If the application directly uses user input to build a file path, an attacker can inject traversal sequences like:

```
../
```

#### Example Vulnerable Request

```
https://example.com/view?file=report.pdf
```

#### Malicious Request

```
https://example.com/view?file=../../../../etc/passwd
```

If not properly secured, the server may return sensitive system files.

---

## Common Traversal Patterns

Attackers use different encoding and variations to bypass filters:
```
../
```
```
..
```
```
....//
```
```
%2e%2e%2f
```
```
%252e%252e%252f
```
```
..%c0%af
```

These techniques are often combined with encoding tricks to evade input validation.

---

## Commonly Targeted Files

#### Linux / Unix

```code
/etc/passwd
/etc/shadow
/etc/hosts
/var/log/apache2/access.log
```

#### Windows

```code
C:\Windows\System32\drivers\etc\hosts
C:\boot.ini
C:\Windows\win.ini
```
---

## Impact & Risks

**Successful Directory Traversal attacks can lead to:**

-  Disclosure of sensitive files
-  Source code leakage
-  Credential exposure
-  Information disclosure for further attacks
-  Remote Code Execution (when combined with other vulnerabilities)

---

## Real-World Impact
| Case | Impact |
|-----|--------|
| Apache Struts Vulnerabilities | Sensitive file exposure |
| Joomla Path Traversal Bugs | Full website compromise |
| Custom PHP Applications | Configuration and credential leaks |

Directory Traversal is frequently used as a **stepping stone** for more severe attacks.

---

## Prevention & Mitigation

#### Best Practices

1. **Never trust user input** for file paths
2. **Use allowlists** for permitted files
3. **Normalize paths** before processing
4. **Use secure APIs** for file handling
5. **Remove traversal sequences** before usage
6. **Run applications with least privilege**

#### Secure Example (Conceptual)

```plaintext
Allowed files:
- report.pdf
- invoice.pdf
```

**Reject any input that does not match the allowlist exactly.**

---

## Tools for Testing Directory Traversal

-	Burp Suite – Manual and automated testing
-	OWASP ZAP – Passive and active scanning
-	Nikto – Server misconfiguration detection
-	FFUF / Gobuster – Fuzzing file paths

---

## References

-	OWASP: Path Traversal - https://owasp.org/www-community/attacks/Path_Traversal
-	PortSwigger Web Security Academy - https://portswigger.net/web-security/file-path-traversal
-	CWE-22: Improper Limitation of a Pathname - https://cwe.mitre.org/data/definitions/22.html

---
