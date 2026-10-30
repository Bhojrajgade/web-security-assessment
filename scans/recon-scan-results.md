# Reconnaissance and Scan Results

**Target:** DVWA Local Lab  
**Tester:** Bhojraj Gade  
**Environment:** Kali Linux VM → Windows Host  
**Target:** `10.0.2.2:8080`

---

## 1. Nmap Port Scan

### Command

```text
nmap -Pn -p 8080 10.0.2.2
```

### Result

```text
8080/tcp open http-proxy
```

**Observation:** TCP port 8080 was accessible and running an HTTP service.

---

## 2. Nmap Service Detection

### Command

```text
nmap -Pn -sV -p 8080 10.0.2.2
```

### Result

```text
Apache httpd 2.4.25 (Debian)
```

**Observation:** The exposed HTTP service was identified as Apache HTTP Server 2.4.25 on Debian.

---

## 3. HTTP Header Enumeration

### Command

```text
curl -I http://10.0.2.2:8080
```

### Key Observations

- HTTP redirect to the DVWA login page.
- Apache server information was exposed.
- PHP session cookie was present.
- DVWA security level information was observable in the response.
- The application redirected unauthenticated requests to `login.php`.

---

## 4. HTTP OPTIONS Request

### Command

```text
curl -i -X OPTIONS http://10.0.2.2:8080/
```

### Observation

The server redirected the request to the login page.

No useful `Allow` header listing supported HTTP methods was observed.

---

## 5. Directory and Resource Enumeration

### Tool

Gobuster

### Discovered Resources

| Resource | Status | Observation |
|---|---:|---|
| `.hta` | 403 | Access forbidden |
| `.htpasswd` | 403 | Access forbidden |
| `.htaccess` | 403 | Access forbidden |
| `config` | 301 | Redirect |
| `docs` | 301 | Redirect |
| `external` | 301 | Redirect |
| `favicon.ico` | 200 | Accessible |
| `index.php` | 302 | Redirect |
| `php.ini` | 200 | Accessible |
| `phpinfo.php` | 302 | Redirect |
| `robots.txt` | 200 | Accessible |
| `server-status` | 403 | Access forbidden |

---

## 6. Configuration Observation

The accessible `php.ini` resource contained the following settings:

```text
magic_quotes_gpc = Off
allow_url_fopen = On
allow_url_include = On
```

These were recorded as configuration observations and were not independently classified as confirmed vulnerabilities.

---

## 7. Scan Summary

| Tool | Purpose | Result |
|---|---|---|
| Nmap | Port discovery | TCP/8080 open |
| Nmap | Service detection | Apache 2.4.25 (Debian) |
| cURL | HTTP response analysis | Redirect and header information identified |
| cURL | OPTIONS testing | Redirect observed |
| Gobuster | Resource enumeration | Multiple resources discovered |

---

## 8. Assessment Note

These results were obtained from the authorized local DVWA laboratory environment.

No public or third-party systems were scanned.