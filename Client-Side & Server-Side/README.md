# **Client-Side & Server-Side Vulnerabilities – Pentesting & Bug Bounty**  
**Tags:** `#Pentesting` `#BugBounty` `#WebSecurity` `#ClientSide` `#ServerSide`  
**Date:** `[[2025-02-14]]`  

---

## **1. Introduction**  
In **penetration testing & bug bounty**, vulnerabilities are categorized into **client-side** and **server-side** issues.  

- **Client-Side Vulnerabilities** → Affect the browser and user interaction.  
- **Server-Side Vulnerabilities** → Affect the backend logic, databases, and server-side processing.  

Both can lead to **account takeover, data breaches, privilege escalation, and business logic bypasses**.  

---

## **2. Vulnerabilities - Tree Structure**  

```plaintext
Vulnerabilities
├── Client-Side Vulnerabilities
│   ├── Cross-Site Scripting (XSS)
│   │   ├── Stored XSS
│   │   ├── Reflected XSS
│   │   ├── DOM-Based XSS
│   ├── Cross-Site Request Forgery (CSRF)
│   │   ├── Token-Based CSRF
│   │   ├── SameSite Bypass CSRF
│   ├── Clickjacking
│   ├── HTML Injection
│   ├── JavaScript Prototype Pollution
│   ├── WebSockets Security Issues
│   │   ├── WebSocket Hijacking (CSWSH)
│   │   ├── WebSocket API Tampering
│   ├── Local Storage & Session Storage Attacks
│   │   ├── Sensitive Data Exposure
│   │   ├── Token Theft via XSS
│   ├── CORS Misconfiguration
│   ├── Open Redirects
│   ├── Cache Poisoning Attacks
│   ├── CSS Injection
│   ├── UI Redressing Attacks
│   │   ├── Clickjacking
│   │   ├── Cursorjacking
│   ├── WebAssembly Security Issues
│   ├── JavaScript Logic Manipulation
│   ├── Source Code Disclosure
│   │   ├── Exposed `.git` Directories
│   │   ├── API Key Leakage in JavaScript Files
│   ├── Browser Extensions Security Issues
│   │   ├── Extension Privilege Escalation
│   │   ├── Extension API Abuse
│   ├── DNS Rebinding
│   ├── Cross-Origin Data Leakage
│   ├── JavaScript Execution via JSONP Abuse
│   ├── Client-Side Path Traversal
│   ├── Shadow DOM Injection
│   ├── WebRTC IP Leak
│   ├── HTML5 API Exploitation
│   │   ├── Geolocation Hijacking
│   │   ├── File API Abuse
│   │   ├── Web Workers Injection
│   ├── Content Security Policy (CSP) Bypass
│   ├── JavaScript Injection via Third-Party Libraries
│   ├── Trusted Types Policy Bypass
│   ├── JavaScript Heap Exploitation
│   ├── Side-Channel Attacks
│   │   ├── Pixel Stealing
│   │   ├── Timing Attacks
│   ├── Malicious JavaScript Execution via Polyglot Attacks
│   ├── Subresource Integrity (SRI) Bypass
│   ├── MIME-Type Sniffing Attack
│   ├── Referrer Policy Abuse
│   ├── DOM Clobbering
│   ├── Cross-Site Script Inclusion (XSSI)
│   ├── Service Worker Hijacking
│   ├── CSP Misconfiguration Exploitation
│   ├── Offline Storage Data Theft
│   ├── WebGL Exploits
│   ├── Session Puzzling
│   ├── JavaScript URL Scheme Abuse
│   ├── Attack via JavaScript Debugging APIs
│
├── Server-Side Vulnerabilities
│   ├── SQL Injection (SQLi)
│   │   ├── Error-Based SQLi
│   │   ├── Blind SQLi
│   │   ├── Time-Based SQLi
│   │   ├── Union-Based SQLi
│   │   ├── Second-Order SQLi
│   ├── NoSQL Injection
│   ├── Server-Side Request Forgery (SSRF)
│   │   ├── Internal Network Access SSRF
│   │   ├── AWS Metadata Exploitation
│   ├── Insecure Deserialization
│   │   ├── PHP Object Injection
│   │   ├── Java Deserialization Exploit
│   │   ├── Python Pickle Exploits
│   ├── Remote Code Execution (RCE)
│   │   ├── Command Injection
│   │   ├── Template Injection (SSTI)
│   │   ├── Deserialization RCE
│   ├── Race Condition Attacks
│   │   ├── TOCTOU (Time-of-Check Time-of-Use)
│   │   ├── Multi-Threaded Race Condition Exploits
│   ├── Privilege Escalation
│   │   ├── Role-Based Access Control Bypass
│   │   ├── Token Manipulation
│   ├── Directory Traversal
│   ├── Broken Authentication
│   │   ├── Token Hijacking
│   │   ├── Credential Stuffing
│   │   ├── Weak Session Management
│   ├── XML External Entity (XXE)
│   │   ├── Local File Read via XML
│   │   ├── Out-of-Band XXE Exploitation
│   ├── Server-Side Template Injection (SSTI)
│   ├── Memory Corruption Attacks
│   ├── Buffer Overflow
│   ├── API Security Vulnerabilities
│   │   ├── Broken Object-Level Authorization (BOLA)
│   │   ├── Excessive Data Exposure
│   │   ├── Mass Assignment
│   ├── Webhook Exploitation
│   ├── WebSockets Server-Side Attacks
│   ├── Cache Poisoning
│   ├── Log Injection
│   ├── Unrestricted File Upload
│   │   ├── Web Shell Injection
│   │   ├── MIME Type Bypass
│   ├── Path Normalization Bypass
│   ├── GraphQL Security Issues
│   │   ├── Introspection Abuse
│   │   ├── Batch Query Abuse
│   ├── JWT Security Issues
│   │   ├── None Algorithm Exploit
│   │   ├── Weak JWT Secret Exploitation
│   ├── Information Disclosure
│   │   ├── Stack Trace Leakage
│   │   ├── Debug Mode Exposure
│   ├── Denial-of-Service (DoS) Attacks
│   │   ├── Regular Expression (ReDoS)
│   │   ├── Slowloris Attack
│   ├── Server Misconfigurations
│   │   ├── Exposed `.env` Files
│   │   ├── Misconfigured Headers
│   ├── Cloud Security Issues
│   │   ├── S3 Bucket Misconfiguration
│   │   ├── IAM Role Abuse
│
```

---

## **3. Summary**
✅ **Client-Side Vulnerabilities** → Exploit **JavaScript, DOM, WebSockets, HTML5, and CORS issues**.  
✅ **Server-Side Vulnerabilities** → Target **database injections, RCE, API flaws, authentication issues, and privilege escalation**.  
✅ **Pentesters & Bug Bounty Hunters should focus on both** for comprehensive security testing.  

---

