# **Nmap Scripting Engine (NSE) and Lua Guide**

## **Table of Contents**

1. [What is Nmap NSE?](#what-is-nmap-nse)
2. [NSE Built-in Scripts](#nse-built-in-scripts)
3. [How NSE Works](#how-nse-works)
4. [Lua Programming Essentials](#lua-programming-essentials)
   - [Variables](#variables)
   - [Conditional Statements](#conditional-statements)
   - [Loops](#loops)
   - [Functions](#functions)
   - [Tables (Arrays and Dictionaries)](#tables-arrays-and-dictionaries)
   - [Error Handling](#error-handling)
5. [Writing NSE Scripts](#writing-nse-scripts)
   - [NSE Script Structure](#nse-script-structure)
   - [NSE Library Functions](#nse-library-functions)
   - [Using NSE Categories](#using-nse-categories)
   - [Interacting with Nmap](#interacting-with-nmap)
6. [Examples of NSE Scripts](#examples-of-nse-scripts)
7. [Debugging NSE Scripts](#debugging-nse-scripts)
8. [Resources and Further Reading](#resources-and-further-reading)

---

## **What is Nmap NSE?**

Nmap's Scripting Engine (NSE) extends the capabilities of Nmap by allowing users to write and run custom scripts for:

- Vulnerability detection
- Exploitation
- Information gathering
- Service enumeration

NSE scripts are written in the lightweight Lua programming language and use Nmap's rich set of libraries for networking, protocol handling, and utility functions.

---  
Below is an updated and expanded section on **NSE built-in scripts** and their uses. These scripts, included with Nmap, provide robust functionality across a variety of categories. 

---

## **NSE Built-in Scripts**

The Nmap Scripting Engine (NSE) ships with a large number of built-in scripts that cover everything from basic service detection to advanced vulnerability assessment and exploitation. These scripts are organized into categories for ease of use and are invoked using the `--script` option.

### **Script Categories and Their Uses**

#### 1. **`auth`**
Scripts in this category are used for authentication purposes, such as brute-forcing credentials or testing authentication mechanisms.

- **Example Scripts**:
  - `ftp-anon`: Checks for anonymous FTP access.
  - `ssh-brute`: Attempts to brute-force SSH credentials.
  - `http-auth`: Tests HTTP authentication mechanisms.

- **Usage Example**:
  ```bash
  nmap --script ftp-anon <target>
  ```

#### 2. **`broadcast`**
Scripts in this category perform discovery by broadcasting queries over the local network.

- **Example Scripts**:
  - `broadcast-dhcp-discover`: Detects DHCP servers on the network.
  - `broadcast-ping`: Sends broadcast pings to identify hosts.

- **Usage Example**:
  ```bash
  nmap --script broadcast-dhcp-discover
  ```

#### 3. **`default`**
Scripts in the `default` category are safe and useful for general scanning. They run automatically with the `-sC` option.

- **Example Scripts**:
  - `ssl-cert`: Fetches and displays SSL/TLS certificates.
  - `http-title`: Displays the title of web pages.
  - `ssh-hostkey`: Fetches SSH host keys.

- **Usage Example**:
  ```bash
  nmap -sC <target>
  ```

#### 4. **`discovery`**
Discovery scripts gather information about hosts, services, and networks.

- **Example Scripts**:
  - `dns-brute`: Performs DNS brute-forcing.
  - `snmp-brute`: Attempts to brute-force SNMP community strings.
  - `http-enum`: Enumerates files and directories on HTTP servers.

- **Usage Example**:
  ```bash
  nmap --script dns-brute <target>
  ```

#### 5. **`exploit`**
Exploit scripts attempt to exploit known vulnerabilities to gain access or execute code.

- **Example Scripts**:
  - `ms08-067`: Exploits the MS08-067 vulnerability in Windows.
  - `samba-vuln-cve-2012-1182`: Checks for a remote code execution vulnerability in Samba.

- **Usage Example**:
  ```bash
  nmap --script ms08-067 <target>
  ```

#### 6. **`external`**
These scripts interact with external resources to gather information about the target.

- **Example Scripts**:
  - `http-google-malware`: Checks Google Safe Browsing for malware status of URLs.
  - `ip-geolocation-geoplugin`: Retrieves IP geolocation information using the GeoPlugin API.

- **Usage Example**:
  ```bash
  nmap --script ip-geolocation-geoplugin <target>
  ```

#### 7. **`fuzzer`**
Fuzzing scripts are used to send a range of inputs to a service to identify unexpected behavior or crashes.

- **Example Scripts**:
  - `http-fuzz`: Fuzzes web servers for common vulnerabilities.
  - `ftp-fuzz`: Fuzzes FTP servers for vulnerabilities.

- **Usage Example**:
  ```bash
  nmap --script http-fuzz <target>
  ```

#### 8. **`intrusive`**
These scripts may cause harm to the target system (e.g., crashes or disruptions). Use with caution.

- **Example Scripts**:
  - `http-sql-injection`: Checks for SQL injection vulnerabilities.
  - `smtp-brute`: Attempts to brute-force SMTP credentials.

- **Usage Example**:
  ```bash
  nmap --script smtp-brute <target>
  ```

#### 9. **`malware`**
Scripts in this category identify malware infections or behavior.

- **Example Scripts**:
  - `http-malware-host`: Checks if a host serves malware.
  - `irc-malware-check`: Detects malware-related IRC servers.

- **Usage Example**:
  ```bash
  nmap --script http-malware-host <target>
  ```

#### 10. **`safe`**
Safe scripts are non-intrusive and unlikely to cause any harm to the target.

- **Example Scripts**:
  - `whois-domain`: Fetches WHOIS information for a domain.
  - `traceroute-geolocation`: Traces the route to a host and provides geolocation data.

- **Usage Example**:
  ```bash
  nmap --script whois-domain <target>
  ```

#### 11. **`vuln`**
Vulnerability detection scripts look for specific vulnerabilities and report them.

- **Example Scripts**:
  - `ssl-poodle`: Detects SSL/TLS POODLE vulnerabilities.
  - `http-cve-2021-44228-log4shell`: Detects Log4Shell vulnerability (CVE-2021-44228).

- **Usage Example**:
  ```bash
  nmap --script ssl-poodle <target>
  ```

---

### **Common NSE Commands**

- **Run all scripts in a category**:
  ```bash
  nmap --script <category> <target>
  ```
  Example:
  ```bash
  nmap --script vuln <target>
  ```

- **Run multiple specific scripts**:
  ```bash
  nmap --script <script1>,<script2> <target>
  ```
  Example:
  ```bash
  nmap --script http-title,ssl-cert <target>
  ```

- **Run scripts with arguments**:
  ```bash
  nmap --script <script-name> --script-args <key=value>
  ```
  Example:
  ```bash
  nmap --script http-brute --script-args userdb=users.txt,passdb=passwords.txt <target>
  ```

---

### **Notable NSE Scripts and Their Use Cases**

| **Script Name**       | **Category**   | **Description**                                                                 |
|------------------------|----------------|---------------------------------------------------------------------------------|
| `ssl-enum-ciphers`     | vuln           | Enumerates SSL/TLS ciphers and checks for vulnerabilities.                     |
| `ftp-anon`             | auth           | Checks if anonymous FTP login is allowed.                                      |
| `smb-os-discovery`     | discovery      | Detects the operating system via SMB.                                          |
| `http-title`           | default        | Fetches and displays the title of HTTP services.                               |
| `dns-zone-transfer`    | discovery      | Attempts DNS zone transfers.                                                   |
| `ms17-010`             | vuln           | Checks for the MS17-010 SMB vulnerability (EternalBlue).                       |
| `rdp-enum-encryption`  | discovery      | Enumerates supported encryption on RDP servers.                                |
| `http-xssed`           | vuln           | Checks for cross-site scripting (XSS) vulnerabilities using XSSed.com.         |

---

### **Tips for Using NSE Scripts**

1. **Start with Default Scripts**:
   Default scripts provide a safe starting point for scanning:
   ```bash
   nmap -sC <target>
   ```

2. **Combine Categories for Depth**:
   Use multiple categories to gain a broad view:
   ```bash
   nmap --script "default,vuln" <target>
   ```

3. **Leverage Arguments**:
   Many scripts require specific arguments to operate effectively. Check documentation using:
   ```bash
   nmap --script-help <script-name>
   ```

4. **Be Cautious with Intrusive Scripts**:
   Avoid running intrusive scripts against production systems unless explicitly permitted.

5. **Update NSE Scripts Regularly**:
   Ensure your Nmap installation and scripts are up-to-date:
   ```bash
   nmap --script-updatedb
   ```

---  
## **How NSE Works**

1. **Phases of Execution**:
   - **Host Scripts**: Run once per host.
   - **Port Scripts**: Run on each port detected on a host.

2. **Script Categories**:
   - `auth`: Authentication-related scripts.
   - `default`: Scripts run with the `-sC` flag.
   - `discovery`: General information gathering.
   - `exploit`: Exploits vulnerabilities.
   - `intrusive`: May impact the target significantly.
   - `safe`: Low impact, safe for production scans.

3. **Execution Command**:
   Run NSE scripts with:
   ```bash
   nmap --script <script-name> <target>
   ```

---

## **Lua Programming Essentials**

Lua is simple yet powerful. Below is a crash course on Lua's syntax and features for NSE scripting.

### **Variables**
```lua
-- Declare a variable
local my_var = 10  -- Local variable
global_var = "Global scope"  -- Global variable
```

### **Conditional Statements**
```lua
if condition then
    -- Code
elseif another_condition then
    -- Code
else
    -- Code
end
```

### **Loops**
```lua
-- While loop
local i = 1
while i <= 5 do
    print("Number: ", i)
    i = i + 1
end

-- For loop
for i = 1, 5 do
    print("Count: ", i)
end

-- Iterating over a table
local fruits = {"apple", "banana", "cherry"}
for index, fruit in ipairs(fruits) do
    print(index, fruit)
end
```

### **Functions**
```lua
-- Function definition
local function greet(name)
    return "Hello, " .. name
end

-- Calling the function
print(greet("World"))
```

### **Tables (Arrays and Dictionaries)**
Tables in Lua act as both arrays and dictionaries.

```lua
-- Array
local array = {1, 2, 3, 4}
print(array[1])  -- Access first element

-- Dictionary
local dict = {key1 = "value1", key2 = "value2"}
print(dict.key1)

-- Mixed table
local mixed = {10, "Lua", key1 = "value1"}
```

### **Error Handling**
```lua
local success, error = pcall(function()
    -- Risky code
end)
if not success then
    print("Error: " .. error)
end
```

---

## **Writing NSE Scripts**

### **NSE Script Structure**

NSE scripts follow a well-defined structure:
```lua
-- Script metadata
description = [[
    This script checks for a specific vulnerability.
]]
categories = {"vuln", "default"}

-- Import libraries
local nmap = require "nmap"
local stdnse = require "stdnse"

-- Define action function
action = function(host, port)
    -- Code for the script
    return "Result: Vulnerability found!"
end
```

### **NSE Library Functions**

1. **`nmap`**: Core Nmap utilities.
   ```lua
   local start_time = nmap.clock()
   local targets = nmap.registry.targets
   ```

2. **`stdnse`**: Helper functions for debugging and logging.
   ```lua
   stdnse.print_debug(1, "Debugging message")
   ```

3. **`socket`**: Networking support.
   ```lua
   local socket = nmap.new_socket()
   socket:connect(host.ip, port.number)
   ```

4. **`shortport`**: For port matching.
   ```lua
   local port_rule = shortport.port_or_service({80, 443}, "http")
   ```

### **Using NSE Categories**

Assign categories to scripts using:
```lua
categories = {"default", "safe"}
```

### **Interacting with Nmap**

Access host and port data:
```lua
local ip = host.ip
local port_number = port.number
```

---

## **Examples of NSE Scripts**

### Example 1: Simple Port Scanner
```lua
description = "Detect open ports"
categories = {"default", "safe"}

action = function(host)
    local open_ports = {}
    for port, status in pairs(host.ports) do
        if status.state == "open" then
            table.insert(open_ports, port.number)
        end
    end
    return "Open Ports: " .. table.concat(open_ports, ", ")
end
```

### Example 2: Banner Grabbing Script
```lua
description = "Banner grabbing on specific ports"
categories = {"discovery"}

local socket = require "socket"

action = function(host, port)
    local conn = nmap.new_socket()
    conn:connect(host.ip, port.number)
    conn:send("GET / HTTP/1.1\r\n\r\n")
    local response = conn:receive()
    conn:close()
    return response
end
```

---

## **Debugging NSE Scripts**

1. Use `stdnse.print_debug()` for debugging messages.
2. Run Nmap with verbosity:
   ```bash
   nmap --script <script-name> -d <target>
   ```
3. Check Nmap logs for errors or unexpected behavior.

---

## **Resources and Further Reading**

1. [Nmap Scripting Engine Documentation](https://nmap.org/book/nse.html)
2. [Lua Programming Guide](https://www.lua.org/manual/)
3. [Nmap GitHub Repository](https://github.com/nmap/nmap)


---  


**Follow Me On X [Link Here](https://x.com/n4itr0_07)**
