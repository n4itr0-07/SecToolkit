# Comprehensive Guide to Open Redirect Vulnerability for Bug Hunters

## Introduction

Open Redirect vulnerabilities occur when a web application allows an attacker to redirect users to an arbitrary URL without proper validation. This flaw can lead to phishing attacks, data theft, or other malicious exploits. It's considered a part of the **OWASP Top Ten (2021)** under security misconfigurations and often manifests in improperly validated URL parameters or redirects.

---

## Table of Contents
1. [What is an Open Redirect Vulnerability?](#what-is-an-open-redirect-vulnerability)
2. [How It Works](#how-it-works)
3. [Types of Open Redirect Vulnerabilities](#types-of-open-redirect-vulnerabilities)
4. [How to Hunt Open Redirect Vulnerabilities](#how-to-hunt-open-redirect-vulnerabilities)
5. [Best Wordlists for Parameters](#best-wordlists-for-parameters)
6. [Tools for Hunting Open Redirects](#tools-for-hunting-open-redirects)
7. [Examples of Exploits](#examples-of-exploits)
8. [Resources](#resources)
   - [Write-ups](#write-ups)
   - [Labs and Challenges](#labs-and-challenges)

---

## What is an Open Redirect Vulnerability?

An **Open Redirect** vulnerability allows attackers to craft malicious URLs that, when clicked by a victim, redirect them to an unintended website. This flaw is typically exploited in phishing campaigns and social engineering attacks. It occurs due to improper validation of user-supplied input in parameters that control redirection logic.

---

## How It Works

1. **Normal Use Case:**
   A legitimate web application may redirect users after login or logout using URL parameters, such as:
   ```
   https://example.com/redirect?url=https://trustedsite.com
   ```

2. **Exploitation:**
   If the `url` parameter is not validated or sanitized, an attacker can modify the URL to point to a malicious site:
   ```
   https://example.com/redirect?url=https://malicious.com
   ```

3. **Impact:**
   - **Phishing:** Redirecting users to malicious websites mimicking legitimate ones.
   - **Session Hijacking:** Redirecting to exploit vulnerable endpoints.
   - **Data Theft:** Users may input sensitive information like credentials.

---

## Types of Open Redirect Vulnerabilities

1. **URL-based Redirects**
   - Parameters like `url`, `next`, `redirect`, or `return` are manipulated to achieve redirection.

2. **Header-based Redirects**
   - The `Location` header in HTTP responses is dynamically set based on user input, enabling abuse.

3. **JavaScript-based Redirects**
   - Use of `window.location` or `document.location` to process unvalidated user input:
     ```javascript
     window.location.href = userInput;
     ```

4. **Meta Tag Redirects**
   - Exploitation of dynamically generated `<meta>` tags for redirection:
     ```html
     <meta http-equiv="refresh" content="0; url=userInput">
     ```

---

## How to Hunt Open Redirect Vulnerabilities

### 1. **Identify Redirect Parameters**
   - Look for parameters named `url`, `next`, `redirect`, `goto`, `return`, or similar in the HTTP requests.

### 2. **Inspect Redirect Behavior**
   - Analyze the application's behavior when modifying these parameters.

### 3. **Test Payloads**
   - Try using common payloads to check for improper validation:
     ```
     /redirect?url=https://evil.com
     /redirect?next=//evil.com
     /redirect?goto=/../../evil.com
     /redirect?next=http://127.0.0.1
     ```

### 4. **Check for Partial Redirects**
   - Test if partial inputs (e.g., `/redirect?next=evil.com`) are expanded into a full redirect.

### 5. **Automate Using Tools**
   - Use tools like **ffuf**, **dirsearch**, and **ParamSpider** to find hidden parameters and endpoints.

### 6. **Use Wordlists**
   - Test with comprehensive wordlists (examples provided below).

---

## Best Wordlists for Parameters

1. **SecLists**
   - URL: [https://github.com/danielmiessler/SecLists](https://github.com/danielmiessler/SecLists)
   - Use the `Fuzzing/parameters.txt` or `Discovery/Web-Content/common-params.txt`.

2. **ParamMiner**
   - Generate dynamic parameter wordlists for custom web applications.

3. **Custom Open Redirect Wordlists**
   ```
   url, next, redirect, return, dest, destination, continue, forward, target
   ```

---

## Tools for Hunting Open Redirects

### 1. **Burp Suite**
   - Use the **Intruder** module to fuzz redirect parameters.
   - Add extensions like **ParamMiner** or **Logger++** for better analysis.

### 2. **ffuf (Fast Web Fuzzer)**
   - Usage:
     ```bash
     ffuf -u https://example.com/redirect?url=FUZZ -w wordlist.txt
     ```

### 3. **ParamSpider**
   - Discover parameters on target domains.
     ```bash
     python3 paramspider.py --domain example.com
     ```

### 4. **HTTPX**
   - Quickly identify redirection behavior:
     ```bash
     echo "https://example.com/redirect?url=evil.com" | httpx -silent
     ```

### 5. **Wayback URLs**
   - Extract historical parameters and endpoints:
     ```bash
     waybackurls example.com | grep redirect
     ```

---

## Examples of Exploits

### HTTP Request Example:
```http
GET /redirect?url=https://malicious.com HTTP/1.1
Host: example.com
```

### Vulnerable Code Example (PHP):
```php
<?php
$redirect_url = $_GET['url'];
header("Location: " . $redirect_url);
?>
```

### Payloads for Testing:
1. `//evil.com`
2. `http://127.0.0.1`
3. `https://trusted.com@evil.com`
4. `/../../../../evil.com`

---

## Resources

### Write-ups
- [Bugcrowd Open Redirect Guide](https://www.bugcrowd.com/blog/open-redirect/)
- [HackerOne Example: Open Redirect Exploit](https://www.hackerone.com/)

### Labs and Challenges
- [PortSwigger Open Redirect Lab](https://portswigger.net/web-security/open-redirect)
- [HackTheBox Academy: Open Redirect](https://academy.hackthebox.com/)
- [OWASP Juice Shop Challenge](https://owasp.org/www-project-juice-shop/)

---

## Conclusion

Open Redirect vulnerabilities might seem trivial, but they are a gateway to serious attacks like phishing and credential theft. By following this guide, you can systematically discover and report these vulnerabilities in web applications. Happy hunting!

---
