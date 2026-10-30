# F-002 — Reflected Cross-Site Scripting (XSS)

**Severity:** Medium  
**Status:** Confirmed  
**Category:** Cross-Site Scripting (XSS)  
**Affected Endpoint:** `/vulnerabilities/xss_r/`  
**Parameter:** `name`

---

## 1. Description

The DVWA reflected XSS functionality reflects user-controlled input into the application's response without appropriate output encoding.

A controlled JavaScript test input was accepted by the application and executed in the browser.

---

## 2. Evidence

A baseline request using ordinary text was first submitted to observe normal application behavior.

A controlled XSS test was then supplied through the `name` parameter.

### Observed Request Pattern

```text
GET /vulnerabilities/xss_r/?name=[controlled-input] HTTP/1.1
```

The application reflected the supplied input into the response, resulting in JavaScript execution in the browser.

---

## 3. Validation

The vulnerability was successfully reproduced within the authorized DVWA laboratory environment.

The browser executed the controlled test script, confirming that untrusted input was being reflected without sufficient output encoding.

---

## 4. Impact

Reflected XSS can allow attacker-controlled JavaScript to execute in a victim's browser within the security context of the vulnerable application.

Depending on the application's functionality and browser context, this could potentially lead to:

- Unauthorized actions performed through the victim's session.
- Manipulation of displayed application content.
- Phishing or social-engineering attacks.
- Exposure of information accessible to client-side scripts.

---

## 5. Root Cause

The application reflects untrusted user input into the HTML response without applying appropriate context-sensitive output encoding.

---

## 6. Remediation

- Apply context-appropriate output encoding before rendering user-controlled data.
- Validate input on the server side.
- Avoid inserting untrusted input directly into HTML.
- Implement a restrictive Content Security Policy where appropriate.
- Use secure framework features for automatic output escaping.

---

## 7. Risk Rating

**Medium**

The vulnerability allows controlled JavaScript execution in the application's browser context.

---

## 8. Testing Environment

Testing was performed exclusively against the authorized local DVWA laboratory environment.

No third-party or public systems were targeted.