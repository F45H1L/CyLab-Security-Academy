# Bookmarklet - Report

## Challenge Information

| Field          | Details          |
| -------------- | ---------------- |
| **Challenge**  | Bookmarklet      |
| **Category**   | Web Exploitation |
| **Platform**   | CyLab            |
| **Difficulty** | Not specified    |
| **Status**     | Solved           |

---

## 1. Challenge Description

> Why search for the flag when I can make a bookmarklet to print it for me?

### Hints

1. A bookmarklet is a bookmark that runs JavaScript instead of loading a webpage.
2. What happens when you click a bookmarklet?
3. Web browsers have other ways to run JavaScript too.

---

# 2. Objective

The objective was to analyze the bookmarklet provided on the webpage and execute its JavaScript to reveal the hidden flag.

---

# 3. Initial Reconnaissance

After opening the challenge webpage, the following content was displayed:

```text
Welcome to my flag distribution website!
If you're reading this, your browser has succesfully received the flag.
Here's a bookmarklet for you to try:
```

A JavaScript bookmarklet was provided directly on the page.

The bookmarklet started with:

```text
javascript:
```

This confirmed the first hint: the provided bookmark was actually JavaScript intended to be executed by the browser.

---

# 4. Bookmarklet Analysis

The bookmarklet contained the following JavaScript:

```javascript
javascript:(function() {
    var encryptedFlag = "àÒÆÞ¦È¬ëÙ£ÖÓÚåÛÑ¢ÕÓÔÅÐÙí";
    var key = "picoctf";
    var decryptedFlag = "";

    for (var i = 0; i < encryptedFlag.length; i++) {
        decryptedFlag += String.fromCharCode(
            (encryptedFlag.charCodeAt(i) -
            key.charCodeAt(i % key.length) + 256) % 256
        );
    }

    alert(decryptedFlag);
})();
```

---

# 5. Understanding the Code

## Encrypted Flag

The encrypted data was stored in:

```javascript
var encryptedFlag = "àÒÆÞ¦È¬ëÙ£ÖÓÚåÛÑ¢ÕÓÔÅÐÙí";
```

This string did not directly contain the readable flag.

---

## Encryption Key

The script defined:

```javascript
var key = "picoctf";
```

The key is repeatedly used against the encrypted string.

The expression:

```javascript
key.charCodeAt(i % key.length)
```

makes the key repeat when the encrypted string is longer than the key.

For example:

```text
p i c o c t f p i c o c t f ...
```

---

# 6. Decryption Logic

The script processes every character using a `for` loop:

```javascript
for (var i = 0; i < encryptedFlag.length; i++)
```

For every character, it obtains the character's numeric Unicode value using:

```javascript
encryptedFlag.charCodeAt(i)
```

The corresponding key character is obtained using:

```javascript
key.charCodeAt(i % key.length)
```

The key value is then subtracted from the encrypted character value:

```javascript
encryptedFlag.charCodeAt(i) -
key.charCodeAt(i % key.length)
```

The script adds `256` and applies modulo `256`:

```javascript
(
    encryptedFlag.charCodeAt(i) -
    key.charCodeAt(i % key.length) +
    256
) % 256
```

This keeps the resulting value within the byte range.

Finally, the numeric value is converted back into a character:

```javascript
String.fromCharCode(...)
```

The resulting characters are appended to:

```javascript
decryptedFlag
```

---

# 7. Executing the Bookmarklet

Instead of creating an actual browser bookmark, the third hint suggested that the JavaScript could be executed through another browser feature.

We opened:

```text
F12 → Developer Tools → Console
```

The `javascript:` prefix was removed, and the JavaScript code was executed in the browser console.

The final line of the script was:

```javascript
alert(decryptedFlag);
```

Therefore, successful execution produced the decrypted flag in an alert box.

---

# 8. Result

The JavaScript successfully decrypted the encrypted string and displayed the flag.

### Flag

```text
picoCTF{p@g3_turn3r_1d1ba7e0}
```

---

# 9. Solution Flow

```text
Open Challenge
      │
      ▼
Read Challenge Description & Hints
      │
      ▼
Identify Bookmarklet
      │
      ▼
Inspect JavaScript
      │
      ▼
Find Encrypted Flag
      │
      ▼
Identify Key: "picoctf"
      │
      ▼
Analyze Decryption Loop
      │
      ▼
Open Browser Developer Tools
      │
      ▼
Open Console
      │
      ▼
Execute Bookmarklet JavaScript
      │
      ▼
JavaScript Decrypts Flag
      │
      ▼
Alert Displays Flag
      │
      ▼
picoCTF{p@g3_turn3r_1d1ba7e0}
```

---

# 10. Key Takeaways

* A **bookmarklet** is a browser bookmark containing JavaScript instead of a normal URL.
* Bookmarklets can execute JavaScript in the context of the current webpage.
* Browser Developer Tools provide a JavaScript console that can be used to execute scripts.
* Client-side JavaScript should always be inspected during web exploitation challenges.
* `charCodeAt()` can convert characters into numeric character codes.
* `String.fromCharCode()` converts numeric character codes back into characters.
* A repeating key can be used to perform simple character-code transformations.
* Sensitive information hidden only through client-side JavaScript can often be recovered by inspecting and executing the code.

---

# 11. Conclusion

The challenge did not require searching the website for a hidden flag manually. Instead, the flag was protected by a simple JavaScript transformation inside a bookmarklet.

By inspecting the bookmarklet, identifying the encrypted string and the `picoctf` key, and executing the JavaScript through the browser console, the encrypted value was successfully decrypted.

**Final Flag:**

```text
picoCTF{p@g3_turn3r_1d1ba7e0}
```
