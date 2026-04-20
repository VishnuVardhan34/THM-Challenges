# Infinity Shell

The challenge focuses on web application forensics, specifically identifying and tracing a malicious web shell left by an attacker.

Identifying the Web Application
```python
	cd /var/www/html
```

we discover a folder named CMSsite-master, indicating the presence of a CMS-based website.

Recognizing Common Web Exploitation Patterns
Googling the CMS, we learned that it’s known to have vulnerabilities. However, instead of chasing those down directly, we decided to go straight to the key folders that often expose issues, includes and img.

A listing of the project root confirms our focus:

```python
	cd /var/www/html/CMSsite-master
	ls
```

Discovering the Web Shell
Inside the /img folder, we find a suspicious file:

```python
	cd /var/www/html/CMSsite-master/img
	cat images.php
```

This was the attacker’s entry point. Based on logs and its content, it’s been used to execute a reverse shell. Time to inspect the logs for interaction with this file.

Extracting Web Shell Commands from Logs
Instead of going through each log line manually, we filter for entries that hit images.php, particularly with a ? parameter , indicating execution of base64-encoded commands.

```python
	cd /var/log/apache2/
	cat other_vhosts_access.log.1 | grep -r 'images.php?'
```

Extracting and Decoding Commands
From the logs, we pulled the following Base64-encoded values:

```
d2hvYW1pCg==
bHMK
ZWNobyAnVEhNe3N1cDNyXzM0c3lfdzNic2gzbGx9Jwo=
aWZjb25maWcK
Y2F0IC9ldGMvcGFzc3dkCg==
aWQK
```

## Final Flag:
```
THM{sup3r_34sy_w3bsh3ll}
```
