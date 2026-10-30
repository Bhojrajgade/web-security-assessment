# F-005 — Weak Authentication / Brute-Force Susceptibility

**Severity:** High  
**Status:** Confirmed  
**Category:** Authentication  
**Affected Endpoint:** `/vulnerabilities/brute/`

---

## 1. Description

The DVWA brute-force functionality does not demonstrate effective protection against repeated authentication attempts.

The assessment also confirmed that the known default DVWA laboratory credential could successfully authenticate to the protected area.

---

## 2. Evidence

A baseline authentication request using an incorrect password resulted in an authentication failure.

A controlled test using the known DVWA laboratory credential successfully authenticated to the protected area.

### Observed Request Pattern

```text id="q7m4vz"
GET /vulnerabilities/brute/?username=[username]&password=[password]&Login=Login HTTP/1.1
```

### Successful Authentication

The application returned a successful response containing the protected-area welcome message after the valid laboratory credential was supplied.

---

## 3. Validation

The finding was validated within the authorized DVWA laboratory environment.

The assessment confirmed that:

- Incorrect credentials were rejected.
- The known default laboratory credential was accepted.
- The application did not demonstrate effective controls against repeated authentication attempts during the assessment.

**Assessment limitation:** A full automated brute-force attack was not performed. Therefore, this finding is documented as weak/default authentication and brute-force susceptibility rather than claiming a completed large-scale brute-force attack.

---

## 4. Impact

Weak authentication controls can allow unauthorized access when default, weak, or compromised credentials are used.

Insufficient protection against repeated authentication attempts can increase the risk of credential-guessing attacks.

Potential impacts include:

- Unauthorized access to protected application functionality.
- Account compromise through weak or default credentials.
- Increased exposure to credential-guessing attacks.

---

## 5. Root Cause

The application relies on a known default credential and does not demonstrate sufficient controls to prevent repeated authentication attempts.

---

## 6. Remediation

- Remove all default credentials before deployment.
- Require strong and unique passwords.
- Implement authentication rate limiting.
- Consider progressive delays after failed attempts.
- Consider temporary account lockout where appropriate.
- Implement multi-factor authentication for sensitive accounts.
- Monitor and log repeated failed authentication attempts.

---

## 7. Risk Rating

**High**

The use of known default credentials combined with insufficient authentication protections can enable unauthorized access.

---

## 8. Testing Environment

Testing was performed exclusively against the authorized local DVWA laboratory environment.

No third-party or public systems were targeted.