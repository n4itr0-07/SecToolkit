# MySQL Injection Documentation

## Overview

**SQL Injection (SQLi)** is a critical web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. This can lead to data leaks, unauthorized access, data modification, and in severe cases, remote code execution. MySQL Injection specifically targets MySQL databases and can exploit vulnerable query handling in applications.

## Types of SQL Injection

SQL Injection attacks vary in technique and impact. Here are the common types:

1. **In-band SQL Injection**  
   - Directly leverages the application's response to gather information.
   - Types:
     - **Error-Based SQL Injection**: Uses database error messages to gain insights into database structure.
     - **Union-Based SQL Injection**: Combines multiple queries to extract data in a single response.

2. **Inferential (Blind) SQL Injection**  
   - Does not rely on database errors or returned data but uses inference to determine success.
   - Types:
     - **Boolean-Based Blind SQL Injection**: Sends queries that evaluate to true or false to extract data bit-by-bit.
     - **Time-Based Blind SQL Injection**: Uses delays in database response time to infer data.

3. **Out-of-Band SQL Injection**  
   - Utilizes external resources (like HTTP requests or DNS lookups) to receive results, often when direct response isn’t possible.

## Performing SQL Injection Attacks

> **Disclaimer**: This information is intended for educational and authorized security testing only. Unauthorized access is illegal and unethical.

### Step 1: Target Selection

1. Identify **potentially vulnerable applications**. Targets typically have input fields, parameters, or other data points that interact with the database.
2. Look for **user-submitted input** areas: URL parameters, forms, headers, or cookies.
3. Check for **dynamic web applications** as they are more likely to contain SQL queries built with user input.

### Step 2: Initial Testing for SQL Injection

1. Start with basic SQL Injection payloads:
   ```sql
   ' OR '1'='1
   ' OR '1'='1' --
   ```

2. Observe the behavior:
   - **Error messages**: Can indicate vulnerable fields.
   - **Behavioral changes**: For instance, using `AND 1=1` or `AND 1=2` to see differences in page response.

3. Use **boolean or time-based payloads** for blind SQLi:
   ```sql
   ' AND IF(1=1, SLEEP(5), 0) --
   ```

### Step 3: Escalation Techniques

1. **Union Select**: Used to combine query results.
   ```sql
   ' UNION SELECT 1,2,3... --
   ```
   - Determine column count using `ORDER BY` or `UNION SELECT NULL`.

2. **Extract Database Information**:
   - To get the database name:
     ```sql
     ' UNION SELECT database(), NULL, NULL...
     ```

3. **Enumerate Tables and Columns**:
   ```sql
   ' UNION SELECT table_name FROM information_schema.tables WHERE table_schema=database() --
   ```

4. **Extract Sensitive Data**:
   - Extract user credentials or other sensitive information by querying known tables and columns.

### Step 4: Post-Exploitation

Once SQLi is confirmed and data extraction is successful, consider additional steps based on your engagement’s scope:
- Data exfiltration (e.g., downloading entire tables).
- Webshell installation (advanced and often illegal without explicit permission).
- Privilege escalation within the database.

## Tools for SQL Injection

Here’s a list of widely used SQLi tools for testing MySQL Injection:

1. **SQLmap**
   - **Description**: Automates SQL Injection discovery and exploitation.
   - **Usage**: `sqlmap -u "http://example.com/page?id=1" --dbs`
   - **Link**: [SQLmap GitHub](https://github.com/sqlmapproject/sqlmap)

2. **Burp Suite**
   - **Description**: Provides a full suite of tools for web app security, including automated SQL Injection scanning.
   - **Link**: [Burp Suite](https://portswigger.net/burp)

3. **Havij**
   - **Description**: An automated SQL Injection tool focused on ease of use.
   - **Link**: [Havij](https://www.itsecteam.com/products/havij-advanced-sql-injection/)

4. **jSQL Injection**
   - **Description**: Java-based tool that supports MySQL and various SQLi methods.
   - **Link**: [jSQL GitHub](https://github.com/ron190/jsql-injection)

5. **NoSQLMap**
   - **Description**: Focuses on exploiting NoSQL injections, especially in MongoDB.
   - **Link**: [NoSQLMap GitHub](https://github.com/codingo/NoSQLMap)

## Best Practices for SQL Injection Mitigation

Developers can secure their applications against SQL Injection by adopting the following practices:

- **Parameterized Queries (Prepared Statements)**: Avoid building SQL queries by concatenating strings. Use parameterized queries to handle user input.
- **ORM (Object-Relational Mapping)**: ORM frameworks help abstract SQL and prevent direct query construction.
- **Input Validation**: Enforce strict validation on user inputs, particularly in numeric or structured fields.
- **Use Web Application Firewalls (WAF)**: WAFs can block common SQL Injection payloads and patterns.
- **Regular Security Audits**: Regularly audit and scan applications for SQL Injection vulnerabilities.

## Useful Resources

- **OWASP SQL Injection**: [OWASP Guide](https://owasp.org/www-community/attacks/SQL_Injection)
- **PentesterLab - SQL Injection**: [PentesterLab](https://pentesterlab.com/exercises/sql_injection)
- **PayloadsAllTheThings (GitHub)**: [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)

---

**Note**: This README is for educational purposes in controlled environments. Unauthorized exploitation of SQLi is illegal and punishable by law. Always obtain permission before testing.