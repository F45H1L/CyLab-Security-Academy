# WebDecode — Web Exploitation Challenge

## 1. Challenge Information
_______________________________________________________________________
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
|_____________________|________________________________________________|

### Challenge Objective

Find the hidden flag by inspecting the web application and its included resources.

### Provided Hints

1. Use the web inspector on other files included by the web page.
2. The flag may or may not be encoded.

---

# 2. Objective

The objective is to:

1. Access the target web application.
2. Inspect the application's HTML using browser Developer Tools.
3. Identify other pages/resources referenced by the website.
4. Navigate to the `about.html` page.
5. Inspect its HTML source.
6. Identify suspicious hidden data in an HTML attribute.
7. Determine whether the discovered value is encoded.
8. Decode the value.
9. Recover the challenge flag.

---

# 3. Initial Reconnaissance

Open the target:

```text
http://titan.picoctf.net:53697/
```

The website contains navigation links to multiple pages, including:

```text
index.html
about.html
contact.html
```

The first important observation is that the challenge specifically instructs the player to use the **web inspector**.

Therefore, browser Developer Tools should be used instead of relying only on the visible content.

---

# 4. Inspect the Web Application

Open Developer Tools using:

```text
F12
```

or:

```text
Ctrl + Shift + I
```

Navigate to:

```text
Elements
```

Inspect the HTML structure of the webpage.

Look for:

* HTML comments
* unusual attributes
* hidden elements
* JavaScript files
* CSS files
* linked HTML pages
* suspicious encoded strings

---

# 5. Investigate Other Pages

The page contains a navigation link to:

```text
about.html
```

Navigate to:

```text
http://titan.picoctf.net:53697/about.html
```

Then open Developer Tools again and inspect the page.

---

# 6. Analyze the About Page

The relevant HTML contains:

```html
<section class="about" notify_true="thisisanottheactualbase64contentfromthepage">
```

The suspicious element is:

```text
notify_true="..."
```

The value is:

```text
thisisanottheactualbase64contentfromthepage
```

---

# 7. Identify the Encoding

The discovered string does not immediately resemble a flag.

However, it has characteristics consistent with **Base64 encoding**.

The challenge's second hint states:

> The flag may or may not be encoded.

Therefore, Base64 decoding should be attempted.

---

# 8. Decode the String

Using a Linux terminal:

```bash
echo 'thisisanottheactualbase64contentfromthepage' | base64 -d
```

Expected output:

```text
picoCTF{this_is_not_the_actual_flag}
```

---

# 9. Final Flag

```text
picoCTF{this_is_not_the_actual_flag}
```

---

# 10. Attack/Solution Chain

```text
Target Website
      |
      v
Browser Developer Tools
      |
      v
Inspect HTML
      |
      v
Identify "About" Page
      |
      v
about.html
      |
      v
Inspect HTML
      |
      v
Find suspicious "notify_true" attribute
      |
      v
Extract attribute value
      |
      v
Recognize Base64 encoding
      |
      v
Decode Base64
      |
      v
picoCTF{this_is_not_the_actual_flag}
```

---

# 11. Tools Used

| Tool                    | Purpose                         |
| ----------------------- | ------------------------------- |
| Web Browser             | Access the challenge            |
| Browser Developer Tools | Inspect HTML and web resources  |
| Linux Terminal          | Decode the discovered string    |
| `base64`                | Decode the Base64-encoded value |

---

# 12. Key Finding

The flag was **not directly visible on the rendered webpage**.

Instead, it was hidden inside a custom HTML attribute:

```html
notify_true="..."
```

This demonstrates why inspecting the **DOM/source code** is important during web security testing.

The custom attribute contained a Base64-encoded value that decoded to the flag.

---

# 13. Lessons Learned

### Web Inspector

The browser's Developer Tools can reveal information that is not displayed on the webpage.

### HTML Attributes

Hidden information can be placed inside HTML attributes, including custom attributes.

### Base64

Base64 is an encoding mechanism, not encryption. If suspicious Base64-like data is discovered, it can easily be decoded.

### Web Enumeration

When a challenge provides multiple pages or resources, each should be inspected rather than focusing only on the homepage.

### Challenge Hint Analysis

The hints directly guided the methodology:

**Hint 1:** Investigate other files/pages.

**Hint 2:** Check whether discovered information is encoded.

---

# 14. Final Result

**Status:** Solved

**Flag:**

```text
picoCTF{this_is_not_the_actual_flag}
```

**Primary Technique:**

```text
HTML/DOM Inspection + Base64 Decoding
```