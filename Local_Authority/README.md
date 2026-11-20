# Local Authority

**Platform:** CYLab Academy  
**Category:** Web Exploitation  
**Difficulty:** Easy

## Challenge Description

The challenge provides a login webpage and asks us to discover a way to authenticate and retrieve the flag.

**Hint:**
> How is the password checked on this website?

## Solution

### Step 1: Inspect the Page Source

Open the challenge website and inspect its HTML source using `Ctrl + U`.

The source reveals the following JavaScript variables:

```javascript
window.username = "fashil";
window.password = "password";
```

It also references an external JavaScript file:

```html
<script src="secure.js"></script>
```

### Step 2: Analyze secure.js

Open the external JavaScript file:

```text
http://xebec.cylabacademy.net:13972/secure.js
```

The file contains the following function:

```javascript
function checkPassword(username, password)
{
  if (username === 'admin' && password === 'strongPassword098765')
  {
    return true;
  }
  else
  {
    return false;
  }
}
```

The function reveals the correct login credentials.

**Discovered Credentials:**

| Parameter | Value |
|---|---|
| Username | `admin` |
| Password | `strongPassword098765` |

### Step 3: Understand the Authentication Logic

The HTML page calls:

```javascript
loggedIn = checkPassword(window.username, window.password);
```

Initially, the page uses incorrect credentials (`fashil` and `password`), causing authentication to fail.

However, the actual credentials are hardcoded in the client-side JavaScript file.

### Step 4: Log In

Return to the challenge webpage and enter the discovered credentials:

- Username: `admin`
- Password: `strongPassword098765`

Authentication succeeds, and the website redirects or submits the hidden admin form.

### Step 5: Retrieve the Flag

After successfully logging in, the challenge reveals the flag.

## Flag

```text
academy{j5_15_7r4n5p4r3n7_df9583b6}
```

## Vulnerability Identified

**Client-Side Authentication – Hardcoded Credentials**

The application stores authentication credentials and password-checking logic in JavaScript files accessible to anyone visiting the website.

This allows an attacker to inspect the source code and discover valid credentials without exploiting the server.

## Key Takeaways

- Always inspect HTML source code during web exploitation challenges.
- Analyze externally linked JavaScript files.
- Never store credentials in client-side code.
- Client-side authentication can be bypassed or manipulated.
- Authentication and authorization must always be enforced on the server side.

**Challenge Status:** Solved ✅