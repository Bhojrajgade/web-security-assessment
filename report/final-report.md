# Web Security Assessment Report

**Target:** Damn Vulnerable Web Application (DVWA)  
**Tester:** Bhojraj Gade  
**Assessment Type:** Web Application Security Assessment — Authorized Local Lab  
**Environment:** Windows Host + Kali Linux VM  
**Date:** October 2026

---

## 1. Introduction

This report documents a security assessment of Damn Vulnerable Web Application (DVWA), a deliberately vulnerable web application designed for security training and practice.

The assessment was performed in a controlled local laboratory environment. The objective was to identify, validate, and document common web application vulnerabilities and provide appropriate remediation recommendations.

---

## 2. Scope

### In Scope

- DVWA application hosted in the local laboratory environment
- HTTP service exposed by the DVWA instance
- Application functionality available through the DVWA interface
- Vulnerability testing of selected DVWA modules

### Out of Scope

- Real-world third-party systems
- Public websites
- Systems without explicit authorization
- Denial-of-service testing
- Destructive testing

---

## 3. Methodology

The assessment followed these general phases:

1. **Reconnaissance**
   - Identified the exposed HTTP service.
   - Performed service and version detection.

2. **Enumeration**
   - Examined HTTP responses and accessible application resources.
   - Performed directory and resource enumeration.

3. **Vulnerability Assessment**
   - Tested selected DVWA modules for common web vulnerabilities.
   - Used controlled inputs within the authorized laboratory.

4. **Validation**
   - Confirmed vulnerabilities through application responses and controlled evidence.

5. **Reporting**
   - Documented the vulnerabilities, impact, evidence, and remediation recommendations.

### Tools Used

- Nmap
- cURL
- Gobuster
- Burp Suite Community Edition
- Kali Linux
- VS Code

---

# 4. Findings

## F-001 — SQL Injection in User ID Parameter

**Severity:** High  
**Status:** Confirmed  
**Category:** Injection

### Description

The DVWA SQL Injection module accepts user-controlled input through the `id` parameter. The application does not properly separate user input from the underlying SQL query.

A controlled SQL injection payload caused the application to return multiple database records instead of processing only the requested user ID.

### Affected Endpoint

`/vulnerabilities/sqli/`

### Validation

A normal request using a single user ID returned the expected user record.

A controlled SQL injection test altered the application's query logic and returned multiple records.

### Impact

An SQL injection vulnerability can allow an attacker to manipulate database queries. Depending on the application's database permissions and implementation, this can potentially expose, modify, or otherwise affect database information.

### Recommendation

- Use parameterized queries/prepared statements.
- Never concatenate untrusted input directly into SQL queries.
- Apply server-side input validation.
- Use database accounts with the minimum required privileges.

---

## F-002 — Reflected Cross-Site Scripting (XSS)

**Severity:** Medium  
**Status:** Confirmed  
**Category:** Cross-Site Scripting

### Description

The DVWA reflected XSS module reflects user-controlled input into the application's response without appropriate output encoding.

A controlled JavaScript test input was reflected and executed by the browser.

### Affected Endpoint

`/vulnerabilities/xss_r/`

### Validation

A normal text input was first submitted to establish baseline behavior.

A controlled script payload was then supplied through the `name` parameter. The browser executed the supplied JavaScript, confirming reflected XSS.

### Impact

Reflected XSS can allow attacker-controlled script content to execute in a victim's browser in the security context of the vulnerable application.

### Recommendation

- Apply context-appropriate output encoding.
- Validate and sanitize untrusted input.
- Use a strong Content Security Policy where appropriate.
- Avoid inserting untrusted data directly into HTML.

---

## F-003 — Stored Cross-Site Scripting (XSS)

**Severity:** High  
**Status:** Confirmed  
**Category:** Cross-Site Scripting

### Description

The DVWA guestbook functionality stores user-supplied content and later displays it without sufficient output encoding.

A controlled JavaScript test input submitted through the guestbook message field was stored and executed when rendered by the application.

### Affected Endpoint

`/vulnerabilities/xss_s/`

### Validation

A controlled script payload was submitted through the guestbook message field.

The application returned the stored content and the browser executed the supplied JavaScript, confirming persistent XSS.

### Impact

Stored XSS is particularly significant because malicious content can remain in the application and execute when other users view the affected content.

### Recommendation

- Encode output according to its HTML context.
- Validate and sanitize user-submitted content.
- Use a restrictive Content Security Policy.
- Avoid rendering untrusted content as executable HTML or JavaScript.

---

## F-004 — OS Command Injection

**Severity:** Critical  
**Status:** Confirmed  
**Category:** Injection

### Description

The DVWA command execution functionality passes user-controlled input to an operating-system command without adequate validation or isolation.

A controlled command injection test caused the application to execute an additional operating-system command.

### Affected Endpoint

`/vulnerabilities/exec/`

### Validation

A normal request was first submitted using the expected input.

A controlled command-separator test was then supplied. The application returned operating-system identity information from the additional command, confirming command execution.

### Impact

OS command injection can allow an attacker to execute operating-system commands with the privileges of the vulnerable application process.

This may result in unauthorized access to application resources, sensitive information, or other system capabilities.

### Recommendation

- Avoid passing user input to operating-system commands whenever possible.
- Use safe APIs instead of shell execution.
- Apply strict allowlist-based validation when command execution is unavoidable.
- Run applications with the minimum required operating-system privileges.
- Isolate sensitive application components.

---

## F-005 — Weak Authentication / Brute-Force Susceptibility

**Severity:** High  
**Status:** Confirmed  
**Category:** Authentication

### Description

The DVWA brute-force module accepts username and password credentials without demonstrating effective protection against repeated authentication attempts.

The controlled lab test also confirmed that the default DVWA administrative credential could successfully authenticate.

### Affected Endpoint

`/vulnerabilities/brute/`

### Validation

An incorrect password produced an authentication failure.

A controlled test using the known DVWA laboratory credential successfully authenticated to the protected area.

**Note:** This assessment did not claim a full automated brute-force attack. The finding therefore focuses on weak/default authentication and susceptibility to repeated credential attempts.

### Impact

Weak or default credentials can allow unauthorized access to protected application functionality.

Without appropriate protections such as rate limiting or account lockout, repeated authentication attempts may also increase the risk of credential guessing attacks.

### Recommendation

- Remove default credentials before deployment.
- Require strong, unique passwords.
- Implement rate limiting for authentication attempts.
- Consider temporary account lockout or progressive delays.
- Implement multi-factor authentication where appropriate.
- Monitor and log suspicious authentication activity.

---

# 5. Additional Observations

During reconnaissance, several application resources and configuration-related files were identified.

The exposed `php.ini` configuration included settings such as `magic_quotes_gpc = Off`, `allow_url_fopen = On`, and `allow_url_include = On`.

These observations should be reviewed as part of secure server configuration. Their presence alone was not treated as a confirmed standalone vulnerability in this assessment.

---

# 6. Findings Summary

| ID | Finding | Severity | Status |
|---|---|---|---|
| F-001 | SQL Injection | High | Confirmed |
| F-002 | Reflected XSS | Medium | Confirmed |
| F-003 | Stored XSS | High | Confirmed |
| F-004 | OS Command Injection | Critical | Confirmed |
| F-005 | Weak Authentication / Brute-Force Susceptibility | High | Confirmed |

---

# 7. Overall Recommendations

The following security improvements are recommended:

1. Use parameterized database queries.
2. Implement robust server-side input validation.
3. Apply context-aware output encoding.
4. Prevent user input from reaching shell commands.
5. Remove default credentials.
6. Implement authentication rate limiting.
7. Apply least-privilege principles.
8. Review server and PHP configuration.
9. Minimize unnecessary information disclosure.
10. Maintain secure application and server configurations.

---

# 8. Conclusion

The assessment identified multiple significant vulnerabilities within the intentionally vulnerable DVWA application.

The most critical finding was OS command injection, followed by SQL injection, stored XSS, and authentication weaknesses. Reflected XSS was also successfully validated.

These findings demonstrate the importance of secure input handling, output encoding, strong authentication controls, least privilege, and secure application configuration.

The assessment was performed exclusively within an authorized local laboratory environment for educational and cybersecurity training purposes.