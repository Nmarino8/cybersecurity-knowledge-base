# File Inclusion (LFI & RFI)

---

## Overview

**File Inclusion** is a web application vulnerability that occurs when an application **dynamically includes files** based on user input **without proper validation**.

**Attackers can exploit this behavior to:**

- Read sensitive files
- Execute arbitrary code
- Bypass authentication
- Fully compromise the server

**File Inclusion vulnerabilities are commonly divided into:**

- **Local File Inclusion (LFI)**
- **Remote File Inclusion (RFI)**

---

## Types of File Inclusion

### Local File Inclusion (LFI)

Allows attackers to include **files already present on the server**.

**Examples:**

- `/etc/passwd`
- Application configuration files
- Log files
- Source code

---

### Remote File Inclusion (RFI)

Allows attackers to include **remote files hosted on external servers**.

⚠️ RFI is usually more severe and often leads to **remote code execution (RCE)**.

> RFI typically requires:
> - `allow_url_include = On`
> - `allow_url_fopen = On` (PHP)

---

## How File Inclusion Works

### Vulnerable Code Example (PHP)

```php
$page = $_GET['page'];
include("pages/" . $page);
```

#### Intended Request

```cpde
index.php?page=home.php
```

#### Malicious Request (LFI)

```code
index.php?page=../../../../etc/passwd
```

#### Result

> **Sensitive system files are disclosed**

---

## Common Attack Techniques

#### Directory Traversal

```
../../../../etc/passwd
```

#### Null Byte Injection (Legacy)

```
../../../../etc/passwd%00
```

#### Log File Poisoning

	1.	Inject PHP code into server logs
	2.	Include the log file
	3.	Achieve code execution

---

## Real‑World Impact

File Inclusion vulnerabilities have been responsible for **severe real‑world security incidents**, often leading to full system compromise.

### Notable Cases

| Case | Impact |
|-----|-------|
| **Joomla LFI Vulnerabilities** | Full server compromise and remote code execution |
| **WordPress Plugin LFIs** | Leakage of database credentials and configuration files |
| **Custom PHP Applications** | Source code disclosure and sensitive file exposure |

### Common Consequences
Exploitation of File Inclusion vulnerabilities frequently results in:
-  Theft of credentials and sensitive configuration data  
-  Remote Code Execution (RCE)  
-  Complete compromise of the application and underlying infrastructure  

---

## 🔍 LFI vs RFI Comparison

| Feature | Local File Inclusion (LFI) | Remote File Inclusion (RFI) |
|------|----------------------------|-----------------------------|
| **File Source** | Files located on the local server | Files hosted on a remote server |
| **Internet Required** | ❌ No | ✅ Yes |
| **Likelihood of Code Execution** | Possible (depends on context) | Very common |
| **Overall Severity** | High | Critical |

---

## Prevention & Mitigation

#### Secure Coding Practices
	•	Never include files directly from user input
	•	Use whitelisting, not blacklisting
	•	Map user input to predefined file names

#### Example (Secure)

```php
$pages = ['home', 'about', 'contact'];
$page = $_GET['page'];

if (in_array($page, $pages)) {
    include("pages/$page.php");
}
```

---

## Additional Defenses

- Disable allow_url_include
-	Set proper file permissions
-	Use a Web Application Firewall (WAF)
-	Restrict access to sensitive files

---

## Tools for Testing File Inclusion

-	Burp Suite – Manual exploitation & request manipulation
- OWASP ZAP – Automated vulnerability scanning
-	Nikto – Server misconfiguration detection
-	Metasploit – Advanced exploitation modules

---

## References

- OWASP: File Inclusion
-	PortSwigger Web Security Academy
-	PHP Security Documentation

---
