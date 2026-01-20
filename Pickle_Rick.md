For this challenge,
First check all the open ports using 'nmap', no interesting results, So curl the given IP(in my case it is 10.49.173.77) inspect the output there is a assest folder, inspect it by appending it to IP, nothing interesting except u will get the username and explore the robots.txt to get the password, Now you have both username and password now you need to find the login page, here i just randomly added /portal to URL and it worked enter the username and password and after that you will need to enter some bash commands, to get the flags.
explore the root of the system by using the 'sudo ls ../../../*' and after that explore the /user/rick to discover the SecretRecipie 1 and 2 and three can be found in /root/3rd.txt to get the final flag.
flag1: mr.meeseek hair
flag2: 1 jerry tear
flag3: fleeb juice
