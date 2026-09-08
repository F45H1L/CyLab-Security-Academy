# WebDecode — Security Test Report

## 1. Test Information

| Field               | Details                                        |
| ------------------- | ---------------------------------------------- |
| Challenge Name      | WebDecode                                      |
| Category            | Web Exploitation                               |
| Platform            | picoCTF / CyLab Challenge                      |
| Target              | `http://titan.picoctf.net:53697/`              |
| Testing Type        | Web Application Source/DOM Inspection          |
| Difficulty          | Easy                                           |
| Testing Environment | Web Browser / Developer Tools / Linux Terminal |
| Objective           | Locate and decode the hidden flag              |
| Status              | **Successfully Completed**                     |

---

## 2. Objective

The objective of this test was to identify information hidden within the target web application by inspecting the webpage and its associated resources.

The challenge provided the following hints:

1. Use the web inspector on other files included by the web page.
2. The flag may or may not be encoded.

The primary goal was to locate the hidden flag and determine whether additional decoding was required.

---

## 3. Scope

Testing was limited to the challenge web application:

```text
http://titan.picoctf.net:53697/
```

The following activities were performed:

* Webpage inspection
* HTML/DOM analysis
* Navigation to linked pages
* Identification of suspicious HTML attributes
* Identification of encoded data
* Base64 decoding

No attacks against systems outside the provided CTF environment were performed.

---

## 4. Tools Used

| Tool             | Purpose                                |
| ---------------- | -------------------------------------- |
| Web Browser      | Access the challenge application       |
| Developer Tools  | Inspect HTML/DOM and webpage resources |
| Linux Terminal   | Decode the discovered Base64 string    |
| `base64` utility | Decode the identified encoded value    |

---

# 5. Test Methodology

## Step 1 — Access the Target

The target application was accessed using a web browser:

```text
http://titan.picoctf.net:53697/
```

The application presented a webpage containing navigation links.

---

## Step 2 — Inspect the Webpage

The browser's Developer Tools were opened using:

```text
F12
```

or:

```text
Ctrl + Shift + I
```

The **Elements** tab was selected to inspect the HTML structure.

The navigation section contained links to:

```html
<a href="index.html">Home</a>
<a href="about.html">About</a>
<a href="contact.html">Contact</a>
```

Based on the challenge hint, the linked pages were investigated.

---

## Step 3 — Access the About Page

The **About** page was opened:

```text
http://titan.picoctf.net:53697/about.html
```

The HTML source/DOM was inspected using Developer Tools.

---

## Step 4 — Identify Suspicious HTML Attribute

During inspection, the following HTML element was identified:

```html
<section class="about"
notify_true="cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfMjgzZTYyZmV9">
```

The attribute:

```text
notify_true
```

is not a standard attribute required for the `<section>` element.

Its value appeared to contain an encoded string:

```text
cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfMjgzZTYyZmV9
```

This was considered a potential location for the hidden flag.

---

## Step 5 — Determine the Encoding

The discovered string contained characters consistent with **Base64 encoding**.

The string was therefore tested using the Linux `base64` decoding utility.

Command executed:

```bash
echo 'cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfMjgzZTYyZmV9' | base64 -d
```

---

## Step 6 — Decode the Data

The decoding operation returned:

```text
picoCTF{web_successfully_d3c0ded_283e62fe}
```

The returned value matched the expected picoCTF flag format.

---

# 6. Finding

### Finding: Hidden Base64-Encoded Flag in HTML DOM

**Severity:** Informational / CTF-specific

A Base64-encoded string containing the challenge flag was embedded directly within a custom HTML attribute:

```html
notify_true="cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfMjgzZTYyZmV9"
```

Because the information was present in the client-side HTML, it could be retrieved without authentication or server-side exploitation.

### Impact

In a real-world application, embedding sensitive information such as credentials, API keys, session information, or other secrets in client-accessible HTML would expose those values to users.

In this CTF, however, the behavior is intentional and forms the basis of the challenge.

---

# 7. Evidence

### Evidence 1 — HTML Element

```html
<section class="about"
notify_true="cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfMjgzZTYyZmV9">
```

### Evidence 2 — Encoded Value

```text
cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfMjgzZTYyZmV9
```

### Evidence 3 — Decoding Command

```bash
echo 'cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfMjgzZTYyZmV9' | base64 -d
```

### Evidence 4 — Decoded Result

```text
picoCTF{web_successfully_d3c0ded_283e62fe}
```

---

# 8. Attack/Testing Path

```text
Target Web Application
        |
        v
Inspect Webpage
        |
        v
Identify Navigation Links
        |
        v
Open about.html
        |
        v
Inspect HTML / DOM
        |
        v
Identify notify_true Attribute
        |
        v
Extract Attribute Value
        |
        v
Identify Base64 Encoding
        |
        v
Decode Base64
        |
        v
Recover Flag
```

---

# 9. Result

The hidden flag was successfully identified and decoded.

**Final Flag:**

```text
picoCTF{web_successfully_d3c0ded_283e62fe}
```

**Test Result:** PASS

---

# 10. Conclusion

The WebDecode challenge was successfully completed through client-side web inspection.

The investigation demonstrated that hidden information can be present within the HTML DOM even when it is not visibly rendered on the webpage. By inspecting the linked `about.html` page, a suspicious custom attribute named `notify_true` was identified. Its value was Base64 encoded and was successfully decoded to recover the challenge flag.

The challenge demonstrates the importance of examining:

* HTML source
* DOM attributes
* Linked pages
* CSS files
* JavaScript files
* Other resources loaded by a webpage
* Encoded data embedded within client-side resources

No exploitation of the underlying server was required to solve this challenge.

**Final Status: Successfully Solved**