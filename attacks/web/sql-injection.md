# SQL Injection (SQLi)

## Overview
**SQL Injection (SQLi)** is a web application vulnerability that allows an attacker to interfere with the queries an application makes to its database. By injecting malicious SQL code into user-controlled input, an attacker can read, modify, or delete database data, bypass authentication, and in some cases execute administrative operations on the database.

SQL Injection remains one of the **most critical and common web vulnerabilities**, consistently ranked in the **OWASP Top 10**.

---

## How SQL Injection Works
Web applications often construct SQL queries dynamically using user input.  
If this input is not properly validated or sanitized, attackers can manipulate the SQL query structure.

### Vulnerable Example

```sql
SELECT * FROM users WHERE username = '$username' AND password = '$password';
```

If an attacker supplies:

```sql
' OR '1'='1
```

The query becomes:

```sql
SELECT * FROM users WHERE username = '' OR '1'='1' AND password = '';
```

This condition always evaluates to true, potentially allowing authentication bypass.

---

## Types of SQL Injection

#### **1. Classic (In-Band) SQL Injection**

The attacker receives results directly through the same channel.

**a) Error-Based SQL Injection**
	•	Relies on database error messages
	•	Useful for database fingerprinting

Example:
```sql
' UNION SELECT NULL,NULL --
```
**b) Union-Based SQL Injection**
	•	Uses the UNION SQL operator to extract data

Example:
```sql
' UNION SELECT username, password FROM users --
```

#### **2. Blind SQL Injection**

The application does not display database errors or output.

**a) Boolean-Based Blind SQLi**
	•	Attacker infers data based on true/false responses

Example:
```sql
' AND 1=1 --
' AND 1=2 --
```
**b) Time-Based Blind SQLi**
	•	Uses time delays to infer results

Example (MySQL):
```sql
' AND IF(1=1, SLEEP(5), 0) --
```

#### **3. Out-of-Band SQL Injection**

- Data is exfiltrated via DNS or HTTP requests
- Rare but powerful
- Requires specific database features

---

## Common Injection Points

- Login forms
- Search fields
- URL parameters
- Cookies
- HTTP headers (User-Agent, Referer)
- API request bodies (JSON, XML)

---

## Impact

SQL Injection vulnerabilities can lead to:
- 🔓 Authentication bypass
- 📂 Unauthorized data disclosure
- 🗑️ Data modification or deletion
- 🛠️Database administrator access
-	🖥️ Remote code execution (in some DBMS)
-	⚠️ Full system compromise

---

## Database-Specific Payloads

#### **MySQL**
```sql
' OR 1=1 --
' UNION SELECT database(), user() --
```
#### **PostgreSQL**
```sql
' UNION SELECT version(), current_user --
```
#### **Microsoft SQL Server**
```sql
' UNION SELECT @@version, SYSTEM_USER --
```
##### **Oracle**
```sql
' UNION SELECT banner FROM v$version --
```
---

## Detection & Testing

**Manual Testing**
- Insert `'`, `"`, `--`, `#`, `/* */`
- Observe error messages or behavioral changes

**Automated Tools**
- SQLmap
- Burp Suite Scanner
-	OWASP ZAP
-	Nuclei

---

## Mitigation & Prevention

1. Prepared Statements (Parameterized Queries)

SELECT * FROM users WHERE username = ? AND password = ?;

2. Input Validation
	•	Allow-list input formats
	•	Reject unexpected characters

3. Stored Procedures
	•	Use safely (avoid dynamic SQL inside them)

4. Least Privilege
	•	Database accounts should have minimal permissions

5. Web Application Firewalls (WAF)
	•	Can detect and block common SQLi payloads

---

## Real-World Examples
- Sony PlayStation Network breach (2011)
- Heartland Payment Systems (2008)
- TalkTalk breach (2015)

**These incidents resulted in millions of compromised records and significant financial losses.**

---

## Related Attacks

- Command Injection
- LDAP Injection
-	XPath Injection
-	NoSQL Injection

---

## Hands-On Practice

- DVWA (Damn Vulnerable Web App)
- Juice Shop
- Hack The Box
- PortSwigger Web Security Academy

---

## References

- OWASP SQL Injection: https://owasp.org/www-community/attacks/SQL_Injection
-	PortSwigger Academy
- MITRE CWE-89
-	NIST SP 800-53

---

## Notes

> SQL Injection is preventable, yet still widespread.
Understanding both offensive techniques and defensive countermeasures is essential for any security professional.

---
