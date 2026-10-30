# Web Security Assessment: DVWA Lab

**Author:** Bhojraj Gade  
**Type:** Web Application Security Assessment — Practice Lab  
**Target:** Damn Vulnerable Web Application (DVWA)  
**Environment:** Local authorized lab

## 1. Objective

The objective of this project is to assess a deliberately vulnerable web application, identify security vulnerabilities, document the findings, and provide remediation recommendations.

All testing was performed against a local DVWA laboratory environment.

## 2. Target

- **Application:** Damn Vulnerable Web Application (DVWA)
- **Target:** Local DVWA instance
- **Environment:** Windows host + Kali Linux VM
- **Authorization:** Personal/controlled laboratory environment

## 3. Assessment Methodology

The assessment followed a practical web application security testing workflow:

1. **Reconnaissance** — Identify exposed services and application information.
2. **Enumeration** — Identify accessible directories, files, and application resources.
3. **Vulnerability Testing** — Test application functionality for common web vulnerabilities.
4. **Validation** — Confirm vulnerabilities using controlled requests and responses.
5. **Documentation** — Record evidence, impact, severity, and remediation recommendations.

## 4. Tools Used

- Nmap
- cURL
- Gobuster
- Burp Suite Community Edition
- Kali Linux
- VS Code

## 5. Confirmed Findings

| ID | Vulnerability | Severity | Status |
|---|---|---|---|
| F-001 | SQL Injection in User ID Parameter | High | Confirmed |
| F-002 | Reflected Cross-Site Scripting (XSS) | Medium | Confirmed |
| F-003 | Stored Cross-Site Scripting (XSS) | High | Confirmed |
| F-004 | OS Command Injection | Critical | Confirmed |
| F-005 | Weak Authentication / Brute-Force Susceptibility | High | Confirmed |

> The authentication finding is documented as **weak authentication / brute-force susceptibility** because testing used a controlled DVWA credential rather than claiming a full automated brute-force attack.

## 6. Reconnaissance Highlights

Initial reconnaissance identified an HTTP service exposed by the local DVWA environment.

Further enumeration identified accessible application resources and configuration-related files, including `php.ini`.

The assessment also examined HTTP responses and application behavior using cURL and Burp Suite.

## 7. Project Structure

```text
web-security-assessment/
├── README.md
├── scope.md
├── recon/
│   └── recon-notes.md
├── scans/
│   └── recon-scan-results.md
├── findings/
│   ├── F-001-SQL-Injection.md
│   ├── F-002-Reflected-XSS.md
│   ├── F-003-Stored-XSS.md
│   ├── F-004-OS-Command-Injection.md
│   └── F-005-Weak-Authentication.md
├── scripts/
│   └── README.md
└── report/
    └── final-report.md
```

## 8. Key Security Areas Tested

- SQL Injection
- Reflected XSS
- Stored XSS
- OS Command Injection
- Authentication weaknesses
- Web service and directory enumeration
- Security-related configuration exposure

## 9. Remediation Focus

General remediation recommendations include:

- Use parameterized queries and prepared statements.
- Apply proper server-side input validation.
- Encode untrusted output according to its context.
- Avoid passing user-controlled input directly to operating-system commands.
- Enforce strong authentication controls and rate limiting.
- Remove unnecessary information disclosure.
- Keep web server and application components securely configured.

Detailed findings and evidence are documented in the assessment report.

## 10. Disclaimer

This project was conducted strictly for educational purposes in an authorized, controlled laboratory environment.

No unauthorized or third-party systems were targeted.