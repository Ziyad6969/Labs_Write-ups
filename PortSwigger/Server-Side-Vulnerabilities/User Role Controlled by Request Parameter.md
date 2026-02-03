<div align="center">

# User Role Controlled by Request Parameter

### Privilege Escalation via Forgeable Cookie

[![Vulnerability](https://img.shields.io/badge/Vulnerability-Access%20Control%20Bypass-red)](https://owasp.org/www-community/Access_Control)
[![CWE](https://img.shields.io/badge/CWE-639-blue)](https://cwe.mitre.org/data/definitions/639.html)
[![Source](https://img.shields.io/badge/Source-PortSwigger-orange)](https://portswigger.net/web-security/access-control/lab-user-role-controlled-by-request-parameter)

</div>

---

## Overview

This lab demonstrates an **access control vulnerability** where administrative privileges are determined entirely by a **client-controlled request parameter**.
By manipulating a forgeable cookie, an attacker can escalate privileges and gain unauthorized access to the admin panel.

---

## Application Behavior

The application contains a restricted administrative panel located at:

```
/admin
```

Access to this panel is intended to be limited to administrators only.
However, the application determines whether a user is an administrator based on a cookie value supplied by the client.

The application provides valid user credentials:

```
Username: wiener
Password: peter
```

---

## Authentication & Initial Request

After logging in as a normal user and trying to access `/admin`.

### Request to Admin Panel

```
GET /admin HTTP/2
Host: <lab-id>.web-security-academy.net
Cookie: Admin=false; session=<session-cookie>
```

### Observation

* The request includes a cookie named `Admin`
* The value is set to `false`
* The server trusts this value to determine authorization

This indicates that **user roles are controlled entirely by a client-side parameter**, which is inherently insecure.

---

## Proof of Concept (PoC)

### Privilege Escalation

The vulnerability is exploited by modifying the `Admin` cookie value.

### Modified Request

```
GET /admin HTTP/2
Host: <lab-id>.web-security-academy.net
Cookie: Admin=true; session=<session-cookie>
```

### Result

* The server responds with **HTTP 200 OK**
* The admin panel becomes accessible
* No additional server-side verification is performed

This confirms a successful **privilege escalation** from a standard user to administrator.

---

## Exploitation

Once authenticated as an administrator, the admin panel provides functionality to manage users.

### Deleting a User

The attacker can use the admin interface to delete the user **carlos**, completing the lab objective.

> `Note:` The `Admin=true` cookie must be preserved across **all requests**, including page navigation and the delete action, for the exploit to succeed.

---

## Impact

Successful exploitation allows an attacker to:

* Gain unauthorized administrative access
* Perform privileged actions such as deleting users
* Fully compromise application integrity
* Escalate attacks to data destruction or account takeover

This represents a **critical access control failure**.

---

## Root Cause

The vulnerability exists because the application:

* Relies on client-controlled cookies for authorization
* Performs no server-side role validation
* Trusts untrusted input for access control decisions
* Does not bind user roles to session data securely

This violates fundamental access control principles outlined by **OWASP**.

---

## Recommended Remediation

To prevent this vulnerability, the application should:

* Enforce access control checks on the server side
* Store user roles securely within the session
* Ignore client-supplied role indicators
* Implement proper authorization middleware for restricted endpoints

### Additional Guidance

* [OWASP Access Control](https://owasp.org/www-community/Access_Control)
* [CWE-639: Authorization Bypass Through User-Controlled Key](https://cwe.mitre.org/data/definitions/639.html)

---

## Conclusion

The application is vulnerable to **access control bypass** due to improper reliance on a user-controlled request parameter.
By modifying the `Admin` cookie value from `false` to `true`, a normal user can gain administrative privileges and perform restricted actions such as deleting other users.

This vulnerability highlights the importance of **server-side authorization enforcement** and must be remediated to prevent full application compromise.

---

<div align="center">

**Lab solved — privilege escalation confirmed**

</div>

---
