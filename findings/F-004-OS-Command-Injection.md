# F-004 — OS Command Injection

**Severity:** Critical  
**Status:** Confirmed  
**Category:** Command Injection  
**Affected Endpoint:** `/vulnerabilities/exec/`  
**Parameter:** `ip`

---

## 1. Description

The DVWA command execution functionality accepts user-controlled input through the `ip` parameter and passes that input to an operating-system command without sufficient validation.

A controlled command injection test demonstrated that additional operating-system command execution was possible.

---

## 2. Evidence

A baseline request was first submitted using the expected input.

A controlled command-separator test was then supplied through the `ip` parameter.

### Observed Request Pattern

```text id="0a5k1r"
POST /vulnerabilities/exec/ HTTP/1.1

ip=[expected-input][controlled-command]&Submit=Submit
```

The response included operating-system identity information produced by the additional command.

Example observed output:

```text id="u9k4xm"
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

---

## 3. Validation

The vulnerability was successfully reproduced within the authorized DVWA laboratory environment.

The additional operating-system command executed with the privileges of the web application process.

This confirms OS command injection.

---

## 4. Impact

OS command injection can allow an attacker to execute operating-system commands with the privileges of the vulnerable application process.

Depending on the application's privileges and system configuration, potential impacts include:

- Unauthorized access to system resources.
- Reading sensitive files.
- Modification of application or system data.
- Execution of unauthorized system operations.
- Further compromise of the affected host.

---

## 5. Root Cause

The application passes untrusted user input to an operating-system command without sufficiently restricting or validating the input.

---

## 6. Remediation

- Avoid operating-system command execution whenever an application-level API can perform the required function.
- Never pass raw user input directly to a shell.
- Use strict allowlist-based validation for required input formats.
- Use safe process-execution APIs that avoid shell interpretation where possible.
- Apply least-privilege permissions to the web application process.
- Isolate sensitive services from unnecessary system resources.

---

## 7. Risk Rating

**Critical**

The vulnerability permits operating-system command execution through user-controlled input.

---

## 8. Testing Environment

Testing was performed exclusively against the authorized local DVWA laboratory environment.

No third-party or public systems were targeted.