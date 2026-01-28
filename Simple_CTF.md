# Challenge description
Get different flags from the Target IP address
# Step1:
Scanning the IP using nmap 
nmap -sC -sV <Target IP>
# Task1: Found no.of services that are running under port 1000?
Ans: 2
# Task2: What is running on the Higher Port?
Ans: SSH
For the next task visit the website and proceed
Now upon visiting we came to know that it is running on Apache2 server, to know additional pages of the website use gobuster command
gobuster dir -u <Target IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 100
/simple is found.
explore /simple directory
At the footer of the page you will find "CMS made simple"
Chack that in Exploit-DB site to get the CVE number
# Task3: What's the CVE you're using aganist the appliaction?
Ans: CVE-2019-9053
# Task4: TO what kind of vulnerability is the application vulnerable?
Ans: SQLi
For the further tasks we need to exploit the vulnerability.
In the Exploit-DB site there is attached exploit script, Download it and run it in your local machine or AttackBox it will ask for url nad wordlist, so
python exploit.py -u <Target IP> --crack -w /usr/share/wordlists/rockyou.txt
username: mitch
password: secret
# Task5: What is the password?
Ans: secret
# Task6: Where can you login with the details obtained?
Ans: SSh
ssh mitch@[Target IP] -p 2222
For the next task exploit the target machine more, for that chekc with id command no passwd required, you need to get into root directory to get the file user.txt, use command  sudo -l to check for the commands that can be run as root
then, sudo vim -c /:!/bin/bash', to enter as user then ls and cat to get the flag
# Task7: What's the user flag?
Ans: It will be in user.txt
# Task8: Is there any other user in the home directory? What's it's name?
use cd.. and then ls
Ans: sunbath
# Task9: What can you leverage to spawn a privileged shell?
use sudo -l to check for the commands that can be run as root
then, sudo vim -c /:!/bin/bash', to enter as user then ls and cat to get the flag
Ans:  vim
Then cd /root and ls
# Task10: What's the root flag?
Ans: It will be in root.txt
