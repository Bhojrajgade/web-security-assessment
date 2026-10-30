# F-003 — Stored Cross-Site Scripting (XSS)

**Severity:** High  
**Status:** Confirmed  
**Category:** Cross-Site Scripting (XSS)  
**Affected Endpoint:** `/vulnerabilities/xss_s/`  
**Affected Fields:** `txtName`, `mtxMessage`

---

## 1. Description

The DVWA guestbook functionality stores user-supplied content and later renders that content without sufficient output encoding.

A controlled JavaScript test submitted through the guestbook message field was stored by the application and executed when the content was rendered.

---

## 2. Evidence

A normal guestbook entry was first submitted to establish baseline behavior.

A controlled XSS test was then submitted through the message field.

### Observed Request Pattern

```text id="0v6w3q"
POST /vulnerabilities/xss_s/ HTTP/1.1

txtName=[test-name]&mtxMessage=[controlled-input]&btnSign=Sign+Guestbook
```

The application returned the stored input in its response without appropriate encoding.

---

## 3. Validation

The controlled test input was successfully stored by the application.

When the guestbook content was rendered in the browser, the test script executed, confirming persistent/stored XSS.

The behavior was also observed during request/response analysis using Burp Suite.

---

## 4. Impact

Stored XSS can be more significant than reflected XSS because malicious content can persist in the application and execute whenever affected content is viewed.

Potential impacts include:

- Unauthorized actions performed through a victim's session.
- Manipulation of application content.
- Phishing or social-engineering attacks.
- Exposure of information accessible to client-side scripts.

The exact impact depends on the application's privileges and functionality.

---

## 5. Root Cause

The application stores and renders untrusted user input without applying appropriate context-sensitive output encoding.

---

## 6. Remediation

- Apply context-aware output encoding when rendering stored content.
- Validate and sanitize user-submitted data.
- Avoid rendering untrusted content as executable HTML or JavaScript.
- Implement a restrictive Content Security Policy where appropriate.
- Use secure framework features that automatically escape output.

---

## 7. Risk Rating

**High**

The vulnerability allows persistent attacker-controlled script content to execute when stored content is rendered.

---

## 8. Testing Environment

Testing was performed exclusively against the authorized local DVWA laboratory environment.

No third-party or public systems were targeted.