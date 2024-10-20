# **Bug Bounty Learning Plan**

## Introduction

*This 6-month learning plan is designed for anyone who wants to learn bug bounty hunting from scratch and aims to make money by finding vulnerabilities. The plan focuses on building the necessary technical knowledge, hands-on practice, and persistence needed to achieve your first bug.*

---

## Phase 1: Fundamentals (1-2 Months)

### Week 1-4: Networking and Web Basics

**Objective**: Understand networking, HTTP, and how the web works.

### Topics:
- Learn TCP/IP, DNS, and HTTP/HTTPS.
- Understand how web applications work (client-server model, cookies, sessions).

### Tools:
- Wireshark
- Postman
- cURL

### Resources:
- [Computer Networking - Coursera](https://www.coursera.org/courses?query=computer%20networking)
- [The Web Developer Bootcamp - Udemy](https://www.udemy.com/course/the-web-developer-bootcamp/)
- [Mozilla HTTP Documentation](https://developer.mozilla.org/en-US/docs/Web/HTTP)

---

### Week 5-8: Programming Basics & Scripting

**Objective**: Learn basic programming and scripting needed for bug hunting.

### Languages:
- HTML, CSS (basic structure of web apps).
- JavaScript (for XSS, DOM manipulation).
- Python (useful for scripting and automating tasks).

### Topics:
- Learn basic syntax, loops, conditionals, and functions.
- Understand web forms, inputs, cookies, and session management.
- Create basic scripts for automating simple tasks.

### Resources:
- [Codecademy Web Development](https://www.codecademy.com/catalog/subject/web-development)
- [FreeCodeCamp JavaScript](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/)
- [Automate the Boring Stuff with Python](https://automatetheboringstuff.com/)

---

## Phase 2: Learn Security Concepts (1 Month)

### Week 9-12: Web Vulnerabilities (OWASP Top 10)

**Objective**: Familiarize yourself with the most common web vulnerabilities.

### Topics:
- Study the OWASP Top 10: SQL Injection, XSS, CSRF, SSRF, IDOR, etc.
- Understand how these vulnerabilities are exploited in real-world scenarios.

### Practical Work:
- Set up a virtual lab using DVWA (Damn Vulnerable Web App), BWAPP, or OWASP Juice Shop to practice these vulnerabilities.

### Resources:
- [OWASP Official Website](https://owasp.org/www-project-top-ten/)
- [PortSwigger Web Security Academy](https://portswigger.net/web-security)
- [HackTheBox](https://www.hackthebox.com/)

---

## Phase 3: Reconnaissance & Enumeration (1 Month)

### Week 13-16: Reconnaissance and Information Gathering

**Objective**: Learn how to gather information about a target before testing.

### Topics:
- Subdomain enumeration, port scanning, directory brute-forcing.
- Passive recon using tools like crt.sh, SecurityTrails, and Wayback Machine.

### Tools:
- Sublist3r
- Amass
- nmap
- ffuf
- Shodan

### Practical Work:
- Choose a bug bounty program (e.g., HackerOne) and practice recon on targets.

### Resources:
- [Bugcrowd University - Reconnaissance](https://www.bugcrowd.com/university/recon/)
- [SecurityTrails - Passive Recon](https://securitytrails.com/)
- [YouTube Recon Tutorials](https://www.youtube.com/results?search_query=bug+bounty+recon)

---

## Phase 4: Vulnerability Discovery (1-2 Months)

### Week 17-22: Hunting Common Bugs

**Objective**: Start actively testing and looking for common vulnerabilities.

### Topics:
- Injection Attacks: Test for SQLi and command injections.
- XSS: Focus on input fields, search boxes, and parameter tampering.
- IDOR: Look for broken access control in web apps.

### Practical Work:
- Use Burp Suite or OWASP ZAP to intercept and modify requests.
- Explore vulnerable applications like Juice Shop or participate in Capture the Flag (CTF) challenges.

### Resources:
- [Burp Suite by PortSwigger](https://portswigger.net/burp)
- [OWASP ZAP](https://www.zaproxy.org/)
- [TryHackMe: CTF Challenges](https://tryhackme.com/)

---

## Phase 5: Bug Bounty Hunting (1 Month+)

### Week 23-24: Start Small

**Objective**: Now that you have the basic skills, start hunting.

### Activities:
- Pick low-hanging fruits such as XSS, IDOR, or exposed admin panels.
- Automate recon with tools like Subfinder, Aquatone, and ffuf.

### Practice:
- Spend 2-3 hours daily hunting on platforms like HackerOne or Bugcrowd.

### Resources:
- [Subfinder](https://github.com/projectdiscovery/subfinder)
- [Aquatone](https://github.com/michenriksen/aquatone)
- [HackerOne](https://www.hackerone.com/)
- [Bugcrowd](https://www.bugcrowd.com/)

---

### Week 25-26: Report Your First Bug

**Objective**: After finding a vulnerability, submit a report.

### Steps:
- Create a Proof of Concept (PoC) with proper screenshots.
- Write a detailed step-by-step report.
- If the bug gets rejected, learn from it and improve your approach.

### Resources:
- [How to Write a Good Bug Report - HackerOne](https://docs.hackerone.com/researchers/how-to-write-a-good-bug-report.html)
- [Bugcrowd Reporting Guidelines](https://www.bugcrowd.com/resource/bugcrowd-reporting-guidelines/)

---

## Weekly Timetable (Sample)

### Monday to Friday:
- **1-2 hours theory/study**: Learning about web vulnerabilities or network basics.
- **1-2 hours hands-on practice**: Recon, fuzzing, and testing for bugs on targets.

### Saturday-Sunday:
- **Full-day practice**: Set up a lab or test programs on bug bounty platforms.
- **Study write-ups**: Read reports and watch CTF challenges on YouTube.

---

## Summary

By following this plan:

- **1st-2nd month**: Focus on learning networking, web basics, and programming.
- **3rd month**: Dive into web security concepts, focusing on OWASP Top 10.
- **4th month**: Master recon and information gathering tools.
- **5th month**: Actively start testing for bugs on real-world targets.
- **6th month**: Start reporting bugs, aiming to find and report your first vulnerability.

---

## Useful Tools & Platforms

- [Wireshark](https://www.wireshark.org/)
- [Postman](https://www.postman.com/)
- [nmap](https://nmap.org/)
- [ffuf](https://github.com/ffuf/ffuf)
- [Shodan](https://www.shodan.io/)
- [HackerOne](https://www.hackerone.com/)
- [Bugcrowd](https://www.bugcrowd.com/)

---

This roadmap provides a structured learning path to help you achieve your first bug bounty within 6 months. Dedication and consistency are key!

---  
