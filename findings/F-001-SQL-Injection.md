# F-001 — SQL Injection in User ID Parameter

**Severity:** High  
**Status:** Confirmed  
**Category:** Injection  
**Affected Endpoint:** `/vulnerabilities/sqli/`  
**Parameter:** `id`

---

## 1. Description

The DVWA SQL Injection functionality accepts user-controlled input through the `id` parameter.

The application does not properly separate user input from the underlying database query, allowing the input to alter the intended SQL query logic.

---

## 2. Evidence

A baseline request using a valid user ID returned the expected user record.

A controlled SQL injection test caused the application to return multiple database records instead of only the requested record.

### Observed Request Pattern

```text
GET /vulnerabilities/sqli/?id=[controlled-input]&Submit=Submit
```

The application response demonstrated that the supplied input affected the database query.

---

## 3. Validation

The vulnerability was validated within the authorized DVWA laboratory environment.

The test demonstrated that user-controlled input could modify the application's SQL query behavior and retrieve additional records.

---

## 4. Impact

Successful SQL injection can potentially allow an attacker to:

- Retrieve unauthorized database information.
- Bypass intended database query restrictions.
- Modify or delete database data, depending on database privileges.
- Potentially affect other application functionality relying on the database.

The exact impact depends on the database permissions and application architecture.

---

## 5. Root Cause

The underlying issue is insufficient separation between untrusted user input and SQL query construction.

---

## 6. Remediation

- Use parameterized queries or prepared statements.
- Never concatenate untrusted input directly into SQL queries.
- Implement strict server-side input validation.
- Apply least-privilege permissions to the application's database account.
- Avoid exposing detailed database errors to users.

---

## 7. Risk Rating

**High**

The vulnerability allows controlled manipulation of database query behavior and may expose sensitive application data.

---

## 8. Testing Environment

Testing was performed exclusively against the authorized local DVWA laboratory environment.

No third-party or public systems were targeted.