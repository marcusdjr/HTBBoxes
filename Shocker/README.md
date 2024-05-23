# Shocker 😲

Lets begin with an NMAP scan to see what ports we see open.

    nmap -sC -sV 10.10.10.56

Results:
![image](https://github.com/marcusdjr/disney/assets/31329300/60980d16-1f76-488f-9304-3396c3f78469)

Ok so we see that we have port 80 open, if we go to the site we dont see much so lets try and spin up the good ole gobuster.

    gobuster dir -u http://10.10.10.56 -w /usr/share/dirb/wordlists/small.txt -o gobuster.txt -x sh  

So after running gobuster with find /cgi-bin/ but with a 403 status code, meaning we potentially don't have access to that sites contents.

![image](https://github.com/marcusdjr/disney/assets/31329300/83c339a3-4862-4d3e-859f-03220d36b2f1)

Lets to use gobuster on  http://10.10.10.56cgin-bin/ 

    gobuster dir -u http://10.10.10.56/cgi-bin/ -w /usr/share/dirb/wordlists/small.txt -o gobuster.txt -x sh 

Aha! /user.sh

![image](https://github.com/marcusdjr/disney/assets/31329300/1cb4233c-aa06-4b42-bfe3-3696fde11825)

When we go to /user.sh, it seems to download a file. If we run burpsuit and look at our request and responses we see the reason why is because it sees "Content-Type: text/x-sh" and doesnt know what to do with it so it just saves it as a file.

![image](https://github.com/marcusdjr/disney/assets/31329300/cc59f9d8-ff14-4c85-95c3-e21f5b89b210)

This 