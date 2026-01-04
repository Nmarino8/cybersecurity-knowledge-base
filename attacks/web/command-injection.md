# Command Injection

---

## Overview
Command Injection is a **critical web application vulnerability** that occurs when an application passes **unsanitized user input** directly to a system shell or operating system command.

This allows an attacker to **execute arbitrary commands** on the underlying server with the privileges of the vulnerable application, potentially leading to full system compromise.

Command Injection is considered **high to critical severity** due to its ability to grant attackers direct access to the operating system.

---

## How Command Injection Works

1. A web application executes system commands (e.g., `ping`, `ls`, `cat`) using user input.
2. The application fails to properly validate or sanitize the input.
3. An attacker injects shell metacharacters (e.g., `;`, `&&`, `|`) into the input.
4. The operating system executes both the intended command and the injected malicious command.

### Example (Vulnerable Code)

```php
$ip = $_GET['ip'];
system("ping -c 4 " . $ip);
```

#### Malicious Input

```code
127.0.0.1; cat /etc/passwd
```

**Result**

The server executes:

```bash
ping -c 4 127.0.0.1
cat /etc/passwd
```

---

## Common Injection Characters

Attackers often rely on special shell characters to chain commands, redirect execution, or substitute command output.

- **`;` — Command separator**  
  > Executes multiple commands sequentially, regardless of success or failure.

- **`&&` — Conditional execution (success)**
  > Executes the next command only if the previous command succeeds.

- **`||` — Conditional execution (failure)**  
  > Executes the next command only if the previous command fails.

- **`|` — Pipe operator**  
  > Passes the output of one command as input to another.

- **`` `command` `` — Command substitution (backticks)**  
  > Executes a command and replaces it with its output.

- **`$(command)` — Command substitution (modern syntax)**  
  > Functionally equivalent to backticks but more readable and safer in scripts.

---

## Impact of Command Injection

**Successful command injection can lead to:**

-	Remote Code Execution (RCE)
-	Server takeover
-	Data exfiltration
- Privilege escalation
-	Malware installation
-	Lateral movement within the network

**In many cases, command injection results in full system compromise.**

---

## Real-World Examples

Incident	Year	Impact
Shellshock Vulnerability	2014	Massive RCE across servers
Cisco ASA Command Injection	2018	Remote unauthenticated code execution
WordPress Plugin Exploits	Various	Full server compromise

---

## Detection & Testing Techniques

#### Manual Testing

- Inject shell metacharacters into parameters
- Observe command output, delays, or errors
- Test blind injection using time delays (sleep, ping)

#### Automated Tools
- Burp Suite (Intruder & Repeater)
- OWASP ZAP
- Commix (Command Injection Exploitation Tool)

---

## Prevention & Mitigation

	1.	Avoid system command execution whenever possible
	2.	Use safe APIs instead of shell calls
	3.	Whitelist allowed input values
	4.	Escape shell arguments using safe libraries
	5.	Run applications with least privilege
	6.	Disable dangerous system functions
	7.	Use containerization / sandboxing

#### Secure Example

```php
$ip = escapeshellarg($_GET['ip']);
system("ping -c 4 " . $ip);
```

---

## Tools Related to Command Injection
-	Burp Suite￼
-	Commix￼
-	Metasploit￼
- Nmap NSE Scripts￼

---

## Command Injection vs SQL Injection

| Feature | Command Injection | SQL Injection |
|-------|------------------|---------------|
| **Target** | Operating System | Database |
| **Executes** | OS-level commands | SQL queries |
| **Typical Severity** | High / Critical | High |
| **Primary Impact** | Full server compromise | Data breach, data manipulation |

---

## References

-	OWASP Command Injection￼
-	PortSwigger Command Injection￼
-	MITRE CWE-78￼

---
