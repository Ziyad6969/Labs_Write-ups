# Solution – Cool Name Effect

> Spoiler warning: This file contains the full solution.

---

### 1. Initial Observation

The page takes user input and displays it in a curved arch-like format.
The lab mentions improper filtering, suggesting a possible XSS vulnerability.

---

### 2. Inspecting the Source Code

Pressing **F12** shows a large obfuscated JavaScript block:

```javascript
eval(function(p,a,c,k,e,d){...})
```

---

### 3. Deobfuscation

Using an online deobfuscation tool, the code is readable.
It defines a function called `newAlert` that overrides:

* `window.alert`
* `window.confirm`
* `window.prompt`

Inside, an array is defined:

```javascript
var z = ['y','o','u','r',' ','f','l','a','g',' ','i','s',':'];
```

Since `window.alert` is overwritten, calling `alert()` will actually execute `newAlert()`.

---

### 4. Testing for XSS

Injecting the payload:

```html
<script>alert(1)</script>
```

was replaced with:

```html
<Forbidden>alert(1)</Forbidden>
```

The lowercase `script` tag is blocked.

---

### 5. Filter Bypass

Using a case variation:

```html
<ScRIpt>alert(1)</ScRIpt>
```

worked successfully, bypassing the filter.

---

### 6. Flag Retrieval

Calling:

```html
<ScRIpt>alert()</ScRIpt>
```

executes the custom alert function and reveals the flag.
The challenge is completed successfully.

---

### Takeaways

* Simple string replacement is not secure.
* Case-sensitive filters can be bypassed.
* Overriding native JS functions doesn’t prevent XSS.
* Client-side filtering is unreliable for security.
