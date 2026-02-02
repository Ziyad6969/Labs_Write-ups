<div align="center">

# Unprotected Admin Functionality with Unpredictable URL  
### Broken Access Control Vulnerability

[![Vulnerability](https://img.shields.io/badge/Vulnerability-Access%20Control-red)](https://owasp.org/www-project-top-ten/2017/A5_2017-Broken_Access_Control)
[![CWE](https://img.shields.io/badge/CWE-284-blue)](https://cwe.mitre.org/data/definitions/284.html)
[![Source](https://img.shields.io/badge/Source-PortSwigger-orange)](https://portswigger.net/web-security/learning-paths/server-side-vulnerabilities-apprentice/access-control-apprentice/access-control/lab-unprotected-admin-functionality-with-unpredictable-url#)

</div>

---

## Overview

This lab demonstrates an **Unprotected Admin Functionality** vulnerability, a form of  
[Broken Access Control](https://owasp.org/www-project-top-ten/2017/A5_2017-Broken_Access_Control).

Although the administrative interface is located at an **unpredictable URL**, the application discloses its location within client-side code. The admin functionality itself lacks authentication and authorization checks, allowing any user to access it once the endpoint is known.

---

## Application Behavior

The application displays a public-facing home page for regular users.  
Administrative functionality is not visible by default and is intended to be shown only to administrators.

However, the application includes client-side JavaScript that conditionally adds a link to the admin panel based on a client-side variable.

---

## Discovery via Client-Side JavaScript

By inspecting the page source / HTML of the home page, the following JavaScript code can be observed:

```
html
<script>
var isAdmin = false;
if (isAdmin) {
   var topLinksTag = document.getElementsByClassName("top-links")[0];
   var adminPanelTag = document.createElement('a');
   adminPanelTag.setAttribute('href', '/admin-ar5wop');
   adminPanelTag.innerText = 'Admin panel';
   topLinksTag.append(adminPanelTag);
   var pTag = document.createElement('p');
   pTag.innerText = '|';
   topLinksTag.appendChild(pTag);
}
</script>
```

Although the isAdmin variable is set to false, the JavaScript code explicitly discloses the location of the administrative endpoint.

---

## Proof of Concept (PoC)

### Accessing the Admin Panel

Using the disclosed path, the admin panel can be accessed directly:

```
GET /admin-ar5wop HTTP/2
Host: <lab-id>.web-security-academy.net
```

The server responds with the administrative interface without requiring any authentication.

This confirms that the admin functionality is unprotected, despite being located at an unpredictable URL.
Exploitation

From the exposed admin panel, privileged actions are available.
The lab was successfully solved by using the admin functionality to delete the user carlos.
Impact

Successful exploitation of this vulnerability allows an attacker to:

- Discover hidden administrative endpoints through client-side code
- Access admin functionality without authentication
- Perform privileged actions such as deleting user accounts
- Compromise the integrity of the application

This represents a critical Broken Access Control vulnerability.

---

## Root Cause

The vulnerability exists because the application:

 - Relies on an unpredictable URL as a security mechanism
 - Exposes sensitive endpoints within client-side JavaScript
 - Performs no server-side authentication or authorization checks on admin functionality
 - Client-side controls and hidden URLs do not provide real security.

---

## Recommended Remediation

To mitigate this vulnerability, the application should:

- Enforce authentication and authorization on all administrative endpoints
- Avoid exposing sensitive URLs in client-side code
- Implement server-side access control checks for privileged actions
- Treat all client-side logic as attacker-controlled

---

## Conclusion

The application is vulnerable to Unprotected Admin Functionality with an Unpredictable URL.

By inspecting client-side JavaScript, an attacker can discover the hidden admin endpoint and access it directly. Since the admin functionality lacks proper access controls, privileged actions such as deleting user accounts can be performed by any user, resulting in a severe Broken Access Control issue.

<div align="center">

Lab solved — hidden admin panel exposed
</div> 
