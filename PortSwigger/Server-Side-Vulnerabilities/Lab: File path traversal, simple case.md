<div align="center">

# File Path Traversal  
### Arbitrary File Read Vulnerability

![Vulnerability](https://img.shields.io/badge/Vulnerability-Path%20Traversal-red)
![Impact](https://img.shields.io/badge/Impact-High-orange)
![Lab](https://img.shields.io/badge/Source-PortSwigger-blue)

</div>

---

## Overview

This lab demonstrates a File Path Traversal vulnerability that allows an attacker to read arbitrary files from the server’s filesystem by manipulating a user-controlled parameter.

---

## Vulnerability Details

- **Type:** File Path Traversal  
- **Category:** Arbitrary File Read  
- **User Input:** `filename` parameter  
- **Trust Boundary:** Client → Server file system  

---

## Affected Endpoint

```http
GET /image?filename=53.jpg
```

The application dynamically loads product images based on the
[`filename`](https://portswigger.net/web-security/file-path-traversal) parameter supplied by the client.

---

## Conclusion

The application is vulnerable to
[**File Path Traversal**](https://owasp.org/www-community/attacks/Path_Traversal),
allowing attackers to read arbitrary files from the underlying operating system via the
[image loading endpoint](https://portswigger.net/web-security/file-path-traversal).
This issue results in serious
[information disclosure](https://owasp.org/www-community/vulnerabilities/Information_exposure)
and must be remediated.

---

<div align="center">

**Lab solved — vulnerability confirmed**

</div>
