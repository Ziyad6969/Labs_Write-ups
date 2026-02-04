<div align = "center">

# Solution – Admin Has the Power

</div>


> Spoiler warning: This file contains the full solution.

---

## 1. Initial Observation

After loading the login page, two important details can be observed:

1. A cookie named `role` is set with the value `support`.
2. While inspecting the page source / elements, hardcoded credentials are visible and appear to have been left unintentionally.

---

## 2. Authentication

Using the exposed credentials, we are able to log in successfully.  
However, after logging in, access to the flag is still restricted.

The application identifies the user as having a **support** role rather than an **admin** role.

---

## 3. Privilege Escalation via Cookies

Since the application relies on a client-side cookie (`role=support`) to determine user privileges, this indicates improper access control.

By modifying the cookie value from:

`role=support`
to:
`role=admin`

and refreshing the page, the application treats the user as an administrator.

---

## 4. Flag Retrieval

Once the role is changed to `admin`, access to the flag is granted and the challenge is completed successfully.

---

## Takeaways

- Client-side data such as cookies should never be trusted for authorization decisions.
- Sensitive roles must be validated on the server side.
- Exposed credentials and weak access control often lead to privilege escalation.
