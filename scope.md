# Web Security Assessment — Scope

## 1. Project Information

**Project:** Web Security Assessment — DVWA Lab  
**Tester:** Bhojraj Gade  
**Assessment Type:** Authorized Web Application Security Assessment  
**Environment:** Personal/controlled laboratory environment  
**Target:** Damn Vulnerable Web Application (DVWA)

---

## 2. Objective

The objective of this assessment is to practice a structured web application security testing methodology by identifying, validating, documenting, and recommending remediation for vulnerabilities within the DVWA laboratory environment.

---

## 3. In-Scope Target

The following components were included in the assessment:

- DVWA web application
- HTTP service exposed by the local DVWA instance
- DVWA application endpoints and functionality
- Selected authentication and input-handling modules
- Application resources discovered during authorized enumeration

---

## 4. Vulnerability Testing Areas

The assessment included testing for:

- SQL Injection
- Reflected Cross-Site Scripting (XSS)
- Stored Cross-Site Scripting (XSS)
- OS Command Injection
- Authentication weaknesses
- Directory and resource enumeration
- Security-related configuration exposure

---

## 5. Out of Scope

The following activities were excluded:

- Third-party or public websites
- Systems without authorization
- Denial-of-service testing
- Destructive actions
- Data destruction or modification outside the laboratory
- Persistence mechanisms
- Attacks against unrelated systems

---

## 6. Testing Constraints

Testing was performed in a controlled laboratory environment using deliberately vulnerable software.

Only controlled test inputs were used to validate vulnerabilities.

No real-world targets were involved.

---

## 7. Authorization

The DVWA instance was deployed and tested as part of a personal cybersecurity training laboratory.

The assessment was limited to the authorized local environment.

---

## 8. Disclaimer

This project is intended solely for cybersecurity education, training, and portfolio development.

The techniques documented in this project must only be used against systems where explicit authorization has been provided.