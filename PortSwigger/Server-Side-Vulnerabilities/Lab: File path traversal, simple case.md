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
