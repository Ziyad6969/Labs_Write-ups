<div align="center">

# Unprotected Admin Functionality  
### Broken Access Control Vulnerability

[![Vulnerability](https://img.shields.io/badge/Vulnerability-Access%20Control-red)](https://owasp.org/www-project-top-ten/2017/A5_2017-Broken_Access_Control)
[![CWE](https://img.shields.io/badge/CWE-284-blue)](https://cwe.mitre.org/data/definitions/284.html)
[![Source](https://img.shields.io/badge/Source-PortSwigger-orange)](https://portswigger.net/web-security/learning-paths/server-side-vulnerabilities-apprentice/access-control-apprentice/access-control/lab-unprotected-admin-functionality#)

</div>

---

## Overview

This lab demonstrates an **Unprotected Admin Functionality** vulnerability, which is a form of  
[Broken Access Control](https://owasp.org/www-project-top-ten/2017/A5_2017-Broken_Access_Control).

The application exposes an administrative interface that lacks authentication and authorization checks, allowing any user to access privileged functionality.

---

## Application Behavior

The application provides standard functionality to unauthenticated users.  
In addition, it contains an administrative panel that allows privileged actions such as **deleting user accounts**.

The location of this admin panel is not linked from the main application interface, but it is disclosed through the application's `robots.txt` file.

---

## Discovery via `robots.txt`

The application exposes a publicly accessible `robots.txt` file:

```
GET /robots.txt HTTP/2
Host: <lab-id>.web-security-academy.net
```

### Server Response

```
User-agent: *
Disallow: /administrator-panel
```

Since `robots.txt` is a public file and does not provide any security controls, this reveals the existence and location of a sensitive administrative endpoint.

---

## Proof of Concept (PoC)

### Accessing the Admin Panel

By directly navigating to the disclosed endpoint:

```
GET /administrator-panel HTTP/2
Host: <lab-id>.web-security-academy.net
```

the server responds with the **administrative interface** without requiring any authentication.

This confirms that the admin functionality is **unprotected**.

---

### Exploitation

From the exposed admin panel, it is possible to perform privileged actions.  
The lab was solved by using the admin functionality to delete the user **`carlos`**, completing the lab objective.

---

## Impact

Successful exploitation of this vulnerability allows an attacker to:

- Access administrative interfaces without authentication
- Perform privileged actions such as deleting users
- Modify or destroy application data
- Fully compromise the integrity of the application

This represents a **critical access control failure**.

---

## Root Cause

The vulnerability exists because the application:

- Relies on obscurity instead of enforcing access control
- Exposes sensitive endpoints via `robots.txt`
- Performs no authentication or authorization checks on administrative functionality

Using `robots.txt` to hide sensitive paths does not provide any security.

---

## Recommended Remediation

To mitigate this vulnerability, the application should:

- Enforce authentication on all administrative endpoints
- Implement proper authorization checks to restrict admin access
- Avoid listing sensitive paths in `robots.txt`
- Treat `robots.txt` as public information, not a security mechanism

---

## Conclusion

The application is vulnerable to **Unprotected Admin Functionality**, allowing unauthenticated users to access the admin panel and perform privileged actions.

By discovering the admin endpoint through the publicly accessible `robots.txt` file and accessing it directly, an attacker can delete arbitrary users, demonstrating a severe **Broken Access Control** vulnerability.

---

<div align="center">

**Lab solved — admin functionality exposed**

</div>
