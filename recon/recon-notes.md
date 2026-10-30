# Reconnaissance Notes

## 1. Target

**Application:** Damn Vulnerable Web Application (DVWA)  
**Environment:** Authorized local laboratory  
**Target Host:** Windows host accessed from Kali Linux VM  
**Target Port:** TCP/8080

---

## 2. Objective

The objective of reconnaissance was to identify exposed services, determine basic service information, enumerate accessible web resources, and collect information useful for subsequent vulnerability assessment.

---

## 3. Nmap — Port Discovery

### Command

```text
nmap -Pn -p 8080 10.0.2.2
```

### Result

The target exposed TCP port **8080**.

```text
8080/tcp open  http-proxy
```

This confirmed that an HTTP-accessible service was available on the target.

---

## 4. Nmap — Service Detection

### Command

```text
nmap -Pn -sV -p 8080 10.0.2.2
```

### Result

The service was identified as:

```text
Apache httpd 2.4.25 (Debian)
```

This provided basic information about the web server software running behind the DVWA application.

---

## 5. HTTP Header Enumeration

### Command

```text
curl -I http://10.0.2.2:8080
```

### Observed Behavior

The server returned an HTTP redirect to the DVWA login page.

The response also exposed server/application-related information including:

- Apache server information
- PHP session cookie
- DVWA security level
- Redirect location

The presence of this information was recorded as reconnaissance data rather than automatically treated as a vulnerability.

---

## 6. HTTP OPTIONS Request

### Command

```text
curl -i -X OPTIONS http://10.0.2.2:8080/
```

### Result

The request resulted in a redirect to the login page.

No useful `Allow` header listing supported HTTP methods was observed in the response.

---

## 7. Directory and Resource Enumeration

### Tool

Gobuster

### Wordlist

```text
/usr/share/wordlists/dirb/
```

### Discovered Resources

The following resources were identified during enumeration:

| Resource | HTTP Status | Observation |
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

## 8. php.ini Observation

The accessible `php.ini` resource was reviewed for potentially interesting configuration settings.

The following settings were observed:

```text
magic_quotes_gpc = Off
allow_url_fopen = On
allow_url_include = On
```

These values were recorded as configuration observations.

They were **not independently classified as confirmed vulnerabilities** during this assessment.

---

## 9. robots.txt

The `robots.txt` resource was accessible and contained a directive disallowing crawling of the site.

This was treated as informational reconnaissance data and not as a confirmed security vulnerability.

---

## 10. Reconnaissance Summary

Reconnaissance established that:

- TCP/8080 was accessible.
- The service was associated with Apache HTTP Server 2.4.25 on Debian.
- The web application redirected unauthenticated requests toward the login page.
- Multiple application resources were discoverable through directory enumeration.
- `php.ini` was accessible and exposed configuration information.
- Several sensitive-looking resources returned HTTP 403 rather than their contents.

The information gathered during reconnaissance was used to guide subsequent application security testing.