# Bookmarklet - Planning Document

## 1. Challenge Overview

**Category:** Web Exploitation

**Challenge Name:** Bookmarklet

### Challenge Description

> Why search for the flag when I can make a bookmarklet to print it for me?

### Provided Hints

1. A bookmarklet is a bookmark that runs JavaScript instead of loading a webpage.
2. What happens when you click a bookmarklet?
3. Web browsers have other ways to run JavaScript too.

---

# 2. Objective

The main objective is to:

1. Open the challenge webpage.
2. Inspect the webpage and its JavaScript.
3. Identify the provided bookmarklet.
4. Understand what JavaScript the bookmarklet executes.
5. Execute the JavaScript using an appropriate browser method.
6. Observe the output.
7. Extract the flag.

---

# 3. Initial Reconnaissance

Start by opening the challenge URL in a web browser.

Observe the webpage carefully.

Look for:

* Visible text
* Links
* Buttons
* JavaScript snippets
* Bookmarklets
* Hidden elements
* External JavaScript files

Do **not** immediately modify anything.

The goal of the first step is to understand how the page is delivering the flag.

---

# 4. Analyze the Hints

## Hint 1

> A bookmarklet is a bookmark that runs JavaScript instead of loading a webpage.

This suggests that the challenge contains a URL-like JavaScript payload beginning with:

```text
javascript:
```

A normal bookmark points to a webpage:

```text
https://example.com
```

A bookmarklet instead contains JavaScript:

```text
javascript:alert("Hello")
```

Therefore, search the page for a bookmarklet.

---

## Hint 2

> What happens when you click a bookmarklet?

Clicking a bookmarklet causes the browser to execute the JavaScript associated with it.

Therefore, determine:

* What JavaScript is executed?
* What variables are created?
* What functions are called?
* What output does the script produce?

---

## Hint 3

> Web browsers have other ways to run JavaScript too.

This suggests that creating an actual bookmark is not necessarily required.

A browser's **Developer Tools Console** can also execute JavaScript.

Therefore, the likely workflow is:

```text
Challenge Page
      ↓
Find Bookmarklet
      ↓
Read JavaScript
      ↓
Understand JavaScript
      ↓
Browser Developer Tools
      ↓
Console
      ↓
Execute JavaScript
      ↓
Observe Output
      ↓
Flag
```

---

# 5. Inspect the Page

Use one or more of the following methods.

### Method 1 — View Page Source

Press:

```text
Ctrl + U
```

Search the source for:

```text
javascript:
```

Also search for:

```text
flag
```

and:

```text
script
```

---

### Method 2 — Developer Tools

Open Developer Tools:

```text
F12
```

Then inspect:

```text
Elements
```

and:

```text
Sources
```

Look for JavaScript embedded directly in the HTML or loaded from an external `.js` file.

---

### Method 3 — Search Loaded Sources

In Developer Tools, use:

```text
Ctrl + Shift + F
```

Search for terms such as:

```text
javascript:
```

```text
flag
```

```text
alert
```

```text
decrypt
```

The exact terms may vary, so the first priority is locating the bookmarklet.

---

# 6. Identify the Bookmarklet

Once a string beginning with:

```text
javascript:
```

is found, treat it as the primary target.

Record the complete JavaScript.

For example:

```javascript
javascript:(function() {
    ...
})();
```

Do not change the code yet.

---

# 7. Analyze the JavaScript

Read the code from top to bottom.

Identify:

### Variables

Look for variables containing:

* Encrypted data
* Keys
* Flags
* Encoded strings
* User input

For example:

```javascript
var encryptedFlag = "...";
var key = "...";
```

---

### Loops

Look for loops such as:

```javascript
for (...)
```

Determine whether the script processes the encrypted data character by character.

---

### Character Operations

Look for functions such as:

```javascript
charCodeAt()
```

and:

```javascript
String.fromCharCode()
```

These often indicate that the script is converting between characters and numeric character codes.

---

### Output

Look for functions such as:

```javascript
alert()
```

```javascript
console.log()
```

```javascript
document.write()
```

or changes to the webpage.

The output mechanism will tell us where the flag is expected to appear.

---

# 8. Execute the Bookmarklet

There are two possible approaches.

## Approach A — Use the Bookmarklet

Create a browser bookmark and place the JavaScript beginning with:

```text
javascript:
```

in the bookmark's URL/location field.

Return to the challenge page and click the bookmark.

Observe the result.

---

## Approach B — Use Developer Tools

Because of Hint 3, the easier approach is usually the browser console.

Open:

```text
F12 → Console
```

Take the JavaScript from the bookmarklet.

If necessary, remove:

```text
javascript:
```

from the beginning.

Then execute the remaining JavaScript in the console.

---

# 9. Observe the Result

After execution, check for:

* Alert boxes
* Console output
* Modified webpage content
* Other visible output

If the script successfully performs the intended operation, the flag should be displayed.

Record the flag exactly as shown.

Pay attention to:

* Uppercase/lowercase characters
* Numbers
* Special characters
* Curly braces
* Underscores

---

# 10. Verify the Flag

Before submitting, verify that the captured value:

* Matches the platform's expected flag format.
* Was produced by the challenge's JavaScript.
* Has not been accidentally modified while copying.

For example, if the platform uses a format similar to:

```text
picoCTF{...}
```

make sure the complete value is copied.

---

# 11. Expected Investigation Flow

The complete planned workflow is:

```text
                START
                  │
                  ▼
          Open Challenge Page
                  │
                  ▼
           Read Challenge Text
                  │
                  ▼
            Analyze Hints
                  │
                  ▼
       Look for "javascript:"
                  │
                  ▼
         Find Bookmarklet Code
                  │
                  ▼
       Inspect JavaScript Logic
                  │
                  ▼
      Identify Encoded/Encrypted Data
                  │
                  ▼
          Identify Decryption
               Operation
                  │
                  ▼
       Open Browser DevTools
                  │
                  ▼
              Console
                  │
                  ▼
        Execute Bookmarklet JS
                  │
                  ▼
          Observe Program Output
                  │
                  ▼
             Extract Flag
                  │
                  ▼
              Verify Flag
                  │
                  ▼
                 END
```

---

# 12. Tools Required

Only basic browser functionality should be required:

| Tool            | Purpose                             |
| --------------- | ----------------------------------- |
| Web Browser     | Access the challenge                |
| View Source     | Inspect HTML                        |
| Developer Tools | Inspect webpage and JavaScript      |
| Console         | Execute JavaScript                  |
| Text Editor     | Optional, for analyzing copied code |

No external exploitation tools should be necessary for the initial solution.

---

# 13. Learning Objectives

After completing the challenge, we should understand:

* What a bookmarklet is.
* How bookmarklets execute JavaScript.
* How JavaScript can be executed through browser Developer Tools.
* How to recognize simple JavaScript encoding/encryption logic.
* How `charCodeAt()` works.
* How `String.fromCharCode()` works.
* How a repeating key can be used to transform character codes.
* How to inspect client-side JavaScript during web exploitation challenges.

---

# 14. Notes for the Write-Up

When documenting the completed challenge, capture:

1. Challenge description
2. Hints
3. Initial webpage
4. Location of the bookmarklet
5. Bookmarklet source code
6. Explanation of the JavaScript
7. Method used to execute it
8. Decrypted output
9. Final flag

The write-up should explain **why** the solution works rather than only showing the final flag.