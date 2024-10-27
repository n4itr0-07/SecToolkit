# Port Guide Reference

## 📖 Overview

In networking, **ports** are critical for defining specific entry points in systems, allowing data to flow between devices and applications. Each port number is unique to a particular service or application, ensuring data reaches its intended target. Every internet-connected device has **65,536 ports** (divided between TCP and UDP), each serving a specific purpose. 

This repository serves as a detailed guide to ports, ranges, and common assignments to help network administrators, developers, and security professionals in understanding port roles and functions.

---

## 📌 Table of Contents

1. [What is a Port?](#what-is-a-port)
2. [Port Ranges](#port-ranges)
3. [Detailed Port Table](#detailed-port-table)
4. [References and Resources](#references-and-resources)

---

## 🌐 What is a Port?

A **port** is an endpoint in network communication where data packets are directed to reach a service or application on a device. Ports enable multiple services to run on a single device by distinguishing traffic intended for different applications. Ports are divided into TCP and UDP categories, each with specific roles for communication.

---

## 🔍 Port Ranges

Ports are divided into three main categories:

- **Well-Known Ports (0-1023):** Commonly used ports reserved for standard services (e.g., HTTP, FTP).
- **Registered Ports (1024-49151):** Used by user applications and sometimes dynamically assigned.
- **Dynamic/Private Ports (49152-65535):** Commonly used for temporary connections in client-server models.

---

## 📊 Detailed Port Table

Below is an extensive list of common ports, their associated services, protocols, and descriptions:

| Port Number | Protocol | Service              | Description                                                                                              |
|-------------|----------|----------------------|----------------------------------------------------------------------------------------------------------|
| **20**      | TCP      | FTP (Data)           | File Transfer Protocol (data channel) for transferring files.                                            |
| **21**      | TCP      | FTP (Control)        | FTP control channel used to establish a connection.                                                      |
| **22**      | TCP      | SSH                  | Secure Shell, provides encrypted remote login and command execution.                                     |
| **23**      | TCP      | Telnet               | Unencrypted text communication protocol, generally insecure.                                             |
| **25**      | TCP      | SMTP                 | Simple Mail Transfer Protocol, used for sending email.                                                   |
| **53**      | TCP/UDP  | DNS                  | Domain Name System, resolves domain names to IP addresses.                                               |
| **67, 68**  | UDP      | DHCP                 | Dynamic Host Configuration Protocol, assigns IP addresses dynamically.                                   |
| **69**      | UDP      | TFTP                 | Trivial File Transfer Protocol, used for simple file transfers without authentication.                   |
| **80**      | TCP      | HTTP                 | Hypertext Transfer Protocol, used for web traffic.                                                       |
| **110**     | TCP      | POP3                 | Post Office Protocol 3, retrieves email from a mail server.                                              |
| **119**     | TCP      | NNTP                 | Network News Transfer Protocol, used for newsgroups.                                                     |
| **123**     | UDP      | NTP                  | Network Time Protocol, synchronizes time across devices.                                                 |
| **135**     | TCP/UDP  | MS RPC               | Microsoft RPC, used for various network services in Windows.                                             |
| **137-139** | TCP/UDP  | NetBIOS              | NetBIOS services for name resolution and file sharing in LAN environments.                               |
| **143**     | TCP      | IMAP                 | Internet Message Access Protocol, retrieves emails from the server.                                      |
| **161, 162**| UDP      | SNMP                 | Simple Network Management Protocol, monitors and manages network devices.                                |
| **389**     | TCP/UDP  | LDAP                 | Lightweight Directory Access Protocol, used for directory services like Active Directory.                |
| **443**     | TCP      | HTTPS                | HTTP over SSL/TLS, encrypted web communication.                                                          |
| **445**     | TCP      | SMB                  | Server Message Block, used for shared access to files and printers in Windows.                           |
| **465**     | TCP      | SMTPS                | Secure SMTP, email transfer over TLS.                                                                    |
| **500**     | UDP      | ISAKMP               | Internet Security Association and Key Management Protocol for VPNs and IPsec.                            |
| **514**     | UDP      | Syslog               | System logging protocol for collecting log messages from network devices.                                |
| **515**     | TCP      | LPR/LPD             | Line Printer Daemon, network printing protocol.                                                          |
| **520**     | UDP      | RIP                  | Routing Information Protocol, a routing protocol for small networks.                                     |
| **587**     | TCP      | SMTP (Secure)        | SMTP over TLS, often used for secure email submission.                                                   |
| **636**     | TCP      | LDAPS                | Secure LDAP over SSL/TLS.                                                                                |
| **993**     | TCP      | IMAPS                | Secure IMAP over SSL/TLS for secure email retrieval.                                                     |
| **995**     | TCP      | POP3S                | Secure POP3 over SSL/TLS for secure email retrieval.                                                     |
| **1080**    | TCP      | SOCKS Proxy          | SOCKS proxy protocol, allows network traffic to traverse firewalls.                                      |
| **1433**    | TCP      | MS SQL Server        | Microsoft SQL Server database communication.                                                             |
| **1521**    | TCP      | Oracle Database      | Oracle database communication.                                                                           |
| **1701**    | UDP      | L2TP                 | Layer 2 Tunneling Protocol, used in VPNs.                                                                |
| **1723**    | TCP/UDP  | PPTP                 | Point-to-Point Tunneling Protocol, used for VPN connections.                                             |
| **1812**    | UDP      | RADIUS               | Remote Authentication Dial-In User Service, for authentication and accounting in networks.               |
| **3306**    | TCP      | MySQL                | MySQL database communication.                                                                            |
| **3389**    | TCP/UDP  | RDP                  | Remote Desktop Protocol, for Windows remote management.                                                  |
| **5060**    | UDP      | SIP                  | Session Initiation Protocol, for voice and video calls (VoIP).                                           |
| **5432**    | TCP      | PostgreSQL           | PostgreSQL database communication.                                                                       |
| **5900**    | TCP      | VNC                  | Virtual Network Computing, for remote desktop access.                                                    |
| **6379**    | TCP      | Redis                | Redis database service.                                                                                  |
| **8080**    | TCP      | HTTP Alternate       | Alternative port for HTTP, often used for development or web servers.                                    |
| **8443**    | TCP      | HTTPS Alternate      | Alternative port for HTTPS, commonly used for secure web traffic in development environments.            |
| **10000**   | TCP      | Webmin               | Web-based system administration for UNIX systems.                                                        |
| **27017**   | TCP      | MongoDB              | MongoDB database service.                                                                                |

---

## 📘 References and Resources

For an exhaustive list of ports, consult the official [IANA Service Name and Transport Protocol Port Number Registry](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml).

---

### Contributions

We welcome contributions to expand this table or improve descriptions. If you have any additions, please submit a pull request.

---

## License

This project is licensed under the MIT License.
