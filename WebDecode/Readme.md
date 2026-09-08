## Step 1 — Open the challenge

Open the challenge URL in your browser:

http://titan.picoctf.net:53697/

You should see a simple webpage.

## Step 2 — Open Developer Tools

Right-click anywhere on the webpage and select:

Inspect

Or use:

Chrome/Edge: F12 or Ctrl + Shift + I
Firefox: F12 or Ctrl + Shift + I

This opens the Developer Tools.

## Step 3 — Look at the HTML

Select the Elements tab.

You'll see the HTML structure of the current page.

Look for navigation links such as:

<a href="index.html">Home</a>
<a href="about.html">About</a>
<a href="contact.html">Contact</a>

The challenge hint says:

Use the web inspector on other files included by the web page.

So we should investigate the other pages.

## Step 4 — Go to the About page

Click:

About

Alternatively, you can navigate to:

http://titan.picoctf.net:53697/about.html

Or simply on your terminal enter:
```bash
curl -s http://titan.picoctf.net:53697/about.html
```

## Step 5 — Inspect the About page

With the About page open, right-click → Inspect.

Go to:

Elements

You should eventually find something similar to:
```html
<section class="about" notify_true="cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfMjgzZTYyZmV9">
```
The interesting part is:
```
notify_true="cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfMjgzZTYyZmV9"
```
## Step 6 — Recognize that the value is encoded

The string:
```
cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfMjgzZTYyZmV9
```
looks like Base64.

A clue is that Base64 commonly contains characters like: A-Z, a-z, 0-9, +, /, =

and this string has the characteristic Base64-looking format.

## Step 7 — Decode it

You can use the Linux terminal.

Run:
```bash
echo 'cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfMjgzZTYyZmV9' | base64 -d
```
The output is:
```bash
picoCTF{web_successfully_d3c0ded_283e62fe}
```
## Step 8 — Submit the flag

Copy:
```bash
picoCTF{web_successfully_d3c0ded_283e62fe}
```
and submit it to the challenge.