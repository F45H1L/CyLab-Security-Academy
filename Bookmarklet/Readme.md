# Bookmarklet


## Step 1 — Open the Challenge

Open the challenge webpage.

The page displays:

```text
Welcome to my flag distribution website!
If you're reading this, your browser has succesfully received the flag.
Here's a bookmarklet for you to try:
```

Below this, the page provides a JavaScript bookmarklet.

---

## Step 2 — Identify the Bookmarklet

The provided code is:

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

A bookmarklet is JavaScript beginning with:

```text
javascript:
```

Instead of opening a webpage, clicking the bookmark executes the JavaScript.

---

## Step 3 — Understand the Code

The encrypted flag is stored in:

```javascript
var encryptedFlag = "àÒÆÞ¦È¬ëÙ£ÖÓÚåÛÑ¢ÕÓÔÅÐÙí";
```

The decryption key is:

```javascript
var key = "picoctf";
```

The code loops through every character of the encrypted flag:

```javascript
for (var i = 0; i < encryptedFlag.length; i++)
```

For each character, it subtracts the corresponding character from the repeating key:

```javascript
encryptedFlag.charCodeAt(i) -
key.charCodeAt(i % key.length)
```

The result is converted back into a character using:

```javascript
String.fromCharCode(...)
```

Finally, the decrypted flag is displayed with:

```javascript
alert(decryptedFlag);
```

---

## Step 4 — Run the JavaScript

Open the browser's Developer Tools:

```text
F12
```

Then select:

```text
Console
```

Remove the `javascript:` prefix from the bookmarklet and paste the remaining JavaScript into the console.

For example:

```javascript
(function() {
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

Execute it.

An alert box will display the decrypted flag.

---

## Step 5 — Flag

```text
picoCTF{p@g3_turn3r_1d1ba7e0}
```

## Key Takeaways

* A **bookmarklet** is a bookmark containing JavaScript.
* Clicking a bookmarklet executes the JavaScript on the current webpage.
* Browser **Developer Tools → Console** can also execute JavaScript.
* The challenge's JavaScript contains an encrypted flag and the key required to decrypt it.
* The decryption is performed using character codes and the repeating key `picoctf`.
